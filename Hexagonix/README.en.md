<!--

Versão deste arquivo: 5.0

Copyright © 2021-2023 Felipe Miguel Nery Lunkes
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
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.en.md">About Hexagonix</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/Hexagonix.en.md#-screenshots">Screenshots</a></td>
<td><a href="https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.en.md#contribute-and-report-bugs">Contribute</a></td>
<td><a href="https://github.com/hexagonix/src">Source code</a></td>
</tr>
</table>

# Starting point

<div align="center">

<img src="https://github.com/hexagonix/Doc/blob/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">
     
First of all, read the license below regarding the use and redistribution of Hexagonix (binaries and source code).

</div>

<details title="License" align='left'>
<br>
<summary align='left'>Hexagonix License (BSD-3-Clause)</summary>

<div align="justify">

Hexagonix Operating System

BSD 3-Clause License

Copyright (c) 2015-2023, Felipe Miguel Nery Lunkes <br>
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

# Download and run the system right now

<div align="justify">

In this document, you will find a tutorial to run Hexagonix on your computer, both in a virtualized version and natively. Remember that it is necessary to have an x86 architecture computer or an emulator, if you are using a device of another architecture for testing.

You can go to the [`releases`](https://github.com/hexagonix/hexagonix/releases) session for the latest stable system releases. You can access the Hexagonix [release announcement](REL.en.md) file. Whenever possible, get the latest release and download the available .img or .vdi images or the complete package in zip format. The versions available directly in this repository are always development versions (beta and release candidate), which can also be run and are minimally functional. At the end of each development cycle, the final versions will be available in the [releases](https://github.com/hexagonix/hexagonix/releases) session.

> Note 1: The version/build of Hexagonix available in the main branch of the repository is always current. That is, the latest trial version. Always go to the [releases](https://github.com/hexagonix/hexagonix/releases) tab to get the final images of each version of the system. As an example on UNIX systems such as FreeBSD, the main branch resembles the CURRENT version of the software and is not necessarily a stable version. For stable releases, go to the releases tab and look for a release without the terms: -dev, -dev.beta* or -CURRENT. Typically these versions have simple names such as H1, H1-R6, H2, H2-R1, and so on.

> Note 2: Always check the [release announcement](REL.en.md) to obtain the complete changelog of the version (what changes in relation to the previous version of the system).

</div>

<details title="System Documentation" align='left'>
<br>
<summary align='left'>System documentation</summary>

<div align="center">

[![Doc](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Doc&theme=dark)](https://github.com/hexagonix/Doc)

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# How to run Hexagonix

## System requirements

<div align="justify">

Below is a list of minimum and recommended requirements for testing Hexagonix on a virtual or physical machine.

</div>

### Minimum requirements:

<div align="justify">
   
* Processor: Pentium III (1999) with SSE and MMX support or newer;
* RAM memory: 32 MB minimum (a minimum installation with 32 MB is usually sufficient in most cases);
* Hard disk: IDE or SATA hard disk with a minimum of 50 MB;
* Necessary peripherals:
  * Serial port (1-4);
  * Parallel port (1-4);
  * PS/2 or USB keyboard;
  * VGA video card with 2 MB of video memory (with color support).

</div>
   
#### Recommended:

<div align="justify">
   
* Processor: Pentium D or newer;
* RAM memory: 50 MB;
* Optional peripherals:
  * PS/2 or USB mouse;
  * Video card with > 2 MB of video memory.

</div>
   
## Get the disk images with the system install

<div align="justify">

To test Hexagonix, you will need one of the available disk images, as well as the [`qemu`](https://www.qemu.org) tool installed on your computer if you want to test the system in a virtualized environment. The image can also be used for writing to a physical disk on a real machine.

To test Hexagonix, get the file [`hexagonix.img`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img) (qemu) or [`hexagonix.vdi`](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.vdi) (VirtualBox and others virtualizers and virtual machine managers).

> Note: The `hexagonix.img` image is compatible with qemu only. Furthermore, it is the only image file that can be used to install the system on a hard disk or USB flash drive. To run Hexagonix using VirtualBox or other virtualizers or virtual machine managers, use the `hexagonix.vdi` image provided. The VDI image cannot be used to install Hexagonix on a physical disk (hard disk, SSD or USB drive).

</div>

## Test Hexagonix on a virtualized system

<div align="justify">

First, you must install the `qemu` tool, which will manage the virtual machine. To do so, you can install qemu using official Linux distributions repositories or by accessing [here](https://www.qemu.org) to get installation files for Windows and macOS.

> Install on Debian, Ubuntu, Pop_OS! and derivatives:

For `Ubuntu`, the following line will install qemu and all its dependencies (root privileges required):

```
sudo apt install qemu qemu-system-i386
```

> Install on Fedora, CentOS and derivatives:

For `Fedora`, the following line will install qemu and all its dependencies (root privileges required):

```
sudo dnf install qemu qemu-system-i386
```

> Install on FreeBSD and derivatives:

For `FreeBSD`, the following line will install qemu and all its dependencies (root privileges required):

```
su
pkg update
pkg install -y qemu
```
     
Now that you have qemu installed on your computer, you can proceed with running the system.

To run the system satisfactorily, you must provide at least 32 MB of RAM to the virtual machine. This is due to Hexagon's memory management architecture, which requires 16 MB of RAM dedicated to the kernel and at least 16 MB for allocating applications, utilities and open files. Hexagon doesn't allow less than that to run. If more memory is provided, the additional memory will always be reserved, with priority, to be made available to user processes. Normally, the command line below fulfills all requirements for running the system:

> Note 1: For FreeBSD users, omit the `--enable-kvm` option. Using KVM is only available on Linux systems. Note that Hexagonix may run slower when running without KVM. In this case, you can use `VirtualBox` on FreeBSD to get good performance. To do so, download VirtualBox [here](https://www.virtualbox.org/) or use, as root user, `pkg install -y virtualbox`. To use VirtualBox, do not download the `hexagonix.img` image, compatible only with `qemu`, but the [hexagonix.vdi](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.vdi), an image compatible with leading virtualizers and virtual machine managers, including qemu.

> Note 2: For systems using PulseAudio, you may encounter sound problems in the virtual machine when using the `-soundhw pcspk` option. If this occurs, omit this parameter when running the system on qemu. For Pipewire users, these artifacts do not seem to occur.

```
qemu-system-i386 -hda hexagonix.img -m 32 -soundhw pcspk --enable-kvm -serial file:"Serial.txt"
```

You can omit the -serial parameter if you want. This parameter ensures that debug output from Hexagon and applications will be directed to a file on your computer, where you can see what was sent. You can also omit the -soundhw parameter, which is responsible for directing the sound output from the virtual system to your physical computer. On some Linux system, the above configuration may conflict with PulseAudio, and may be omitted. Remember that by omitting the parameter, system sounds will not be output to you.

Remembering that you must use a version/edition of qemu that can run software written for the x86 architecture (i386 or x86_64). To download and install `qemu`, click [here](https://www.qemu.org/download/).

</div>

## Test Hexagonix on physical machine

<div align="justify">

You must use Linux/macOS or some tool available for Windows that allows you to burn this image to disk.

On `Linux/BSD/macOS/Unix`, use the line below:

```
dd if=hexagonix.img of=/dev/unit
```
where `drive` is the desired device (usually `sdb` or `sdc` for USB devices and `hda`, `hdb`, `sda` or `sdb` for hard/solid state drives). Restart your computer and test the system. Note that secure boot mode is not supported, and booting is only supported in BIOS or UEFI legacy BIOS mode.

> Note: The disk device name varies between UNIX and Unix-like systems. While you can install the system to a USB flash drive on Linux via the `/dev/sdb` path, for example, on macOS the device name might look like `/dev/rdisk1`. Properly check the device path before executing the command mentioned above.

Note that system performance may vary depending on the machine being tested. Added to this is the fact that the latest versions of the system have not been or are being tested directly on the physical machine, as the main operating system. If any problem occurs when running Hexagonix on a physical machine, please report the detailed error [here](https://github.com/hexagonix/Distro/issues), in Portuguese or English, informing data such as device brand, processor, amount of RAM memory, video card (if available) and peripherals connected, as well as the device used to install the system (internal disk drive or USB removable media).

</div>

## First use and login

<div align="justify">

When starting the system, you will be asked to enter a username and password. For the first use, use:

```
User: root
Password: root
```

or 

```
User: jack
Password: jack
```

You can add another user by changing the `USUARIO.UNX` (Hexagonix H1) or `passwd` (Hexagonix H2 and later versions) file at the root of the disk. Remember not to remove the root user (`root`). This can make the system permanently inoperable.

</div>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Contribute and report bugs

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

<details title="Report system errors" align='left'>
<br>
<summary align='left'>Report system errors</summary>

<div align="justify">
   
You can report bugs and help develop the system. To do so, open an error notification [here](https://github.com/hexagonix/Distro/issues), informing the error as detailed as possible (such as device brand, processor, amount of RAM, video and connected peripherals, as well as the device used to install the system, such as an internal disk drive or USB removable media). Remember to inform which application the error occurred in, if the error occurs while the system is already in operation. If the problem occurs in the boot process, please report what was displayed/observed behavior of the machine.

</div>
   
</details>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Other information

<details title="Languages" align='left'>
<br>
<summary align='left'>🎲 Languages</summary>

<div align="justify">

At the moment, the system is available in English (with some components still in Portuguese) and the documentation is available in both Portuguese and English. The comments and function names in the files that make up the source code of the system are in Portuguese.

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

Hexagonix was developed by [Felipe Lunkes](https://github.com/felipenlunkes).

Read the license for more information about copyright, code ownership, and redistribution that apply only to files available in this repository (does not apply to the set of data and source code files that make up Hexagonix). It is worth mentioning that the code of the system components is being released little by little and that each package can be released with a different license. Always keep an eye on the `LICENSE` file available in each repository to be aware of legal rights and obligations.

</div>

</details>