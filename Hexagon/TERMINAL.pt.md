<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/stars/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/downloads/hexagonix/Hexagon/total.svg)
![](https://img.shields.io/github/release/hexagonix/Hexagon.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

<div align="justify">


# Terminal do console do Hexagon

Os bytes escritos em uma tty com `hx.write` passam por um pequeno emulador de terminal no kernel, o papel que o `teken` cumpre no console `vt(4)` do FreeBSD. Um programa move o cursor, limpa o console e muda as cores escrevendo sequências de escape, em vez de chamar uma função do kernel para cada uma, e assim o mesmo programa funciona para onde quer que a sua saída vá. As macros do [`console.s`](https://github.com/hexagonix/lib/blob/main/fasm/console.s) da libasm, como `fputs`, `gotoxy` e `setConsoleColor`, já escrevem desta forma.

Cada terminal virtual tem o seu próprio cursor, as suas cores e a sua sequência em andamento, então uma sequência dividida entre duas escritas continua sendo entendida. Um processo pode escrever na tty de outro processo, o que desenha naquele terminal virtual, visível ou não.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Caracteres de controle

| Byte | Nome | Efeito |
|:----:|:----:|:------:|
| `0Ah` | LF | Próxima linha, na primeira coluna |
| `0Dh` | CR | Primeira coluna da linha atual |
| `08h` | BS | Uma coluna para trás, sem apagar |
| `09h` | TAB | Um espaço |
| `1Bh` | ESC | Inicia uma sequência de escape |

Qualquer outro caractere de controle, BEL e DEL entre eles, não desenha nada.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Sequências de escape

CSI representa ESC seguido de `[`. Os parâmetros são números decimais separados por `;`, e um parâmetro vazio assume o seu valor padrão. Linhas e colunas contam a partir de 1.

| Sequência | Efeito |
|:---------:|:------:|
| `CSI n A` | Cursor n linhas acima, 1 por padrão, parando na borda |
| `CSI n B` | Cursor n linhas abaixo |
| `CSI n C` | Cursor n colunas à frente |
| `CSI n D` | Cursor n colunas para trás |
| `CSI linha ; coluna H` | Cursor para aquela linha e coluna, também `f` |
| `CSI J` | Limpa do cursor até o fim da tela |
| `CSI 2 J` | Limpa a tela inteira, também `3 J` |
| `CSI K` | Limpa do cursor até o fim da linha |
| `CSI 1 K` | Limpa do início da linha até o cursor |
| `CSI 2 K` | Limpa a linha inteira |
| `CSI ... m` | Cores, como abaixo |

Limpar nunca move o cursor. Uma sequência desconhecida é consumida inteira e não desenha nada, assim como as privadas, como `CSI ? 25 l`, e as que têm byte intermediário, como `ESC ( B`.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Cores

| Parâmetro de `CSI ... m` | Efeito |
|:-------------------------:|:------:|
| `0` | Volta às cores do tema |
| `30` a `37` | Cor da fonte, da paleta abaixo |
| `90` a `97` | Cor da fonte clara |
| `40` a `47` | Cor de fundo |
| `100` a `107` | Cor de fundo clara |
| `38 ; 2 ; r ; g ; b` | Cor da fonte em RGB |
| `48 ; 2 ; r ; g ; b` | Cor de fundo em RGB |
| `39` | Cor da fonte do tema |
| `49` | Cor de fundo do tema |

A paleta, em ordem: preto, vermelho, verde, amarelo, azul, magenta, ciano e branco, como `000000h`, `AA0000h`, `00AA00h`, `AA5500h`, `0000AAh`, `AA00AAh`, `00AAAAh` e `AAAAAAh`. As claras são `555555h`, `FF5555h`, `55FF55h`, `FFFF55h`, `5555FFh`, `FF55FFh`, `55FFFFh` e `FFFFFFh`.

Uma sequência comporta no máximo 5 parâmetros, então uma cor de fonte e uma de fundo em RGB vão em duas sequências.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Leitura de uma tty

O que `hx.read` retorna de uma tty depende do modo daquela tty, que pertence à tty e não aos seus processos, como o `c_lflag` do `termios`. O comando 4 de `hx.ioctl` o retorna em EAX e o comando 5 o define a partir de ECX:

| Bit | Nome | Efeito |
|:---:|:----:|:------:|
| `01h` | Canônico | Linhas, reunidas e editadas pelo kernel |
| `02h` | Eco | O que é digitado aparece enquanto se digita |

Uma tty começa com os dois, o modo "cooked" do BSD. A biblioteca [`tty.s`](https://github.com/hexagonix/lib/blob/main/fasm/tty.s) da libasm tem `ttyGetMode` e `ttySetMode` para eles.

### Modo canônico

Uma leitura espera uma linha inteira e a retorna com o seu `0Ah`. Uma linha maior que o buffer de uma leitura é retornada nas leituras seguintes. Uma linha comporta no máximo 255 bytes, contando o seu fim.

| Tecla | Byte | Efeito |
|:-----:|:----:|:------:|
| Enter | `0Ah` | Termina a linha |
| Backspace | `08h` ou `7Fh` | Apaga o último caractere |
| Ctrl+W | `17h` | Apaga a última palavra |
| Ctrl+U | `15h` | Apaga a linha inteira |
| Ctrl+D | `04h` | Retorna o que foi digitado, sem fim de linha. Em uma linha vazia a leitura retorna 0, o fim do arquivo |

As setas, Home, End e Delete não fazem nada em uma linha. Editar em qualquer ponto dela fica a cargo do programa, como faz a `readLine` do `tty.s` sobre o modo cru.

### Modo cru

Uma leitura retorna o que é digitado conforme chega, pelo menos um byte. Ctrl com uma letra dá o seu caractere de controle, de `01h` a `1Ah`, exceto Ctrl+C, que encerra o processo em primeiro plano. Teclas sem caractere próprio viram as sequências que um terminal envia para elas, enquanto o teclado numérico digita os seus números.

| Tecla | Bytes |
|:-----:|:-----:|
| Para cima | `ESC [ A` |
| Para baixo | `ESC [ B` |
| Para a direita | `ESC [ C` |
| Para a esquerda | `ESC [ D` |
| Home | `ESC [ H` |
| End | `ESC [ F` |
| Delete | `ESC [ 3 ~` |
| Esc | `1Bh` |
| Enter | `0Ah` |
| Backspace | `08h` |

Quando uma sequência não cabe no buffer de uma leitura, o restante é retornado pela próxima.

</div>
