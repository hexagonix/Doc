<!--

Copyright (c) 2015-2026 Felipe Miguel Nery Lunkes
Todos os direitos reservados.

-->

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

# HXBI, o bloco de informações de inicialização do Hexagon

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

O HXBI é o formato que o HBoot utiliza para descrever a máquina ao Hexagon, e é a única coisa que o kernel recebe ao iniciar. Até o Hexagon 1.8.0, o ambiente de inicialização chegava em quatro registradores: `CX` carregava a quantidade de RAM que o HBoot já havia medido, `BL` a unidade de onde o sistema inicializou, `EBP` um ponteiro para o setor de inicialização e `ESI` um ponteiro para o buffer de linha de comando do HBoot. Todo o resto de que o kernel precisava ele próprio obtinha, em modo real, antes de montar sua própria GDT e passar para o modo protegido. Habilitar a linha A20 e pedir à BIOS a memória instalada através da função `E801h` eram feitos duas vezes, uma no HBoot e outra no Hexagon.

Os quatro registradores foram substituídos por um único bloco com um cabeçalho e uma lista de tags. O HBoot monta esse bloco enquanto ainda está em modo real, onde a BIOS está disponível e onde ele já está lendo o disco de qualquer forma, passa o processador para o modo protegido de 32 bits por conta própria e salta para o kernel com o endereço do bloco em `EBX`. O Hexagon não possui mais código de modo real de nenhum tipo, e não faz chamadas à BIOS em nenhum momento de sua execução.

O nome vem da assinatura no início do bloco. A revisão atual é a 1, produzida por `HBoot/hxbi.asm` e consumida por `kern/hxbi.asm`.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Estado da máquina na entrada do kernel

Três condições valem quando o HBoot salta para o kernel, e o kernel não assume nada além delas:

* O processador está em modo protegido de 32 bits.
* A linha A20 está habilitada.
* `EBX` contém o endereço físico de um bloco HXBI válido.

O HBoot leva o processador até esse ponto com uma GDT transitória, em `HBoot/pmode.asm`, feita para sobreviver ao salto e nada além disso. O Hexagon instala sua própria GDT e IDT antes de tocar em qualquer outra coisa, já que os descritores que ele espera (`08h` para código, `10h` e `18h` para dados, todos planos e com base em zero) são aqueles para os quais todos os outros arquivos do kernel foram escritos. A pilha que o HBoot deixa é a sua própria pilha de modo real, lida através de um seletor plano, então o kernel aponta `ESP` para a sua área reservada antes da primeira chamada que faz.

O bloco em si vive dentro do segmento do HBoot, em `0x1000:0000`, e essa memória não é reservada do alocador do kernel. Qualquer coisa que o Hexagon queira guardar do bloco precisa ser copiada para fora dele antes do primeiro acesso a disco, que é o que o `kern.asm` faz com o setor de inicialização.

## O cabeçalho

O bloco começa com dezesseis bytes em deslocamentos fixos. Eles são a única parte do formato que pode ser alcançada sem percorrer a lista.

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `00h` | 4 | signature | Os quatro caracteres `HXBI`. Um kernel que não os encontre recusa o bloco em vez de confiar no que quer que `EBX` esteja carregando |
| `04h` | 4 | version | Revisão do formato. A revisão 1 é a única que existe, e o kernel recusa qualquer outra em vez de adivinhar o leiaute |
| `08h` | 4 | totalSize | Tamanho do bloco inteiro em bytes, este cabeçalho incluído. A leitura das tags para aqui, de modo que uma lista malformada não consiga passar do fim |
| `0Ch` | 4 | flags | O bit 0 definido significa que o HBoot confirma que a linha A20 está habilitada. Os demais bits são reservados e sempre zero |

A lista de tags começa no deslocamento `10h`.

## A lista de tags

Cada tag começa com oito bytes, um tipo e um tamanho, e o tamanho conta esses oito bytes junto com o conteúdo. Um leitor que não conheça um tipo ainda assim consegue passar por ele, porque o tamanho sozinho diz onde a próxima tag começa. As tags começam em fronteiras de quatro bytes, então percorrer a lista significa arredondar o tamanho para o múltiplo de quatro seguinte antes de somá-lo. A lista termina em uma tag de tipo 0.

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `+00h` | 4 | type | O que o conteúdo carrega, conforme a tabela abaixo |
| `+04h` | 4 | size | Tamanho desta tag, cabeçalho e conteúdo juntos |

