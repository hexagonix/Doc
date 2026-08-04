<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
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

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

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

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

`libasm` (libraries for developing utilities in x86 Assembly) is a package of libraries that help in the process of creating utilities for Hexagonix, providing definitions, interfaces, constants and useful macros in the process of developing and testing an application.

Below is a list of each library within the libasm package:

| Library | Functions |
|:-------:|:---------:|
|[`console.s`](https://github.com/hexagonix/lib/blob/main/fasm/console.s)| Functions for manipulating the console (physical and virtual)|
|[`dev.s`](https://github.com/hexagonix/lib/blob/main/fasm/dev.s)| Constants to access devices recognized by Hexagon|
|[`errors.s`](https://github.com/hexagonix/lib/blob/main/fasm/errors.s)| Errors in response to Hexagon System Calls|
|[`HAPP.s`](https://github.com/hexagonix/lib/blob/main/fasm/HAPP.s)| Macro for creating a HAPP header for executable images|
|[`hexagon.s`](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s)| Definitions for making system calls to Hexagon|
|[`hexagonix.s`](https://github.com/hexagonix/lib/blob/main/fasm/hexagonix.s)| Macros and data shared by Hexagonix components|
|[`log.s`](https://github.com/hexagonix/lib/blob/main/fasm/log.s)| Macro for sending messages through Hexagon|
|[`macros.s`](https://github.com/hexagonix/lib/blob/main/fasm/macros.s)| Advanced macros to ease utility development|
|[`memory.s`](https://github.com/hexagonix/lib/blob/main/fasm/memory.s)| Macros for manipulating memory values|
|[`passwdHash.s`](https://github.com/hexagonix/lib/blob/main/fasm/passwdHash.s)| Password hashing and `/shadow` lookup, shared by login, su, adduser, passwd and deluser|
|[`verUtils.s`](https://github.com/hexagonix/lib/blob/main/fasm/verUtils.s)| Functions to get and process Hexagonix version information |

In addition, there is a specific library called `Estelar`. This library is divided into two components, [`Estelar`](https://github.com/hexagonix/lib/blob/main/fasm/Estelar/estelar.s) and [`Bigbang`](https://github.com/hexagonix/lib/blob/main/fasm/Estelar/bigbang.s). This library aims to facilitate the development of graphical utilities that use sound output.

> The libraries are developed with a focus on flat assembler (fasm). Check the `fasm` directory to find the libraries, and `samples` for example applications that use them.

</div>
