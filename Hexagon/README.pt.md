<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/banner.png">
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

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Kernel Hexagon

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="200" height="200">
</p>

<div align="justify">

O Hexagon é um `núcleo` (kernel) monolítico executado em `modo protegido` 32-bit, desenvolvido puramente em Assembly para a arquitetura PC (x86). É um kernel escrito do zero, visando a velocidade e a compatibilidade de harware moderno, mas também sendo capaz de ser executado em hardware mais antigo (Pentium III ou superiores, com 32 MB de memória RAM ou mais). No momento, garante um ambiente monoutilizador, apesar do uso de terminais virtuais, e monotarefa, apesar da capacidade de carregar, manter em memória e controlar mais de um processo por vez, em uma pilha de execução de ordem cronológica. Futuramente o kernel poderá receber suporte a execução de múltiplos processos em multitarefa preemptiva. O Hexagon foi projetado para ser um kernel Unix-like e compõe a base do `Hexagonix`, embora independente deste. Ele executa imagens executáveis no formato `HAPP`, desenvolvido exclusivamente para o Hexagon. Ele também implementa uma API bastante sofisticada acessível através de uma chamada de sistema padronizada e documentada, como você pode ver abaixo.

Algumas características do Hexagon:

- [x] Suporte a processadores x86 (Pentium III ou superiores);
- [x] Suporte a dispositivos com 32 MB de memória RAM ou mais;
- [x] Suporte a ambiente de usuário;
- [x] [Chamada de sistema](SYSCALL.pt.md) com 68 funções sofisticadas acessadas pelo ambiente de usuário;
- [x] Formato binário executável próprio (HAPP);
- [x] Unix-like;
- [x] Completamente escrito em Assembly x86;
- [x] Self-hosting (o montador usado para construir o Hexagon pode ser executado sobre ele);
- [x] Sistema de arquivos virtual;
- [x] Abstração de dispositivos;
- [x] Suporte total a leitura e escrita em sistemas de arquivos FAT16;
- [x] Suporte a gráficos VESA VBE e em múltiplas resoluções;
- [x] Suporte a modo texto;
- [x] Motor de renderização de fontes gráficas, que podem ser alteradas pelo usuário;
- [x] Suporte a relógio em tempo real;
- [x] Suporte a portas seriais e paralelas (comunicação serial, debug e impressão);
- [x] Compatível com carregador de inicialização próprio (Hexagon Boot - HBoot);
- [x] Suporte a usuários e permissões.

Outras características que estão sendo desenvolvidas:

- [ ] Procura e enumeração de todos os dispositivos PCI;
- [ ] Multitarefa preemptiva.

> Você pode ajudar a implementar as funções em desenvolvimento acima!

<details title="Licença" align='left'>
<br>
<summary align='left'>Licença do Hexagon</summary>

<div align="justify">

