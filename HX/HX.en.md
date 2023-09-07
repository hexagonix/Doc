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

hx accepts a series of parameters to determine which actions to perform. Below is a list of available actions and their parameters.

| Parameter | Action performed | Required secondary parameters |
|:---------:|:----------------:|:-----------------------------:|
| `-h`| Displays help with commonly used key parameters | No secondary parameters|
| `-v`| Start an instance of `qemu` with the last generated disk image| `hx`: (default, selected automatically in the absence of secondary parameters); `hx.som`: enable audio output device; `hx.serial`: do not redirect serial output to file; `bsd-hx`: allows execution in BSD environments (no KVM)|
| `-i`| Builds the system components and creates a disk image (raw) and .vdi | `hx`: (default, selected automatically in the absence of secondary parameters); `hx.test`: creates a test image with reduced final size |
| `-b` | Build individual Hexagonix components| `hexagon`: builds the Hexagon; `HBoot`: builds HBoot; `saturno`: builds Saturno; `unixland`: build Unix-like utilities; `andromedaland`: builds the Hexagonix-Andromeda utilities; `hx`: builds all components but does not generate a disk image|
| `-u`| Updates all local repositories with the server, using the already defined branch | No secondary parameters|
| `-ui`| It only updates disk images with the server. The rest of the repositories are unaffected | No secondary parameters|
| `-br`| Display branch in use for all repositories | No secondary parameters|
| `-un`| Change the branch and synchronize all repositories with the given branch| Desired branch|
| `-m`| Clones all repositories locally and prepares the build tools. Useful for generating a new build directory| No secondary parameters|
| `-c`| Clears all temp files created during a system build| No secondary parameters|
| `--ver`| Displays version and copyright information| No secondary parameters|
| `--depend`| Install build dependencies (Debian, Ubuntu and derivative systems only)| No secondary parameters|
| `--info`| Displays Hexagonix information such as version, revision, development branch, etc| No secondary parameters|
| `--indent`| Starts an hx utility that formats and optimizes Hexagonix source code, manuals, and definition files | No secondary parameters|
| `--configure`| Runs the `configure.sh` module to generate the static files needed for the build| No secondary parameters|
| `--stat`| Displays statistical information about Hexagonx (cloc installed required)| No secondary parameters|

### hx modules

To build Hexagonix, hx looks for and executes a series of modules scattered throughout the source tree. Are they:

| Module | Function | Location |
|:------:|:--------:|:--------:|
| `configure.sh` | Generate all static files and check dependencies for building Hexagonix| Root dir|
| `Unix.sh`| Individually build all Hexagonix Unix-like utilities | Apps/Unix/|
| `Apps.sh`| Individually Build All Hexagonix Graphics Utilities | Apps/Andromeda/|
| `Contrib.sh`| Build all components external to Hexagonix, such as assemblers (fasmX)| Contrib/|
| `fontes.sh` | Responsible for building Hexagonix graphic fonts | Fontes/|
| `indent.sh` | Responsible for indenting system fonts| Scripts/|

> `Only configure.sh can be run directly by the user, other than hx`. No other modules should be run directly.

### Build steps

hx, together with the modules already mentioned, builds the components in the following order:

* MBR and HBoot boot loader;
* Hexagon (kernel);
* Unix-like utilities;
* Andromeda utilities;
* External packages, by another authors (contrib);
* Graphic fonts.

If the critical components (with the exception of external packages) have been successfully built, hx starts building the disk images, being a disk image (raw), used to run the system in qemu and install on physical devices, and a VDI, used for VirtualBox. Both images are moved to `hexagonix/` if the process completes successfully. In addition, hx generates a log file, `log.log`, with information on the system version, revision, branch, hx version, operating system used for construction, date and time, among others, which can be used to identify errors in the process, as well as to identify the exact build of the system. The log file is also moved to `hexagonix/`.

</div>
