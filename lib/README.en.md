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
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.en.md#system-components">Components</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.en.md#system-development-libraries">Libraries</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.en.md#-screenshots">Screenshots</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.en.md#contribute-and-report-bugs">Contribute</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.en.md#other-information">More information</a></td>
<td><a href="https://github.com/hexagonix/src">Source code</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.pt.md">Download</a></td>
</tr>
</table>

# The libasm

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

`libasm` (libraries for developing utilities in x86 Assembly) is a package of libraries that help in the process of creating utilities for Hexagonix, providing definitions, interfaces, constants and useful macros in the process of developing and testing an application.

Below is a list of each library within the libasm package:

| Library | Functions |
|:-------:|:---------:|
|[`console.s`](https://github.com/hexagonix/lib/blob/main/fasm/console.s)| Functions for manipulating the console (physical and virtual)|
|[`devices.s`](https://github.com/hexagonix/lib/blob/main/fasm/devices.s)| Constants to access devices recognized by Hexagon|
|[`erros.s`](https://github.com/hexagonix/lib/blob/main/fasm/erros.s)| Errors in response to Hexagon System Calls|
|[`HAPP.s`](https://github.com/hexagonix/lib/blob/main/fasm/HAPP.s)| Macro for creating a HAPP header for executable images|
|[`hexagon.s`](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s)| Definitions for making system calls to Hexagon|
|[`log.s`](https://github.com/hexagonix/lib/blob/main/fasm/log.s)| Macro for sending messages through Hexagon|
|[`macros.s`](https://github.com/hexagonix/lib/blob/main/fasm/macros.s)| Advanced macros to ease utility development|
|[`verUtils.s`](https://github.com/hexagonix/lib/blob/main/fasm/verUtils.s)| Functions to get and process Hexagonix version information |

In addition, there is a specific library called `Estelar`. This library is divided into two components, [`Estelar`](https://github.com/hexagonix/lib/blob/main/fasm/Estelar/estelar.s) and [`Bigbang`](https://github .com/hexagonix/lib/blob/main/fasm/Starfire/bigbang.s). This library aims to facilitate the development of graphical utilities that use sound output.

> The libraries are developed with a focus on flat assembler (fasm), and have been ported to NASM. Check the `fasm` and `nasm` directories inside the package to find the libraries available for each assembler.

</div>
