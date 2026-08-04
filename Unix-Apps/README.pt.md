<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/stars/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/downloads/hexagonix/Unix-Apps/total.svg)
![](https://img.shields.io/github/release/hexagonix/Unix-Apps.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

# Utilitários Unix-like do Hexagonix

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

Um dos componentes mais importantes do Hexagonix é o Unix-Apps. Esse pacote inclui diversos utilitários comumente encontrados em sistemas Unix-like, como Linux, FreeBSD e macOS. 

A sintaxe dos utilitários do Hexagonix segue a encontrada nas versões do `Version 7 UNIX` e FreeBSD, embora não busque precisão na implementação. Sempre que você tiver dúvidas ao utilizar um dos utilitários Unix, insira `utilitário --help`, `utilitário ?` ou `man utilitário`.

Abaixo, você encontra a licença de uso e distribuição dos utilitários Unix do Hexagonix.

</div>

<details title="Licença dos utilitários Unix-like do Hexagonix" align='left'>
<summary align='left'>Licença dos utilitários Unix-like do Hexagonix</summary>
<br>

<div align="justify">

Leia a [licença](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) para mais informações sobre direitos autorais, propriedade de código e redistribuição que se aplicam aos arquivos disponíveis neste repositório. O Hexagonix é totalmente licenciado sob [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Sempre fique atento ao arquivo `LICENSE` disponível em cada repositório para estar ciente dos direitos e obrigações legais, bem como à lista de contribuidores do projeto.
</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Utilitários incluídos

<div align="justify">

Diversos utilitários no padrão Unix estão incluidos até o momento. São eles:

* adduser
* arch
* cat
* clear
* cowsay
* cp
* date
* deluser
* echo
* file
* free
* hostname
* init
* kill
* login
* ls
* man
* mount
* mv
* passwd
* ps
* rm
* sh
* shutdown
* su
* syslogd
* top
* uname
* whoami

Outros utilitários são exclusivos do Hexagonix. São eles:

* clock (exibe o horário atual no canto superior direito do console, atualizado a cada segundo. Pensado para ser executado em segundo plano, com `clock &`);
* fnt (altera a fonte gráfica utilizada pelo console);
* hash (shell alternativo);
* logind (daemon que administra o ciclo de login em cada terminal virtual;
* lshapp (lê e exibe informações de imagens HAPP);
* lshmod (lê e exibe informações de imagens HBoot).

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Infraestrutura de login e gerenciamento de usuários

<div align="justify">

O Hexagonix mantém sua base de usuários em um único arquivo, `/shadow`, com um registro por linha no formato `usuario:hashDaSenha:codigo:shell:tema`. O campo `codigo` reflete o mesmo esquema de IDs usado pelo Hexagon (555 para usuários comuns, 777 para root), e o campo `tema` ("dark" ou "light") é aplicado pelo `login` logo após uma autenticação bem-sucedida, permitindo que cada usuário tenha sua própria preferência de cores, em vez de um tema único para todo o sistema.

* `adduser`, restrito ao usuário root, solicita interativamente o nome do novo usuário, a senha (duas vezes, sem eco no console), o shell e o tema, e acrescenta o registro correspondente ao `/shadow`, com um shell e tema padrão caso o usuário deixe os campos em branco;
* `deluser` remove o registro de um usuário do `/shadow`, reescrevendo o arquivo sem a linha correspondente;
* `passwd` permite que o próprio usuário logado troque sua senha, atualizando apenas o campo do hash em sua linha;
* `login` e `su` consultam o `/shadow` para validar as credenciais fornecidas, e é o `login` quem aplica o tema do usuário autenticado;
* `logind` é o daemon responsável configurar o terminal virtual e gerenciar algumas características da sessão.

As senhas nunca são gravadas em texto puro: o campo do hash em `/shadow` é calculado a partir de um hash DJB2 (função implementada na `libasm`, e compartilhada por todos esses utilitários). Vale destacar que esse não é um hash criptográfico, não usa salt e é reversível por força bruta, mas já evita que a senha fique visível a olho nu para quem ler o arquivo diretamente.

</div>
