<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/hexagonix.svg)
![](https://img.shields.io/github/stars/hexagonix/hexagonix.svg)
![](https://img.shields.io/github/issues/hexagonix/hexagonix.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/hexagonix.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/hexagonix.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/hexagonix.svg)
![](https://img.shields.io/github/downloads/hexagonix/hexagonix/total.svg)
![](https://img.shields.io/github/release/hexagonix/hexagonix.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# O sistema operacional Hexagonix

<table align="center">
<tr>
<td> [Componentes](#componentes-do-sistema) </td>
<td> [Screenshots](#🌙-capturas-de-tela) </td>
<td> [Download](README.pt.md) </td>
</tr>
</table>

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">
   
O `Hexagonix` é um novo sistema operacional Unix-like desenvolvido do zero, escrito em linguagem Assembly para a arquitetura PC (x86). Ele é construído sobre o `Hexagon`, um núcleo (kernel) monolítico simples e leve, desenvolvido para ser rápido e compatível com hardware recente e mais antigo (Pentium III e mais recentes, com 32 MB de memória RAM ou mais).

O Hexagonix é composto pelo [carregador de inicialização do Hexagon](https://github.com/hexagonix/HBoot) (`Hexagon Boot` ou `HBoot`), pelo [Hexagon](https://github.com/hexagonix/Hexagon) (kernel), shells compatíveis, utilitários e bibliotecas. Todos esses componentes foram liberados sob licença BSD-3-Clause.
   
Algumas características do Hexagonix:

- [x] Sistema operacional de 32-bit;
- [x] Completamente escrito em Assembly x86, sendo rápido e leve;
- [x] Inspirado no design do Version 7 UNIX[^1], mas totalmente escrito do zero;
- [x] Compatível com processadores Intel Pentium III (1999) ou mais recentes;
- [x] Compatível com dispositivos com 32 MB de memória RAM ou mais;
- [x] Kernel completo com menos de 30 kbytes;
- [x] Suporte a ambiente de usuário;
- [x] Chamada de sistema com 68 funções sofisticadas acessadas pelo ambiente de usuário;
- [x] Requisitos mínimos baixos, compatível com uma ampla gama de dispositivos;
- [x] Formato binário executável próprio (HAPP);
- [x] Self-hosting (o [montador usado para construir o Hexagonix](https://github.com/hexagonix/fasmX) pode ser executado sobre ele);
- [x] Abstração de dispositivos;
- [x] Suporte a gráficos VESA VBE em várias resoluções;
- [x] Suporte a modo texto;
- [x] Suporte a portas seriais e paralelas (comunicação serial, debug e impressão);
- [x] Sistema de arquivos virtual (VFS);
- [x] Suporte de leitura e escrita em sistemas de arquivos FAT16 (FAT16B);
- [x] Motor de renderização de fontes gráficas, que podem ser alteradas pelo usuário;
- [x] Suporte a relógio em tempo real;
- [x] Compatível com carregador de inicialização próprio (Hexagon Boot - HBoot);
- [x] Suporte a usuários e permissões.
- [x] Facilmente extensível;
- [x] Totalmente licenciado sob `BSD-3-Clause`[^2].

[^1]: A arquitetura do Hexagonix (estrutura e utilitários) foi inspirada na elegância e simplicidade do [Version 7 UNIX](https://github.com/dspinellis/unix-history-repo/tree/Research-V7-Snapshot-Development), embora não vise qualquer compatibilidade ou compartilhe qualquer estrutura de código com este. O Hexagonix não utiliza qualquer código derivado do Version 7 UNIX ou de outros projetos. Os utilitários do Hexagonix foram escritos para se assemelharem à função e aparência dos correspondentes no Version 7 UNIX.
[^2]: Você pode obter mais informações sobre a licença [neste artigo](https://docs.freebsd.org/en/articles/bsdl-gpl/) do projeto FreeBSD, [no artigo](https://opensource.org/licenses/BSD-3-Clause) da Open Source Initiative ou na página sobre licença na [Wikipedia](https://pt.wikipedia.org/wiki/Licen%C3%A7a_BSD).

O Hexagonix não tem como objetivo a construção de um sistema de produção, mas sim de um sistema simples, bem documentado e com interfaces fáceis de compreender, podendo ser utilizado para fins educacionais. Objetiva também a pesquisa e implementação de um sistema operacional moderno puramente em Assembly x86. Para sistemas mais complexos, completos e profissionais, visite projetos como o [MenuetOS](https://www.menuetos.net/) ou [KolibriOS](https://kolibrios.org/en/), também desenvolvidos em Assembly x86.

</div>

<details title="Licença" align='left'>
<br>
<summary align='left'>Licença do Hexagonix</summary>

<div align="justify">

Hexagonix Operating System

BSD 3-Clause License

Copyright (c) 2015-2023, Felipe Miguel Nery Lunkes<br>
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## Componentes do sistema

O Hexagonix é dividido em uma série de componentes, que atuam em conjunto desde que o dispositivo é ligado. Abaixo você poderá conhecer brevemente cada um deles.

### Saturno

<div align="justify">

O primeiro componente do Hexagonix é o Saturno. Ele é responsável por receber o controle do processo de inicialização realizado pelo BIOS/UEFI e procurar no volume o segundo estágio de inicialização. Para isso, ele implementa um driver para leitura de um sistema de arquivos FAT16. O segundo estágio de inicialização (ver adiante) pode implementar drivers para outros sistemas de arquivos e é responsável por encontrar o Hexagon, carregar módulos HBoot ou carregar um sistema do tipo DOS compatível (versão BETA).

</div>

<div align="center">

[![Saturno](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Saturno&theme=dark)](https://github.com/hexagonix/Saturno)

</div>

<hr>

### Hexagon Boot (HBoot)

<div align="justify">

O Hexagon Boot (HBoot) é um componente desenvolvido para permitir a inicialização do kernel Hexagon. Até então, a inicialização era realizada por apenas um estágio, que definia um ambiente bem básico, carregava o Hexagon na memória e imediatamente o executava, fornecendo um conjunto bem limitado de parâmetros. Isso se deve ao fato de que o código desse estágio fica restrito a 512 bytes, o que limita a realização de diversos testes e processamento de dados. Como o HBoot, foi possível expandir o número de tarefas realizadas antes da execução do Hexagon, além da possibilidade de fornecer mais informações a respeito do ambiente do dispositivo e de inicialização. Isso é particularmente importante para permitir a criação de uma árvore de dispositivos que pode ser utilizada pelo Hexagon para decidir como manipular cada dispositivo identificado. O HBoot é capaz de verificar quais unidades de disco estão disponíveis na máquina, emitir um tom de inicialização, obter a quantidade de memória RAM disponível instalada e permitir ou não o prosseguimento do processo de boot de acordo com essa informação. Caso nenhuma interação do usuário seja detectada (em um tempo de 3 segundos após a inicialização do HBoot e exibição de mensagens ao usuário), o HBoot irá realizar testes adicionais para verificar a capacidade do dispositivo em executar o sistema e irá carregar e executar o Hexagon (presente em um arquivo no volume nomeado de `HEXAGON.SIS` no Hexagonix H1 e `HEXAGON` no Hexagonix H2). Após o carregamento, o HBoot transfere o controle para o Hexagon, que é inicializado e armazena no ambiente do kernel os dados fornecidos pelo HBoot.

</div>

<div align="center">
   
[![HBoot](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=HBoot&theme=dark)](https://github.com/hexagonix/Hboot)

</div>

#### Como interagir com o HBoot

<div align="justify">

A interação com o HBoot se dá pelo pressionamento da tecla `F8` após a inicialização e exibição de mensagens na tela. O HBoot aguarda por 3 segundos alguma intereação e, caso nenhuma tenha ocorrido, continua a executar o protocolo de boot. A interação com o HBoot pode ser interessante para carregar módulos no formato HBoot, fornecer parâmetros de inicialização ao Hexagon, carregar algum sistema do tipo DOS cujos arquivos estejam presentes no mesmo volume ou ainda carregar imagens HAPP de outros núcleos (caso o desenvolvedor deseje utilizar a implementação HBoot em seu projeto). Abaixo, veja mais alguns detalhes de funções adicionais e de diagnóstico que podem ser realizadas via interação com o HBoot antes do carregamento do Hexagonix.

</div>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/HBoot.png" width="600" height="500">
</p>

##### Reportar bugs

<div align="justify">

O HBoot ganhou muita complexidade desde o início de seu desenvolvimento, em 2020. Devido a esse aumento de código e a natureza de sua operação (16-bit), bugs podem ser encontrados. Os mesmos podem ser reportados no repositório ou por email, disponível no final deste arquivo.

</div>

<hr>

### Kernel Hexagon

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="180" height="180">
</p>

<div align="justify">

O Hexagon é um núcleo (kernel) monolítico executado em modo protegido 32-bit, desenvolvido tendo como alvo a arquitetura PC (x86). É um kernel escrito do zero, visando a velocidade e a compatibilidade de harware moderno mas também sendo capaz de ser executado em hardware mais antigo (como um Pentium III). No momento, garante um ambiente monoutilizador, apesar do uso de terminais virtuais, e monotarefa, apesar da capacidade de carregar, manter em memória e controlar mais de um processo, em uma pilha de execução. Futuramente, o kernel poderá receber suporte a execução de múltiplos processos em multitarefa preemptiva. O Hexagon é um kernel Unix-like e tenta implementar uma compatibilidade POSIX, embora longe desta, e compõe a base do Sistema Operacional Hexagonix, embora independente deste. Ele executa imagens executáveis no formato HAPP, desenvolvido exclusivamente para o Hexagon. O Hexagon também implementa uma API bastante sofisticada acessível através de uma chamada de sistema.

</div>

<div align="center">

[![Hexagon Kernel](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Hexagon&theme=dark)](https://github.com/hexagonix/Hexagon)

</div>

<hr>

### Utilitários do ambiente Unix do Hexagonix

<div align="justify">

O Hexagonix implementa, junto ao Hexagon, uma série de utilitários Unix-like, com funcionalidade e sintaxe de uso semelhante à sistemas UNIX e Unix-like. **Utilitários como init, login, sh, top, ps, cp, rm, cat, clear, man, dentre outros, estão inclusos na distribuição padrão do Hexagonix**. Estes utilitários compõem o pacote de utilitários base do Hexagonix. As ferramentas de inicialização de ambiente de modo usuário e login estão neste pacote, bem como vários arquivos de configuração deste ambiente. Estes utilitários não apresentam, no geral, uma interface gráfica, apenas uma interface em linha de comando (CLI). Entretanto, podem ser solicitados por aplicativos que apresentem interface gráfica. Este ambiente está disponível na distribuição do [`Hexagonix`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img).

</div>

<div align="center">

[![Unix-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=unix-apps&theme=dark)](https://github.com/hexagonix/unix-apps)

</div>

#### Alguns aplicativos e utilitários do ambiente Hexagonix

<div align="justify">

O Hexagonix inclui muitos dos utilitários Unix que você pode já estar familiarizado, como por exemplo:

- [x] init
- [x] login
- [x] ls
- [x] cat
- [x] cp
- [x] rm
- [x] clear
- [x] top
- [x] ps
- [x] man
- [x] sh (shell padrão)
- [x] shutdown
- [x] su
- [x] uname
- [x] whoami, entre outros.

Alguns aplicativos e utilitários foram desenvolvidos exclusivamente para o Hexagonix, como:

* htop (versão alternativa de top)
* fnt (utilitário de alteração de fonte gráfica do Hexagonix)
* hash (shell alternativo)
* lshapp (obtém e exibe informações de imagens HAPP)
* lshmod (obtém e exibe informações de módulos HBoot e do próprio HBoot)

Vale lembrar que os utilitários do Hexagonix tentam implementar uma interface POSIX na medida do possível, se assemelhando em sintaxe aos utilitários Unix (FreeBSD e Linux utilizados como modelo).
### Aplicativos de terceiros disponíveis para o Hexagonix

* [flat assembler (fasm)](https://flatassembler.net/index.php)

O Hexagonix recebeu um port do montador [`fasm`](https://flatassembler.net/index.php), que foi adaptado para o Hexagonix, permitindo ao usuário desenvolver aplicativos diretamente no sistema. Este port é chamado de `fasmX`. As alterações adicionadas ao código, assim como licença do software, podem ser encontradas no [repositório do fasm para o Hexagonix](https://github.com/hexagonix/fasm). Este repositório é um fork do [repositório original](https://github.com/tgrysztar/fasm). O código adicionado é baseado em modificações realizadas do código original e adições autorais. Esse código modificado/autoral pode ser encontrado no repositório, [clicando aqui](https://github.com/hexagonix/fasm/tree/master/SOURCE/HEXAGONIX). O fasmX, port do fasm para Hexagonix, sempre é atualizado quando novidades são adicionadas no repositório do fasm. Para indicar que se trata de uma versão estável e testada, o número de versão do fasmX sempre herda a numeração do fasm, sucedido por um caractere x (como exemplo, a versão baseada no fasm 1.73.30, após teste, recebe a numeração 1.73.30x). Você pode reportar bugs ou problemas de geração ou otimização de código na versão para Hexagonix [aqui](https://github.com/hexagonix/fasm/issues). Para reportar erros gerais do fasm, utilize o repositório [oficial](https://github.com/tgrysztar/fasm).

</div>

<hr>

### Utilitários do ambiente gráfico do Hexagonix

<div align="justify">

O ambiente Andromeda do Hexagonix (Hexagonix-Andromeda) é construído sobre a base sólida fornecida pelo Hexagonix, incluindo aplicativos e utilitários que não implementam a filosofia Unix ou apresentam sintaxe e forma de uso bastante diferentes do que se esperaria de um ambiente Unix. Aqui estão o aplicativo de configurações do sistema, calculadora, gerenciador de fontes, editores de texto e a IDE desenvolvida para o Hexagonix. Estes utilitários podem ou não apresentar uma interface gráfica. Juntamente a eles, compõem o ambiente Hexagonix-Andromeda bibliotecas desenvolvidas para permitir o desenvolvimento de aplicativos, como a biblioteca `Estelar`.

</div>

<div align="center">

[![Andromeda-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=andromeda-apps&theme=dark)](https://github.com/hexagonix/andromeda-apps)

</div>

#### Alguns aplicativos e utilitários do ambiente Hexagonix-Andromeda

<div align="justify">

- [x] Configurações do sistema (config)
- [x] Editor de texto Quartzo
- [x] IDE Lyoko para desenvolvimento de aplicativos
- [x] Piano eletrônico return Piano;
- [x] Utilitário de comunicação serial
- [x] Andromeda Shell (ASH) - Um novo shell para o Hexagonix
- [x] Calculadora do Hexagonix
- [x] Utilitário de alteração de fonte
- [x] Utilitário de desligamento do Hexagonix

### Aplicativos de terceiros disponíveis para o Hexagonix-Andromeda

Ainda não existem aplicativos de terceiros disponíveis para o ambiente Hexagonix-Andromeda. Caso esteja interessado, você pode construir o primeiro!

</div>

<hr>

### Fontes gráficas do Hexagonix

<div align="justify">

A instalação padrão do Hexagonix também fornece uma série de fontes que podem ser carregadas pelo aplicativo de configurações ou utilitário de fontes (gerenciador de fontes). Esses arquivos são utilizados para alterar a fonte de exibição em modo gráfico do Hexagonix.

As fontes de modo gráfico para Hexagon são desenvolvidas como um bitmap em Assembly que, compiladas, geram uma imagem binária da fonte com representações de cada caractere. Os códigos das fontes padrão do Hexagonix já foram liberados como código livre e estão disponíveis no [repositório de fontes do Hexagonix](https://github.com/hexagonix/xfnt).

</div>

<div align="center">

[![Hexagonix-xfnt](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=xfnt&theme=dark)](https://github.com/hexagonix/xfnt)

</div>

<hr>

### Bibliotecas de desenvolvimento do sistema

<div align="justify">

O Hexagonix também fornece funções que devem ser utilizadas para interagir com o próprio ambiente do sistema. As bibliotecas são utilizadas para acessar funções implementadas pelo Hexagon ou pelas próprias bibliotecas, permitindo o desenvolvimento facilitado de aplicativos e utilitários tanto para o ambiente Hexagonix quanto para o Hexagonix-Andromeda. As bibliotecas implementam funções para exibição de texto, cálculos matemáticos, envio de mensagens, abertura de arquivos e dispositivos e muito mais. A biblioteca básica (hexagon.s) fornece funções acessíveis para ambos os ambientes possíveis de distribuição, enquanto outras bibliotecas podem ser exclusivas do ambiente Hexagonix-Andromeda. Essas bibliotecas incluem funções gráficas para montar interfaces em modo gráfico (Hexagonix-Andromeda), bem como funções para verificar a versão do sistema atualmente em execução. Os utilitários base Hexagonix realizam a checagem da versão do Hexagon para verificar se podem ser executados, utilizando o utilitário Unix uname ou diretamente por uma chamada de sistema do Hexagon.

Para saber mais e verificar cada função disponível nas bibliotecas de desenvolvimento do sistema, veja o repositório da [libasm](https://github.com/hexagonix/lib).

</div>

<div align="center">

[![lib](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=lib&theme=dark)](https://github.com/hexagonix/lib)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## 🌙 Capturas de tela

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix1.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix2.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix3.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix4.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix5.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix6.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix7.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix8.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix9.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix10.png" width="500" height="400">
</p>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## Contribuir e reportar erros

<div align="justify">
   
Abaixo você poderá saber mais sobre como contribuir e reportar erros encontrados no Hexagonix.

</div>

<details title="Contribua no desenvolvimento do Hexagonix" align='left'>
<br>
<summary align='left'>Contribua no desenvolvimento do Hexagonix</summary>

<div align="justify">

Se você tem conhecimento em criar código em Assembly x86 e gostaria de ajudar no desenvolvimento do sistema, sinta-se a vontade em me enviar um email! Veja [aqui](https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.pt.md#autor) como entrar em contato comigo!

</div>

</details>

<details title="Reportar erros" align='left'>
<br>
<summary align='left'>Reportar erros</summary>

<div align="justify">

Você pode reportar erros e ajudar a desenvolver o sistema. Para isso, abra uma notificação de erro [aqui](https://github.com/hexagonix/Distro/issues), informando o erro da forma mais detalhada possível (como marca do dispositivo, processador, quantidade de memória RAM, placa de vídeo e periféricos conectados, bem como o dispositivo utilizado para instalar o sistema, como unidade de disco interna ou mídia removível USB). Lembre-se de informar em qual aplicativo ocorreu o erro, caso o erro ocorra já com o sistema em operação. Caso o problema se dê no processo de inicialização, informe o que foi exibido/o comportamento observado da máquina.

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## Outras informações

<details title="Idiomas" align='left'>
<br>
<summary align='left'>🎲 Idiomas</summary>

<div align="justify">

Neste momento, o sistema está disponível em inglês (com alguns componentes ainda em português) e a documentação está disponível tanto em português quanto em inglês. Os comentários e nomes de função nos arquivos que compõem o código-fonte do sistema estão em português.

</div>

</details>

<details title="Inspirações" align='left'>
<br>
<summary align='left'>🖼 Inspirações</summary>

<div align="justify">

Diversos outros projetos foram importantes para permitir o desenvolvimento do Hexagonix. Esses projetos contribuiram com ideias e conceitos de design, com código bem documentado que permite o entendimento de diversas estruturas de um sistema operacional ou ainda contribuiram com códigos que inspiraram o funcionamento de diversas funções hoje disponíveis no Hexagonix (tanto código do kernel como do ambiente de usuário).

Dentre estes, dois projetos de domínio público, escritos em Assembly x86, possibilitaram que o funcionamento de determinados componentes do computador fossem melhor compreendidos, abrindo portas para que códigos autorais que hoje compõem o Hexagonix fossem escritos com base no estudo e reimplementação do código deles.

* [Snowdrop OS](http://www.sebastianmihai.com/snowdrop/), sistema operacional em domínio público que me inspirou a escrever várias rotinas de acesso ao hardware em 16-bit e outras funções 16-bit disponíveis no HBoot. Este sistema permanece em desenvolvimento. Acesse o [site](http://www.sebastianmihai.com/snowdrop/) para obter imagens de disco e os arquivos-fonte do sistema.
* [Alotware](https://github.com/0x5CE/alotware), sistema operacional em domínio público que auxiliou na criação de funções de gerenciamento de processos e outras funções do kernel nas versões iniciais do Hexagon (hoje, já reescritas inúmeras vezes para expandir o kernel). O código deste sistema foi liberado em domínio público e não vem mais sendo atualizado desde então.

Junto dos projetos citados acima, o Hexagonix só é possível graças a uma série de projetos, desenvolvedores e pessoas que vieram antes. Vários projetos contribuiram com uma documentação consolidada, ideias de design sólidas e demonstrações de como desenvolver um sistema operacional, mesmo que simples. São eles (além dos dois já mencionados):

* [Version 7 UNIX](https://github.com/dspinellis/unix-history-repo/tree/Research-V7-Snapshot-Development)
* [FreeBSD](https://www.freebsd.org/)
* [MenuetOS](http://www.menuetos.net/)
* [KolibriOS](https://kolibrios.org/en/)
* [Linux 0.1.1](https://kernel.org)
* [XNU/Darwin](https://github.com/apple/darwin-xnu)
* MS-DOS, com código disponível [aqui](https://github.com/microsoft/MS-DOS)
* [MikeOS](http://mikeos.sourceforge.net/)
* [LittleKernel](https://github.com/littlekernel/lk)
* [Google Fuchsia](https://fuchsia.googlesource.com/fuchsia/)

A todos esses projetos e desenvolvedores, um enorme agradecimento :heart:.

</div>

</details>

<details title="Autor, contribuidores e contatos" align='left'>
<summary align='left'>👥️️ Autor, contribuidores e contatos</summary>

## Autor

* [Felipe Miguel Nery Lunkes](https://github.com/felipenlunkes)
## Contribuidores

* [Felipe Miguel Nery Lunkes](https://github.com/felipenlunkes)

## E-mail

* hexagonixdev@gmail.com (PT/EN)

## Redes sociais

* [Felipe Miguel Nery Lunkes](https://twitter.com/felipeldev) (Twitter)

</details>

<details title="Notas de direitos autorais" align='left'>
<br>
<summary align='left'>📝 Notas de direitos autorais</summary>

<div align="justify">

Leia a licença para mais informações sobre direitos autorais, propriedade de código e redistribuição que se aplicam apenas aos arquivos disponíveis neste repositório (não se aplicam ao conjunto de arquivos de dados e de código fonte que compõem o Hexagonix). Sempre fique atento ao arquivo `LICENSE` disponível em cada repositório para estar ciente dos direitos e obrigações legais.

</div>

</details>
