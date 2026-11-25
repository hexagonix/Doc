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
| `CSI n G` | Cursor para a coluna n da sua linha |
| `CSI linha ; coluna H` | Cursor para aquela linha e coluna, também `f` |
| `CSI J` | Limpa do cursor até o fim da tela |
| `CSI 2 J` | Limpa a tela inteira, também `3 J` |
| `CSI K` | Limpa do cursor até o fim da linha |
| `CSI 1 K` | Limpa do início da linha até o cursor |
| `CSI 2 K` | Limpa a linha inteira |
| `CSI ... m` | Cores, como abaixo |
| `ESC 7` | Guarda a posição do cursor |
| `ESC 8` | Devolve o cursor aonde o `ESC 7` o guardou, ao canto superior esquerdo se nada foi guardado |

Limpar nunca move o cursor. Uma sequência desconhecida é consumida inteira e não desenha nada, assim como as privadas, como `CSI ? 25 l`, e as que têm byte intermediário, como `ESC ( B`.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Cores

| Parâmetro de `CSI ... m` | Efeito |
|:-------------------------:|:------:|
| `0` | Volta às cores do tema |
| `7` | Fonte e fundo trocados, até um `27` ou `0` |
| `27` | Fonte e fundo de volta aos seus lugares |
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
| `04h` | ISIG | Ctrl+C encerra o que roda no terminal |

Uma tty começa com os três, o modo "cooked" do BSD. A biblioteca [`tty.s`](https://github.com/hexagonix/lib/blob/main/fasm/tty.s) da libasm tem `ttyGetMode` e `ttySetMode` para eles, e a `readKey`, que espera uma única tecla em modo cru e devolve o modo depois.

Com ISIG, o Ctrl+C em um terminal virtual encerra o seu processo em primeiro plano, como faria o `SIGINT` do BSD, embora ainda não existam sinais. Sem ele, o Ctrl+C chega ao programa como o byte `03h`, o que permite a um programa em modo cru, como um emulador de terminal, repassá-lo. A `readLine` desliga o ISIG enquanto lê e descarta a linha ao receber `03h`, mostrando `^C` como os shells do BSD, então o Ctrl+C em um prompt não encerra o shell.

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

Uma leitura retorna o que é digitado conforme chega, pelo menos um byte. Ctrl com uma letra dá o seu caractere de controle, de `01h` a `1Ah`, incluindo o Ctrl+C quando o ISIG está desligado. Teclas sem caractere próprio viram as sequências que um terminal envia para elas, enquanto o teclado numérico digita os seus números.

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

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Tamanho da janela

O comando 6 de `hx.ioctl` retorna o tamanho da janela de uma tty, colunas em EAX e linhas em EBX, como o `TIOCGWINSZ` do BSD. Em um terminal virtual, é o tamanho do console. Em um pty, é o que o gerenciador definiu com o comando 7, colunas em ECX e linhas em EDX, e 80 por 25 até que ele o faça. A `ttyGetSize` do `tty.s` o lê para o descritor 0.

Um programa que alinha o texto pelo tamanho da sua janela, e se move apenas pelas sequências acima, mostra o mesmo em um terminal virtual e em um pty.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Pseudoterminais

Um pty é um par de dispositivos, como o `pty(4)` do BSD. O lado subsidiário, de `ttyp0` a `ttyp7`, é uma tty como qualquer terminal virtual, com o seu próprio modo, linha e eco, onde roda um programa como o `sh`. O lado gerenciador, de `ptyp0` a `ptyp7`, pertence a quem faz o papel do terminal, como um emulador, e toma o lugar do teclado e do console:

| Operação | Efeito |
|:--------:|:------:|
| Escrita no gerenciador | Os bytes chegam ao subsidiário como se digitados |
| Leitura do subsidiário | Pelo modo do subsidiário, como em um terminal virtual |
| Escrita no subsidiário | Os bytes, com o eco, esperam o gerenciador |
| Leitura do gerenciador | O que o subsidiário escreveu, pelo menos um byte |

Cada lado tem uma fila de 1 KB, alocada quando o gerenciador é aberto. Ler uma fila vazia ou escrever numa cheia espera até o outro lado agir. Um gerenciador já aberto recusa outro `hx.open`, assim como um par cuja última sessão ainda tem o subsidiário aberto, então um gerenciador que abre é sempre um par livre. Um subsidiário só abre enquanto o seu gerenciador está aberto. Os comandos 4 e 5 de `hx.ioctl` funcionam nos dois lados, agindo sobre o modo do subsidiário.

Quando nenhum descritor segura o gerenciador, ler o subsidiário retorna 0, o fim do arquivo. Quando nenhum descritor segura o subsidiário e o que ele escreveu já foi lido, ler o gerenciador retorna 0. É assim que um emulador fica sabendo que o seu shell saiu. As filas são liberadas quando nenhum descritor segura nenhum dos lados.

Um programa roda em um pty porque os seus descritores 0, 1 e 2 apontam para o subsidiário, o que quem o cria arranja com `hx.dup2` antes do `hx.spawn`. O seu terminal virtual continua sendo aquele de onde veio, que é o que o `ps` mostra.

O byte `03h` escrito no gerenciador, enquanto o subsidiário tem ISIG, não é entregue. Ele encerra os processos que leem o subsidiário pelo descritor 0, exceto os que esperam um filho, já que um shell sobrevive ao comando que rodou, e descarta a entrada ainda não lida.

O utilitário [`script`](https://github.com/hexagonix/Unix-Apps/tree/main/script) roda um shell em um pty e grava a sessão em um arquivo, do jeito que um emulador o conduziria. O [`raava`](https://github.com/hexagonix/Unix-Apps/tree/main/raava) mostra vários shells em janelas num terminal virtual só seu, cada um em um pty, guardando o que cada um desenhou com a biblioteca `term.s` da libasm, que interpreta os bytes como este terminal.

</div>
