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

# Ponto de partida

<div align="center">

<img height="200" src="https://github.com/hexagonix/Doc/blob/main/Img/HX.png">

</div>

</div align="justify">

Neste documento você poderá obter mais informações sobre o utilitário `hx` e sua participação na construção do Hexagonix.

## HX

O utilitário `hx` tem a função de unificar o processo de construção do Hexagonix em sistemas Linux (o WSL2 também é suportado no Windows 10 e Windows 11). A ferramenta é responsável por construir cada componente individualmente e, em caso de sucesso para todos os componentes críticos, como `HBoot`, `Hexagon` e utilitários Unix-like, criar a imagem inicializável do sistema (utilizada para execução do sistema no `qemu` e para instalação em um disco rígido físico), bem como um disco rígido virtual `.vdi` para utilização fácil em virtualizadores como o VirtualBox. Ele foi escrito para ser facilmente adaptável e extensível. O hx também utiliza módulos, escritos como scripts de shell, para construir os componentes do sistema, utilizando um protocolo de passagem de parâmetros bem estabelecido.

> Embora se trate de um sistema Linux, a execução do hx `não é compatível` com o Chrome OS no ambiente de desenvolvimento Linux. Isso se deve à indisponibilidade de montagem de dispositivos loop (/dev/loop). A mesma limitação impede a execução do hx em sistemas BSD (como FreeBSD, NetBSD e OpenBSD), além do macOS. Para o último caso, uma nova implementação do hx está sendo desenvolvida, para tornar o processo de construção de imagem de disco compatível com sistemas BSD e derivados, como o macOS.

### Funções e parâmetros

O hx aceita uma série de parâmetros para determinar quais ações devem ser executadas. Abaixo, uma lista das ações disponíveis e seus parâmetros.

| Parâmetro | Ação executada | Parâmetros secundários necessários |
|:---------:|:--------------:|:----------------------------------:|
| `-h`| Exibe a ajuda com os principais parâmetros normalmente utilizados| `-v`: informações sobre virtualização; `-i`: informações sobre a construção de imagens|
| `-v`| Inicia uma instância do `qemu` com a última imagem de disco gerada| Utilize `hx -h -v` para obter todos os parâmetros possíveis.|
| `-i`| Constrói os componentes do sistema e cria uma imagem de disco (raw) e .vdi | Utilize `hx -h -v` para obter todos os parâmetros possíveis.|
| `-b`| Constrói componentes individuais do Hexagonix| `hexagon`: constrói o Hexagon; `HBoot`: constrói o HBoot; `saturno`: constrói o Saturno; `unixland`: constrói os utilitários Unix-like; `andromedaland`: constrói os utilitários Hexagonix-Andromeda; `hx`: constrói todos os componentes, mas não gera uma imagem de disco| 
| `-u`| Atualiza todos os repositórios locais com o servidor, usando o ramo já definido | Sem parâmetros secundários|
| `-ui`| Atualiza apenas as imagens de disco com o servidor. O restante dos repositórios não são afetados| Sem parâmetros secundários|
| `-br`| Exibe o ramo em uso para todos os repositórios| Sem parâmetros secundários|
| `-un`| Altera o ramo e sincroniza todos os repositórios com o ramo informado| Ramo desejado|
| `-m`| Clona todos os repositórios localmente e prepara as ferramentas de construção. Útil para gerar um novo diretório de construção| Sem parâmetros secundários| 
| `-c`| Limpa todos os arquivos temporários criados durante uma construção do sistema| Sem parâmetros secundários|
| `--version`| Exibe informações de versão e copyright| Sem parâmetros secundários|
| `--depend`| Instala as dependências de construção (sistemas Debian, Ubuntu e derivados, apenas)| Sem parâmetros secundários|
| `--info`| Exibe informações do Hexagonix, como versão, revisão, ramo de desenvolvimento, etc| Sem parâmetros secundários|
| `--indent`| Inicializa um utilitário hx que formata e otimiza os códigos-fonte, manuais e arquivos de definição do Hexagonix | Sem parâmetros necessários|
| `--configure`| Executa o módulo `configure.sh` para gerar os arquivos estáticos necessários para a construção| Sem parâmetros secundários|
| `--stat`| Exibe informações estatísticas sobre o Hexagonix (necessário cloc instalado)| Sem parâmetros secundários|