Os tipos que o HBoot escreve hoje:

| Tipo | Tag | Tamanho | Escrita quando |
|:----:|:----|:-------:|:---------------|
| 0 | Fim da lista | 8 | Sempre, como última tag |
| 1 | Informações do carregador | 28 | Sempre |
| 2 | Mapa de memória | 16 mais 20 por região | Sempre |
| 3 | Dispositivo de inicialização | 524 | Sempre |
| 4 | Linha de comando | 264 | Sempre |
| 5 | Framebuffer | 32 | Sempre, zerada se nenhum modo VBE foi definido |

Os deslocamentos nas tabelas abaixo são contados a partir do início da tag, então um campo de conteúdo em `+08h` é o primeiro byte após o cabeçalho da tag.

### Informações do carregador, tipo 1

Diz o que carregou o kernel e qual versão dele fez isso. O Hexagon ignora esta tag hoje, e ela existe para que um log de inicialização possa nomear o carregador assim que houver onde escrevê-lo.

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `+08h` | 4 | loaderVersion | Versão empacotada, `00010000h` para 1.0 |
| `+0Ch` | 16 | loaderName | Nome do carregador em ASCII, preenchido com zeros. O HBoot escreve `HBoot` |

### Mapa de memória, tipo 2

A lista de regiões que a BIOS devolve através da função `E820h`, copiada como está. O HBoot reúne até 32 regiões em `HBoot.Memory.E820.entries` e descarta o atributo ACPI de quatro bytes que a `E820h` acrescenta a cada uma, deixando os vinte bytes que importam.

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `+08h` | 4 | entrySize | Tamanho de uma região, 20 bytes na revisão 1. Uma revisão posterior pode aumentar a região sem mover nenhum campo anterior a ela |
| `+0Ch` | 4 | entryCount | Número de regiões que seguem |

Cada região, repetida `entryCount` vezes a partir de `+10h`:

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `+00h` | 8 | base | Endereço físico em que a região começa |
| `+08h` | 8 | length | Tamanho da região em bytes |
| `+10h` | 4 | type | 1 utilizável, 2 reservada, 3 recuperável pela ACPI, 4 ACPI NVS, 5 defeituosa. A numeração é a mesma que a `E820h` usa, repassada sem tradução |

### Dispositivo de inicialização, tipo 3

A unidade de onde o sistema inicializou e o setor de onde o Saturno leu o BPB. O setor viaja dentro da tag, e não como um ponteiro para uma memória que o HBoot está prestes a abandonar.

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `+08h` | 1 | driveNumber | Índice da unidade, comparado com a tabela de discos que o Hexagon monta por conta própria |
| `+09h` | 3 | reserved | Preenchimento, sempre zero |
| `+0Ch` | 512 | bootSector | O setor de inicialização, byte a byte |

### Linha de comando, tipo 4

Os parâmetros digitados na linha de comando do HBoot, vindos de `HBoot.Parameters.readBuffer`. Esse buffer tem 64 bytes, e a tag reserva 256 para que o formato não seja o que limita o tamanho de um parâmetro mais adiante. Os bytes além dos 64 primeiros são zero.

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `+08h` | 256 | text | Parâmetros em ASCII, terminados por um zero dentro dos 256 bytes |

### Framebuffer, tipo 5

O HBoot lê a resolução desejada de `/boot/boot.cfg`, na mesma forma `chave=valor` que o `/etc/rc` utiliza, define o modo VBE correspondente e lê de volta o bloco de informações do modo para obter o endereço, o passo e a profundidade que a placa de fato entregou. O Hexagon monta seu console gráfico apenas a partir desta tag, e não consegue definir um modo de vídeo sozinho. Mudar a resolução significa editar aquela linha e reinicializar, que é o que o utilitário `config` faz.

Se o `HBoot.VBE.setupVBE` não encontrar nenhum modo correspondente à resolução pedida, a tag é escrita com todos os campos zerados.

