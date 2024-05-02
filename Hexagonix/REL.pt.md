<!-- Vamos adicionar o logotipo do sistema -->

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
<td><a href="https://github.com/hexagonix/src">Código fonte</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.pt.md">Download</a></td>
</tr>
</table>

# Lançamentos do Hexagonix

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

Aqui você pode obter mais informações sobre todas as versões do Hexagonix já lançadas. As informações estão disponíveis tanto para as versões estáveis do sistema, quanto para as versões de desenvolvimento, chamadas de `projetos`, no contexto do Hexagonix.

> Você pode obter as versões `estáveis` mais recentes do Hexagonix acessando a área de [lançamentos](https://github.com/hexagonix/hexagonix/releases).

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## Versões estáveis lançadas

Aqui você irá encontrar mais informações sobre as versões estáveis do Hexagonix, bem como mais informações sobre elas e o changelog oficial.

<details title="Hexagonix System I" align='left'>
<br>
<summary align='left'>Hexagonix System I</summary>

<div align="justify">                                                                                                                          


> :construction: Aviso! Em 19/10/2023, a versão do Hexagon foi alterada para v1.0.0. Isso se deve à diversas melhorias e correções de estabilidade do sistema, além da correção de diversos bugs durante a execução do sistema. Desta forma, assim como versões antigas descontinuadas, o Hexagonix System I foi lançado com um kernel v1.0.0.

O Hexagonix System I é a versão mais estável e com mais recursos lançada até o momento. Diversas melhorias foram realizadas para garantir estabilidade, segurança e menor uso de recursos. Abaixo, mais informações técnicas sobre essa versão do sistema.

Informações técnicas relevantes desta versão:

- Saturno v1.9.0:
  - MBR v1.2.0:
    - Correções de erros diversos;
    - Comentários e código-fonte completamente em inglês, facilitando a colaboração.
  - Comentários e código-fonte completamente em inglês, facilitando a colaboração.
- HBoot v0.11.0:
  - Correções de erros diversos;
  - Melhorias na detecção de hardware;
  - Mensagens, logs, comentários e código-fonte completamente em inglês, facilitando a colaboração.
- Kernel Hexagon v1.0.0:
  - Diversos refinamentos de estabilidade e performance;
  - Menor consumo de recursos;
  - Gerenciamento aprimorado de erros;
  - Correções gerais de bugs;
  - Novas chamadas de sistema;
  - Padronização das chamadas de sistema, utilizando como referência o UNIX Version 7;
  - Mensagens, logs, comentários e código-fonte completamente em inglês, facilitando a colaboração.
- UnixUtils e CoreUtils v9.0:
  - Novo utilitário mv, utilizado para renomear arquivos no volume;
  - Tratamento aprimorado de erros;
  - Menor consumo de recursos;
  - Correção de diversos bugs;
  - Melhorias nas mensagens para o usuário, incluindo correções ortográficas e de formatação;
  - Mensagens, logs, comentários e código-fonte completamente em inglês, facilitando a colaboração.
- Andromeda Apps (ambiente Hexagonix-Andromeda):
  - Atualização das bibliotecas de desenvolvimento;
  - Correções de erros diversos;
  - Melhorias no consumo de recursos;
  - Mensagens, logs, comentários e código-fonte completamente em inglês, facilitando a colaboração.
- libasm v2.2.1:
  - Padronização de todas as bibliotecas de desenvolvimento;
  - Padronização das chamadas de sistema, utilizando como referência o UNIX Version 7 e o Hexagon v1.0.0;
  - Melhorias nos comentários, tornando melhor a compreensão do código;
  - Melhorias nos utilitários de exemplo da libasm (tapp.asm e gapp.asm);
  - Comentários e código-fonte completamente em inglês, facilitando a colaboração.
- fasm (flat assembler) v1.73.32:
  - Padronização do código responsável pela compatibilidade com o Hexagonix;
  - Atualização do código responsável pela compatibilidade com o Hexagonix para uso das bibliotecas mais recentes;
  - Melhorias da versão base do fasm v1.73.32;
  - Comentários e código-fonte do código dependente do Hexagonix traduzidos para o inglês, facilitanto a colaboração.
- Scripts (ferramentas de construção do Hexagonix):
  - Correção de diversos erros;
  - Melhorias nas mensagens para o usuário, incluindo correções ortográficas e de formatação;
  - Novas opções de execução virtualizada do sistema;
  - Compatibilidade do utilitário `hx` com as versões mais recentes do `qemu`;
  - Início de compatibilidade de construção em sistemas BSD;
  - Compatibilidade com sistemas BSD para execução do Hexagonix em ambiente virtualizado (qemu);
  - Melhorias no módulo de formatação de código;
  - Melhorias nos módulos de construção de utilitários Unix e Andromeda (Hexagonix-Andromeda);
  - Melhorias no log de construção do sistema;
  - Melhorias no módulo de configuração de construção (`configure.sh`);
  - Refatoração do utilitário hx para reusar código e remover código duplicado;
  - Novos parâmetros e funções disponíveis no utilitário hx:
    - "--flags": exibe informações dos parâmetros de construção enviados ao montador.
  - Comentários e código-fonte do código dependente do Hexagonix traduzidos para o inglês, facilitanto a colaboração.
- Documentação:
  - Atualização da documentação do sistema no repositório disponível no GitHub;
  - Refinamentos nos textos em português e nas traduções para o inglês;
  - Atualização das tabelas de chamadas de sistema.

</div>

</details>

## Versões de desenvolvimento

As versões de desenvolvimento do Hexagonix são nomeadas como `projetos` e podem dar origem à novas versões de lançamento do sistema. Essas versões apresentam componentes e características que podem estar instáveis ou inacabadas, e são inicialmente versões de pesquisa em design e implementação do Hexagonix. As versões de desenvolvimento, denominadas projetos, podem ou não ser a origem de uma nova versão de lançamento do Hexagonix, embora recursos possam ser implementados integralmente ou não em uma versão de lançamento. `Independente disso, o Hexagonix está sendo desenvolvido continuamente`.

<details title="Projeto Raava" align='left'>
<br>
<summary align='left'>Projeto Raava (2023-2024) - projeto concluído</summary>

<div align="justify">

O Projeto Raava é um fork do Hexagonix H2 Release 2 (ramo CURRENT), que objetiva desenvolver o próximo lançamento estável do sistema, a versão H3 (sem cronograma de lançamento definido - o lançamento pode não ocorrer em 2023). Para isso, o sistema parte de:

- Hexagon baseado na antiga v1.3.6 (versão 1.3 revisão 6);
- Base do Hexagonix H2 Release 2 (H2R2): H2-CURRENT+290320231532;
- Hexagon v1.3.7 (versão 1.3 revisão 7) - 20/05/2023;

</div>

</details>

## Versões descontinuadas do Hexagonix

<div align="justify">

Abaixo você pode ter acesso direto a algumas versões que se tornaram marcos na distribuição do sistema e ter acesso a informações resumidas sobre elas. Essas versões foram descontinuadas e não recebem mais atualizações e correções.

> :construction: Aviso! Em 19/10/2023, a versão do Hexagon foi alterada para v1.0.0. Isso se deve à diversas melhorias e correções de estabilidade do sistema, além da correção de diversos bugs durante a execução do sistema. Além disso, novas funcionalidades adicionadas mostraram que agora o kernel se aproxima de uma maturidade e estabilidade. Desta forma, assim como a versão H1, a próxima edição do sistema será lançado com um kernel v1.0.0. Sendo assim, todas as versões do kernel reportadas abaixo dizem respeito a uma numeração anterior à remarcação de versão, e não dizem respeito às versões atuais do Hexagon.

</div>

<details title="Versões descontinuadas do Hexagonix" align='left'>
<br>
<summary align='left'>Versões descontinuadas do Hexagonix</summary>

<details title="Hexagonix H1" align='left'>
<br>
<summary align='left'><strong>Hexagonix H1</strong></summary>

<div align="justify">

Essa é a primeira versão amplamente testada e marcada como estável do sistema. O Hexagonix H1 também é a base do Andromeda H1. Muitas melhorias foram feitas desde as versões anteriores do sistema, que utilizavam séries de números para identificar as versões. A versão 1.2-beta, na numeração anterior, foi aprimorada e serviu de base para o desenvolvimento da versão mais estável até hoje, a versão H1, o lançamento público do sistema. Você pode obter essa versão [aqui](https://github.com/hexagonix/hexagonix/releases/tag/H1). Essa versão continuará sendo aprimorada e as alterações serão disponibilizadas continuamente.

</div>

<details title="Hexagonix H1 R1 (Caladan)" align='left'>
<br>
<summary align='left'>Hexagonix H1 R1 (Caladan)</summary>

<div align="justify">

O Hexagonix H1 R1 (codenome Caladan) é o primeiro pacote de correções para a versão H1 do Hexagonix. Muitas melhorias foram feitas em vários utilitários Unix do Hexagonix, bem como aprimoramentos e correções foram feitas no userland Andromeda. O Hexagon foi atualizado para a versão 9.3, com muitas correções de bugs, melhorias de estabilidade, maior desempenho e menor footprint de memória, bem como suporte corrigido à mouses PS/2 (e USB por emulação PS/2) e outros dispositivos. Vá até a área de [lançamentos](https://github.com/hexagonix/hexagonix/releases) e busque a versão H1 R1.

</div>

</details>

<details title="Hexagonix H1 R2 (Caladan)" align='left'>
<br>
<summary align='left'>Hexagonix H1 R2 (Caladan)</summary>

<div align="justify">

Segundo pacote de atualizações para a versão H1 do Hexagonix/Andromeda, que inclui:

- Kernel Hexagon v9.4A;
- Melhorias em vários utilitários do Hexagonix;
- Melhorias em vários utilitários Andromeda;

Foram identificadas diversas falhas de execução em vários utilitários do Andromeda que foram corrigidas nessa versão. Atualizações também foram adicionadas ao Hexagon, diminuindo a pressão de memória e mirando erros identificados durante a execução do sistema. Os manuais do sistema também foram atualizados, bem como a nomenclatura usada em uma série de utilitários. A partir de agora, a próxima atualização da versão H1 irá focar em melhorias e adição de novos recursos.  Vá até a área de [lançamentos](https://github.com/hexagonix/hexagonix/releases) e busque a versão H1 R2.

</div>

</details>

<details title="Hexagonix H1 R3 (Duna)" align='left'>
<br>
<summary align='left'>Hexagonix H1 R3 (Duna)</summary>

<div align="justify">

Lançamento final da versão H1 do sistema. Esse é o lançamento análogo a uma versão 1.0 do software. Para tanto, os números de versão interno de diversos componentes do sistema foram alterados para celebrar esse marco. O Hexagon passa a se identificar como na versão 1.0, bem como outros componentes. A versão foi amplamente testada e está estável. O lançamento H1 R3 inclui:

- Kernel Hexagon v1.0;
- Correções gerais em vários utilitários Hexagonix e Andromeda;
- Melhorias nas bibliotecas do sistema;
- Correções de estabilidade em vários utilitários;
- Melhorias no Configuações do Andromeda;

Vá até a área de [lançamentos](https://github.com/hexagonix/hexagonix/releases) e busque a versão H1 R3.

</div>

</details>

<details title="Hexagonix H1 R4 (Vega)" align='left'>
<br>
<summary align='left'>Hexagonix H1 R4 (Vega)</summary>

<div align="justify">

Melhorias e correção de bugs em todo o sistema.

- Correções gerais em vários utilitários Hexagonix e Andromeda;
- Melhorias nas bibliotecas do sistema;
- Correções de estabilidade em vários utilitários;
- Melhorias no Configuações do Andromeda;

</div>

</details>

<details title="Hexagonix H1 R5 (Orion)" align='left'>
<br>
<summary align='left'>Hexagonix H1 R5 (Orion)</summary>

<div align="justify">

Essa atualização do sistema conserta vários bugs no sistema, incluindo problemas encontrados ao iniciar em máquinas físicas e em ambientes virtualizados no HBoot e no Hexagon.

- Kernel Hexagon v1.1;
- Correções gerais em vários utilitários Hexagonix e Andromeda;
- Melhorias nas bibliotecas do sistema;
- Correções de estabilidade em vários utilitários;
- Melhorias no Configuações do Andromeda;

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

</details>

<details title="Hexagonix H2" align='left'>
<br>
<summary align='left'><strong>Hexagonix H2</strong></summary>

<details title="Hexagonix H2 (versões de desenvolvimento)" align='left'>
<br>
<summary align='left'>Hexagonix H2 (versões de desenvolvimento)</summary>

<details title="Hexagonix H2-dev.beta1" align='left'>
<br>
<summary align='left'>Hexagonix H2-dev.beta1</summary>

<div align="justify">

A versão em desenvolvimento, H2 (codenome Vita Nova), é a próxima versão do Hexagonix. Até o momento, as alterações e melhorias em relação ao Hexagonix H1-R6 são:

- Kernel Hexagon v1.1.2;
- Fusão das distribuições Hexagonix e Andromeda em uma única distribuição;
- Remoção de extensão de arquivo para os binários do sistema;
- Adição de termos de licença na imagem do sistema;
- Melhorias em utilitários Unix e Hexagonix-Andromeda (antigos aplicativos Andromeda);
- Hexagon Boot v0.3 (incompatível com a versão H1).

</div>

</details>

<details title="Hexagonix H2-dev.beta4" align='left'>
<br>
<summary align='left'>Hexagonix H2-dev.beta4</summary>

<div align="justify">

- Alteração profunda no utilitário Unix atop;
- atop renomeado para htop;
- Melhorias no daemon logind;
- Fonte hint renomeada para Avatar;
- Remoção do arquivo Unix.sh da libasm;
- Constantes de Unix.s movidas para o utilitário Unix man.

</div>

</details>

<details title="Hexagonix H2-dev.beta5" align='left'>
<br>
<summary align='left'>Hexagonix H2-dev.beta5</summary>

<div align="justify">

- Correção de emergência do Hexagon (v1.1.7), devido à problemas de vazamento de memória ao solicitar a reinicialização do dispositivo (afeta as versões H2-dev.beta1 a H2-dev.beta4);
- Utilitário init v2.0, com suporte a execução de múltiplos serviços em lista.
- Desativação do modo de login "moderno" em logind. A interface de login padrão segue o observado em sistemas Unix-like (FreeBSD como maior inspiração);
- Melhorias gerais nos seguintes utilitários Unix:
  - [x] login;
  - [x] energia;
  - [x] htop;
  - [x] man;
  - [x] su;
  - [x] top;
  - [x] uname;
- Testes executados para verificar o funcionamento correto do sistema (nenhum novo problema encontrado).

</div>

</details>

<details title="Hexagonix H2-dev.beta6" align='left'>
<br>
<summary align='left'>Hexagonix H2-dev.beta6</summary>

<div align="justify">

A versão H2-dev.beta6 veio padronizar uma série de serviços do Hexagonix, bem como aplicar conformidade nos fontes e comentários do sistema. A maioria das alterações dessa versão não são visíveis ao usuário, mas são importantes para garantir a estabilidade do sistema. Veja as alterações mais importantes:

* Melhorias nas mensagens dos utilitários do sistema, sobretudo em mensagens de erro;
* Correções nos seguintes utilitários do sistema:
  - [x] DOSsh;
  - [x] init;
  - [x] su;
  - [x] login;
* Um erro de definição em su poderia levar ao travamento ou não funcionamento do utilitário, uma vez que tentaria carregar o shell padrão (sh) com o nome sh.app;
* Remoção total de referências ao Andromeda, uma vez que a distribuição foi fundida ao Hexagonix (ver Hexagonix H2-dev.beta1). A remoção se deu em:
  - Nome de funções;
  - Nome de variáveis e constantes;
  - Comentários;
* Melhoria nas páginas de manual de todos os utilitários;
* Melhoria na documentação online do Hexagon;
* Alteração no nome de versão de "Vita Nova" para "VitaNova", impedindo problemas ao verificar o nome de host gerado durante a construção do sistema;
* Alteração na formatação da declaração de serviços de init.

- [x] Data de lançamento: 28/11/2022

</div>

</details>

<details title="Hexagonix H2-dev.beta7" align='left'>
<br>
<summary align='left'>Hexagonix H2-dev.beta7</summary>

<div align="justify">

Tradução das mensagens dos utilitários Unix para o inglês.

- [x] Data de lançamento: 30/11/2022

</div>

</details>

<details title="Hexagonix H2-dev.beta8" align='left'>
<br>
<summary align='left'>Hexagonix H2-dev.beta8</summary>

<div align="justify">

* Mensagens dos utilitários Andromeda-Hexagonix e do HBoot traduzidas para o inglês;
* Mensagens do Hexagon traduzidas para o inglês;

> Vale ressaltar que os nomes de funções, bem como os comentários em arquivos que compõem o sistema, permanecerão em português nesse momento.

- [x] Data de lançamento: 04/12/2022

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

</details>

<details title="Hexagonix H2 Release 1" align='left'>
<br>
<summary align='left'>Hexagonix H2 Release 1</summary>

Consolidação das versões de desenvolvimento, com:

- Kernel Hexagon v1.2.5;
- HBoot v0.4 (incompatível com Hexagonix H1 a H1-R6);
- Fusão das distribuições Hexagonix e Andromeda em uma única distribuição;
- Remoção de extensão de arquivo para os binários do sistema;
- Adição de termos de licença na imagem do sistema;
- Melhorias em utilitários Unix e Hexagonix-Andromeda (antigos aplicativos Andromeda);
- Alteração profunda no utilitário Unix atop;
- atop renomeado para htop;
- Melhorias no daemon logind;
- Fonte hint renomeada para Avatar;
- Utilitário init v2.0, com suporte a execução de múltiplos serviços em lista.
- Desativação do modo de login "moderno" em logind. A interface de login padrão segue o observado em sistemas Unix-like (FreeBSD como maior inspiração);
- Melhorias gerais nos seguintes utilitários Unix:
  - [x] login;
  - [x] energia;
  - [x] htop;
  - [x] man;
  - [x] su;
  - [x] top;
  - [x] uname;
- Melhorias nas mensagens dos utilitários do sistema, sobretudo em mensagens de erro;
- Correções nos seguintes utilitários do sistema:
  - [x] DOSsh;
  - [x] init;
  - [x] su;
  - [x] login;
- Um erro de definição em su poderia levar ao travamento ou não funcionamento do utilitário, uma vez que tentaria carregar o shell padrão (sh) com o nome sh.app;
- Remoção total de referências ao Andromeda, uma vez que a distribuição foi fundida ao Hexagonix (ver Hexagonix H2-dev.beta1). A remoção se deu em:
  - Nome de funções;
  - Nome de variáveis e constantes;
  - Comentários;
- Melhoria nas páginas de manual de todos os utilitários;
- Melhoria na documentação online do Hexagon;
- Alteração no nome de versão de "Vita Nova" para "VitaNova", impedindo problemas ao verificar o nome de host gerado durante a construção do sistema;
- Alteração na formatação da declaração de serviços de init;
- Tradução das mensagens dos utilitários Unix para o inglês;
- Mensagens dos utilitários Andromeda-Hexagonix e do HBoot traduzidas para o inglês;
- Mensagens do Hexagon traduzidas para o inglês.

- [x] Data de lançamento: 12/12/2022

</details>

<details title="Hexagonix H2 Release 2" align='left'>
<br>
<summary align='left'>Hexagonix H2 Release 2</summary>

- Kernel Hexagon v1.3.2;
- HBoot v0.7.1;
- Melhorias em utilitários Unix e Hexagonix-Andromeda;
- Melhorias no daemon logind;
- Nova experiência de primeiro uso (OOBE - Out of Box Experience);
- Fonte Avatar renomeada para Aurora;
- Melhorias nas mensagens dos utilitários do sistema, sobretudo em mensagens de erro;
- Melhoria nas páginas de manual de todos os utilitários;
- Melhoria na documentação online do Hexagon, incluindo chamadas de sistema;
- Alteração no nome de versão de "VitaNova" para "Darwin";
- Alteração na formatação da declaração de serviços de init;
- Tradução das mensagens dos utilitários Unix para o inglês concluídas;
- Melhorias no utilitário de configurações (Config);
- Bibliotecas de desenvolvimento Assembly versão 0.10.1;

- [x] Data de lançamento: 28/02/2023

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

</details>

</details>