### Módulos do hx

Para a construção do Hexagonix, o hx procura e executa uma série de módulos espalhados por toda a árvore de código. São eles:

| Módulo | Função | Localização |
|:------:|:------:|:-----------:|
|`configure.sh`| Gerar todos os arquivos estáticos e verificar dependências para a construção do Hexagonix| Raiz do projeto|
|`andromeda.hx`| Responsável por construir todos os utilitários Hexagonix-Andromeda (utilitários gráficos)| Scripts/modules|
|`buildInfo.hx`| Obtêm e exibe informações sobre versão do Hexagonix, canal de desenvolvimento e build| Scripts/modules|
|`buildOnBSD.hx`| Responsável por constuir uma imagem de disco para a instalação do Hexagonix a partir de um sistema BSD| Scripts/modules|
|`buildOnLinux.hx`| Responsável por constuir uma imagem de disco para a instalação do Hexagonix a partir de uma distribuição Linux| Scripts/modules|
|`buildOnUNIX.hx`| Responsável por constuir uma imagem de disco para a instalação do Hexagonix a partir de um sistema UNIX (OpenIndiana)| Scripts/modules|
|`common.hx`| Funções comuns utilizadas por todos os módulos| Scripts/modules|
|`contribBuilder.hx`| Responsável por executar o script de construção dos pacotes externos (como fasm)| Scripts/modules|
|`contribChecker.hx`| Responsável por verificar se existem pacotes externos construídos para instalação na imagem do sistema| Scripts/modules|
|`diskBuilder.hx`| Responsável por instalar os componentes do Hexagonix na imagem de disco do sistema gerada anteriormente| Scripts/modules|
|`fonts.hx`| Responsável por identificar e construir fontes gráficas compatíveis com o sistema| Scripts/modules|
|`git.hx`| Responsável por atualizar os repositórios do sistema com o servidor remoto| Scripts/modules|
|`hboot.hx`| Responsável por executar a construção do componente `HBoot`| Scripts/modules|
|`hexagon.hx`| Responsável por executar a construção do componente `Hexagon` (kernel)| Scripts/modules|
|`logUtils.hx`| Funções úteis para a criação de logs de construção do sistema| Scripts/modules|
|`macros.hx`| Funções úteis para executar módulos a partir do utilitário `hx` ou de outros módulos| Scripts/modules|
|`saturno.hx`| Responsável por executar a construção do componente `Saturno`| Scripts/modules|
|`systemBuilder.hx`| Executa todos os módulos de construção dos componentes do sistema e os instala em um diretório temporário[^1]| Scripts/modules|
|`unix.hx`| Responsável por construir todos os utilitários Unix| Scripts/modules|
|`vm.hx`| Permite a configuração e execução de máquinas virtuais utilizando uma imagem de disco gerada| Scripts/modules|

[^1]: Para saber quais módulos são executados, veja o conteúdo do módulo.

> `Apenas configure.sh pode ser executado diretamente pelo usuário, além de hx`. Nenhum outro módulo deve ser executado diretamente.

### Etapas de construção

O hx, juntamente aos módulos já citados, constrói os componentes na seguinte ordem:

* MBR e carregador de inicialização HBoot;
* Hexagon (kernel);
* Utilitários Unix-like;
* Utilitários Andromeda;
* Pacotes externos, de outra autoria (contrib);
* Fontes gráficas.

Caso os componentes críticos (excetuando-se os pacotes externos) tenham sido construídos com sucesso, o hx inicia a construção das imagens de disco, sendo uma imagem de disco (raw), utilizada para execução do sistema no qemu e instalação em dispositivos físicos, e outra VDI, utilizada para o VirtualBox. Ambas as imagens são movidas para `hexagonix/` caso o processo termine com sucesso. Além disso, o hx gera um arquivo de log, `log.log`, com informações de versão do sistema, revisão, ramo, versão do hx, do sistema operacional utilizado para a construção, data e hora, entre outras, que podem ser utilizadas para identificar erros no processo, bem como servem para identificar a build exata do sistema. O arquivo de log também é movido para `hexagonix/`.

</div>
