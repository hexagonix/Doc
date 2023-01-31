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

# The Hexagonix Operating System

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix1.png" width="500" height="400">

</div>

<div align="justify">

`Hexagonix` is a new Unix-like operating system developed from scratch, written in Assembly language for the PC architecture (x86). It is built on `Hexagon`, a simple and lightweight monolithic kernel designed to be fast and compatible with newer and older hardware (Pentium III and newer with 32 MB of RAM or more).

Hexagonix consists of the [Hexagon boot loader](https:\\github.com/hexagonix/HBoot) (`Hexagon Boot` or `HBoot`), the [Hexagon](https://github.com/hexagonix/Hexagon) (kernel), compatible shells, utilities and libraries. All these components were released under the BSD-3-Clause license.

Some features of Hexagonix:

- [x] 32-bit operating system;
- [x] Completely written in x86 Assembly, being fast and light;
- [x] Inspired by the design of Version 7 UNIX[^1], but completely written from scratch;
- [x] Compatible with Intel Pentium III (1999) or newer processors;
- [x] Compatible with devices with 32 MB of RAM or more;
- [x] Full kernel with less than 30 kbytes;
- [x] User environment support;
- [x] System call with 68 sophisticated functions accessed by the user environment;
- [x] Low minimum requirements, compatible with a wide range of devices;
- [x] Own executable binary format (HAPP);
- [x] Self-hosting ([the assembler used to build Hexagonix](https://github.com/hexagonix/fasmX) can run on top of it);
- [x] Device abstraction;
- [x] VESA VBE graphics support in various resolutions;
- [x] Text mode support;
- [x] Support for serial and parallel ports (serial communication, debug and printing);
- [x] Virtual File System (VFS);
- [x] Support for reading and writing on FAT16 (FAT16B) file systems;
- [x] Graphic font rendering engine, which can be changed by the user;
- [x] Real-time clock support;
- [x] Supports own boot loader (Hexagon Boot - HBoot);
- [x] Support for users and permissions.
- [x] Easily extensible;
- [x] Fully licensed under `BSD-3-Clause`[^2].

[^1]: The architecture of Hexagonix (structure and utilities) was inspired by the elegance and simplicity of [Version 7 UNIX](https://github.com/dspinellis/unix-history-repo/tree/Research-V7-Snapshot-Development), although it does not aim for any compatibility or share any code with it. Hexagonix does not use any code derived from Version 7 UNIX or other projects. Hexagonix utilities were written to resemble the function and appearance of their counterparts in Version 7 UNIX.
[^2]: You can get more information about the license [in this article](https://docs.freebsd.org/en/articles/bsdl-gpl/) of the FreeBSD project, [in the article](https://opensource.org/licenses/BSD-3-Clause) from the Open Source Initiative or the license page on [Wikipedia](https://en.wikipedia.org/wiki/BSD_licenses).

Hexagonix is ​​not intended to build a production system, but rather a simple, well-documented system with easy-to-understand interfaces that can be used for educational purposes. It also aims at researching and implementing a modern operating system purely in x86 Assembly. For more complex, complete and professional systems, visit projects like [MenuetOS](https://www.menuetos.net/) or [KolibriOS](https://kolibrios.org/en/), also developed in x86 Assembly.

</div>

<details title="License" align='left'>
<br>
<summary align='left'>Hexagonix License</summary>

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

## System components

Hexagonix is divided into a series of components, which work together as soon as the device is turned on. Below you will be able to know briefly each one of them.

### Saturno

<div align="justify">

The first component of Hexagonix is the Saturno. It is responsible for taking control of the boot process performed by the BIOS/UEFI and looking in the volume for the second boot stage. For that, it implements a driver for reading a FAT16 file system. The second boot stage (see below) can implement drivers for other filesystems and is responsible for finding Hexagon, loading HBoot modules or loading a compatible DOS-like system (BETA version).

</div>

<div align="center">

[![Saturno](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Saturno&theme=dark)](https://github.com/hexagonix/Saturno)

</div>

<hr>

### Hexagon Boot (HBoot)

<div align="justify">

Hexagon Boot (HBoot) is a component designed to allow the Hexagon kernel to boot. Until then, initialization was performed by just one stage, which defined a very basic environment, loaded Hexagon into memory and immediately executed it, providing a very limited set of parameters. This is due to the fact that the code of this stage is restricted to 512 bytes, which limits the performance of several tests and data processing. With HBoot, it was possible to expand the number of tasks performed before running Hexagon, as well as the possibility to provide more information about the device and boot environment. This is particularly important to allow the creation of a device tree that Hexagon can use to decide how to handle each identified device. HBoot is able to check which disk drives are available on the machine, emit a boot tone, obtain the amount of available RAM memory installed and allow or not to proceed with the boot process according to this information. If no user interaction is detected (within 3 seconds after booting HBoot and displaying messages to the user), HBoot will perform additional tests to verify the device's ability to run the system and will load and run Hexagon (present in a file on the volume named `HEXAGON.SIS` on Hexagonix H1 and `HEXAGON` on Hexagonix H2). After loading, HBoot transfers control to Hexagon, which is initialized and stores the data provided by HBoot in the kernel environment.

</div>

<div align="center">
   
[![HBoot](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=HBoot&theme=dark)](https://github.com/hexagonix/Hboot)

</div>

#### How to interact with HBoot

<div align="justify">

The interaction with HBoot is done by pressing the `F8` key after booting and displaying messages on the screen. HBoot waits for 3 seconds for any interaction and, if none has occurred, continues executing the boot protocol. The interaction with HBoot can be interesting to load modules in the HBoot format, provide boot parameters to Hexagon, load a DOS-type system whose files are present on the same volume or even load HAPP images from other cores (if the developer wants to use the HBoot implementation in your project). Below, see some more details of additional and diagnostic functions that can be performed via interaction with HBoot before loading Hexagonix.

</div>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/HBoot.png" width="600" height="500">
</p>

##### Report bugs

<div align="justify">

HBoot has gained a lot of complexity since the beginning of its development in 2020. Due to this increase in code and the nature of its operation (16-bit), bugs can be found. They can be reported in the repository or by email, available at the end of this file.

</div>

<hr>

### Hexagon kernel

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="180" height="180">
</p>

<div align="justify">

Hexagon is a monolithic kernel running in 32-bit protected mode, designed with the PC architecture in mind (x86). It's a kernel written from scratch, aiming for the speed and compatibility of modern hardware but also being able to run on older hardware (like a Pentium III). At the moment, it guarantees a single-user environment, despite the use of virtual terminals, and single-tasking, despite the ability to load, keep in memory and control more than one process, in an execution stack. In the future, the kernel may support running multiple processes in preemptive multitasking. Hexagon is a Unix-like kernel and tries to implement POSIX compatibility, although far from it, and forms the basis of the Hexagonix Operating System, although independent of it. It runs executable images in the HAPP format, developed exclusively for Hexagon. Hexagon also implements a very sophisticated API accessible through a system call.

</div>

<div align="center">

[![Hexagon Kernel](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Hexagon&theme=dark)](https://github.com/hexagonix/Hexagon)

</div>

<hr>

### Hexagonix Unix Environment Utilities

<div align="justify">

Hexagonix implements, together with Hexagon, a series of Unix-like utilities, with functionality and syntax similar to UNIX and Unix-like systems. **Utilities such as init, login, sh, top, ps, cp, rm, cat, clear, man, among others, are included in the standard distribution of Hexagonix**. These utilities make up the Hexagonix base utilities package. The login and user-mode environment startup tools are in this package, as well as several configuration files for this environment. These utilities generally do not have a graphical interface, only a command-line interface (CLI). However, they can be requested by applications that have a graphical interface. This environment is available from the [`Hexagonix`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img) distribution.

</div>

<div align="center">
   
[![Unix-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=unix-apps&theme=dark)](https://github.com/hexagonix/unix-apps)

</div>

#### Some applications and utilities in the Hexagonix environment

<div align="justify">

Hexagonix includes many of the Unix utilities you may already be familiar with, such as:

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
- [x] sh (default shell)
- [x] shutdown
- [x] su
- [x] uname
- [x] whoami, among others.

Some applications and utilities were developed exclusively for Hexagonix, such as:
   
* htop (alternate version of top)
* fnt (Hexagonix graphic font change utility)
* hash (alternate shell)
* lshapp (gets and displays information from HAPP images)
* lshmod (gets and displays information from HBoot modules and HBoot itself)

It is worth remembering that Hexagonix utilities try to implement a POSIX interface as far as possible, similar in syntax to Unix utilities (FreeBSD and Linux used as a model).

### Third party applications available for Hexagonix

* [flat assembler (fasm)](https://flatassembler.net/index.php)

Hexagonix received a port of the [`fasm`](https://flatassembler.net/index.php) assembler, which was adapted for Hexagonix, allowing the user to develop applications directly on the system. This port is called `fasmX`. Changes added to the code, as well as the software license, can be found in the [fasm repository for Hexagonix](https://github.com/hexagonix/fasm). This repository is a fork of [original repository](https://github.com/tgrysztar/fasm). Added code is based on modifications made to the original code and authorial additions. This modified/authored code can be found in the repository, [clicking here](https://github.com/hexagonix/fasm/tree/master/SOURCE/HEXAGONIX). fasmX, the fasm port for Hexagonix, is always updated when new features are added to the fasm repository. To indicate that it is a stable and tested version, the fasmX version number always inherits the fasm numbering, followed by a character x (for example, the version based on fasm 1.73.30, after testing, receives the numbering 1.73.30x). You can report bugs or code generation or optimization issues in the Hexagonix version [here](https://github.com/hexagonix/fasm/issues). To report general fasm bugs, use the [official](https://github.com/tgrysztar/fasm) repository.

</div>

<hr>

### Hexagonix GUI Utilities

<div align="justify">

The Hexagonix Andromeda environment (Hexagonix-Andromeda) is built on the solid foundation provided by Hexagonix, including applications and utilities that do not implement the Unix philosophy or have a very different syntax and usage than you would expect from a Unix environment. Here are the system settings app, calculator, font manager, text editors and the IDE developed for Hexagonix. These utilities may or may not have a graphical interface. Together with them, the Hexagonix-Andromeda environment comprises libraries developed to allow the development of applications, such as the `Estelar` library.

</div>

<div align="center">

[![Andromeda-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=andromeda-apps&theme=dark)](https://github.com/hexagonix/andromeda-apps)

</div>

#### Some Hexagonix-Andromeda applications and utilities

<div align="justify">

- [x] System Settings (config)
- [x] Quartzo text editor
- [x] Lyoko IDE for application development
- [x] Electronic piano return Piano;
- [x] Serial communication utility
- [x] Andromeda Shell (ASH) - A new shell for Hexagonix
- [x] Hexagonix calculator
- [x] Font change utility
- [x] Hexagonix shutdown utility

### Third party applications available for Hexagonix-Andromeda

There are no third-party applications available for the Hexagonix-Andromeda environment yet. If interested, you can build the first one!

</div>

<hr>

### Hexagonix graphic fonts

<div align="justify">

The default installation of Hexagonix also provides a number of fonts that can be loaded by the settings application or the font utility (font manager). These files are used to change the Hexagonix graphical display font.

Graphics mode fonts for Hexagon are developed as a bitmap in Assembly which, when compiled, generate a binary image of the font with representations of each character. The standard Hexagonix fonts have now been released as open source and are available in the [Hexagonix font repository](https://github.com/hexagonix/xfnt).

</div>

<div align="center">
   
[![Hexagonix-xfnt](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=xfnt&theme=dark)](https://github.com/hexagonix/xfnt)

</div>

<hr>

### System development libraries

<div align="justify">

Hexagonix also provides functions that must be used to interact with the system environment itself. Libraries are used to access functions implemented by Hexagon or by the libraries themselves, allowing easy development of applications and utilities for both the Hexagonix and Hexagonix-Andromeda environments. The libraries implement functions for displaying text, mathematical calculations, sending messages, opening files and devices, and much more. The core library (hexagon.s) provides functions accessible to both possible distribution environments, while other libraries may be unique to the Hexagonix-Andromeda environment. These libraries include graphical functions to build interfaces in graphical mode (Hexagonix-Andromeda), as well as functions to check the currently running system version. The base Hexagonix utilities perform the Hexagon version check to see if they can be run, using the Unix utility uname or directly via a Hexagon system call.

To learn more and check each function available in the system development libraries, see the [libasm] repository (https://github.com/hexagonix/lib).

</div>

<div align="center">

[![lib](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=lib&theme=dark)](https://github.com/hexagonix/lib)

</div>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## 🌙 Screenshots

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

## Contribute and report bugs

<div align="justify">

Below you can learn more about how to contribute and report bugs found in Hexagonix.

</div>

<details title="Contribute to the development of Hexagonix" align='left'>
<br>
<summary align='left'>Contribute to the development of Hexagonix</summary>

<div align="justify">

If you have knowledge of creating x86 assembly code and would like to help with system development, feel free to send me an email! See [here](https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.en.md#author) how to contact me!

</div>

</details>

<details title="Report Errors" align='left'>
<br>
<summary align='left'>Report Errors</summary>

<div align="justify">

You can report bugs and help develop the system. To do so, open an error notification [here](https://github.com/hexagonix/Distro/issues), informing the error as detailed as possible (such as device brand, processor, amount of RAM, video and connected peripherals, as well as the device used to install the system, such as an internal disk drive or USB removable media). Remember to inform which application the error occurred in, if the error occurs while the system is already in operation. If the problem occurs in the boot process, please report what was displayed/observed behavior of the machine.

</div>

</details>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## Other information

<details title="Languages" align='left'>
<br>
<summary align='left'>🎲 Languages</summary>

<div align="justify">

At the moment, the system is available in English (with some components still in Portuguese) and the documentation is available in both Portuguese and English. The comments and function names in the files that make up the source code of the system are in Portuguese.

</div>

</details>

<details title="Inspirations" align='left'>
<br>
<summary align='left'>🖼 Inspirations</summary>

<div align="justify">

Several projects helped in the development of Hexagonix, either providing new design ideas that deviate from the traditional organization of an operating system, or providing well-documented code that allowed understanding the functioning of certain parts of an operating system, or contributing with examples of code that more later came to inspire functions that were written exclusively for Hexagonix (mainly the kernel code). The code of these public domain projects, written in Assembly x86, made it possible for the operation of certain computer components to be better understood, opening doors for copyright codes to be written based on the study and reimplementation of the code of these projects. It is worth noting that the projects below were released by their original developers into the public domain. Are they:

* [Snowdrop OS](http://www.sebastianmihai.com/snowdrop/), which inspired me to write various hardware control routines and other 16-bit functions available in HBoot. The code for this system has been released into the public domain. This system remains under development. Access the [project page](http://www.sebastianmihai.com/snowdrop/) to get disk images and system source files.
* [Alotware](https://github.com/0x5CE/alotware), which helped create process management functions in early versions of Hexagon (now rewritten numerous times to expand the kernel). The code for this system was released into the public domain and has not been updated since then.

In addition to the projects mentioned above, Hexagonix is ​​only possible thanks to a series of projects, developers and people that came before. Several projects contributed with consolidated documentation, solid design ideas and demonstrations of how to develop an operating system, even a simple one. Are they:

* [Version 7 UNIX](https://github.com/dspinellis/unix-history-repo/tree/Research-V7-Snapshot-Development)
* [FreeBSD](https://www.freebsd.org/)
* [MenuetOS](http://www.menuetos.net/)
* [KolibriOS](https://kolibrios.org/en/)
* [Linux 0.1.1](https://kernel.org)
* [XNU/Darwin](https://github.com/apple/darwin-xnu)
* MS-DOS, with code available [here](https://github.com/microsoft/MS-DOS)
* [MikeOS](http://mikeos.sourceforge.net/)
* [LittleKernel](https://github.com/littlekernel/lk)
* [Google Fuchsia](https://fuchsia.googlesource.com/fuchsia/)

To all these projects and developers, a huge thank you :heart:.

</div>

</details>

<details title="Author, contributors and contacts" align='left'>
<summary align='left'>👥️️ Author, contributors and contacts</summary>

## Author

* [Felipe Miguel Nery Lunkes](https://github.com/felipenlunkes)
## Contributors

* [Felipe Miguel Nery Lunkes](https://github.com/felipenlunkes)

## Email

* hexagonixdev@gmail.com (PT/EN)

## Social networks

* [Felipe Miguel Nery Lunkes](https://twitter.com/felipeldev) (Twitter)

</details>

<details title="Copyright Notes" align='left'>
<br>
<summary align='left'>📝 Copyright notice</summary>

<div align="justify">

Read the license for more information about copyright, code ownership, and redistribution that apply only to files available in this repository (does not apply to the set of data and source code files that make up Hexagonix). Always keep an eye on the `LICENSE` file available in each repository to be aware of legal rights and obligations.

</div>

</details>