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

<img height="200" src="https://github.com/hexagonix/Doc/blob/main/Img/HX.png">

</div>

</div align="justify">

In this document you can get more information about the `hx` utility and its participation in the construction of Hexagonix.

## HX

The `hx` utility is intended to unify the Hexagonix build process on Linux systems (WSL2 is also supported on Windows 10 and Windows 11). The tool is responsible for building each component individually and, on success for all critical components such as `HBoot`, `Hexagon` and Unix-like utilities, creating the bootable system image (used for running the system on `qemu` and for installation on a physical hard disk), as well as a `.vdi` virtual hard disk for easy use in virtualizers like VirtualBox. It was written to be easily adaptable and extensible. hx also uses modules, written as shell scripts, to build system components, using a well-established parameter passing protocol.

> Although this is a Linux system, running hx is `not compatible` with Chrome OS in the Linux development environment. This is due to the unavailability of mounting loop devices (/dev/loop). The same limitation prevents hx from running on BSD systems (such as FreeBSD, NetBSD and OpenBSD) in addition to macOS. For the latter case, a new implementation of hx is being developed, to make the disk image building process compatible with BSD and derivative systems, such as macOS.

### Functions and parameters

`hx` accepts a series of parameters to determine which actions to perform. Below is a list of available actions and their parameters.

> **Warning! Compatible with version 14.0 or later.**

| Parameter | Action performed | Required secondary parameters |
|:---------:|:----------------:|:-----------------------------:|
| `-h`| Displays help with commonly used key parameters | Use `-v` for virtualization information and `-i`: image building information|
| `-v`| Start an instance of `qemu` with the last generated disk image| Use `hx -h -v` to get all possible parameters.|
| `-i`| Builds the system components and creates a disk image (raw) and .vdi | Use `hx -h -i` to get all possible parameters.|
| `-b` | Build individual Hexagonix components| `hexagon`: builds the Hexagon; `HBoot`: builds HBoot; `saturno`: builds Saturno; `unixland`: build Unix-like utilities; `andromedaland`: builds the Hexagonix-Andromeda utilities; `hx`: builds all components but does not generate a disk image|
| `-u`| Updates all local repositories with the server, using the already defined branch | No secondary parameters|
| `-ui`| It only updates disk images with the server. The rest of the repositories are unaffected | No secondary parameters|
| `-br`| Display branch in use for all repositories | No secondary parameters|
| `-un`| Change the branch and synchronize all repositories with the given branch| Desired branch|
| `-m`| Clones all repositories locally and prepares the build tools. Useful for generating a new build directory| No secondary parameters|
| `-c`| Clears all temp files created during a system build| No secondary parameters|
| `--version`| Displays version and copyright information| No secondary parameters|
| `--depend`| Install build dependencies (Debian, Ubuntu and derivative systems only)| No secondary parameters|
| `--info`| Displays Hexagonix information such as version, revision, development branch, etc| No secondary parameters|
| `--indent`| Starts an hx utility that formats and optimizes Hexagonix source code, manuals, and definition files | No secondary parameters|
| `--configure`| Runs the `configure.sh` module to generate the static files needed for the build| No secondary parameters|
| `--stat`| Displays statistical information about Hexagonix (cloc installed required)| No secondary parameters|

### hx modules

To build Hexagonix, hx looks for and executes a series of modules scattered throughout the source tree. Are they:

> **Warning! Compatible with version 14.0 or later.**

| Module | Function | Location |
|:------:|:--------:|:--------:|
|`configure.sh`| Generate all static files and check dependencies for building Hexagonix| Root dir|
|`andromeda.hx`| Responsible for building all Hexagonix-Andromeda utilities (graphics utilities) | Scripts/modules|
|`buildInfo.hx`| Get and display information about Hexagonix version, development channel and build| Scripts/modules|
|`buildOnBSD.hx`| Responsible for creating a disk image for installing Hexagonix from a BSD system| Scripts/modules|
|`buildOnLinux.hx`| Responsible for creating a disk image for installing Hexagonix from a Linux distribution| Scripts/modules|
|`buildOnUNIX.hx`| Responsible for creating a disk image for installing Hexagonix from a UNIX system (OpenIndiana)| Scripts/modules|
|`common.hx`| Common functions used by all modules| Scripts/modules|
|`contribBuilder.hx`| Responsible for executing the construction script for external packages (such as fasm)| Scripts/modules|
|`contribChecker.hx`| Responsible for checking whether there are external packages built for installation in the system image| Scripts/modules|
|`diskBuilder.hx`| Responsible for installing Hexagonix components on the previously generated system disk image| Scripts/modules|
|`fonts.hx`| Responsible for identifying and building graphic fonts compatible with the system| Scripts/modules|
|`git.hx`| Responsible for updating system repositories with the remote server| Scripts/modules|
|`hboot.hx`| Responsible for executing the construction of the `HBoot` component (boot)| Scripts/modules|
|`hexagon.hx`| Responsible for executing the construction of the `Hexagon` component (kernel)| Scripts/modules|
|`logUtils.hx`| Useful functions for creating system build logs| Scripts/modules|
|`macros.hx`| Useful functions for running modules from the `hx` utility or other modules| Scripts/modules|
|`saturno.hx`| Responsible for executing the construction of the `Saturno` component (boot)| Scripts/modules|
|`systemBuilder.hx`| Runs all system component build modules and installs them in a temporary directory[^1]| Scripts/modules|
|`unix.hx`| Responsible for building all Unix utilities| Scripts/modules|
|`vm.hx`| Allows the configuration and execution of virtual machines using a generated disk image| Scripts/modules|
|`indent.sh`| Responsible for indenting system fonts| Scripts/|

[^1]: To find out which modules are executed, see the module contents.

> `Only configure.sh can be run directly by the user, other than hx`. No other modules should be run directly.

### Build steps

`hx`, **together with the modules already mentioned**, builds the components in the following order:

* MBR, Saturno (first stage) and HBoot boot loader;
* Hexagon (kernel);
* Unix-like utilities;
* Andromeda utilities;
* External packages, by another authors (contrib);
* Graphic fonts.

If the critical components (with the exception of external packages) have been successfully built, hx starts building the disk images, being a disk image (raw), used to run the system in qemu and install on physical devices, and a VDI, used for VirtualBox. Both images are moved to `hexagonix/` if the process completes successfully. In addition, hx generates a log file, `log.log`, with information on the system version, revision, branch, hx version, operating system used for construction, date and time, among others, which can be used to identify errors in the process, as well as to identify the exact build of the system. The log file is also moved to `hexagonix/`.

</div>
