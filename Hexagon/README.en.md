## Hexagon Kernel

### What is it

Hexagon is a monolithic kernel (the operating system core) running in 32-bit protected mode, developed with the PC architecture (x86) in mind. It is a kernel written from scratch, aiming for the speed and compatibility of modern hardware but also being able to run on older hardware. For the moment, it guarantees a single-user environment, despite the use of virtual terminals, and monotasking, despite the ability to load, keep in memory and control more than one process, in a chronologically-ordered execution stack. In the future, the kernel will be able to support the execution of multiple processes in preemptive multitasking. Hexagon is a Unix-like kernel and forms the basis of the Hexagonix/Andromeda Operating System, although independent of it. It runs executable images in HAPP format, developed for Hexagon. It implements a very sophisticated API accessible via a system call.

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="250" height="250">
</p>

### History

The kernel was initially designed and written aiming at a structure and functioning similar to DOS (Disk Operating System) systems, such as MS-DOS, in the year 2015 to 2017. Therefore, many system calls and device names followed a syntax and names FROM. Over time, there was an interest in bringing the then core of Andromeda, which at that time did not have a name and was kept with the distribution code, to a structure and operation closer to Unix-like systems, such as BSD or Linux , for example. In this way, many parts of the kernel were redeployed with the new goal in mind. The core code was separated from the rest of the System and became independent, in terms of development as well as functioning, in addition to gaining a name, Hexagon. A hardware abstraction layer was written with the inclusion of system calls known in the Unix world, such as open(), close(), read() and write(). Devices were named and disk drives were changed from DOS naming to Unix device names. The kernel then goes through a known boot process, with the execution, with PID 1, of the user's first process, init, which then loads the rest of the components. Unix-like utilities were then written that used the kernel's Unix-like API, and several Unix-like tools have been written since then (2017 onwards).

### The HAPP executable format

The HAPP executable image format was developed for Hexagon to allow the development of images that can be verified and validated for architecture and minimum kernel versions necessary for correct execution. The header also stores important information, allowing the developer to directly add an entry point, regardless of where it is inside the image, something that would have been redirected earlier when the executable image was in pure binary format. The HAPP image also allows you to validate that the image to be loaded is really an executable image, thus preventing unsupported files from being executed, even if they are not even executable files. It also allows the system to check code dependencies, such as the aforementioned architecture, as well as Hexagon version numbers, which must be equal to or greater than the minimum specified by the header. All HAPP images must have this complete header, including reserved sessions, in order to function correctly in later versions of the System. HAPP images are always 32-bit.

In assembly language, the system development language, the header, in its 2.0 specification:

```assembly
APPHeader:

.signature:          db "HAPP" ;; Signature
.architecture:       db 01h ;; Architecture (i386 = 01h)
.Minimum Version:    db 8 ;; Minimal version of Hexagon
.Minimum subversion: db 40 ;; Minimal Hexagon Subversion
.entrypoint:         dd ;; Input point offset (reference to main function here)
.ImageType:          db 01h ;; Executable image type (executable = 01h)
.reserved0:          dd 0 ;; Reserved (Dword)
.reserved1:          db 0 ;; Reserved (Byte)
.reserved2:          db 0 ;; Reserved (Byte)
.reserved3:          db 0 ;; Reserved (Byte)
.reserved4:          dd 0 ;; Reserved (Dword)
.reserved5:          dd 0 ;; Reserved (Dword)
.reserved6:          dd 0 ;; Reserved (Dword)
.reserved7:          db 0 ;; Reserved (Byte)
.reserved8:          dw 0 ;; Reserved (Word)
.reserved9:          dw 0 ;; Reserved (Word)
.reserved10:         dw 0 ;; Reserved (Word)
```

Below is an implementation of a small application written as an example, which uses the Hexagon header and system calls, written in x86 assembly language in Intel syntax and assembled with the aid of a flat assembler (FASM). This app sends a message to the terminal and then exits.

```assembly
;; This is a template for building a text mode app for
;; the Hexagonix/Andromeda!
;;
;; Written by Felipe Miguel Nery Lunkes on 12/04/2020
;;
;; You can generate an executable HAPP image using the assembler
;; FASM. To do this, use the command line below:
;;
;; fasmX tapp.asm
;; or
;; fasmX tapp.asm tapp.app

use32

APPHeader:

.signature:          db "HAPP" ;; Signature
.architecture:       db 01h ;; Architecture (i386 = 01h)
.Minimum Version:    db 8 ;; Minimal version of Hexagon
.Minimum subversion: db 40 ;; Minimal Hexagon Subversion
.Entrypoint:         dd startAPP ;; Input point offset
.ImageType:          db 01h ;; executable image
.reserved0:          dd 0 ;; Reserved (Dword)
.reserved1:          db 0 ;; Reserved (Byte)
.reserved2:          db 0 ;; Reserved (Byte)
.reserved3:          db 0 ;; Reserved (Byte)
.reserved4:          dd 0 ;; Reserved (Dword)
.reserved5:          dd 0 ;; Reserved (Dword)
.reserved6:          dd 0 ;; Reserved (Dword)
.reserved7:          db 0 ;; Reserved (Byte)
.reserved8:          dw 0 ;; Reserved (Word)
.reserved9:          dw 0 ;; Reserved (Word)
.reserved10:         dw 0 ;; Reserved (Word)

;;**************************************************** *************

include "andrmda.s" ;; Include system calls

;;**************************************************** *************

;; Variables and constants

msg: db 10, 10, "This is a template with a simple HAPP application example!", 10, 0

;;**************************************************** *************

;; entry point

startAPP:

    mov esi, msg

    printString ;; Here we have a macro that configures and calls an API function.

    Andromeda terminateProcess ;; Another macro that asks which call to make
```
# Software license (source code)

## Copyright Notes

Hexagonix/Andromeda was developed from scratch by [Felipe Lunkes](https://github.com/felipenlunkes).

Read the [license](LICENSE) for more information on copyright, code ownership and redistribution.

* Hexagonix Operating System & Andromeda Operating System. Copyright © 2016-2022 Felipe Miguel Nery Lunkes. All rights reserved.
* Hexagon kernel. Copyright © 2016-2022 Felipe Miguel Nery Lunkes. All rights reserved.
* Saturn Boot Manager. Copyright © 2016-2022 Felipe Miguel Nery Lunkes. All rights reserved.
* Hexagon Boot (HBoot). Copyright © 2020-2022 Felipe Miguel Nery Lunkes. All rights reserved.

## License

English version

Hexagonix Operating System
Andromeda Operating System

Copyright (C) 2015-2022 Felipe Miguel Nery Lunkes. All rights reserved.

Hexagonix Software License version 2.0 (2021)

This license applies to any files present in this repository,
including any directory, which make up the Hexagonix/Andromeda operating system,
as well as its components, libraries, data files, binary images and adjacent files.

Hereby, possession, publication, use, commercial or otherwise, is prohibited.
and redistribution of any code under this license without authorization
explicit written statement from the developer. In addition, this license expresses the
obligatory destruction of any copies of files obtained illegally
of this repository, at the risk of applicable legal sanctions.

THE SOFTWARE IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NON-INFRINGEMENT. UNDER NO EVENT
AUTHORS OR COPYRIGHT HOLDERS ARE RESPONSIBLE FOR ANY CLAIM,
DAMAGES OR OTHER LIABILITIES, WHETHER IN CONTRACT, TORT OR OTHERWISE,
ARISING OUT OF, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER NEGOTIATIONS ON
SOFTWARE.
