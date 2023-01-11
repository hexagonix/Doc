<!--

Copyright © 2021-2023 Felipe Miguel Nery Lunkes
Todos os direitos reservados.

-->

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/HBoot.svg)
![](https://img.shields.io/github/stars/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/HBoot.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/HBoot.svg)
![](https://img.shields.io/github/downloads/hexagonix/HBoot/total.svg)
![](https://img.shields.io/github/release/hexagonix/HBoot.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Hexagon Boot process

## Saturno

The first component of Hexagonix is Saturno. It is responsible for taking control of the boot process performed by the BIOS/UEFI and searching the volume for the second boot stage. For that, it implements a driver for reading a FAT16 file system. The second boot stage (see below) can implement drivers for other file systems and is responsible for finding Hexagon, loading HBoot modules or loading a DOS compatible system (BETA).

## Hexagon Boot (HBoot)

Hexagon Boot (HBoot) is a component designed to allow booting of the Hexagon kernel. Until then, initialization was performed by just one stage, which defined a very basic environment, loaded the Hexagon into memory, and immediately passed control to it, providing a very small and limited set of parameters, since the code for that stage stays restricted to 512 bytes, which limits the performance of several tests and data processing. With HBoot, it was possible to expand the number of tasks performed before the Hexagon runs, as well as the possibility to provide more information regarding the machine and boot environment. This is particularly important to allow the creation of a device tree that Hexagon can use to decide how to handle each identified device. HBoot is able to check which disk drives are available on the machine, emit a boot tone, obtain the amount of available RAM memory installed and allow or disallow the boot process to proceed according to this information. If no user interaction is detected 3 seconds after all essential tests and activities to create a boot environment for Hexagon, the system will load and run Hexagon (present in a file in the volume named **HEXAGON.SIS** in Hexagonix H1 and **HEXAGON** in Hexagonix H2) being unloaded from memory. The interaction with HBoot is done by pressing the F8 key after the respective message appears on the screen.

### Other functions

* HBoot allows loading modules in HBoot format, which may be useful in the future to allow for hardware tests such as memory and disk tests if the modules are available on disk. Modules can also be used to extend HBoot's functions. The format specification is now available and an example can be found below. These modules can be used to test specific devices, obtain hardware information, or load files into file systems not originally supported by HBoot.
* In the context of Hexagonix development, HBoot can also load directly, from a currently built-in module (this function will be moved to a standalone module as soon as possible) the core of the FreeDOS open source operating system[^1] , so that established and robust utility tools that run in a DOS environment can run on the Hexagonix volume and files. FreeDOS was chosen because of its feature of a kernel composed of a single file, usually "KERNEL.SYS"[^2], in addition to its free and free distribution. On the other hand, other DOS, such as MS-DOS, prior to version 7.0, use two files that must be contiguous on the disk, and this is not possible here, since the installation of FreeDOS already takes place on a Hexagonix volume, with the kernel copy , command interpreter and other DOS utilities, the main operating system being Hexagonix, with optional launching of FreeDOS for some special activity[^3]. If the FreeDOS system components are not present on the disk (the copy of FreeDOS files is not part of the default image), booting into DOS compatibility mode will not occur.

[^1]: You can find the project page [here](https://www.freedos.org/).
[^2]: Booting in DOS mode was possible after searching the FreeDOS documentation, especially the "SYS.C" file (which can be found [here](http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/dos/sys/2043/), which indicates in which segment the kernel expects to be loaded and which parameters are needed. Each DOS system has a preferred loading segment and this loading of other DOS editions can be implemented in the future with the help of HBoot modules. All code for core loading was developed from scratch and is not based on any existing ones.
[^3]: Booting in DOS compatibility mode of HBoot can be useful for running volume error checking, volume defragmenting, partitioner and other diagnostic tools, as well as development tools such as non-compilers and assemblers. supported by Hexagonix/Andromeda (the 16-bit tools, for example).

### HBoot Module Example

Below you can find an example of HBoot module implementation:

```assembly
;;**************************************************** ***********************************
;;
;;
;; HBoot Module
;;
;; Hexagon® Boot - HBoot
;;
;; Copyright © 2020-2021 Felipe Miguel Nery Lunkes
;; All rights reserved
;;
;;**************************************************** ***********************************

use16

;; The module must have a special HBoot image header
;; There are 6 bytes, with signature (magic number) and target architecture

HBootHeader:

.signature:    db "HBOOT" ;; Signature, 5 bytes
.architecture: db 01h     ;; Architecture (i386), 1 byte

;; Configure stack and pointer

    cli;; Disable interrupts
    
    mov ax, 0x2000 ;; Define stack registers here
    mov ss, ax
    mov sp, 0
    
    sti;; Enable interrupts
     
    clc

    mov ax, 0x2000 ;; Define segment registers here
    mov ds, ax
    mov es, ax
    
    sti;; Enable interrupts

;; Your code here

```

### Supported file systems

* FAT16B
* FAT12 (under development)

New file systems will be implemented in the future.

### Report bugs

HBoot has gained a lot of complexity since the beginning of its development in 2020. Due to this increase in code and the nature of its operation (16-bit), bugs can be found. They can be reported in the repository or by email, available at the end of this file.