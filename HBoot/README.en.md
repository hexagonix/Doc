<!--

Copyright (c) 2015-2026 Felipe Miguel Nery Lunkes
Todos os direitos reservados.

-->

<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
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

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

# Hexagon Boot process

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

The Hexagonix initialization process is done by two components that come into action before the Hexagon (kernel). These are `Saturno` (responsible for loading the second boot stage) and `HBoot`, which is responsible for loading, configuring and running Hexagon, as well as offering other resources.

Shall we meet each one of them?

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Saturno

The first component of Hexagonix is Saturno. It is responsible for taking control of the boot process performed by the BIOS/UEFI and searching the volume for the second boot stage. For that, it implements a driver for reading a FAT16 file system. The second boot stage (see below) can implement drivers for other file systems and is responsible for finding Hexagon, loading HBoot modules or loading a DOS compatible system (BETA).

<details title="Saturno license" align='left'>
<summary align='left'>Saturno License</summary>
<br>

<div align="justify">

Please read the [license](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) for more information about copyright, code ownership, and redistribution that applies to files available in this repository. Hexagonix is ​​fully licensed under [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Always pay attention to the `LICENSE` file available in each repository to be aware of legal rights and obligations, as well as the list of project contributors.

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## HBoot

HBoot is a component designed to allow booting of the Hexagon kernel. Until then, initialization was performed by just one stage, which defined a very basic environment, loaded the Hexagon into memory, and immediately passed control to it, providing a very small and limited set of parameters, since the code for that stage stays restricted to 512 bytes, which limits the performance of several tests and data processing. With HBoot, it was possible to expand the number of tasks performed before the Hexagon runs, as well as the possibility to provide more information regarding the machine and boot environment. This is particularly important to allow the creation of a device tree that Hexagon can use to decide how to handle each identified device. HBoot is able to check which disk drives are available on the machine, emit a boot tone, obtain the amount of available RAM memory installed and allow or disallow the boot process to proceed according to this information. If no user interaction is detected 3 seconds after all essential tests and activities to create a boot environment for Hexagon, the system will load and run Hexagon (present in a file in the volume named **HEXAGON.SIS** in Hexagonix H1 and **HEXAGON** in Hexagonix H2) being unloaded from memory. The interaction with HBoot is done by pressing the F8 key after the respective message appears on the screen.

<details title="HBoot license" align='left'>
<summary align='left'>HBoot License</summary>
<br>

<div align="justify">

Please read the [license](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) for more information about copyright, code ownership, and redistribution that applies to files available in this repository. Hexagonix is ​​fully licensed under [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Always pay attention to the `LICENSE` file available in each repository to be aware of legal rights and obligations, as well as the list of project contributors.

</div>

</details>

### Other functions

* HBoot allows loading modules in HBoot format, which may be useful in the future to allow for hardware tests such as memory and disk tests if the modules are available on disk. Modules can also be used to extend HBoot's functions. The format specification is now available and an example can be found below. These modules can be used to test specific devices, obtain hardware information, or load files into file systems not originally supported by HBoot.
* In the context of Hexagonix development, HBoot can also load directly, from a currently built-in module (this function will be moved to a standalone module as soon as possible) the core of the FreeDOS open source operating system[^1] , so that established and robust utility tools that run in a DOS environment can run on the Hexagonix volume and files. FreeDOS was chosen because of its feature of a kernel composed of a single file, usually "KERNEL.SYS"[^2], in addition to its free and free distribution. On the other hand, other DOS, such as MS-DOS, prior to version 7.0, use two files that must be contiguous on the disk, and this is not possible here, since the installation of FreeDOS already takes place on a Hexagonix volume, with the kernel copy , command interpreter and other DOS utilities, the main operating system being Hexagonix, with optional launching of FreeDOS for some special activity[^3]. If the FreeDOS system components are not present on the disk (the copy of FreeDOS files is not part of the default image), booting into DOS compatibility mode will not occur.

[^1]: You can find the project page [here](https://www.freedos.org/).
[^2]: Booting in DOS mode was possible after searching the FreeDOS documentation, especially the "SYS.C" file (which can be found [here](http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/dos/sys/2043/), which indicates in which segment the kernel expects to be loaded and which parameters are needed. Each DOS system has a preferred loading segment and the execution of other DOS editions (MS-DOS, DR-DOS, etc) can be implemented in the future with the help of HBoot modules. All code for core loading was developed from scratch and is not based on any existing ones.
[^3]: Booting in DOS compatibility mode of HBoot can be useful for running volume error checking, volume defragmenting, partitioner and other diagnostic tools, as well as development tools such as compilers and assemblers not supported by Hexagonix (the 16-bit tools, for example).

### HBoot Module Example

Below you can find an example of HBoot module implementation:

```assembly
;;************************************************************************************
;;
;;
;; HBoot Module
;;
;; Hexagon Boot - HBoot
;;
;; Copyright © 2020-2026 Felipe Miguel Nery Lunkes
;; All rights reserved
;;
;;************************************************************************************

use16

;; The module must have a special HBoot image header
;; There are 16 bytes, with signature (magic number), target architecture,
;; version, subversion and internal name, the last one with up to 8 bytes

HBootHeader:

.signature:    db "HBOOT"    ;; Signature, 5 bytes
.architecture: db 01h        ;; Architecture (i386), 1 byte
.modVersion:   db 01h        ;; Version
.modSubversion: db 00h       ;; Subversion
.modName:      db "EXAMPLE " ;; Module name

HBOOT_SERVICES = 20h ;; Interrupt HBoot answers a module on

;; The module starts executing at offset 10h, right past the header. The stack
;; and the segment it was loaded into come from HBoot, so all it takes is
;; reading the segment out of CS, which cannot be copied straight into DS and ES

moduleStart:

    mov ax, cs
    mov ds, ax
    mov es, ax

;; Your code here. Printing is a service of HBoot itself, so a module does not
;; have to carry its own routine to write on the screen

    mov si, message
    mov ah, 01h

    int HBOOT_SERVICES

;; Returning to HBoot is a service as well, and it is the only way back

    mov ah, 05h

    int HBOOT_SERVICES

message:
db 13, 10, "Module started successfully.", 13, 10, 0
```

### HBoot services for modules

A module gets the whole machine to itself and, left alone, would have to carry its own copy of everything HBoot already does. To avoid that, HBoot installs interrupt `20h` before handing control over, and answers the module on it.

The contract is the same for every function and does not vary:

* On entry, `AH` holds the function number;
* On exit, `CF` is clear if the function succeeded and set if it failed, with `EAX` holding the result of the function or, when `CF` is set, the error code;
* Every other register, and every segment register, comes back exactly as it went in. A module may call from any stack, at any depth.

The available functions:

| Function | Description | Entry | Exit |
| --- | --- | --- | --- |
| `00h` | Interface information | `ES:DI` points to a buffer of 8 bytes | Buffer filled and `EAX` = 0 |
| `01h` | Print a string | `DS:SI` points to it, terminated by a 0, with no limit to its length | `EAX` = 0 |
| `02h` | Volume information | `ES:DI` points to a buffer of 12 bytes | Buffer filled and `EAX` = 0 |
| `03h` | Load a file | `DS:SI` points to its path, terminated by a 0, and `ES` is the segment it is loaded into, from offset 0 | `EAX` holds the size of the file in bytes |
| `04h` | Read sectors | `EBX` is the first LBA, `CX` the number of sectors and `ES:DI` the buffer they are read into | `EAX` = 0 |
| `05h` | Return to HBoot | — | Does not come back |

The error codes returned in `EAX`:

| Code | Meaning |
| --- | --- |
| `01h` | No such function number |
| `02h` | Path names nothing the volume can hold |
| `03h` | No such file on the volume |
| `04h` | Function called with a value it cannot use |

The buffer function `00h` fills lets a module tell which interface it is talking to before relying on it:

```assembly
.signature:   db "HBSV" ;; 4 bytes
.version:     db 01h
.subversion:  db 00h
.functions:   db 05h    ;; Highest function number answered
.reserved:    db 00h
```

The buffer function `02h` fills describes the volume HBoot booted from:

```assembly
.BPB:          dd 0 ;; Flat address of the BIOS Parameter Block
.partitionLBA: dd 0 ;; LBA the partition starts at
.drive:        db 0 ;; Drive HBoot was loaded from
.filesystem:   db 0 ;; Filesystem code
.reserved:     dw 0
```

Function `03h` takes the path written the same way the user writes it on HBoot's own command line, as in `/boot/hexagon`. A path may name at most one directory, and each name in it at most 8 characters plus a 3 character extension.

Returning to HBoot is function `05h`, and it is the only way back. Restarting the machine or jumping straight to HBoot's entry point does not work: HBoot reads the volume it was loaded from out of registers a module is free to have used. After function `05h`, HBoot picks the boot up again from where it left off.

### Supported file systems

* FAT16B
* FAT12 (under development)

New file systems will be implemented in the future.

### Report bugs

HBoot has gained a lot of complexity since the beginning of its development in 2020. Due to this increase in code and the nature of its operation (16-bit), bugs can be found. They can be reported in the repository or by email, available at the end of this file.
