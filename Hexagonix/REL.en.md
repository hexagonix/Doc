<!-- Let's add the system logo -->

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

<!-- Will act as an <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

<table align="center">
<tr>
<td><a href="/Hexagonix/Hexagonix.en.md">About Hexagonix</a></td>
<td><a href="/Hexagonix/Hexagonix.en.md#screenshots">Screenshots</a></td>
<td><a href="/Hexagonix/Hexagonix.en.md#contribute-and-report-bugs">Contribute</a></td>
<td><a href="https://github.com/hexagonix/src">Source code</a></td>
<td><a href="/Hexagonix/README.en.md">Download</a></td>
</tr>
</table>

# Hexagonix Releases

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

Here you can find more information about all versions of Hexagonix released so far. The information is available both for the stable versions of the system and for the development versions, called `projects`.

> You can get the latest `stable` versions of Hexagonix by visiting the [releases](https://github.com/hexagonix/hexagonix/releases) area.

</div>

<!-- Will act as an <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Supported versions

Here you will find more information about the supported versions of Hexagonix, as well as more information about them and the official changelog.

<details title="Hexagonix 1.0" align='left'>
<summary align='left'><strong>Hexagonix 1.0</strong></summary>
<br>

<div align="justify">

Hexagonix 1.0 is the most complete and polished version to date, and it is also the first fully functional version of the system. See below for more information about this release.

> Previous versions, marked with the pattern `Hexagonix H*` or `Hexagonix v1.*`-`Hexagon v*.*`, dated before the release of this version, should be considered legacy, non-functional and unsupported versions.

**Release date**: 15/08/2026<br>

- **Hexagon kernel** v1.7.0:
  - This version introduces preemptive multitasking to the kernel:
    - New process model, with states (*IDLE*, *BLOCKED*, *RUNNING*, *READY*, *ZOMBIE*) that mark the life cycle of a process;
    - Per-process environment variables: each process can set and retrieve environment variables, which are inherited from the process that created it;
    - Non-reusable PIDs: now each process has a unique PID;
    - Each process has its own *stack*;
    - New spawn() and kill() calls to start processes in a non-blocking way and to kill a process by its PID;
    - New round-robin scheduler, which ensures that all loaded processes get their execution time and are preemptively interrupted/resumed;
    - Use of malloc() and free() to allocate any process structure under the kernel's responsibility;
    - Moves data that used to be loaded at fixed addresses in kernel memory to areas allocated via malloc(), ensuring greater stability;
  - General rewrite of FAT16 file system management:
    - Ability to create and remove directories;
    - Ability to change the current directory via a system call;
    - New relative and absolute path functionality:
      - Files can be identified by a full path, such as `/bin/ls`;
      - Files can be identified by a relative path, by entering a directory and looking for `ls`;
    - Exposure of the touch() call, responsible for creating an empty file;
  - Various bug fixes in process management;
  - Critical bug fixes in memory management:
    - Fix where the value of a pointer was being used when it should actually have been dereferenced, causing corruption in the process table;
    - Makes malloc() and free() safe in a multitasking environment, where interrupts and preemption occur;
  - Change to the timer handler, to trigger preemption;
  - Fixes a number of race conditions that were identified during the implementation of preemptive multitasking.
  - Deeply changes the system's user management:
    - The system no longer stores the name of the logged-in user; this is now handled by the userland, as done in systems like UNIX V7 and Linux;
    - The `root` user identifier becomes 0, as in UNIX v7 or Linux. Until then, the fixed identifier 777 was used;
    - Each non-administrative user now has its own identifier, starting at 1. This is managed in the userland. Until then, they all had the fixed identifier 555;
  - Reorganization of all system calls:
    - Grouping of related calls;
    - Breaking of the contract with all applications created for previous versions; all utilities need to be assembled/compiled again, with the latest version of `libasm`. Although the call numbering has changed, the contract of each of them remains the same;
  - This version has been in the works for the last 14 months.
- **HBoot** v0.22.0:
  - Removes the term Hexagon Boot in favor of HBoot;
- **Unix utilities** (UnixUtils and CoreUtils):
  - New `mkdir` utility:
    - Responsible for creating new directories on the volume;
  - New `rmdir` utility:
    - Responsible for removing a directory, if empty;
  - New `kill` utility:
    - Responsible for killing a process with the given PID;
  - New `adduser` utility:
    - Responsible for creating a new user in the system, referencing the user database at `/etc/shadow`;
    - Incrementing the user identifier and creating the record;
    - Uses password hashing to prevent password exposure;
  - New `deluser` utility:
    - Responsible for removing a user from the system, as long as they are not currently logged in;
  - New `passwd` utility:
    - Responsible for changing a user's password.
  - New `touch` utility:
    - Responsible for creating an empty file on the volume;
  - New `clock` utility:
    - This new utility should be run in the background, with `clock &`. It is also started with the system by default;
    - It shows the time in the top-right corner of the screen, updating constantly with second-level granularity;
  - Changes to the environment shells (`sh` and `hash`):
    - New shell-script execution feature; scripts must have a `#!/bin/sh` identifier (or the name of another compatible shell) at the start of the file;
    - New command to get and set environment variables;
    - Executable lookup using the `PATH` variable, allowing the execution of utilities that are segregated in other system directories;
    - Ability to start a process in the background, using `&`:
      - For example, starting the `clock` utility in the background: `clock &`;
  - Changes to the `man` utility:
    - It now pauses when the manual is larger than the available screen space, asking for user interaction;
    - Exiting the utility is now prompted to the user, following the Linux tool used as a usability reference;
    - Manual lookup changed from `/` to `/usr/share/man`, improving organization and segregating the files into a dedicated, standardized directory;
  - Changes to the `cowsay` utility:
    - Character-file lookup changed from `/` to `/usr/share/cowsay`, improving organization and segregating the files into a dedicated, standardized directory;
  - New version of the `init` utility:
    - Startup script changed from `/rc` to `/etc/rc`;
    - Ability to start and monitor processes, using the `respawn=` expression. Ability to monitor 8 processes;
    - Ability to start unmonitored processes, with the `spawn=` expression;
    - Ability to define environment variables that will be inherited by all processes, such as `PATH`;
  - New version of the `login` utility:
    - Now uses the new user database format, for example: `user:password:user_id:default_shell:theme`;
    - User database moved from `/passwd` to `etc/shadow`. Renamed to avoid conflicting with the new `passwd` utility;
    - `login` no longer monitors the shell. It starts the logged-in user's shell in a blocking manner and, when the shell exits, it exits as well, returning control to init, which holds the respawn rule;
  - New version of the `logind` utility:
    - Now uses the new user database format, for example: `user:password:user_id:default_shell:theme`;
    - User database moved from `/passwd` to `etc/shadow`. Renamed to avoid conflicting with the new `passwd` utility;
  - New version of the `su` utility:
    - Now uses the new user database format, for example: `user:password:user_id:default_shell:theme`;
    - User database moved from `/passwd` to `etc/shadow`. Renamed to avoid conflicting with the new `passwd` utility;
  - New version of the `top` utility:
    - Compatibility with the new process model, showing the full process path and its state;
    - Displays, for each process, the parent process (*PARENT*), which can be a user process or the kernel;
    - Fix in data formatting, standardizing the columns;
  - New version of the `ps` utility:
    - Compatibility with the new process model, showing the full process path and its state;
    - Displays, for each process, the parent process (*PARENT*), which can be a user process or the kernel;
  - New multitasking test utilities, as a proof of concept:
    - `testa`, `testb`, `workera`, `workerb` and `launcher`;
  - New organization of the utilities:
    - `init`, `login`, `logind`, `shutdown`, `ps`, `top`, `passwd`, `adduser` and `deluser` were moved to `/sbin`;
    - The remaining utilities were moved from `/` to `/bin`;
  - General bug fixes;
  - Improvements to the structure of the utilities;
  - Minimum Hexagon version changed to v1.7.0 for all utilities;
- **Andromeda Apps (Hexagonix-Andromeda environment)**:
  - Changes to the environment shells (`ash` and `dossh`):
    - New shell-script execution feature; scripts must have a `#!/bin/ash` identifier (or the name of another compatible shell) at the start of the file;
    - New command to get and set environment variables;
    - Executable lookup using the `PATH` variable, allowing the execution of utilities that are segregated in other system directories;
    - Ability to start a process in the background, using `&`:
      - For example, starting the `clock` utility in the background: `clock &`;
  - Changes to the `config` utility:
    - Adaptation of configuration files to use the `/etc` directory as their base directory;
    - Changes to some of the utility's messages;
  - Moves utilities from `/` to `/bin`;
- **Scripts**:
  - Renames the `indent.sh` utility to `formatter.sh`;
  - Changes the modules used to build the image:
    - `init`, `login`, `logind`, `shutdown`, `ps`, `top`, `passwd`, `adduser` and `deluser` were moved to `/sbin`;
    - The remaining utilities were moved from `/` to `/bin`;
  - Changes the VirtualBox image identifier to a fixed one, allowing updated images to be used in VirtualBox without needing to change settings or remove/add a new image;
  - Changes the virtual machine memory, via qemu, from 32 to 64 MB.

</div>

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

</details>

## Development versions

Development versions of Hexagonix are named `projects` and may lead to new release versions of the system. These releases introduce components and features that may be unstable or unfinished, and are initially research on design and implementation of Hexagonix. Development versions, called projects, may or may not be the source of a new release version of Hexagonix, although features may or may not be fully implemented in a release version. `Regardless, Hexagonix is ​​continually being developed`.

<details title="Dormin Project" align='left'>
<summary align='left'><strong>Dormin Project (2025-2026) - completed project</strong></summary>
<br>

<div align="justify">

> The `Dormin` project starts from the code base of the **System II** version of Hexagonix, after the official release of that version.

The project begins aiming at the continuous improvement of the system, adding preemptive multitasking and other improvements.

</div>

</details>

<details title="Zonai Project" align='left'>
<summary align='left'><strong>Project Zonai (2024-2025) - project completed</strong></summary>
<br>

<div align="justify">

> The `Zonai` project starts with the code base of the **System I** version of Hexagonix, after the official release of the version.

The Zonai project is a fork of Hexagonix System I (CURRENT branch), which aims to develop the next stable release of the system, the System II version (no defined release schedule - the release may not occur in 2024). More information about the project soon.

</div>

</details>

<details title="Project Raava" align='left'>
<summary align='left'><strong>Project Raava (2023-2024) - project completed</strong></summary>
<br>

<div align="justify">

> The Project Raava was completed, giving rise to the **Hexagonix System I**.

Project Raava is a fork of Hexagonix H2 Release 2 (CURRENT branch), which aims to develop the next stable release of the system, version H3 (no release schedule defined - the release may not occur in 2023). For this, the system starts from:

- Hexagon based in early v1.3.6 (version 1.3 revision 6);
- Base of Hexagonix H2 Release 2 (H2R2): H2-CURRENT+290320231532;
- Hexagon v1.3.7 (version 1.3 revision 7) - 05/20/2023;

</div>

</details>

## Discontinued versions (unsupported)

<div align="justify">

Below you can have direct access to some versions that have become milestones in the distribution of the system and have access to summary information about them. These versions have been discontinued and no longer receive updates and fixes.

> :construction: Warning! On 10/19/2023, the Hexagon version was downgraded to v1.0.0. This is due to several system stability improvements and corrections, in addition to the correction of several bugs during the system's execution. Furthermore, new features added showed that the kernel is now approaching maturity and stability. Therefore, just like the H1 version, the next edition of the system will be released with a v1.0.0 kernel. Therefore, all kernel versions reported below refer to a numbering prior to the version rescheduling, and do not relate to current versions of Hexagon.

</div>

<details title="Discontinued versions of Hexagonix" align='left'>
<summary align='left'>Discontinued versions of Hexagonix</summary>
<br>

<details title="Hexagonix System I" align='left'>
<summary align='left'><strong>Hexagonix System I</strong></summary>
<br>

<div align="justify">

> :construction: Warning! On 10/19/2023, the Hexagon version was changed to v1.0.0. This is due to several system stability improvements and fixes, in addition to the correction of several bugs during the system's execution. Thus, like older discontinued versions, Hexagonix System I was released with a v1.0.0 kernel.

Hexagonix System I is the most stable and feature-rich version released to date. Several improvements were made to ensure stability, security and lower resource usage. Below, more technical information about this version of the system.

Changelog and technical information for this version:

**Release date**: 06/05/2024<br>
**Version buildId**: 36a232aa-d2d2-4e78-99d8-f8df3c96b0d6<br>
**Commit**: [6809c99](https://github.com/hexagonix/hexagonix/commit/6809c99865610087f6f786641cbfa5e663cd4f02)

- **Saturno** v1.9.0:
  - Saturno MBR v1.2.0:
    - Corrections of various errors;
    - Comments and source code completely in English, facilitating collaboration.
  - Comments and source code completely in English, facilitating collaboration.
- **HBoot** v0.11.0:
  - Corrections of various errors;
  - Improvements in hardware detection;
  - Messages, logs, comments and source code completely in English, facilitating collaboration.
- **Hexagon Kernel** v1.0.0:
  - Various stability and performance refinements;
  - Lower resource consumption;
  - Improved error management;
  - General bug fixes;
  - New system calls;
  - Standardization of system calls, using UNIX Version 7 as a reference;
  - Messages, logs, comments and source code completely in English, facilitating collaboration.
- **UnixUtils** and **CoreUtils** v9.0:
  - New `mv` utility, used to rename files on the volume;
  - Improved error handling;
  - Lower resource consumption;
  - Correction of several bugs;
  - Improvements in user messages, including spelling and formatting corrections;
  - Messages, logs, comments and source code completely in English, facilitating collaboration.
- **Andromeda Apps (Hexagonix-Andromeda environment)**:
  - Update of development libraries;
  - Corrections of various errors;
  - Improvements in resource consumption;
  - `poweroff` utility renamed to `power`.
  - Messages, logs, comments and source code completely in English, facilitating collaboration.
- **libasm** v2.2.1:
  - Standardization of all development libraries;
  - Standardization of system calls, using UNIX Version 7 and Hexagon v1.0.0 as references;
  - Improvements in comments, making the code better understood;
  - Improvements to the libasm example utilities (tapp.asm and gapp.asm);
  - Comments and source code completely in English, facilitating collaboration.
- **fasm (flat assembler)** v1.73.32:
  - Standardization of the code responsible for compatibility with Hexagonix;
  - Update of the code responsible for compatibility with Hexagonix to use the latest libraries;
  - Improvements to the base version of fasm v1.73.32;
  - Comments and source code of Hexagonix-dependent code translated into English, facilitating collaboration.
- **Scripts** (Hexagonix build tools):
  - Correction of several errors;
  - Improvements in user messages, including spelling and formatting corrections;
  - New virtualized system execution options;
  - Compatibility of the `hx` utility with the latest versions of `qemu`;
  - Start of build compatibility on BSD systems;
  - Compatibility with BSD systems to run Hexagonix in a virtualized environment (qemu);
  - Improvements in the code formatting module;
  - Improvements in Unix and Andromeda utility construction modules (Hexagonix-Andromeda);
  - Improvements in the system construction log;
  - Improvements to the build configuration module (`configure.sh`);
  - Refactoring of the hx utility to reuse code and remove duplicate code;
  - New parameters and functions available in the hx utility:
    - "--flags": displays information about the build parameters sent to the assembler.
  - Comments and source code of Hexagonix-dependent code translated into English, facilitating collaboration.
- **Documentation**:
  - Update of system documentation in the repository available on GitHub;
  - Refinements in Portuguese texts and English translations;
  - Copyright corrections and contact and development information;
  - Update of system call tables;
  - Update of Unix and Andromeda utilities.

</div>

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

</details>

<details title="Hexagonix H1" align='left'>
<summary align='left'><strong>Hexagonix H1</strong></summary>
<br>

<div align="justify">

This is the first extensively tested and marked stable version of the system. Hexagonix H1 is also the basis of Andromeda H1. Many improvements have been made since previous versions of the system, which used series of numbers to identify versions. Version 1.2-beta, in the previous numbering, was improved and served as the basis for the development of the most stable version to date, the H1 version, the public release of the system. You can get that release [here](https://github.com/hexagonix/hexagonix/releases/tag/H1). This version will continue to be improved and changes will be made available continuously.
  
</div>

<details title="Hexagonix H1 R1 (Caladan)" align='left'>
<summary align='left'>Hexagonix H1 R1 (Caladan)</summary>
<br>

<div align="justify">

Hexagonix H1 R1 (codenamed Caladan) is the first patch pack for the H1 version of Hexagonix. Many improvements have been made to various Hexagonix Unix utilities, as well as enhancements and fixes have been made to the Andromeda userland. Hexagon has been updated to version 9.3, with many bug fixes, stability improvements, increased performance and smaller memory footprint, as well as fixed support for PS/2 mice (and USB via PS/2 emulation) and other devices. Go to the [releases](https://github.com/hexagonix/hexagonix/releases) area and look for version H1 R1.

</div>

</details>

<details title="Hexagonix H1 R2 (Caladan)" align='left'>
<summary align='left'>Hexagonix H1 R2 (Caladan)</summary>
<br>

<div align="justify">

Second update package for Hexagonix/Andromeda version H1, which includes:

- Kernel Hexagon v9.4A;
- Improvements in several Hexagonix utilities;
- Improvements in various Andromeda utilities;

Several runtime failures have been identified in various Andromeda utilities that have been fixed in this release. Updates were also added to Hexagon, decreasing memory pressure and targeting errors identified during system execution. The system manuals have also been updated, as well as the naming used in a number of utilities. From now on, the next H1 version update will focus on improvements and adding new features. Go to the [releases](https://github.com/hexagonix/hexagonix/releases) area and look for the H1 R2 version.

</div>

</details>

<details title="Hexagonix H1 R3 (Duna)" align='left'>
<summary align='left'>Hexagonix H1 R3 (Duna)</summary>
<br>

<div align="justify">

Final release of the H1 version of the system. This is the release analogous to a 1.0 version of the software. To this end, the internal version numbers of several system components have been changed to celebrate this milestone. Hexagon now identifies itself as in version 1.0, as well as other components. The version has been extensively tested and is stable. The H1 R3 release includes:

- Kernel Hexagon v1.0;
- General fixes in various Hexagonix and Andromeda utilities;
- Improvements in the system libraries;
- Stability fixes in various utilities;
- Improvements in Andromeda Settings;

Go to the [releases](https://github.com/hexagonix/hexagonix/releases) area and look for version H1 R3.

</div>

</details>

<details title="Hexagonix H1 R4 (Vega)" align='left'>
<summary align='left'>Hexagonix H1 R4 (Vega)</summary>
<br>

<div align="justify">

System-wide bug fixes and improvements.

- General fixes in various Hexagonix and Andromeda utilities;
- Improvements in the system libraries;
- Stability fixes in various utilities;
- Improvements in Andromeda Settings;

</div>

</details>

<details title="Hexagonix H1 R5 (Orion)" align='left'>
<summary align='left'>Hexagonix H1 R5 (Orion)</summary>
<br>

<div align="justify">

This system update fixes several bugs in the system, including issues encountered when booting on physical machines and in virtualized environments on HBoot and Hexagon.

- Kernel Hexagon v1.1;
- General fixes in various Hexagonix and Andromeda utilities;
- Improvements in the system libraries;
- Stability fixes in various utilities;
- Improvements in Andromeda Settings;

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

</details>

<details title="Hexagonix H2" align='left'>
<summary align='left'><strong>Hexagonix H2</strong></summary>
<br>

<details title="Hexagonix H2 (development versions)" align='left'>
<summary align='left'>Hexagonix H2 (development versions)</summary>
<br>

<details title="Hexagonix H2-dev.beta1" align='left'>
<summary align='left'>Hexagonix H2-dev.beta1</summary>
<br>

<div align="justify">

The development version, H2 (codenamed Vita Nova), is the next version of Hexagonix. So far, the changes and improvements over the Hexagonix H1-R6 are:

- Kernel Hexagon v1.1.2;
- Fusion of the Hexagonix and Andromeda distributions into a single distribution;
- Removal of file extension for system binaries;
- Adding license terms to the system image;
- Improvements in Unix utilities and Hexagonix-Andromeda apps (old Andromeda applications);
- Hexagon Boot v0.3 (incompatible with H1 version).

</div>

</details>

<details title="Hexagonix H2-dev.beta4" align='left'>
<summary align='left'>Hexagonix H2-dev.beta4</summary>
<br>

<div align="justify">

- Deep change in the Unix atop utility;
- Renamed atop to htop;
- Improvements in the logind daemon;
- Font hint renamed to Avatar;
- Removal of Unix.sh file from libasm;
- Constants from Unix.s moved to Unix man utility.

</div>

</details>

<details title="Hexagonix H2-dev.beta5" align='left'>
<summary align='left'>Hexagonix H2-dev.beta5</summary>
<br>

<div align="justify">

- Hexagon emergency fix (v1.1.7) due to memory leak issues when requesting device restart (affects versions H2-dev.beta1 to H2-dev.beta4);
- init v2.0, with support to run multiple services in list.
- Disabling "modern" login mode in logind. The default login interface follows that seen on Unix-like systems (FreeBSD as a major inspiration);
- General improvements to the following Unix utilities:
  - [x] login;
  - [x] energia;
  - [x] htop;
  - [x] man;
  - [x] su;
  - [x] top;
  - [x] uname;
- Tests run to verify proper system operation (no new issues found).

</div>

</details>

<details title="Hexagonix H2-dev.beta6" align='left'>
<summary align='left'>Hexagonix H2-dev.beta6</summary>
<br>

<div align="justify">

The H2-dev.beta6 version came to standardize a number of Hexagonix services, as well as enforce compliance in system fonts and comments. Most of the changes in this version are not visible to the user, but they are important to guarantee the stability of the system. See the most important changes:

* Improvements in the messages of the system utilities, especially in error messages;
* Fixes in the following system utilities:
  - [x] DOSsh;
  - [x] init;
  - [x] su;
  - [x] login;
* A definition error in su could lead to the utility crashing or not working, since it would try to load the default shell (sh) with the name sh.app;
* Complete removal of references to Andromeda, since the distribution was merged into Hexagonix (see Hexagonix H2-dev.beta1). Removal took place at:
  - Name of functions;
  - Name of variables and constants;
  - Comments;
* Improved manual pages for all utilities;
* Improved Hexagon online documentation;
* Changed the version name from "Vita Nova" to "VitaNova", preventing issues when checking the hostname generated during system build;
* Changed the formatting of the init services declaration.

- [x] Release date: 28/11/2022 (dd/mm/yyyy)

</div>

</details>

<details title="Hexagonix H2-dev.beta7" align='left'>
<summary align='left'>Hexagonix H2-dev.beta7</summary>
<br>

<div align="justify">

Translation of messages from Unix utilities into English.

- [x] Release date: 30/11/2022 (dd/mm/yyyy)

</div>

</details>

<details title="Hexagonix H2-dev.beta8" align='left'>
<summary align='left'>Hexagonix H2-dev.beta8</summary>
<br>

<div align="justify">

* Messages from Andromeda-Hexagonix utilities and HBoot translated into English;
* Hexagon messages translated into English;

> It is worth noting that the names of functions, as well as comments in files that make up the system, will remain in Portuguese at this time.

- [x] Release date: 04/12/2022 (dd/mm/yyyy)

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

</details>

<details title="Hexagonix H2 Release 1" align='left'>
<summary align='left'>Hexagonix H2 Release 1</summary>
<br>

Consolidation of development versions, with:

- Hexagon kernel v1.2.5;
- HBoot v0.4 (incompatible with Hexagonix H1 to H1-R6);
- Fusion of Hexagonix and Andromeda distributions into a single distribution;
- File extension removal for system binaries;
- Addition of license terms in the system image;
- Improvements in Unix utilities and Hexagonix-Andromeda (old Andromeda applications);
- Deep change in the Unix atop utility;
- Renamed atop to htop;
- Improvements in the logind daemon;
- Font hint renamed to Avatar;
- Utility init v2.0, with support to run multiple services in list.
- Disabling "modern" login mode in logind. The default login interface follows that seen on Unix-like systems (FreeBSD as a major inspiration);
- General improvements to the following Unix utilities:
  - [x] login;
  - [x] energia;
  - [x] htop;
  - [x] man;
  - [x] su;
  - [x] top;
  - [x] uname;
- Improvements in the messages of the system utilities, especially in error messages;
- Fixes in the following system utilities:
  - [x] DOSsh;
  - [x] init;
  - [x] su;
  - [x] login;
- A definition error in su could lead to the utility crashing or not working, since it would try to load the default shell (sh) with the name sh.app;
- Complete removal of references to Andromeda, since the distribution was merged into Hexagonix (see Hexagonix H2-dev.beta1). Removal took place at:
  - Name of functions;
  - Name of variables and constants;
  - Comments;
- Improved manual pages for all utilities;
- Improved Hexagon online documentation;
- Changed the version name from "Vita Nova" to "VitaNova", preventing problems when checking the hostname generated during system construction;
- Change in the formatting of the init services declaration;
- Translation of messages from Unix utilities into English;
- Messages from Andromeda-Hexagonix and HBoot utilities translated into English;
- Hexagon messages translated into English.

- [x] Release date: 12/12/2022 (dd/mm/yyyy)

</details>

<details title="Hexagonix H2 Release 2" align='left'>
<summary align='left'>Hexagonix H2 Release 2</summary>
<br>

- Hexagon kernel v1.3.2;
- HBoot v0.7.1;
- Improvements in Unix utilities and Hexagonix-Andromeda applications;
- Improvements in the logind daemon;
- New first use experience (OOBE - Out of Box Experience);
- Avatar font renamed to Aurora;
- Improvements in the messages of the system utilities, especially in error messages;
- Improved manual pages for all utilities;
- Improved Hexagon online documentation, including system calls;
- Version name change from "VitaNova" to "Darwin";
- Change in the formatting of the init services declaration;
- Translation of messages from Unix utilities to English completed;
- Improvements in the settings utility (Config);
- Assembly development libraries version 0.10.1;

- [x] Release date: 28/02/2023 (dd/mm/yyyy)

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

</details>

</details>