| Deslocamento | Tamanho | Campo | Significado |
|:------------:|:-------:|:-----:|:------------|
| `+08h` | 8 | address | Endereço físico do framebuffer linear. A metade alta é sempre zero, já que o framebuffer fica abaixo de 4 GB |
| `+10h` | 4 | pitch | Bytes do início de uma linha de varredura até o início da seguinte |
| `+14h` | 4 | width | Largura em pixels |
| `+18h` | 4 | height | Altura em pixels |
| `+1Ch` | 1 | bpp | Bits por pixel |

### Fim da lista, tipo 0

Não carrega conteúdo e seu tamanho é sempre 8. Um bloco contendo apenas um cabeçalho e esta tag continua válido.

## O que o Hexagon faz com cada tag

O `Hexagon.HXBI.parse` roda antes de qualquer outra coisa no kernel, logo depois que a GDT entra. Ele confere a assinatura e a revisão, entra em pânico se alguma das duas estiver errada, e percorre a lista até a tag de fim ou até o cursor alcançar `totalSize`.

| Tag | O que o kernel faz com ela |
|:----|:---------------------------|
| 1 | Ignorada |
| 2 | Soma o tamanho de cada região de tipo 1 e guarda o total em `Hexagon.Memory.totalMemory`. Apenas a metade baixa de cada tamanho é lida, já que o kernel é de 32 bits. As regiões reservadas ainda não são consultadas pelo alocador |
| 3 | Guarda a unidade em `Hexagon.Dev.Disk.Control.bootDisk` e aponta `Hexagon.Memory.addressBPB` para o setor embutido. O `kern.asm` então copia esses 512 bytes para `Hexagon.Memory.BPBCopy` e reaponta `addressBPB` para a cópia, antes de qualquer acesso a disco, porque o bloco HXBI não é reservado do alocador e o `Hexagon.Heap.DiskCache` alcança o bloco durante uma montagem normal |
| 4 | Copia os 256 bytes inteiros para `Hexagon.Boot.Parameters.commandLine`. Nada no kernel lê esse buffer ainda |
| 5 | Guarda o endereço, o passo, a largura, a altura e a profundidade em `Hexagon.Boot.Framebuffer`, de onde o console e a biblioteca gráfica passam a ler |

Uma tag de qualquer outro tipo é pulada pelo seu próprio tamanho. Um tamanho menor que o cabeçalho de oito bytes deixaria o cursor parado e percorreria a mesma tag para sempre, então o kernel trata isso como um bloco malformado e entra em pânico.

## Acrescentando uma tag

Uma tag nova é um número de tipo novo e um conteúdo, acrescentados antes da tag de fim no `HBoot.HXBI.buildBlock`. Nada mais no formato se move, e um kernel construído antes de o tipo existir passa por ela pelo seu tamanho sem notar. Esta é a razão de uma lista de tags em vez de uma estrutura fixa: uma estrutura teria que colocar cada campo novo em algum deslocamento, e todo par de HBoot e Hexagon construído antes daquele ponto leria os bytes errados ali.

O mesmo vale no sentido contrário. Um carregador que não produza uma tag simplesmente a deixa de fora, e o kernel não encontra nada onde ela estaria.

## O que um carregador UEFI mudaria

Nada do lado do kernel. Um carregador rodando sob UEFI preenche o mesmo cabeçalho e as mesmas tags, tirando as regiões de `GetMemoryMap()` em vez da `E820h` e o framebuffer do GOP em vez do VBE. O ponto de entrada e a leitura da lista permanecem como estão.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

<details title="Licença do Hexagon" align='left'>
<summary align='left'>Licença do Hexagon</summary>
<br>

<div align="justify">

Por favor, leia a [licença](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) para obter mais informações sobre direitos autorais, propriedade do código e redistribuição que se aplicam aos arquivos disponíveis neste repositório. O Hexagonix é totalmente licenciado sob a licença [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Sempre preste atenção ao arquivo `LICENSE` disponível em cada repositório para tomar conhecimento dos direitos e obrigações legais, bem como da lista de contribuidores do projeto.

</div>

</details>

</div>
