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

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Ponto de partida

<div align="justify")
     
Antes de mais nada, leia a licença abaixo, relativa ao uso e redistribuição do Hexagonix (binários e código-fonte).

</div>

<details title="Licença" align='left'>
<br>
<summary align='left'>Licença de uso e redistribuição do Hexagonix (BSD-3-Clause)</summary>

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

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Fazer download e testar o sistema agora mesmo

<div align="justify">

Neste documento, você encontra um tutorial para executar o Hexagonix em seu computador, tanto em uma versão virtualizada como de forma nativa. Lembre-se que é necessário possuir um computador de arquitetura x86 ou um emulador, caso esteja utilizando um dispositivo de outra arquitetura para testes.

Você pode ir até a sessão de [`lançamentos`](https://github.com/hexagonix/hexagonix/releases) para obter as versões estáveis mais recentes do sistema. Você pode acessar o arquivo de [anúncio de versões](REL.pt.md) do Hexagonix. Sempre que possível, obtenha o último lançamento e realize o download das imagens .img ou .vdi disponíveis ou do pacote completo em formato zip. As versões disponíveis diretamente neste repositório são sempre versões de desenvolvimento (beta e release candidate), que também podem ser executadas e estão minimamente funcionais. Ao final de cada ciclo de desenvolvimento, as versões finais estarão disponíveis na sessão [lançamentos](https://github.com/hexagonix/hexagonix/releases).

</div>

<details title="Documentação do sistema" align='left'>
<br>
<summary align='left'>Documentação do sistema</summary>

* [Documentação do sistema (em construção)](https://github.com/hexagonix/Doc)

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Como testar o Hexagonix

## Requisitos do sistema

<div align="justify">

Abaixo, uma lista de requisitos mínimos e recomendados para testar o Hexagonix em uma máquina virtual ou máquina física.

</div>

### Requisítos mínimos:

<div align="justify">
   
* Processador: Pentium III (1999) com suporte a SSE e MMX ou mais recente;
* Memória RAM: 32 Mb mínimo (uma instalação mínima com 32 Mb costuma ser suficiente, na maioria dos casos);
* Disco rígido: disco rígido IDE ou SATA com mínimo de 50 Mb;
* Periféricos necessários:
  * Porta serial (1-4);
  * Porta paralela (1-4);
  * Teclado PS/2 ou USB;
  * Placa de vídeo VGA com 2 Mb de memória de vídeo (com suporte a cores).

</div>
   
#### Recomendado:

<div align="justify">
   
* Processador: Pentium D ou mais recente;
* Memória RAM: 50 Mb;
* Periféricos opcionais:
  * Mouse PS/2 ou USB;
  * Placa de vídeo com > 2 Mb de memória de vídeo.

</div>
   
## Obter as imagens de disco com a instação do sistema

<div align="justify">

Para testar o Hexagonix, você vai precisar de uma das imagens de disco disponíveis, bem como a ferramenta [`qemu`](https://www.qemu.org) instalada em seu computador, caso deseje testar o sistema em ambiente virtualizado. A imagem também pode ser utilizada para a gravação em um disco físico em uma máquina real.

Para testar o Hexagonix, obtenha o arquivo [`hexagonix.img`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img).

</div>

## Testar o Hexagonix em sistema virtualizado

<div align="justify">

Primeiramente, você deve instalar a ferramenta `qemu`, que irá gerenciar a máquina virtual. Para isso, você pode instalar o qemu utilizando repositórios oficiais de distribuições Linux ou acessando [aqui](https://www.qemu.org) para obter os arquivos de instalação para Windows e macOS.

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

> Instalar no FreeBSD e derivados:

Para o `FreeBSD`, a linha a seguir irá instalar o qemu e todas as suas dependências (privilégios de superusuário necessários):

```
su
pkg update
pkg install qemu
```
     
Agora que você tem o qemu instalado em seu computador, você pode prosseguir com a execução do sistema.

Para executar o sistema de maneira satisfatória, você deve fornecer ao menos 32 MB de RAM para a máquina virtual. Isso se deve a arquitetura de gerenciamento de memória do Hexagon, que exige 16 MB de RAM exclusiva para o kernel a ao menos 16 MB para alocar os aplicativos, utilitários e arquivos abertos. O Hexagon não admite menos que isso para ser executado. Caso mais memória seja fornecida, a memória adicional será sempre reservada, com prioridade, para ser disponibilizada aos processos do usuário. Normalmente, a linha de comando abaixo cumpre todos os requisitos para a execução do sistema:

```
qemu-system-i386 -hda hexagonix.img -m 32 -soundhw pcspk --enable-kvm -serial file:"Serial.txt"
```

Você pode omitir o parâmetro -serial caso queira. Esse parâmetro garante que a saída de debug do Hexagon e aplicativos serão direcionados para um arquivo em seu computador, onde você poderá consultar o que foi enviado. Você também pode omitir o parâmetro -soundhw, responsável por direcionar a saída de som do sistema virtual para seu computador físico. Em alguns sistema Linux, a configuração acima pode entrar em conflito com o pulseaudio, e pode ser omitida. Lembre-se que ao omitir o parâmetro, os sons do sistema não serão emitidos para você.

Lembrando que você deve utilizar uma versão/edição do qemu que consiga executar software escrito para a arquitetura x86 (i386 ou x86_64). Para realizar o download e instalação do `qemu`, clique [aqui](https://www.qemu.org/download/).

</div>

## Testar o Hexagonix em máquina física</summary>

<div align="justify">

Você deve utilizar o Linux/macOS ou alguma ferramenta disponível para o Windows que te permita gravar essa imagem em disco.

No `Linux/macOS/Unix`, use a linha abaixo:

```
dd if=hexagonix.img of=/dev/unidade
```
onde `unidade` equivale ao dispositivo desejado (geralmente `sdb` ou `sdc`, em caso de dispositivos USB e `hda`, `hdb`, `sda` ou `sdb`, para unidades de disco rígido/estado sólido). Reinicie seu computador e teste o sistema. Vale lembrar que o modo de boot seguro não é suportado, além de que o boot só é suportado em BIOS ou no modo legado BIOS do UEFI.

Vale ressaltar que o desempenho do sistema pode variar de acordo com a máquina testada. Junta-se a isso o fato de que as versões mais recentes do sistema não foram ou estão sendo testadas diretamente na máquina física, como sistema operacional principal. Caso algum problema ocorra ao executar o Hexagonix em uma máquina física, por favor reporte o erro detalhado [aqui](https://github.com/hexagonix/Distro/issues), em português ou inglês, informando dados como marca do dispositivo, processador, quantidade de memória RAM, placa de vídeo (se disponível) e periféricos conectados, bem como o dispositivo utilizado para instalar o sistema (unidade de disco interna ou mídia removível USB).

</div>

## Primeiro uso e login</summary>

<div align="justify">

Ao iniciar o sistema, você deverá introduzir um nome de usuário e senha. Para o primeiro uso, utilize

```
Usuário: root
Senha: root
```

Você pode adicionar outro usuário alterando o arquivo `USUARIO.UNX` na raiz do disco. Lembre-se de não remover o usuário raiz (`root`). Isso pode tornar o sistema inoperante de forma permanente.

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

<details title="Contribua no desenvolvimento do Hexagonix" align='left'>
<br>
<summary align='left'>Contribua no desenvolvimento do Hexagonix</summary>

<div align="justify">

Se você tem conhecimento em criar código em Assembly x86 e gostaria de ajudar no desenvolvimento do sistema, sinta-se a vontade em me enviar um email! Veja [aqui](https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.pt.md#autor) como entrar em contato comigo!

</div>

</details>

<details title="Reporte erros no sistema" align='left'>
<br>
<summary align='left'>Reporte erros no sistema</summary>

<div align="justify">
   
Você pode reportar erros e ajudar a desenvolver o sistema. Para isso, abra uma notificação de erro [aqui](https://github.com/hexagonix/Distro/issues), informando o erro da forma mais detalhada possível (como marca do dispositivo, processador, quantidade de memória RAM, placa de vídeo e periféricos conectados, bem como o dispositivo utilizado para instalar o sistema, como unidade de disco interna ou mídia removível USB). Lembre-se de informar em qual aplicativo ocorreu o erro, caso o erro ocorra já com o sistema em operação. Caso o problema se dê no processo de inicialização, informe o que foi exibido/o comportamento observado da máquina.

</div>
   
</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

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
