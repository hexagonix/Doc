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
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md#componentes-do-sistema">Componentes</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md#bibliotecas-de-desenvolvimento-do-sistema">Bibliotecas</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md#-capturas-de-tela">Screenshots</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md#contribuir-e-reportar-erros">Contribuir</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.pt.md#outras-informa%C3%A7%C3%B5es">Outras informações</a></td>
<td><a href="https://github.com/hexagonix/src">Código-fonte</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.pt.md">Download</a></td>
</tr>
</table>

# libasm - Biblioteca para desenvolvimento de utilitários em Assembly x86

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

A `libasm` é um pacote de bibliotecas que auxiliam no processo de criação de utilitários para o Hexagonix, fornecendo definições, interfaces, constantes e macros úteis no processo de desenvolvimento e teste de um aplicativo.

Abaixo, uma lista com cada biblioteca dentro do pacote da libasm:

| Biblioteca | Funções |
|:----------:|:-------:|
|[`console.s`]()https://github.com/hexagonix/lib/blob/main/fasm/console.s| Funções para manipulação do console (físico e virtual)|
|[`dispositivos.s`](https://github.com/hexagonix/lib/blob/main/fasm/dispositivos.s)| Constantes para acessar os dispositivos reconhecidos pelo Hexagon|
|[`erros.s`](https://github.com/hexagonix/lib/blob/main/fasm/erros.s)| Definição de erros em resposta a chamadas de sistema do Hexagon|
|[`HAPP.s`](https://github.com/hexagonix/lib/blob/main/fasm/HAPP.s)| Macro para a criação de um cabaçalho HAPP para imagens executáveis|
|[`hexagon.s`](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s)| Definições para realizar chamadas de sistema ao Hexagon|
|[`log.s`](https://github.com/hexagonix/lib/blob/main/fasm/log.s)| Macro para envio de mensagens pelo Hexagon|
|[`macros.s`](https://github.com/hexagonix/lib/blob/main/fasm/macros.s)| Macros avançados para facilitar o desenvolvimento de utilitários|
|[`verUtils.s`](https://github.com/hexagonix/lib/blob/main/fasm/verUtils.s)| Funções para obter e processar informações de versão do Hexagonix|

Além disso, existe uma biblioteca específica, chamada Estelar. Essa biblioteca é dividida em dois componentes, [`Estelar`](https://github.com/hexagonix/lib/blob/main/fasm/Estelar/estelar.s) e [`Bigbang`](https://github.com/hexagonix/lib/blob/main/fasm/Estelar/bigbang.s). Essa biblioteca tem como objetivo facilitar o desenvolvimento de utilitários gráficos e que utilizem saída de som.

> As bibliotecas são desenvolvidas com foco no flat assembler (fasm), e vêm sendo portadas para o NASM. Verifique dentro do pacote os diretórios `fasm` e `nasm` para encontrar as bibliotecas disponíveis para cada montador.

</div>