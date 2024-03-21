<!--

Versão deste arquivo: 5.0

Copyright (c) 2015-2024 Felipe Miguel Nery Lunkes
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
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

<table align="center">
<tr>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md">Sobre o Hexagonix</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md#-capturas-de-tela">Screenshots</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md#contribuir-e-reportar-erros">Contribuir</a></td>
<td><a href="https://github.com/hexagonix/src">Código-fonte</a></td>
</tr>
</table>

# Ponto de partida

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

Neste documento, você encontra um tutorial para executar o Hexagonix em seu computador, tanto em uma versão virtualizada como de forma nativa. Lembre-se que é necessário possuir um computador de arquitetura x86 ou um emulador, caso esteja utilizando um dispositivo de outra arquitetura para testes.

Leia a licença abaixo, relativa ao uso e redistribuição do Hexagonix (binários e código-fonte).

</div>

<details title="Licença" align='left'>
<br>
<summary align='left'>Licença de uso e redistribuição do Hexagonix (BSD-3-Clause)</summary>

<div align="justify">

Hexagonix Operating System

BSD 3-Clause License

Copyright (c) 2015-2024, Felipe Miguel Nery Lunkes <br>
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

# Fazer download e executar o sistema agora mesmo

<div align="justify">

Você pode ir até a sessão de [`lançamentos`](https://github.com/hexagonix/hexagonix/releases) para obter as versões estáveis mais recentes do sistema. Você pode acessar o arquivo de [anúncio de versões](REL.pt.md) do Hexagonix. Sempre que possível, obtenha o último lançamento e realize o download das imagens .img ou .vdi disponíveis ou do pacote completo em formato zip. As versões disponíveis diretamente neste repositório são sempre versões de desenvolvimento (beta e release candidate), que também podem ser executadas e estão minimamente funcionais. Ao final de cada ciclo de desenvolvimento, as versões finais estarão disponíveis na sessão [lançamentos](https://github.com/hexagonix/hexagonix/releases).

> Nota 1: A versão/build do Hexagonix disponibilizada no ramo principal do repositório é sempre a corrente. Isso é, a versão de testes mais recente. Vá sempre na aba de [lançamentos](https://github.com/hexagonix/hexagonix/releases) para obter as imagens finais de cada versão do sistema. Como exemplo em sistemas UNIX, como o FreeBSD, o ramo principal se assemelha à versão CURRENT do software, não sendo necessariamente uma versão estável. Para obter versões estáveis, vá na aba de lançamentos e busque uma versão sem os termos: -dev, -dev.beta* ou -CURRENT. Normalmente essas versões apresentam nomes simples, como H1, H1-R6, H2, H2-R1 e assim por diante, ou apresentam -RELEASE em seu nome.

> Nota 2: Confira sempre o [anúncio de versões](REL.pt.md) para obter o changelog completo da versão (o que muda em relação à versão anterior do sistema).

</div>

<details title="Documentação do sistema" align='left'>
<br>
<summary align='left'>Documentação do sistema</summary>

<div align="center">

[![Doc](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Doc&theme=dark)](https://github.com/hexagonix/Doc)

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Como executar o Hexagonix

## Requisitos do sistema

<div align="justify">

Abaixo, uma lista de requisitos mínimos e recomendados para testar o Hexagonix em uma máquina virtual ou máquina física.

</div>

### Requisítos mínimos:

<div align="justify">
   
* Processador: `Pentium III` (1999) com suporte a `SSE` e `MMX` ou mais recente;
* Memória RAM: `32 MB mínimo` (uma instalação mínima com 32 MB costuma ser suficiente, na maioria dos casos);
* Disco rígido: disco rígido IDE ou SATA com mínimo de 50 MB (`para instalação`);
* Periféricos necessários:
  * Porta serial (1-4);
  * Porta paralela (1-4);
  * Teclado PS/2 ou USB;
  * Placa de vídeo VGA com `2 MB` de memória de vídeo (com suporte a cores).

</div>
   
#### Recomendado:

<div align="justify">
   
* Processador: Pentium D ou mais recente;
* Memória RAM: 50 MB;
* Periféricos opcionais:
  * Mouse PS/2 ou USB;
  * Placa de vídeo com > 2 MB de memória de vídeo.

</div>
   
## Obter as imagens de disco com o sistema

<div align="justify">

Para testar o Hexagonix, você vai precisar de uma das imagens de disco disponíveis, bem como a ferramenta [`qemu`](https://www.qemu.org) instalada em seu computador, caso deseje testar o sistema em ambiente virtualizado. A imagem também pode ser utilizada para a gravação em um disco físico em uma máquina real.

Para testar o Hexagonix, obtenha o arquivo [`hexagonix.img`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img) (qemu) ou [`hexagonix.vdi`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.vdi) (VirtualBox e outros virtualizadores e gerenciadores de máquinas virtuais).

> Nota: A imagem `hexagonix.img` é compatível apenas com o qemu. Além disso, ela é o único arquivo de imagem que pode ser utilizado para instalar o sistema em um disco rígido ou dispositivo flash USB. Para executar o Hexagonix utilizando o VirtualBox ou outros virtualizadores ou gerenciadores de máquinas virtuais, use a imagem `hexagonix.vdi` disponibilizada. A imagem VDI não pode ser utilizada para instalar o Hexagonix em um disco físico (disco rígido, SSD ou unidade USB).

</div>

## Testar o Hexagonix em ambiente virtualizado

<div align="justify">

Primeiramente, você deve instalar a ferramenta `qemu`, que irá gerenciar a máquina virtual. Para isso, você pode instalar o qemu utilizando repositórios oficiais de distribuições Linux ou acessando [aqui](https://www.qemu.org) para obter os arquivos de instalação para Windows e macOS.

> Instalar no Debian, Ubuntu, Pop_OS! e derivados:

Para o `Ubuntu`, a linha a seguir irá instalar o qemu e todas as suas dependências (privilégios de usuário root necessários):

```
sudo apt install qemu qemu-system-i386
```

> Instalar no Fedora, CentOS e derivados:

Para o `Fedora`, a linha a seguir irá instalar o qemu e todas as suas dependências (privilégios de usuário root necessários):

```
sudo dnf install qemu qemu-system-i386
```

> Instalar no FreeBSD e derivados:

Para o `FreeBSD`, a linha a seguir irá instalar o qemu e todas as suas dependências (privilégios de usuário root necessários):

```
su
pkg update
pkg install -y qemu
```
     
Agora que você tem o qemu instalado em seu computador, você pode prosseguir com a execução do sistema.

Para executar o sistema de maneira satisfatória, você deve fornecer ao menos 32 MB de RAM para a máquina virtual. Isso se deve a arquitetura de gerenciamento de memória do Hexagon, que exige 16 MB de RAM exclusiva para o kernel a ao menos 16 MB para alocar os aplicativos, utilitários e arquivos abertos. O Hexagon não admite menos que isso para ser executado. Caso mais memória seja fornecida, a memória adicional será sempre reservada, com prioridade, para ser disponibilizada aos processos do usuário. Normalmente, a linha de comando abaixo cumpre todos os requisitos para a execução do sistema:

> Nota 1: Para usuários do FreeBSD, omita a opção `--enable-kvm`. O uso de KVM só está disponível em sistemas Linux. Note que a execução do Hexagonix pode ficar mais lenta ao ser executado sem KVM. Neste caso, você pode utilizar o `VirtualBox` no FreeBSD para obter um bom desempenho. Para isso, faça o download do VirtualBox [aqui](https://www.virtualbox.org/) ou utilize, como usuário root, `pkg install -y virtualbox`. Para utilizar o VirtualBox, não faça o download da imagem `hexagonix.img`, compatível apenas com o `qemu`, mas sim a imagem [hexagonix.vdi](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.vdi), uma imagem compatível com os principais virtualizadores e gerenciadores de máquinas virtuais, incluindo o qemu.

> Nota 2: Para sistemas que utilizam PulseAudio, você pode encontrar problemas de som na máquina virtual ao utilizar a opção `-soundhw pcspk`. Caso isso ocorra, omita esse parâmetro ao executar o sistema no qemu. Aos usuários de Pipewire, esses artefatos parecem não ocorrer.

```
qemu-system-i386 -hda hexagonix.img -m 32 -soundhw pcspk --enable-kvm -serial file:"Serial.txt"
```

Você pode omitir o parâmetro -serial caso queira. Esse parâmetro garante que a saída de debug do Hexagon e aplicativos serão direcionados para um arquivo em seu computador, onde você poderá consultar o que foi enviado. Você também pode omitir o parâmetro -soundhw, responsável por direcionar a saída de som do sistema virtual para seu computador físico. Em alguns sistema Linux, a configuração acima pode entrar em conflito com o PulseAudio, e pode ser omitida. Lembre-se que ao omitir o parâmetro, os sons do sistema não serão emitidos para você.

Lembrando que você deve utilizar uma versão/edição do qemu que consiga executar software escrito para a arquitetura x86 (i386 ou x86_64). Para realizar o download e instalação do `qemu`, clique [aqui](https://www.qemu.org/download/).

</div>

## Testar o Hexagonix em máquina física

<div align="justify">

Você deve utilizar o Linux/macOS ou alguma ferramenta disponível para o Windows que te permita gravar essa imagem em disco.

No `Linux/BSD/macOS/UNIX`, use a linha abaixo:

```
dd if=hexagonix.img of=/dev/unidade
```
onde `unidade` equivale ao dispositivo desejado (geralmente `sdb` ou `sdc`, em caso de dispositivos USB e `hda`, `hdb`, `sda` ou `sdb`, para unidades de disco rígido/estado sólido). Reinicie seu computador e teste o sistema. Vale lembrar que o modo de boot seguro não é suportado, além de que o boot só é suportado em BIOS ou no modo legado BIOS do UEFI.

> Nota: O nome de dispositivo de disco varia entre os sistemas UNIX e Unix-like. Enquanto você pode instalar o sistema em uma unidade flash USB no Linux através do caminho `/dev/sdb`, por exemplo, no macOS o nome de dispositivo pode se parecer com `/dev/rdisk1`. Verifique corretamente o caminho do dispositivo antes de executar o comando citado acima.

Vale ressaltar que o desempenho do sistema pode variar de acordo com a máquina testada. Junta-se a isso o fato de que as versões mais recentes do sistema não foram ou estão sendo testadas diretamente na máquina física, como sistema operacional principal. Caso algum problema ocorra ao executar o Hexagonix em uma máquina física, por favor reporte o erro detalhado [aqui](https://github.com/hexagonix/Distro/issues), em português ou inglês, informando dados como marca do dispositivo, processador, quantidade de memória RAM, placa de vídeo (se disponível) e periféricos conectados, bem como o dispositivo utilizado para instalar o sistema (unidade de disco interna ou mídia removível USB).

</div>

## Primeiro uso e login

<div align="justify">

Ao iniciar o sistema, você deverá introduzir um nome de usuário e senha. Para o primeiro uso, utilize:

```
Usuário: root
Senha: root
```

ou

```
Usuário: jack
Senha: jack
```

Você pode adicionar outro usuário alterando o arquivo `USUARIO.UNX` (Hexagonix H1) ou `passwd` (Hexagonix H2 e versões posteriores) na raiz do disco. Lembre-se de não remover o usuário raiz (`root`). Isso pode tornar o sistema inoperante de forma permanente.

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Contribuir e reportar erros

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

Neste momento, o sistema está disponível em **inglês** e a documentação está disponível tanto em **português (Brasil)** quanto em **inglês**. 

> Os comentários e código nos arquivos que compõem o código-fonte do sistema estão disponíveis apenas em inglês (**a partir da versão System I**. Em versões anteriores, o sistema e o código-fonte estavam disponíveis apenas em português do Brasil).

Para permitir colaboração e atrair desenvolvedores que não falam português, todo o sistema foi traduzido para o inglês e seu desenvolvimento será integralmente realizado neste idioma.

> Sinta-se livre e incentivado a reportar erros de tradução, seja criando uma `issue` no repositório que contém o arquivo, enviando um email ou criando um `Pull Request` com a correção.

</div>

</details>

<details title="Contribuidores e contatos" align='left'>
<br>
<summary align='left'>👥️️ Contribuidores e contatos</summary>

Veja os arquivos **AUTHORS** e **CREDITS** disponíveis em cada repositório para mais informações sobre autoria, contribuições e contato. 

</details>

<details title="Notas de direitos autorais" align='left'>
<br>
<summary align='left'>📝 Notas de direitos autorais</summary>

<div align="justify">

Leia a licença para mais informações sobre direitos autorais, propriedade de código e redistribuição que se aplicam apenas aos arquivos disponíveis neste repositório (não se aplicam ao conjunto de arquivos de dados e de código-fonte que compõem o Hexagonix). Sempre fique atento ao arquivo `LICENSE` disponível em cada repositório para estar ciente dos direitos e obrigações legais.

</div>

</details>