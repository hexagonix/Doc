<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/stars/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/Hexagon.svg)
![](https://img.shields.io/github/downloads/hexagonix/Hexagon/total.svg)
![](https://img.shields.io/github/release/hexagonix/Hexagon.svg)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

# Kernel Hexagon

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="200" height="200">
</p>

<div align="justify">

Hexagon is a monolithic `kernel` running in 32-bit `protected mode`, developed purely in Assembly for the PC (x86) architecture. It is a kernel written from scratch, aiming for the speed and compatibility of modern hardware, but also being able to run on older hardware (Pentium III or higher, with 32 MB of RAM or more). At the moment, it guarantees a single-user environment, despite the use of virtual terminals, and single-tasking, despite the ability to load, keep in memory and control more than one process at a time, in a chronological order execution stack. In the future, the kernel may support the execution of multiple processes in preemptive multitasking. Hexagon was designed to be a Unix-like kernel and forms the basis of `Hexagonix`, albeit independently of it. It runs executable images in the `HAPP` format, developed exclusively for Hexagon. It also implements a very sophisticated API accessible through a standardized and documented system call, as you can see below.

Some features of Hexagon:

- [x] Support for x86 processors (Pentium III or higher);
- [x] Support for devices with 32 MB of RAM or more;
- [x] User environment support;
- [x] System call with 68 sophisticated functions accessed by the user environment;
- [x] Own executable binary format (HAPP);
- [x] Unix-like;
- [x] Completely written in x86 Assembly;
- [x] Self-hosting (the assembler used to build the Hexagon can run on top of it);
- [x] Virtual file system;
- [x] Device abstraction;
- [x] Full support for reading and writing on FAT16 file systems;
- [x] VESA VBE and multi-resolution graphics support;
- [x] Text mode support;
- [x] Graphic font rendering engine, which can be changed by the user;
- [x] Real-time clock support;
- [x] Support for serial and parallel ports (serial communication, debug and printing);
- [x] Supports own boot loader (Hexagon Boot - HBoot);
- [x] Support for users and permissions.

Other features being developed:

- [ ] Search and enumeration of all PCI devices;
- [ ] Preemptive multitasking.
    
> You can help implement the above development functions!
</div>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### History of development

<div align="justify">

The kernel was initially designed and written aiming at a structure and operation close to DOS (Disk Operating System) systems, such as MS-DOS, in the years 2015 to 2017. Therefore, many system calls and device names followed a syntax and names FROM. Over time, there was an interest in bringing the then core of Andromeda, which at that time had no name and was kept together with the distribution's code, closer to a structure and functioning closer to Unix-like systems, such as BSD or Linux. , for example. In this way, many parts of the kernel were re-implemented with the new purpose in mind. The core code was separated from the rest of the System and became independent, both in terms of development and operation, in addition to gaining a name, Hexagon. A hardware abstraction layer was written with the inclusion of system calls known in the Unix world, such as open(), close(), read() and write(). Devices were named and disk drives changed from DOS naming to Unix device names. The kernel then proceeds to follow a known initialization process, executing, with PID 1, the first user process, init, which then loads the rest of the components. Unix-like utilities were then written that used the kernel's Unix-like API, and several Unix-like tools have been written since then (2017 onwards).

</div>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### Hexagon Documentation Sessions

<div align="justify">

Now, you'll find more relevant information about Hexagon's particulars, how to develop compatible utilities, and how to help develop the kernel.

</div>

<details title="Developing for Hexagon" align='left'>
<br>
<summary align='left'><strong>Developing for Hexagon</strong></summary>

<div align="justify">

In this session, you will find relevant documentation on how to develop Hexagon compatible utilities. Select the topic of interest to you below. You can also suggest new topics. Just open an `issue` with your proposal.

</div>

<details title="Hexagon System Calls" align='left'>
<br>
<summary align='left'>Hexagon System Calls</summary>

<div align="justify">

Hexagon implements a series of functions that are exposed to the user environment, so that they can be used by developers to build utilities that use the Hexagon API. This API is accessible via system calls, or more easily via compatible development libraries such as [libasm](https://github.com/hexagonix/lib).

The number of system calls may vary with new Hexagon releases, as the tendency is for most non-critical functions to be moved to libraries, not staying in the core. However, with the natural evolution of the kernel, other functions and calls can be implemented.

At this time, there are 68 system calls that are exposed to the user environment by Hexagon. To do so, it implements an interrupt system that is accessible by any application via interrupt 69h (`int 69h`).

The format for passing parameters to the Hexagon interrupt handler is a mix of what is observed for what is implemented in MS-DOS and BSD systems. Some of the parameters are passed on the stack (as in BSD systems), while other parameters are passed through registers (as in MS-DOS), as follows:

* The desired function number is `always` supplied to Hexagon by the stack;
* The remaining parameters, which serve as input for the requested function, are provided exclusively by the registers, noting that each function accepts defined parameters and registers.

An example of a system call, to terminate the currently running process, can be seen below:

```assembly
    
    push 4     ;; Request function 4, to terminate process
    
    mov eax, 0 ;; Report error code 0
    
    int 69h    ;; call the hexagon
```
    
The `hexagon.s` file, present in the Hexagonix library by [libasm]() specifies all system calls currently supported by the current version of Hexagon, as well as lists the outputs and inputs for each requested function. Below you can see the system calls supported by Hexagon v1.0, extracted from `hexagon.s`. It is worth remembering that the calls below may change, so rely on libasm to identify which calls to use when writing an application.

```assembly
    
```
    
In the next section you can get more information on how to develop a simple application (text mode) for Hexagonix, using Hexagon services, libasm libraries and macros that make it easy to trigger system calls.

</div>

</details>

<details title="The HAPP executable format" align='left'>
<br>
<summary align='left'>The HAPP executable format</summary>

<div align="justify">

The HAPP executable image format was developed for Hexagon to allow the development of images that can be verified and validated for architecture and minimum kernel versions required for correct execution. The header also stores important information, allowing the developer to directly add an entry point, regardless of where it is inside the image, something that should have been redirected earlier when the executable image was in pure binary format. The HAPP image also allows you to validate that the image to be loaded is really an executable image, preventing unsupported files from being executed, even if they are not even executable files. It also allows the system to check code dependencies, such as the aforementioned architecture, as well as Hexagon version numbers, which must be equal to or greater than the minimum specified by the header. All HAPP images must have this full header, including reserved sessions, in order to function correctly in later versions of the System. HAPP images are always 32-bit.

In Assembly language, the system development language, the header, in its 2.0 specification:
    
```assembly
headerAPP:

.signature: db "HAPP"      ;; Signature
.architecture: db 01h      ;; Architecture (i386 = 01h)
.MinimumVersion: db 1      ;; Minimal version of Hexagon
.Minimum subversion: db 00 ;; Minimal Hexagon Subversion
.inputpoint: dd           ;; Input point offset (reference to main function here)
.ImageType: db 01h        ;; Executable image type (executable = 01h)
.reserved0: dd 0 ;; Reserved (Dword)
.reserved1: db 0 ;; Reserved (Byte)
.reserved2: db 0 ;; Reserved (Byte)
.reserved3: db 0 ;; Reserved (Byte)
.reserved4: dd 0 ;; Reserved (Dword)
.reserved5: dd 0 ;; Reserved (Dword)
.reserved6: dd 0 ;; Reserved (Dword)
.reserved7: db 0 ;; Reserved (Byte)
.reserved8: dw 0 ;; Reserved (Word)
.reserved9: dw 0 ;; Reserved (Word)
.reserved10: dw 0 ;; Reserved (Word)
```

Below is an implementation of a small application written as an example, which uses the Hexagon header and system calls, written in x86 assembly language in Intel syntax and assembled with the help of the flat assembler (FASM). This application sends a message to the terminal and then exits.

```assembly
;; This is a template for building a text mode app for
;; the Hexagonix!
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

headerAPP:

.signature: db "HAPP"      ;; Signature
.architecture: db 01h      ;; Architecture (i386 = 01h)
.MinimumVersion: db 1      ;; Minimal version of Hexagon
.Minimum subversion: db 00 ;; Minimal Hexagon Subversion
.EntryPoint: dd startAPP   ;; Entry point offset
.ImageType: db 01h         ;; Executable image
.reserved0: dd 0 ;; Reserved (Dword)
.reserved1: db 0 ;; Reserved (Byte)
.reserved2: db 0 ;; Reserved (Byte)
.reserved3: db 0 ;; Reserved (Byte)
.reserved4: dd 0 ;; Reserved (Dword)
.reserved5: dd 0 ;; Reserved (Dword)
.reserved6: dd 0 ;; Reserved (Dword)
.reserved7: db 0 ;; Reserved (Byte)
.reserved8: dw 0 ;; Reserved (Word)
.reserved9: dw 0 ;; Reserved (Word)
.reserved10: dw 0 ;; Reserved (Word)

;;************************************************ *************

include "hexagon.s" ;; Include system calls
include "macros.s"  ;; Includes macros

;;************************************************ *************

;; Variables and constants

msg: db 10, 10, "This is a template with a simple HAPP application example!", 10, 0

;;************************************************ *************

;; entry point

startAPP:

    mov esi, msg

    imprimirString ;; Here we have a macro that configures and calls an API function

    Hexagonix terminarProcesso ;; Another macro that asks which call to make
    
```
    
</div>

</details>

</details>

<!-- Will work like <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

### Looking for collaborators

<div align="justify">

Hexagon, like all of Hexagonix, seeks collaborators willing to contribute to its development. If you are interested in Assembly, C, helping with writing documentation or just helping with translation, feel free to get in touch! All are welcome!

</div>
