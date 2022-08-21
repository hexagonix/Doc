<!--

Version of this file: 5.0

Copyright © 2021-2022 Felipe Miguel Nery Lunkes
All rights reserved.

-->

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix.png" width="150" height="150">
</p>

# Starting point

<details title="License" align='left'>
<br>
<summary align='left'><strong>1️⃣ License</strong></summary>

<div align="justify">

Hexagonix Operating System

BSD 3-Clause License

Copyright (c) 2015-2022, Felipe Miguel Nery Lunkes <br>
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

<details title="Download and test the system right now" align='left'>
<br>
<summary align='left'><strong>2️⃣ Download and test the system right now</strong></summary>

<div align="justify">

At [end of this file](https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.en.md#how-to-test-hexagonix-or-andromeda) you will find a tutorial to run the Hexagonix/Andromeda on your computer, either in a virtualized or native version. Remember that it is necessary to have an x86 architecture computer or an emulator, if you are using a device of another architecture for testing.

You can go to the [releases](https://github.com/hexagonix/hexagonix/releases) session for stable system releases. You can access the Hexagonix/Andromeda [release announcement](REL.en.md) file. Whenever possible, get the latest release and download the available .img images or the complete package in zip format. The versions available directly in this repository are always development versions (beta and release candidate), which can also be run and are minimally functional. At the end of each development cycle, the final versions will be available in the [releases](https://github.com/hexagonix/hexagonix/releases) session.

</div>

</details>

<details title="System documentation" align='left'>
<br>
<summary align='left'><strong>3️⃣ System documentation</strong></summary>

* [System Documentation (under construction)](https://github.com/hexagonix/Doc)

</details>

<details title="Building the system" align='left'>
<br>
<summary align='left'><strong>4️⃣ Building the system</strong></summary>

* [Build the system](https://github.com/hexagonix/build/blob/main/README.en.md)

</details>

<details title="Help with the development of Hexagonix" align='left'>
<br>
<summary align='left'><strong>5️⃣ Help with the development of Hexagonix</strong></summary>

<div align="justify">

If you have knowledge of writing x86 assembly code and would like to help develop the system, feel free to send me an email! See [here](https://github.com/hexagonix/Doc/blob/main/Hexagonix/README.en.md#author) how to contact me!

</div>

</details>

<details title="Report system errors" align='left'>
<br>
<summary align='left'><strong>6️⃣ Report system errors</strong></summary>

You can report system errors [here](https://github.com/hexagonix/hexagonix/issues).

</details>

<hr>

# About the system

<details title="Development" align='left'>
<br>
<summary align='left'><strong>1️⃣ Development</strong></summary>

<div align="justify">

Hexagonix/Andromeda and all its components have been developed since 2015 and were written completely in Assembly language.

</div>

</details>

<details title="Why two names? Hexagonix and Andromeda, what are they?" align='left'>
<br>
<summary align='left'><strong>2️⃣ Why two names? Hexagonix and Andromeda, what are they?</strong></summary>

<div align="justify">

In the beginning, Andromeda was intended to be a complete operating system, composed of the kernel, libraries, graphical and text interface and utilities. Later, with the passage of time and the change in approach to the architecture and objectives of the system, the components were separated and became independent projects in terms of functioning, organization and development. As you will see below, the Andromeda core was separated from the rest of the Andromeda code tree, becoming an independent project, even given a name, Hexagon. From then on, the idea arose of making the composition of the system more flexible and allowing the development of distributions, as in GNU/Linux. In this way, distributions of Hexagon could be created, grouping the necessary components for the basic functioning (Hexagonix) and allowing the extension of the system if necessary, with new components, modules and utilities, being the userland defined in each case. With the change of architecture of the system itself, with the core approaching a Unix-like architecture, new utilities in Unix style and syntax were developed and kept separate, in another project. From the original Andromeda project we have the Andromeda-specific applications and graphics libraries. A base system was then created, which by itself can already be fully executed, and became the base of Andromeda. This base system is called Hexagonix, and it consists of the HBoot boot loader, the Hexagon kernel, the shell, Unix environment libraries (here called Hexagonix environment) and Unix utilities. This system is fully functional, but lacks graphical resources and applications developed for the Andromeda environment. In this way, Andromeda stands out for being one more layer built on top of Hexagonix, with graphics resources, a graphics library and utilities that work on top of Hexagonix and extend its function. This built environment was named the Andromeda environment. To meet different needs, the two distributions will always be kept. Both versions are functional and can be used depending on the desired end use. In summary, both Hexagonix and Andromeda are distributions of the Hexagon kernel, differing in the components included.

To better understand this distribution model, a suitable example would be what happens with macOS (Apple)[^1]. macOS is a Unix-like operating system built on top of Darwin, a free operating system composed of the XNU kernel, libraries and utilities, adding on top of Darwin the Aqua graphical interface and other applications and utilities developed by Apple and other vendors. The Darwin environment is easily accessed and observed through macOS, such as using the terminal, for example. Darwin is a complete and functional system, but it lacks some graphical resources, for example, which are only distributed together with macOS. In this analogy, we have macOS as Andromeda and Darwin as Hexagonix.

</div>

</details>

<details title="What about the source code?" align='left'>
<br>
<summary align='left'><strong>3️⃣ What about the source code?</strong></summary>

<div align="justify">

The project's source code has now been made publicly available. The kernel code and Unix-like utilities and Andromeda applications are available, as well as the source package that makes up HBoot. Disk images with both Hexagonix and Andromeda are now available and free to distribute. Please note the [license](LICENSE) available in this repository for more information. It is worth mentioning that the license of each code package that makes up the system (Hexagon, HBoot, Hexagonix utilities, Andromeda utilities, fonts and other components) may vary. Each package can be released with a different license type (like GPL, MIT or BSD, for example). Keep an eye on each license in the respective repositories. Components that are not available in the official repository are still closed source, governed by a proprietary Hexagonix license, which can be found [here](https://github.com/hexagonix/Doc/blob/main/LICENSES/Hexagonix ).

</div>

</details>

<details title="The Hexagonix/Andromeda story" align='left'>
<br>
<summary align='left'><strong>4️⃣ The Hexagonix/Andromeda story</strong></summary>

<div align="justify">

Andromeda started as an implementation in a structure similar to DOS-like systems, with an interpreter with built-in commands with names, syntax and results similar to a generic DOS. The command interpreter presented file manipulation and other commands internally, as in conventional DOS systems. Disk drives were also defined as letters. Later, there was a growing interest and fascination in the functioning of Unix systems and all the code was rewritten or adapted to make the System kernel a Unix-like kernel. All System components, as in DOS, were kept in a single tree until then. With Andromeda version 1.5, codenamed "Unix-like", the kernel has been heavily modified and rewritten to fit the Unix philosophy. Changes even included the way devices were handled, writing a hardware abstraction layer managing devices as files, and adding open(), close(), write() and to read(). The Unix-based utilities were also written, removing commands from the default interpreter, which was rewritten to make way for a Unix-like shell. The internal commands were moved to the utilities, which now have a Unix-style structure and syntax. The rest of the utilities, such as mount, were written already taking advantage of the open() call, which is also used to mount volume, in addition to opening ordinary files from the volume. The open() call is also used to start other peripherals such as serial and parallel ports. The write() call also works with devices and files, as well as close(). A VFS (Virtual File System) was also introduced, which will support multiple file systems in the future and make file management transparent to the System, programmers and users. Also included are new hardware management functions and many improvements and bug fixes. The System gains new Unix utilities until the present moment. After this change in the proposal and architecture, the components were separated and allocated to specific projects. The union of these components forms the operating system. In the case of Hexagonix, we have HBoot, Hexagon, shell, Unix libraries and applications, while Andromeda extends Hexagonix, incorporating other libraries and applications that use them.

</div>

</details>

<hr>

# System components

<details title="Saturno" align='left'>
<br>
<summary align='left'><strong>1️⃣ Saturno</strong></summary>

<div align="justify">

The first component of Hexagonix/Andromeda is Saturno. It is responsible for taking control of the boot process performed by the BIOS/UEFI and looking in the volume for the second boot stage. For that, it implements a driver for reading a FAT16 file system. The second boot stage (see below) can implement drivers for other filesystems and is responsible for finding Hexagon, loading HBoot modules or loading a compatible DOS-like system (BETA version).

</div>

</details>

<details title="Hexagon Boot (HBoot)" align='left'>
<br>
<summary align='left'><strong>2️⃣ Hexagon Boot (HBoot)</strong></summary>

<div align="justify">

Hexagon Boot (HBoot) is a component designed to allow the Hexagon kernel to boot. Until then, initialization was performed by just one stage, which defined a very basic environment, loaded Hexagon into memory and immediately executed it, providing a very limited set of parameters. This is due to the fact that the code of this stage is restricted to 512 bytes, which limits the performance of several tests and data processing. With HBoot, it was possible to expand the number of tasks performed before running Hexagon, as well as the possibility to provide more information about the device and boot environment. This is particularly important to allow the creation of a device tree that Hexagon can use to decide how to handle each identified device. HBoot is able to check which disk drives are available on the machine, emit a boot tone, obtain the amount of available RAM memory installed and allow or not to proceed with the boot process according to this information. If no user interaction is detected (within 3 seconds after HBoot is started and messages are displayed to the user), HBoot will perform additional tests to verify the device's ability to run the system and will load and run Hexagon (present in a file on the volume named **HEXAGON.SIS**). After loading, HBoot transfers control to Hexagon, which is initialized and stores the data provided by HBoot in the kernel environment.

</div>

[![HBoot](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=HBoot&theme=dark)](https://github.com/hexagonix/Hboot)

### How to interact with HBoot

<div align="justify">

Interaction with HBoot takes place by pressing the F8 key after booting and displaying messages on the screen. HBoot waits for 3 seconds for any interaction and, if none has occurred, it continues executing the boot protocol. The interaction with HBoot can be interesting to load modules in the HBoot format, provide boot parameters to Hexagon, load a DOS-type system whose files are present on the same volume or even load HAPP images from other cores (if the developer wants to use the HBoot implementation in your project). Below, see some more details of additional and diagnostic functions that can be performed via interaction with HBoot before loading Hexagonix.

<div>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/HBoot.png" width="600" height="500">
</p>

### Report bugs

<div align="justify">

HBoot has gained a lot of complexity since the beginning of its development in 2020. Due to this increase in code and the nature of its operation (16-bit), bugs can be found. They can be reported in the repository or by email, available at the end of this file.

</div>

</details>

<details title="Hexagon kernel" align='left'>
<br>
<summary align='left'><strong>3️⃣ Hexagon kernel</strong></summary>

### Which is

<div align="justify">

Hexagon is a monolithic kernel running in 32-bit protected mode, designed with the PC architecture in mind (x86). It's a kernel written from scratch, aiming for the speed and compatibility of modern hardware but also being able to run on older hardware (like a Pentium III). At the moment, it guarantees a single-user environment, despite the use of virtual terminals, and single-tasking, despite the ability to load, keep in memory and control more than one process, in an execution stack. In the future, the kernel may support running multiple processes in preemptive multitasking. Hexagon is a Unix-like kernel and tries to implement POSIX compatibility, although far from it, and forms the basis of the Hexagonix/Andromeda Operating System, although independent of it. It runs executable images in the HAPP format, developed exclusively for Hexagon. Hexagon also implements a very sophisticated API accessible through a system call.

</div>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/LogoHexagon.png" width="180" height="180">
</p>

[![Hexagon Kernel](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=Hexagon&theme=dark)](https://github.com/hexagonix/Hexagon)

### System calls

<div align="justify">

System calls are BSD-style, with the function number present on the stack and the parameters/arguments next to the registers. For a complete list of system calls available in the current version of the system, have a look at the Hexagon library at [libasm for fasm](https://github.com/hexagonix/libasm/blob/main/fasm/hexagon.s ) or [libasm to nasm](https://github.com/hexagonix/libasm/blob/main/nasm/hexagon.s).

</div>

</details>

<details title="Hexagonix environment" align='left'>
<br>
<summary align='left'><strong>4️⃣ Hexagonix environment</strong></summary>

<div align="justify">

Hexagonix implements, together with Hexagon, a series of Unix-like utilities, with functionality and syntax similar to UNIX and Unix-like systems. **Utilities such as init, login, sh, top, ps, cp, rm, cat, clear, man, among others, are included in the standard distribution of Hexagonix**. These utilities make up the Hexagonix base utilities package. The login and user-mode environment startup tools are in this package, as well as several configuration files for this environment. These utilities generally do not have a graphical interface, only a command-line interface (CLI). However, they can be requested by applications that have a graphical interface. This environment is available in both the [Hexagonix](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img) distribution and the [Andromeda](https://github.com/hexagonix/hexagonix/blob/main/andromeda.img) distribution.

</div>

[![Unix-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=unix-apps&theme=dark)](https://github.com/hexagonix/unix-apps)

### Some applications and utilities in the Hexagonix environment

<div align="justify">

Hexagonix includes many of the Unix utilities you may already be familiar with, such as:

* init
* login
* ls
* cat
* cp
* rm
* clear
* top
* ps
* man
* su
* sh (shell padrão)
* uname
* whoami, among others.

Some applications and utilities were developed exclusively for Hexagonix, such as:

* atop (alternate version of top)
* power (computer state control)
* fnt (Hexagonix graphic font change utility)
* hash (alternate shell)
* log (used to get internal Hexagon logs, but now deprecated)
* lshapp (gets and displays information from HAPP images)
* lshmod (gets and displays information from HBoot modules and HBoot itself)

It is worth remembering that Hexagonix utilities try to implement a POSIX interface as far as possible, similar in syntax to Unix utilities (FreeBSD and Linux used as a template).

### Third party applications available for Hexagonix

* [Flat Assembler (fasm)](https://flatassembler.net/index.php)

Hexagonix received a port of the [fasm](https://flatassembler.net/index.php) assembler, which was adapted for Hexagonix, allowing the user to develop applications directly on the system. This port is called fasmX. Changes added to the code, as well as the software license, can be found in the [fasm repository for Hexagonix](https://github.com/hexagonix/fasm). This repository is a fork of [original repository](https://github.com/tgrysztar/fasm). Added code is based on modifications made to the original code and authorial additions. This modified/authored code can be found in the repository, [clicking here](https://github.com/hexagonix/fasm/tree/master/SOURCE/HEXAGONIX). fasmX, the fasm port for Hexagonix, is always updated when new features are added to the fasm repository. To indicate that it is a stable and tested version, the fasmX version number always inherits the fasm numbering, followed by a character x (for example, the version based on fasm 1.73.30, after testing, receives the numbering 1.73 .30x). You can report bugs or code generation or optimization issues in the Hexagonix version [here](https://github.com/hexagonix/fasm/issues). To report general fasm bugs, use the [official](https://github.com/tgrysztar/fasm) repository.

</div>

</details>

<details title="Andromeda environment" align='left'>
<br>
<summary align='left'><strong>5️⃣ Andromeda environment</strong></summary>

<div align="justify">

The Andromeda environment is built on the solid foundation provided by Hexagonix, including applications and utilities that do not implement the Unix philosophy or have a very different syntax and usage than you would expect from a Unix environment. In this way, they are separated as **Andromeda apps**, and are not part of the standard distribution of Hexagonix. Here are the System settings app, calculator, font manager, text editors and the IDE developed for Andromeda. These utilities may or may not have a graphical interface. Together with them, the Andromeda environment comprises libraries developed to allow the development of applications, such as the **Estelar** library. This environment is only available in the [Andromeda](andromeda.img) distribution.

</div>

[![Andromeda-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=andromeda-apps&theme=dark)](https://github.com/hexagonix/andromeda-apps)

### Some Andromeda applications and utilities

<div align="justify">

* System Settings (config)
* Quartzo text editor
* Lyoko IDE for application development
* Electronic piano return Piano;
* Serial communication utility
* Andromeda Shell (ASH) - A new shell for Andromeda
* Andromeda calculator
* Font change utility
* Andromeda shutdown utility

### Third-party apps available for Andromeda

There are no third-party applications available for the Andromeda environment yet. If interested, you can build the first one!

</div>

</details>

<details title="Hexagonix graphic fonts" align='left'>
<br>
<summary align='left'><strong>6️⃣ Hexagonix graphic fonts</strong></summary>

<div align="justify">

The default installation of Hexagonix also provides a number of fonts that can be loaded by the settings application or the font utility (font manager). These files are used to change the graphical display font for Hexagonix and Andromeda.

Graphics mode fonts for Hexagon are developed as a bitmap in Assembly which, when compiled, generate a binary image of the font with representations of each character. The standard Hexagonix fonts have now been released as open source and are available in the [Hexagonix font repository](https://github.com/hexagonix/xfnt).

</div>

[![Andromeda-Apps](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=xfnt&theme=dark)](https://github.com/hexagonix/xfnt)

</details>

<details title="System development libraries" align='left'>
<br>
<summary align='left'><strong>7️⃣ System development libraries</strong></summary>

<div align="justify">

Hexagonix/Andromeda also provides functions that must be used to interact with the system environment itself. Libraries are used to access functions implemented by Hexagon or by the libraries themselves, allowing easy development of applications and utilities for both the Hexagonix and Andromeda environments. The libraries implement functions for displaying text, mathematical calculations, sending messages, opening files and devices, and much more. The core library (hexagon.s) provides functions accessible to both possible distribution environments, while other libraries may be unique to the Andromeda environment. These libraries include graphical functions to build interfaces in graphical mode (Andromeda), as well as functions to check the currently running system version (Hexagonix and Andromeda). The Hexagonix base utilities perform the Hexagon version check to see if they can be run, using the Unix utility uname or directly via a Hexagon system call.

To learn more and check each function available in the system development libraries, see the [libasm] repository (https://github.com/hexagonix/libasm).

</div>

[![lib](https://github-readme-stats.vercel.app/api/pin/?username=Hexagonix&repo=lib&theme=dark)](https://github.com/hexagonix/lib)

</details>

<hr>

# How to test Hexagonix or Andromeda

<details title="System requirements" align='left'>
<br>
<summary align='left'><strong>1️⃣ System requirements</strong></summary>

<div align="justify">

Below is a list of minimum and recommended requirements for testing Hexagonix/Andromeda on a virtual machine or physical machine.

</div>

<details title="Minimum requeriments" align='left'>
<br>
<summary align='left'><strong>☑️ Minimum requeriments</strong></summary>

* Processor: Pentium III (1999) with SSE and MMX support or newer;
* RAM memory: 32 Mb minimum (a minimum installation with 32 Mb is usually enough, in most cases);
* Hard disk: IDE or SATA hard disk with a minimum of 50 Mb;
* Necessary peripherals:
  * Serial port (1-4);
  * Parallel port (1-4);
  * PS/2 or USB keyboard;
  * VGA video card with 2 Mb of video memory (with color support).

</details>

<details title="Recommended" align='left'>
<br>
<summary align='left'><strong>✅️ Recommended</strong></summary>

* Processor: Pentium D or newer;
* RAM memory: 50 Mb;
* Optional peripherals:
  * PS/2 or USB mouse;
  * Video card with > 2 Mb of video memory.

</details>

</details>

> Running Hexagonix

<details title="Get the disk images with the system instalation" align='left'>
<br>
<summary align='left'><strong>2️⃣ Get the disk images with the system instalation</strong></summary>

<div align="justify">

To test Hexagonix or Andromeda, you will need one of the disk images available, as well as the [qemu](https://www.qemu.org) tool installed on your computer, if you want to test the system in an environment virtualized. The image can also be used for writing to a physical disk on a real machine.

To test Hexagonix, get the ['hexagonix.img'](https://github.com/hexagonix/hexagonix/blob/main/hexagonix.img).
To test Andromeda, get the ['andromeda.img'](https://github.com/hexagonix/hexagonix/blob/main/andromeda.img).

</div>

</details>

<details title="For testing in virtualized environment" align='left'>
<br>
<summary align='left'><strong>3️⃣ For testing in virtualized environment</strong></summary>

<div align="justify">

First, you must install the qemu tool, which will manage the virtual machine. To do so, you can install qemu using official Linux distribution repositories or by accessing [here](https://www.qemu.org) to get installation files for Windows and macOS. 

> Install on Debian, Ubuntu, Pop!_OS and derivatives

For Ubuntu, the following line will install qemu and all its dependencies (root privileges required):

```
sudo apt install qemu qemu-system-i386
```

> Install on Fedora, CentOS and derivatives

For Fedora, the following line will install qemu and all its dependencies (root privileges required):

```
sudo dnf install qemu qemu-system-i386
```

Now that you have qemu installed on your computer, you can proceed with running the system.

To run the system satisfactorily, you must provide at least 32 MB of RAM for the virtual machine. This is due to Hexagon's memory management architecture, which requires 16 MB of RAM dedicated to the kernel and at least 16 MB for allocating applications, utilities and open files. Hexagon doesn't allow less than that to run. If more memory is provided, the additional memory will always be reserved, with priority, to be made available to user processes. Normally, the command line below fulfills all requirements for running the system:

```
qemu-system-i386 -hda andromeda.img -m 32 -soundhw pcspk --enable-kvm -serial file:"Serial.txt"
qemu-system-i386 -hda hexagonix.img -m 32 -soundhw pcspk --enable-kvm -serial file:"Serial.txt"
```

You can omit the -serial parameter if you want. This parameter ensures that debug output from Hexagon and applications will be directed to a file on your computer, where you can see what was sent. You can also omit the -soundhw parameter, which is responsible for directing the sound output from the virtual system to your physical computer. On some Linux systems, the above configuration may conflict with pulseaudio, and may be omitted. Remember that by omitting the parameter, system sounds will not be output to you.

Remembering that you must use a version/edition of qemu that can run software written for the x86 architecture (i386 or x86_64). To download and install qemu, click [here](https://www.qemu.org/download/).

</div>

</details>

<details title="For testing on physical machine" align='left'>
<br>
<summary align='left'><strong>4️⃣ For testing on physical machine</strong></summary>

<div align="justify">

You must use Linux/macOS or some tool available for Windows that allows you to burn this image to disk.

On Linux/macOS/Unix, use the line below:

```
dd if=andromeda.img of=/dev/device
```
where drive is the desired device (usually sdb or sdc for USB devices and hda, hdb, sda or sdb for hard/solid state drives). Restart your computer and test the system. It is worth remembering that secure boot mode is not supported, and booting is only supported in BIOS or UEFI legacy BIOS mode.

Note that system performance may vary depending on the machine being tested. Added to this is the fact that the latest versions of the system have not been or are being tested directly on the physical machine, as the main operating system. If any problem occurs when running Hexagonix/Andromeda on a physical machine, please report the detailed error [here](https://github.com/hexagonix/Distro/issues), in Portuguese or English, informing data such as device, processor, amount of RAM memory, video card (if available) and peripherals connected, as well as the device used to install the system (internal disk drive or USB removable media).

</div>

</details>

<details title="First use and login" align='left'>
<br>
<summary align='left'><strong>5️⃣ First use and login</strong></summary>

<div align="justify">

When starting the system, you will be asked to enter a username and password. For the first use, use

```
User: root
Password: root
```

You can add another user by changing the 'USUARIO.UNX' file at the root of the disk. Remember not to remove the root user. This can make the system permanently inoperable.

</div>

</details>

<details title="Report errors" align='left'>
<br>
<summary align='left'><strong>6️⃣ Report errors</strong></summary>

<div align="justify">

You can report bugs and help develop the system. To do this, open an error notification [here](https://github.com/hexagonix/Distro/issues), informing the error as detailed as possible (such as device brand, processor, amount of RAM, video and connected peripherals, as well as the device used to install the system, such as an internal disk drive or USB removable media). Remember to inform which application the error occurred in, if the error occurs while the system is already in operation. If the problem occurs in the boot process, please report what was displayed/observed behavior of the machine.

</div>

</details>

<hr>

# Screenshots

<details title="Hexagonix" align='left'>
<br>
<summary align='left'><strong>🌙 Hexagonix</strong></summary>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix1.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix2.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix3.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix4.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Hexagonix5.png" width="500" height="400">
</p>

You can see more [here](https://github.com/hexagonix/Distro/tree/main/Img).

</details>

<details title="Andromeda" align='left'>
<br>
<summary align='left'><strong>🌌 Andromeda</strong></summary>

<p align="center">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Andromeda1.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Andromeda2.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Andromeda3.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Andromeda4.png" width="500" height="400">
<img src="https://github.com/hexagonix/Doc/blob/main/Img/Andromeda5.png" width="500" height="400">
</p>

You can see more [here](https://github.com/hexagonix/Distro/tree/main/Img).

</details>

<hr>

# More information

<details title="Languages" align='left'>
<br>
<summary align='left'><strong>🎲 Languages</strong></summary>

<div align="justify">

At this moment, both the system and the documentation are only available in Portuguese. Documentation in English will be available soon, and there are plans to support English as a primary language possibility in addition to Portuguese (most of the work has already been done).

</div>

</details>

<details title="Inspiration" align='left'>
<br>
<summary align='left'><strong>🖼 Inspiration</strong></summary>

<div align="justify">

This project was possible and today it can see the light of day thanks to many others who have come before and contributed with design ideas and teaching how an operating system should work, even if simple. Are they:

* MS-DOS, with code available [here](https://github.com/microsoft/MS-DOS)
* [MikeOS](http://mikeos.sourceforge.net/)
* [Linux 0.1.1](https://kernel.org)
* [FreeBSD](https://www.freebsd.org/)
* [XNU/Darwin](https://github.com/apple/darwin-xnu)
* [LittleKernel](https://github.com/littlekernel/lk)
* [Google Fuchsia](https://fuchsia.googlesource.com/fuchsia/)

In addition, other projects helped in the development of Hexagonix/Andromeda, either by providing new design ideas that go beyond the traditional organization of an operating system, or providing well-documented code that allowed us to understand how certain parts of an operating system work, or contributing with code examples that later came to inspire functions that were written exclusively for Hexagonix/Andromeda (mainly the kernel code). Despite not having been copied and reused in the system, the code of these public domain projects made it possible to understand the operation of certain computer components, opening doors for copyright codes to be written based on the study of the code of these projects. It is worth mentioning that the projects below were released by their original developers in the public domain. Are they:

* [Snowdrop OS](http://www.sebastianmihai.com/snowdrop/), which inspired me to write various hardware control routines and other 16-bit functions available in HBoot. The code for this system has been released into the public domain.
* [Alotware](https://github.com/0x5CE/alotware), which inspired me to create the process management functions in early versions of Hexagon (now rewritten numerous times). The code for this system has been released into the public domain.

</div>

</details>

<details title="Author, contributors and contact" align='left'>
<br>
<summary align='left'><strong>👥️️ Author, contributors and contact</strong></summary>

## Author

* [Felipe Miguel Nery Lunkes](https://github.com/felipeldev)

Feel free to contact me, report bugs or be interested in participating in the project.

## Contributors

* [Felipe Miguel Nery Lunkes](https://github.com/felipeldev)

## Email

* hexagonixdev@gmail.com (PT/EN)

## Social networks

* [Felipe Miguel Nery Lunkes](https://twitter.com/felipeldev) (Twitter)

</details>

<details title="Copyright notes" align='left'>
<br>
<summary align='left'><strong>📝 Copyright notes</strong></summary>

<div align="justify">

Hexagonix/Andromeda was developed from scratch by [Felipe Lunkes](https://github.com/felipenlunkes).

Read the license for more information on copyright, code ownership, and redistribution that apply only to files available in this repository (does not apply to the set of data and source code files that make up Hexagonix/Andromeda) . It is worth mentioning that the code of the system components is being released little by little and that each package can be released with a different license. Always keep an eye on the 'LICENSE' file available in each repository to be aware of legal rights and obligations.

</div>

</details>

[^1]: You can get more information about the relationship between Darwin and macOS [here](https://en.wikipedia.org/wiki/Darwin_(operating_system)).
[^2]: You can find the project page [here](https://www.freedos.org/).
[^3]: Booting in DOS mode was possible after searching the FreeDOS documentation, especially the "SYS.C" file (which can be found [here](http://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/dos/sys/2043/), which indicates in which thread the kernel expects to be loaded and which parameters are needed to boot the kernel correctly. Each DOS system has a preferred loading segment and this loading of other DOS editions can be implemented in the future with the help of new HBoot modules (in case of a proprietary system, the user must have a license). All the code for loading the kernel was developed from scratch and not based on any existing ones (the implementation of HBoot and modDOS, which are original, is done in Assembly, while the FreeDOS loading code is developed in C). The original implementation of modDOS code and other modules for DOS systems also frees the implementation from any legal problems.
[^4]: HBoot's DOS compatibility mode (modDOS) boot can be useful for running volume error checking, volume defrag, partitioning and other diagnostic as well as development tools such as compilers and assemblers which are not supported by Hexagonix/Andromeda (the 16-bit tools for example).
[^5]: Supports open(), close(), read() and write() calls, at least in concept. System calls are always BSD-style, with a function number on the stack and parameters in the registers.
