<!--

Versão deste arquivo: 5.0

Copyright © 2021-2022 Felipe Miguel Nery Lunkes
Todos os direitos reservados.

-->

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

</div>

<hr>

# Ponto de partida

<details title="Licença" align='left'>
<br>
<summary align='left'>1️⃣ Licença</summary>

<div align="justify">

Hexagonix Operating System

BSD 3-Clause License

Copyright (c) 2015-2022, Felipe Miguel Nery Lunkes <br>
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
   list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its
   contributors may be used to endorse or promote products derived from
   this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

</div>

</details>

<details title="Fazer download e testar o sistema agora mesmo" align='left'>
<br>
<summary align='left'>2️⃣ Fazer download e testar o sistema agora mesmo</summary>

<div align="justify">

No [`final deste arquivo`](https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.pt.md#como-testar-o-hexagonix) você encontra um tutorial para executar o Hexagonix em seu computador, tanto em uma versão virtualizada como de forma nativa. Lembre-se que é necessário possuir um computador de arquitetura x86 ou um emulador, caso esteja utilizando um dispositivo de outra arquitetura para testes.

Você pode ir até a sessão de [`lançamentos`](https://github.com/hexagonix/hexagonix/releases) para obter as versões estáveis do sistema. Você pode acessar o arquivo de [anúncio de versões](REL.pt.md) do Hexagonix. Sempre que possível, obtenha o último lançamento e realize o download das imagens .img disponíveis ou do pacote completo em formato zip. As versões disponíveis diretamente neste repositório são sempre versões de desenvolvimento (beta e release candidate), que também podem ser executadas e estão minimamente funcionais. Ao final de cada ciclo de desenvolvimento, as versões finais estarão disponíveis na sessão [lançamentos](https://github.com/hexagonix/hexagonix/releases).

</div>

</details>

<details title="Documentação do sistema" align='left'>
<br>
<summary align='left'>3️⃣ Documentação do sistema</summary>


* [Documentação do sistema (em construção)](https://github.com/hexagonix/Doc)

</details>

<details title="Construir o sistema" align='left'>
<br>
<summary align='left'>4️⃣ Construir o sistema</summary>

* [Construir o sistema (Português)](https://github.com/hexagonix/build/blob/main/README.pt.md)

</details>

<details title="Contribua no desenvolvimento do Hexagonix" align='left'>
<br>
<summary align='left'>5️⃣ Contribua no desenvolvimento do Hexagonix</summary>

<div align="justify">

Se você tem conhecimento em criar código em Assembly x86 e gostaria de ajudar no desenvolvimento do sistema, sinta-se a vontade em me enviar um email! Veja [aqui](https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.pt.md#autor) como entrar em contato comigo!

</div>

</details>

<details title="Reporte erros no sistema" align='left'>
<br>
<summary align='left'>6️⃣ Reporte erros no sistema</summary>

Você pode reportar erros do sistema [aqui](https://github.com/hexagonix/hexagonix/issues).

</details>

<hr>

# Sobre o sistema

<details title="Desenvolvimento" align='left'>
<br>
<summary align='left'>1️⃣ Desenvolvimento</summary>

<div align="justify">

O Hexagonix e todos os seus componentes vêm sendo desenvolvidos desde 2015 e foram escritos completamente em linguagem `Assembly`.

</div>

</details>

<details title="E o código-fonte?" align='left'>
<br>
<summary align='left'>2️⃣ E o código-fonte?</summary>

<div align="justify">

O código-fonte do projeto já foi disponibilizado publicamente. O código do kernel e dos utilitários Unix-like e aplicativos Hexagonix-Andromeda estão disponíveis, assim como o pacote de fontes que compõe o HBoot. As imagens de disco do Hexagonix já estão disponíveis e sua distribuição é livre, obedescendo a licenã BSD-3-clause. Observe a [`licença`](LICENSE) disponível neste repositório para mais informações. Vale ressaltar que a licença de cada pacote de código que compõe o sistema (Hexagon, HBoot, utilitarios Hexagonix, utilitários Hexagonix-Andromeda, fontes e outros componentes) pode variar. Cada pacote pode ser liberado com um tipo de licença diferente (como GPL, MIT ou BSD, por exemplo). Fique atento a cada licença nos respectivos repositórios. Os componentes que não estão disponíveis no repositório oficial ainda se encontram em código fechado, regidos por uma licença proprietária Hexagonix, que pode ser encontrada [aqui](https://github.com/hexagonix/Doc/blob/main/LICENSES/Hexagonix).

</div>

</details>

<details title="A história do Hexagonix" align='left'>
<br>
<summary align='left'>3️⃣ A história do Hexagonix</summary>

<div align="justify">

O Hexagonix começou como uma implementação em estrutura similar a sistemas do tipo DOS, com um interpretador com comandos internos com nomes, sintaxes e resultados semelhantes a um DOS genérico. O interpretador de comandos apresentava comandos de manipulação de arquivos e outros internamente, como em sistemas DOS convencionais. As unidades de disco também eram definidas como letras. Mais tarde, surgiu um interesse crescente e fascínio no funcionamento de sistemas Unix e todo o código foi reescrito ou adaptado para tornar o kernel do sistema um kernel Unix-like. Todos os componentes do sistema, assim como no DOS, eram mantidos em uma única árvore até então. Com a versão 1.5 do Hexagonix, com nome de código "Unix-like", o kernel foi bastante modificado e reescrito para se adequar à filosofia Unix. As mudanças incluíram até mesmo a forma como os dispositivos eram tratados, com a escrita de uma camada de abstração de hardware com o gerenciamento de dispositivos como arquivos, além da adição das chamadas de sistema abrir(), fechar(), escrever() e ler(). Também foram escritos os utilitários base Unix, com a retirada de comandos do interpretador padrão, que foi reescrito para dar lugar a um shell Unix-like. Os camandos internos foram movidos para os utilitários, que passaram a contar com estrutura e sintaxe no estilo Unix. O restante dos utilitários, como mount, foi escrita, já tirando proveito da chamada abrir(), que também é utilizada para montar volume, além de abrir arquivos ordinários do volume. A chamada abrir() também é utilizada para iniciar outros periféricos, como portas seriais e paralelas. A chamada escrever() também funciona com dispositivos e arquivos, bem como fechar(). Foi também introduzido um VFS (Virtual File System), que suportará no futuro vários sistemas de arquivos e que torna transparente para o sistema, programadores e usuários o gerenciamento de arquivos. Também foram incluídas novas funções de gerenciamento de hardware e muitos aprimoramentos e correções de bugs. O sistema ganha novos utilitários Unix até o presente momento. Após essa alteração da proposta e arquitetura, os componentes foram separados e alocados em projetos específicos. A união destes componentes forma o sistema operacional. No caso do Hexagonix, temos o HBoot, Hexagon, shell, bibliotecas e aplicativos Unix, enquanto o ambiente Hexagonix-Andromeda estende o Hexagonix, incorporando outras bibliotecas e aplicativos que as utilizam.

</div>

</details>

<hr>

# Componentes do sistema

<details title="Saturno" align='left'>
<br>
<summary align='left'>1️⃣ Saturno</summary>

<div align="justify">

O primeiro componente do Hexagonix é o Saturno. Ele é responsável por receber o controle do processo de inicialização realizado pelo BIOS/UEFI e procurar no volume o segundo estágio de inicialização. Para isso, ele implementa um driver para leitura de um sistema de arquivos FAT16. O segundo estágio de inicialização (ver adiante) pode implementar drivers para outros sistemas de arquivos e é responsável por encontrar o Hexagon, carregar módulos HBoot ou carregar um sistema do tipo DOS compatível (versão BETA).

</div>

[![Saturno](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Saturno&theme=dark)](https://github.com/hexagonix/Saturno)

</details>

<details title="Hexagon Boot (HBoot)" align='left'>
<br>
<summary align='left'>2️⃣ Hexagon Boot (HBoot)</summary>

<div align="justify">

O Hexagon Boot (HBoot) é um componente desenvolvido para permitir a inicialização do kernel Hexagon. Até então, a inicialização era realizada por apenas um estágio, que definia um ambiente bem básico, carregava o Hexagon na memória e imediatamente o executava, fornecendo um conjunto bem limitado de parâmetros. Isso se deve ao fato de que o código desse estágio fica restrito a 512 bytes, o que limita a realização de diversos testes e processamento de dados. Como o HBoot, foi possível expandir o número de tarefas realizadas antes da execução do Hexagon, além da possibilidade de fornecer mais informações a respeito do ambiente do dispositivo e de inicialização. Isso é particularmente importante para permitir a criação de uma árvore de dispositivos que pode ser utilizada pelo Hexagon para decidir como manipular cada dispositivo identificado. O HBoot é capaz de verificar quais unidades de disco estão disponíveis na máquina, emitir um tom de inicialização, obter a quantidade de memória RAM disponível instalada e permitir ou não o prosseguimento do processo de boot de acordo com essa informação. Caso nenhuma interação do usuário seja detectada (em um tempo de 3 segundos após a inicialização do HBoot e exibição de mensagens ao usuário), o HBoot irá realizar testes adicionais para verificar a capacidade do dispositivo em executar o sistema e irá carregar e executar o Hexagon (presente em um arquivo no volume nomeado de `HEXAGON.SIS`). Após o carregamento, o HBoot transfere o controle para o Hexagon, que é inicializado e armazena no ambiente do kernel os dados fornecidos pelo HBoot.

</div>

[![HBoot](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=HBoot&theme=dark)](https://github.com/hexagonix/Hboot)

### Como interagir com o HBoot

<div align="justify">

A interação com o HBoot se dá pelo pressionamento da tecla `F8` após a inicialização e exibição de mensagens na tela. O HBoot aguarda por 3 segundos alguma intereação e, caso nenhuma tenha ocorrido, continua a executar o protocolo de boot. A interação com o HBoot pode ser interessante para carregar módulos no formato HBoot, fornecer parâmetros de inicialização ao Hexagon, carregar algum sistema do tipo DOS cujos arquivos estejam presentes no mesmo volume ou ainda carregar imagens HAPP de outros núcleos (caso o desenvolvedor deseje utilizar a implementação HBoot em seu projeto). Abaixo, veja mais alguns detalhes de funções adicionais e de diagnóstico que podem ser realizadas via interação com o HBoot antes do carregamento do Hexagonix.

</div>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/HBoot.png" width="600" height="500">
</p>

### Reportar bugs

<div align="justify">

O HBoot ganhou muita complexidade desde o início de seu desenvolvimento, em 2020. Devido a esse aumento de código e a natureza de sua operação (16-bit), bugs podem ser encontrados. Os mesmos podem ser reportados no repositório ou por email, disponível no final deste arquivo.

</div>

</details>

<details title="Kernel Hexagon" align='left'>
<br>
<summary align='left'>3️⃣ Kernel Hexagon</summary>

### O que é

<div align="justify">

O Hexagon é um núcleo (kernel) monolítico executado em modo protegido 32-bit, desenvolvido tendo como alvo a arquitetura PC (x86). É um kernel escrito do zero, visando a velocidade e a compatibilidade de harware moderno mas também sendo capaz de ser executado em hardware mais antigo (como um Pentium III). No momento, garante um ambiente monoutilizador, apesar do uso de terminais virtuais, e monotarefa, apesar da capacidade de carregar, manter em memória e controlar mais de um processo, em uma pilha de execução. Futuramente, o kernel poderá receber suporte a execução de múltiplos processos em multitarefa preemptiva. O Hexagon é um kernel Unix-like e tenta implementar uma compatibilidade POSIX, embora longe desta, e compõe a base do Sistema Operacional Hexagonix, embora independente deste. Ele executa imagens executáveis no formato HAPP, desenvolvido exclusivamente para o Hexagon. O Hexagon também implementa uma API bastante sofisticada acessível através de uma chamada de sistema.

</div>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="180" height="180">
</p>

[![Hexagon Kernel](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Hexagon&theme=dark)](https://github.com/hexagonix/Hexagon)

### Chamadas de sistema

<div align="justify">

As chamadas de sistema são realizadas no estilo `BSD`, com o número da função presente na pilha e os parâmetros/argumentos junto aos registradores. Para uma lista completa de chamadas de sistema disponíveis na versão atual do sistema, dê uma olhada na biblioteca do Hexagon na [libasm para fasm](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s) ou [libasm para nasm](https://github.com/hexagonix/lib/blob/main/nasm/hexagon.s).

</div>

</details>

<details title="Ambiente Hexagonix" align='left'>
<br>
<summary align='left'>4️⃣ Ambiente Hexagonix</summary>

<div align="justify">

O Hexagonix implementa, junto ao Hexagon, uma série de utilitários Unix-like, com funcionalidade e sintaxe de uso semelhante à sistemas UNIX e Unix-like. **Utilitários como init, login, sh, top, ps, cp, rm, cat, clear, man, dentre outros, estão inclusos na distribuição padrão do Hexagonix**. Estes utilitários compõem o pacote de utilitários base do Hexagonix. As ferramentas de inicialização de ambiente de modo usuário e login estão neste pacote, bem como vários arquivos de configuração deste ambiente. Estes utilitários não apresentam, no geral, uma interface gráfica, apenas uma interface em linha de comando (CLI). Entretanto, podem ser solicitados por aplicativos que apresentem interface gráfica. Este ambiente está disponível na distribuição do [`Hexagonix`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img).

</div>

[![Unix-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=unix-apps&theme=dark)](https://github.com/hexagonix/unix-apps)

### Alguns aplicativos e utilitários do ambiente Hexagonix

<div align="justify">

O Hexagonix inclui muitos dos utilitários Unix que você pode já estar familiarizado, como por exemplo:

* `init`
* `login`
* `ls`
* `cat`
* `cp`
* `rm`
* `clear`
* `top`
* `ps`
* `man`
* `su`
* `sh` (shell padrão)
* `uname`
* `whoami`, entre outros.

Alguns aplicativos e utilitários foram desenvolvidos exclusivamente para o Hexagonix, como:

* atop (versão alternativa de top)
* energia (controle de estado do computador)
* fnt (utilitário de alteração de fonte gráfica do Hexagonix)
* hash (shell alternativo)
* log (utilizado para obter logs internos do Hexagon, mas já obsoleto)
* lshapp (obtém e exibe informações de imagens HAPP)
* lshmod (obtém e exibe informações de módulos HBoot e do próprio HBoot)

Vale lembrar que os utilitários do Hexagonix tentam implementar uma interface POSIX na medida do possível, se assemelhando em sintaxe aos utilitários Unix (FreeBSD e Linux utilizados como modelo).
### Aplicativos de terceiros disponíveis para o Hexagonix

* [Flat Assembler (fasm)](https://flatassembler.net/index.php)

O Hexagonix recebeu um port do montador [`fasm`](https://flatassembler.net/index.php), que foi adaptado para o Hexagonix, permitindo ao usuário desenvolver aplicativos diretamente no sistema. Este port é chamado de `fasmX`. As alterações adicionadas ao código, assim como licença do software, podem ser encontradas no [repositório do fasm para o Hexagonix](https://github.com/hexagonix/fasm). Este repositório é um fork do [repositório original](https://github.com/tgrysztar/fasm). O código adicionado é baseado em modificações realizadas do código original e adições autorais. Esse código modificado/autoral pode ser encontrado no repositório, [clicando aqui](https://github.com/hexagonix/fasm/tree/master/SOURCE/HEXAGONIX). O fasmX, port do fasm para Hexagonix, sempre é atualizado quando novidades são adicionadas no repositório do fasm. Para indicar que se trata de uma versão estável e testada, o número de versão do fasmX sempre herda a numeração do fasm, sucedido por um caractere x (como exemplo, a versão baseada no fasm 1.73.30, após teste, recebe a numeração 1.73.30x). Você pode reportar bugs ou problemas de geração ou otimização de código na versão para Hexagonix [aqui](https://github.com/hexagonix/fasm/issues). Para reportar erros gerais do fasm, utilize o repositório [oficial](https://github.com/tgrysztar/fasm).

</div>

</details>

<details title="Ambiente Hexagonix-Andromeda" align='left'>
<br>
<summary align='left'>5️⃣ Ambiente Hexagonix-Andromeda</summary>

<div align="justify">

O ambiente Andromeda do Hexagonix (Hexagonix-Andromeda) é construído sobre a base sólida fornecida pelo Hexagonix, incluindo aplicativos e utilitários que não implementam a filosofia Unix ou apresentam sintaxe e forma de uso bastante diferentes do que se esperaria de um ambiente Unix. Aqui estão o aplicativo de configurações do sistema, calculadora, gerenciador de fontes, editores de texto e a IDE desenvolvida para o Hexagonix. Estes utilitários podem ou não apresentar uma interface gráfica. Juntamente a eles, compõem o ambiente Hexagonix-Andromeda bibliotecas desenvolvidas para permitir o desenvolvimento de aplicativos, como a biblioteca `Estelar`. 

</div>

[![Andromeda-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=andromeda-apps&theme=dark)](https://github.com/hexagonix/andromeda-apps)

### Alguns aplicativos e utilitários do ambiente Andromeda

<div align="justify">

* `Configurações do sistema (Config)`
* `Editor de texto Quartzo`
* `IDE Lyoko para desenvolvimento de aplicativos`
* `Piano eletrônico return Piano;`
* `Utilitário de comunicação serial`
* `Andromeda Shell (ASH)` - Um novo shell para o Hexagonix
* `Calculadora do Hexagonix`
* `Utilitário de alteração de fonte`
* `Utilitário de desligamento do Hexagonix`

### Aplicativos de terceiros disponíveis para o Hexagonix-Andromeda

Ainda não existem aplicativos de terceiros disponíveis para o ambiente Hexagonix-Andromeda. Caso esteja interessado, você pode construir o primeiro!

</div>

</details>

<details title="Fontes gráficas do Hexagonix" align='left'>
<br>
<summary align='left'>6️⃣ Fontes gráficas do Hexagonix</summary>

<div align="justify">

A instalação padrão do Hexagonix também fornece uma série de fontes que podem ser carregadas pelo aplicativo de configurações ou utilitário de fontes (gerenciador de fontes). Esses arquivos são utilizados para alterar a fonte de exibição em modo gráfico do Hexagonix.

As fontes de modo gráfico para Hexagon são desenvolvidas como um bitmap em Assembly que, compiladas, geram uma imagem binária da fonte com representações de cada caractere. Os códigos das fontes padrão do Hexagonix já foram liberados como código livre e estão disponíveis no [repositório de fontes do Hexagonix](https://github.com/hexagonix/xfnt).

</div>

[![Hexagonix-xfnt](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=xfnt&theme=dark)](https://github.com/hexagonix/xfnt)

</details>

<details title="Bibliotecas de desenvolvimento do sistema" align='left'>
<br>
<summary align='left'>7️⃣ Bibliotecas de desenvolvimento do sistema</summary>

<div align="justify">

O Hexagonix também fornece funções que devem ser utilizadas para interagir com o próprio ambiente do sistema. As bibliotecas são utilizadas para acessar funções implementadas pelo Hexagon ou pelas próprias bibliotecas, permitindo o desenvolvimento facilitado de aplicativos e utilitários tanto para o ambiente Hexagonix quanto para o Hexagonix-Andromeda. As bibliotecas implementam funções para exibição de texto, cálculos matemáticos, envio de mensagens, abertura de arquivos e dispositivos e muito mais. A biblioteca básica (hexagon.s) fornece funções acessíveis para ambos os ambientes possíveis de distribuição, enquanto outras bibliotecas podem ser exclusivas do ambiente Hexagonix-Andromeda. Essas bibliotecas incluem funções gráficas para montar interfaces em modo gráfico (Hexagonix-Andromeda), bem como funções para verificar a versão do sistema atualmente em execução. Os utilitários base Hexagonix realizam a checagem da versão do Hexagon para verificar se podem ser executados, utilizando o utilitário Unix uname ou diretamente por uma chamada de sistema do Hexagon.

Para saber mais e verificar cada função disponível nas bibliotecas de desenvolvimento do sistema, veja o repositório da [libasm](https://github.com/hexagonix/lib).

</div>

[![lib](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=lib&theme=dark)](https://github.com/hexagonix/lib)

</details>

<hr>

# Como testar o Hexagonix

<details title="Requisitos do sistema" align='left'>
<br>
<summary align='left'>1️⃣ Requisitos do sistema</summary>

<div align="justify">

Abaixo, uma lista de requisitos mínimos e recomendados para testar o Hexagonix em uma máquina virtual ou máquina física.

</div>

<details title="Requisitos mínimos" align='left'>
<br>
<summary align='left'>⚠️ Requisitos mínimos</summary>

* Processador: Pentium III (1999) com suporte a SSE e MMX ou mais recente;
* Memória RAM: 32 Mb mínimo (uma instalação mínima com 32 Mb costuma ser suficiente, na maioria dos casos);
* Disco rígido: disco rígido IDE ou SATA com mínimo de 50 Mb;
* Periféricos necessários:
  * Porta serial (1-4);
  * Porta paralela (1-4);
  * Teclado PS/2 ou USB;
  * Placa de vídeo VGA com 2 Mb de memória de vídeo (com suporte a cores).

</details>

<details title="Recomendado" align='left'>
<br>
<summary align='left'>✅️ Recomendado</summary>

* Processador: Pentium D ou mais recente;
* Memória RAM: 50 Mb;
* Periféricos opcionais:
  * Mouse PS/2 ou USB;
  * Placa de vídeo com > 2 Mb de memória de vídeo.

</details>

</details>

> Executando o Hexagonix

<details title="Obter as imagens de disco com a instação do sistema" align='left'>
<br>
<summary align='left'>2️⃣ Obter as imagens de disco com a instação do sistema</summary>

<div align="justify">

Para testar o Hexagonix, você vai precisar de uma das imagens de disco disponíveis, bem como a ferramenta [`qemu`](https://www.qemu.org) instalada em seu computador, caso deseje testar o sistema em ambiente virtualizado. A imagem também pode ser utilizada para a gravação em um disco físico em uma máquina real.

Para testar o Hexagonix, obtenha o arquivo [`hexagonix.img`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img).

</div>

</details>

<details title="Testar o Hexagonix em sistema virtualizado" align='left'>
<br>
<summary align='left'>3️⃣ Testar o Hexagonix em sistema virtualizado</summary>

<div align="justify">

Primeiramente, você deve instalar a ferramenta qemu, que irá gerenciar a máquina virtual. Para isso, você pode instalar o qemu utilizando repositórios oficiais de distribuições Linux ou acessando [aqui](https://www.qemu.org) para obter os arquivos de instalação para Windows e macOS.

> Instalar no Debian, Ubuntu, Pop_OS! e derivados:

Para o `Ubuntu`, a linha a seguir irá instalar o qemu e todas as suas dependências (privilégios de superusuário necessários):

```
sudo apt install qemu qemu-system-i386
```

> Instalar no Fedora, CentOS e derivados:

Para o `Fedora`, a linha a seguir irá instalar o qemu e todas as suas dependências (privilégios de superusuário necessários):

```
sudo dnf install qemu qemu-system-i386
```

Agora que você tem o qemu instalado em seu computador, você pode prosseguir com a execução do sistema.

Para executar o sistema de maneira satisfatória, você deve fornecer ao menos 32 MB de RAM para a máquina virtual. Isso se deve a arquitetura de gerenciamento de memória do Hexagon, que exige 16 MB de RAM exclusiva para o kernel a ao menos 16 MB para alocar os aplicativos, utilitários e arquivos abertos. O Hexagon não admite menos que isso para ser executado. Caso mais memória seja fornecida, a memória adicional será sempre reservada, com prioridade, para ser disponibilizada aos processos do usuário. Normalmente, a linha de comando abaixo cumpre todos os requisitos para a execução do sistema:

```
qemu-system-i386 -hda hexagonix.img -m 32 -soundhw pcspk --enable-kvm -serial file:"Serial.txt"
```

Você pode omitir o parâmetro -serial caso queira. Esse parâmetro garante que a saída de debug do Hexagon e aplicativos serão direcionados para um arquivo em seu computador, onde você poderá consultar o que foi enviado. Você também pode omitir o parâmetro -soundhw, responsável por direcionar a saída de som do sistema virtual para seu computador físico. Em alguns sistema Linux, a configuração acima pode entrar em conflito com o pulseaudio, e pode ser omitida. Lembre-se que ao omitir o parâmetro, os sons do sistema não serão emitidos para você.

Lembrando que você deve utilizar uma versão/edição do qemu que consiga executar software escrito para a arquitetura x86 (i386 ou x86_64). Para realizar o download e instalação do `qemu`, clique [aqui](https://www.qemu.org/download/).

</div>

</details>

<details title="Testar o Hexagonix em máquina física" align='left'>
<br>
<summary align='left'>4️⃣ Testar o Hexagonix em máquina física</summary>

<div align="justify">

Você deve utilizar o Linux/macOS ou alguma ferramenta disponível para o Windows que te permita gravar essa imagem em disco.

No `Linux/macOS/Unix`, use a linha abaixo:

```
dd if=hexagonix.img of=/dev/unidade
```
onde `unidade` equivale ao dispositivo desejado (geralmente `sdb` ou `sdc`, em caso de dispositivos USB e `hda`, `hdb`, `sda` ou `sdb`, para unidades de disco rígido/estado sólido). Reinicie seu computador e teste o sistema. Vale lembrar que o modo de boot seguro não é suportado, além de que o boot só é suportado em BIOS ou no modo legado BIOS do UEFI.

Vale ressaltar que o desempenho do sistema pode variar de acordo com a máquina testada. Junta-se a isso o fato de que as versões mais recentes do sistema não foram ou estão sendo testadas diretamente na máquina física, como sistema operacional principal. Caso algum problema ocorra ao executar o Hexagonix em uma máquina física, por favor reporte o erro detalhado [aqui](https://github.com/hexagonix/Distro/issues), em português ou inglês, informando dados como marca do dispositivo, processador, quantidade de memória RAM, placa de vídeo (se disponível) e periféricos conectados, bem como o dispositivo utilizado para instalar o sistema (unidade de disco interna ou mídia removível USB).

</div>

</details>

<details title="Primeiro uso e login" align='left'>
<br>
<summary align='left'>5️⃣ Primeiro uso e login</summary>

<div align="justify">

Ao iniciar o sistema, você deverá introduzir um nome de usuário e senha. Para o primeiro uso, utilize

```
Usuário: root
Senha: root
```

Você pode adicionar outro usuário alterando o arquivo `USUARIO.UNX` na raiz do disco. Lembre-se de não remover o usuário raiz (`root`). Isso pode tornar o sistema inoperante de forma permanente.

</div>

</details>

<details title="Reportar erros" align='left'>
<br>
<summary align='left'>6️⃣ Reportar erros</summary>

<div align="justify">

Você pode reportar erros e ajudar a desenvolver o sistema. Para isso, abra uma notificação de erro [aqui](https://github.com/hexagonix/Distro/issues), informando o erro da forma mais detalhada possível (como marca do dispositivo, processador, quantidade de memória RAM, placa de vídeo e periféricos conectados, bem como o dispositivo utilizado para instalar o sistema, como unidade de disco interna ou mídia removível USB). Lembre-se de informar em qual aplicativo ocorreu o erro, caso o erro ocorra já com o sistema em operação. Caso o problema se dê no processo de inicialização, informe o que foi exibido/o comportamento observado da máquina.

</div>

</details>

<hr>

# Capturas de tela

<details title="Hexagonix" align='left'>
<br>
<summary align='left'>🌙 Hexagonix</summary>

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

Você pode ver mais [aqui](https://github.com/hexagonix/Distro/tree/main/Img).

</details>

<hr>

# Outras informações

<details title="Idiomas" align='left'>
<br>
<summary align='left'>🎲 Idiomas</summary>

<div align="justify">

Neste momento, tanto o sistema quanto a documentação estão disponíveis apenas em português. O documentação em inglês estará disponível em breve, e há planos para suporte ao inglês como possibilidade de idioma principal, além do português (grande parte do trabalho já foi feita).

</div>

</details>

<details title="Inspirações" align='left'>
<br>
<summary align='left'>🖼 Inspirações</summary>

<div align="justify">

Este projeto foi possível e hoje consegue ver a luz do dia graças a muitos outros que já vieram antes e contribuiram com ideias de design e ensinando como deve funcionar um sistema operacional, mesmo que simples. São eles:

* MS-DOS, com código disponível [aqui](https://github.com/microsoft/MS-DOS)
* [MikeOS](http://mikeos.sourceforge.net/)
* [Linux 0.1.1](https://kernel.org)
* [FreeBSD](https://www.freebsd.org/)
* [XNU/Darwin](https://github.com/apple/darwin-xnu)
* [LittleKernel](https://github.com/littlekernel/lk)
* [Google Fuchsia](https://fuchsia.googlesource.com/fuchsia/)

Além disso, outros projetos auxiliaram no desenvolvimento do Hexagonix, seja fornecendo novas ideias de design que fogem da organização tradicional de um sistema operacional, seja fornecendo código bem documentado que permitia entender o funcionamento de certas partes de um sistema operacional, seja contribuindo com exemplos de código que mais tarde vieram a inspirar funções que foram escritas exclusivamente para o Hexagonix (principalmente o código do kernel). Apesar de não terem sido copiados e reutilizados no sistema, o código destes projetos de domínio público possibilitaram que o funcionamento de determinados componentes do computador fossem compreendidos, abrindo portas para que códigos autorais fossem escritos com base no estudo do código destes projetos. Vale ressaltar que os projetos abaixo foram liberados pelos seus desenvolvedores originais em domínio público. São eles:

* [Snowdrop OS](http://www.sebastianmihai.com/snowdrop/), que me inspirou a escrever várias rotinas de controle de hardware e outras funções 16-bit disponíveis no HBoot. O código deste sistema foi liberado em domínio público.
* [Alotware](https://github.com/0x5CE/alotware), que me inspirou a criar as funções de gerenciamento de processos nas versões iniciais do Hexagon (hoje, já reescritas inúmeras vezes). O código deste sistema foi liberado em domínio público.

</div>

</details>

<details title="Autor, contribuidores e contatos" align='left'>
<br>
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

O Hexagonix foi desenvolvido por [Felipe Lunkes](https://github.com/felipenlunkes).

Leia a licença para mais informações sobre direitos autorais, propriedade de código e redistribuição que se aplicam apenas aos arquivos disponíveis neste repositório (não se aplicam ao conjunto de arquivos de dados e de código fonte que compõem o Hexagonix). Vale ressaltar que o código dos componentes do sistema estão sendo liberados aos poucos e que cada pacote pode ser liberado com uma licença diferente. Sempre fique atento ao arquivo `LICENSE` disponível em cada repositório para estar ciente dos direitos e obrigações legais.

</div>

</details>

[^1]: Você pode obter mais informações sobre a relação entre o Darwin e o macOS [aqui](https://pt.wikipedia.org/wiki/Darwin_(sistema_operacional)).
[^2]: Você pode encontrar a página do projeto [aqui](https://www.freedos.org/).
[^3]: A inicialização em modo DOS foi possível após pesquisa na documentação do FreeDOS, especialmente no arquivo "SYS.C" (que pode ser encontrado [aqui](http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/dos/sys/2043/)), que indica em qual segmento o kernel espera ser carregado e quais parâmetros são necessários para inicializar o kernel corretamente. Cada sistema DOS apresenta um segmento de carregamento preferencial e esse carregamento de outras edições do DOS pode ser implementada futuramente com o auxílio de novos módulos HBoot (em caso de sistema proprietário, o usuário deverá ter uma licença). Todo o código para o carregamento do núcleo foi desenvolvido do zero e não se baseia em algum existente (a implementação do HBoot e do modDOS, que são originais, se dão em Assembly, enquanto o código de carregamento do FreeDOS é desenvolvido em C). A implementação original do código do modDOS e de outros módulos para sistemas DOS também livra a implementação de qualquer problema legal.
[^4]: A iniciação em modo de compatibilidade DOS do HBoot (modDOS) pode ser útil para rodar ferramentas de verificação de erros no volume, desfragmentação do volume, particionador e outras ferramentas de diagnóstico, bem como de desenvolvimento, como compiladores e montadores que não são suportados pelo Hexagonix (as ferramentas de 16 bits, por exemplo).
[^5]: Compatível com as chamadas open(), close(), read() e write(), pelo menos em conceito. As chamadas de sistema são realizadas sempre no estilo BSD, com número de função na pilha e parâmetros nos registradores.