Leia a [licença](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) para mais informações sobre direitos autorais, propriedade de código e redistribuição que se aplicam aos arquivos disponíveis neste repositório. O Hexagonix é totalmente licenciado sob [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Sempre fique atento ao arquivo `LICENSE` disponível em cada repositório para estar ciente dos direitos e obrigações legais, bem como à lista de contribuidores do projeto.

</div>

</details>

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### História do desenvolvimento

<div align="justify">

O kernel foi inicialmente desenhado e escrito visando uma estrutura e funcionamento próximos de sistemas DOS (Disk Operating System), como MS-DOS, nos ano de 2015 a 2017. Sendo assim, muitas chamadas de sistema e nomes de dispositivo seguiam uma sintaxe e nomes DOS. Com o passar do tempo, houve o interesse de aproximar o então núcleo do Andromeda, que a essa altura não possuia nome e era mantido junto ao código da distribuição, a uma estrutura e funcionamento mais próximos de sistemas do tipo Unix, como BSD ou Linux, por exemplo. Desta forma, muitas partes do kernel foram reimplementadas tendo em mente o novo objetivo. O código do núcleo foi separado do restante do Sistema e se tornou independente, em questão de desenvolvimento e também de funcionamento, além de ganhar um nome, Hexagon. Foi escrita uma camada de abstração de hardware com a inclusão de chamadas de sistema conhecidas no mundo Unix, como abrir(), fechar(), ler() e escrever(). Os dispositivos ganharam nome e as unidades de disco mudaram da nomenclatura DOS e foram para nomes de dispositivo Unix. O kernel então passa a seguir um processo de inicialização conhecido, com a execução, com PID 1, do primeiro processo do usuário, init, que então carrega o restante dos componentes. Foram então escritos utilitários Unix-like que passassem a utilizar a API Unix-like do kernel, e várias ferramentas Unix-like já foram escritas desde então (2017 em diante).

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### Sessões da documentação sobre o Hexagon

<div align="justify">

Agora, você encontrará mais informações relevantes sobre particularidades do Hexagon, como desenvolver utilitários compatíveis e como ajudar a desenvolver o kernel.

</div>

<details title="Desenvolvendo para o Hexagon" align='left'>
<br>
<summary align='left'><strong>Desenvolvendo para o Hexagon</strong></summary>

<div align="justify">

Nessa sessão, você encontrará a documentação relevante sobre como desenvolver utilitários compatíveis com o Hexagon. Selecione o tópico de seu interesse abaixo. Você também pode sugerir novos tópicos. Basta abrir uma `issue` com a sua proposta.

</div>

<details title="Chamadas de sistema do Hexagon" align='left'>
<br>
<summary align='left'>Chamadas de sistema do Hexagon</summary>

<div align="justify">

O Hexagon implementa uma série de funções que são expostas ao ambiente de usuário, para que possam ser utilizadas pelos desenvolvedores para construir utilitários que utilizem a API do Hexagon. Essa API é acessível atráves de [chamadas de sistema](SYSCALL.pt.md), ou mais facilmente pelas bibliotecas de desenvolvimento compatíveis, como a [libasm](https://github.com/hexagonix/lib).

O número de chamadas de sistema podem variar de acordo com os lançamentos de novas versões do Hexagon, visto que a tendência é que a maioria das funções não críticas sejam movidas para bibliotecas, não permanecendo no núcleo. Entretanto, com a evolução natural do kernel, outras funções e chamadas podem ser implementadas.

Neste momento, existem [68 chamadas de sistema](SYSCALL.pt.md) que são expostas ao ambiente de usuário pelo Hexagon. Para isso, ele implementa um sistema de interrupção que é acessível por qualquer aplicativo através da interrupção 69h (`int 69h`).

O formato de passagem de parâmetros para o manipulador de interrupção do Hexagon é um misto do observado para o implementado no MS-DOS e nos sistemas BSD. Parte dos parâmetros é passado na pilha (como em sistemas BSD), enquanto outros parâmetros são passados por meio dos registradores (como no MS-DOS), da seguinte forma:

* O número da função desejada é `sempre` fornecido ao Hexagon pela pilha;
* Os parâmetros restantes, que servem de entrada para a função solicitada, são fornecidos exclusivamente pelos registradores, observado que cada função aceita parâmetros e registradores definidos.

Um exemplo de chamada de sistema, para finalizar o processo atual em execução, pode ser observado abaixo:

```assembly

    push 4 ;; Solicitar a função 4, de encerrar processo

    mov eax, 0 ;; Informar o código de erro 0

    int 69h ;; Chamar o Hexagon

```

O arquivo `hexagon.s`, presente na biblioteca Hexagonix da [libasm](https://github.com/hexagonix/lib) especifica todas as chamadas de sistema atualmente suportadas pela versão corrente do Hexagon, bem como lista as saídas e entradas para cada função solicitada. Você também pode acessar a documentação de chamadas de sistema [aqui](SYSCALL.pt.md).

Na próxima sessão você poderá obter mais informações sobre como desenvolver um aplicativo simples (modo texto) para o Hexagonix, utilizando os serviços do Hexagon, bibliotecas da libasm e macros que facilitam o disparo de chamadas de sistema.

</div>

</details>

<details title="O formato executável HAPP" align='left'>
<br>
<summary align='left'>O formato executável HAPP</summary>

<div align="justify">

O formato de imagem executável HAPP foi desenvolvida para o Hexagon para permitir o desenvolvimento de imagens que possam ser verificadas e validadas quanto a arquitetura e versões mínimas do kernel necessárias para a correta execução. O cabeçalho também armazena informações importantes, permitindo ao desenvolvedor adicionar diretamente um ponto de entrada, independente de onde ele esteja no interior da imagem, algo que deveria ser redirecionado anteriormente, quando a imagem executável era no formato binário puro. A imagem HAPP também permite validar se a imagem a ser carregada é realmente uma imagem executável, impedindo então que arquivos não suportados sejam executados, mesmo que não se tratem sequer de arquivos executáveis. Também permite que o sistema verifique as dependências do código, como a já citada arquitetura, bem como os números de versão do Hexagon, que devem ser iguais ou superiores ao mínimo especificado pelo cabeçalho. Todas as imagens HAPP devem apresentar este cabeçalho completo, incluindo as sessões reservadas, a fim de funcionarem corretamente em versões posteriores do Sistema. As imagens HAPP são sempre 32-bit.

Em linguagem Assembly, a linguagem de desenvolvimento do sistema, o cabeçalho, em sua especificação 2.0:

```assembly
cabecalhoAPP:

.assinatura:      db "HAPP" ;; Assinatura
.arquitetura:     db 01h    ;; Arquitetura (i386 = 01h)
.versaoMinima:    db 1      ;; Versão mínima do Hexagon
.subversaoMinima: db 00     ;; Subversão mínima do Hexagon
.pontoEntrada:    dd        ;; Offset do ponto de entrada (referência à função principal aqui)
.tipoImagem:      db 01h    ;; Tipo de imagem executável (executável = 01h)
.reservado0:      dd 0      ;; Reservado (Dword)
.reservado1:      db 0      ;; Reservado (Byte)
.reservado2:      db 0      ;; Reservado (Byte)
.reservado3:      db 0      ;; Reservado (Byte)
.reservado4:      dd 0      ;; Reservado (Dword)
.reservado5:      dd 0      ;; Reservado (Dword)
.reservado6:      dd 0      ;; Reservado (Dword)
.reservado7:      db 0      ;; Reservado (Byte)
.reservado8:      dw 0      ;; Reservado (Word)
.reservado9:      dw 0      ;; Reservado (Word)
.reservado10:     dw 0      ;; Reservado (Word)
```

Abaixo, uma implementação de um pequeno aplicativo escrito como exemplo, que utiliza o cabeçalho e chamadas de sistema do Hexagon, escrito em linguagem Assembly x86 em sintaxe Intel e montada com o auxílio do flat assembler (FASM). Este aplicativo envia uma mensagem ao terminal e se encerra em seguida.

```assembly
;; Este é um template para a construção de um app de modo texto para 
;; o Hexagonix!
;;
;; Escrito por Felipe Miguel Nery Lunkes em 04/12/2020
;;
;; Voce pode gerar uma imagem HAPP executável utilizando o montador
;; FASM. Para isso, utilize a linha de comando abaixo:
;;
;; fasmX tapp.asm
;;       ou
;; fasmX tapp.asm tapp.app

use32

cabecalhoAPP:

.assinatura:      db "HAPP"    ;; Assinatura
.arquitetura:     db 01h       ;; Arquitetura (i386 = 01h)
.versaoMinima:    db 1         ;; Versão mínima do Hexagon
.subversaoMinima: db 00        ;; Subversão mínima do Hexagon
.pontoEntrada:    dd inicioAPP ;; Offset do ponto de entrada
.tipoImagem:      db 01h       ;; Imagem executável
.reservado0:      dd 0         ;; Reservado (Dword)
.reservado1:      db 0         ;; Reservado (Byte)
.reservado2:      db 0         ;; Reservado (Byte)
.reservado3:      db 0         ;; Reservado (Byte)
.reservado4:      dd 0         ;; Reservado (Dword)
.reservado5:      dd 0         ;; Reservado (Dword)
.reservado6:      dd 0         ;; Reservado (Dword)
.reservado7:      db 0         ;; Reservado (Byte)
.reservado8:      dw 0         ;; Reservado (Word)
.reservado9:      dw 0         ;; Reservado (Word)
.reservado10:     dw 0         ;; Reservado (Word)

;;*************************************************************

include "hexagon.s" ;; Incluir as chamadas de sistema
include "macros.s"  ;; Inclui macros

;;*************************************************************

;; Variaveis e constantes

msg: db 10, 10, "Este e um template com um exemplo de aplicativo HAPP simples!", 10, 0

;;*************************************************************

;; Ponto de entrada

inicioAPP:

    mov esi, msg

    imprimirString ;; Aqui temos um macro que configura e chama uma função da API

    Hexagonix encerrarProcesso ;; Outro macro que solicita qual chamada realizar
``` 

</div>

</details>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### Em busca de colaboradores

<div align="justify">

O Hexagon, bem como todo o Hexagonix, busca colaboradores dispostos a contribuir em seu desenvolvimento. Caso você se interesse por Assembly, C, em ajudar na escrita de documentação ou apenas ajudar na tradução, sinta-se a vontade em entrar em contato! Todos são bem vindos!

</div>

<!--

Versão deste arquivo: 2.0

-->
