<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
</p>

<div align="center">

![](https://img.shields.io/github/license/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/stars/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues-closed/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues-pr/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/issues-pr-closed/hexagonix/Unix-Apps.svg)
![](https://img.shields.io/github/downloads/hexagonix/Unix-Apps/total.svg)
![](https://img.shields.io/github/release/hexagonix/Unix-Apps.svg)
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

# Hexagonix Unix-like utilities

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

<div align="justify">

One of the most important components of Hexagonix is ​​Unix-Apps. This package includes several utilities commonly found on Unix-like systems such as Linux, FreeBSD and macOS.

The syntax of the Hexagonix utilities follows that found in `Version 7 UNIX` and FreeBSD versions, although it does not strive for precision in implementation. Whenever you get stuck using one of the Unix utilities, enter `utility --help`, `utility ?` or `man utility`.

Below you will find the license for the use and distribution of Hexagonix's Unix utilities.

</div>

<details title="Hexagonix Unix-like utilities License" align='left'>
<summary align='left'>Hexagonix Unix-like utilities License</summary>
<br>

<div align="justify">

Please read the [license](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) for more information about copyright, code ownership, and redistribution that applies to files available in this repository. Hexagonix is ​​fully licensed under [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Always pay attention to the `LICENSE` file available in each repository to be aware of legal rights and obligations, as well as the list of project contributors.

</div>

</details>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Utilities included

<div align="justify">

Several Unix-standard utilities are included so far. Are they:

* adduser
* arch
* cat
* clear
* cowsay
* cp
* date
* deluser
* echo
* file
* free
* hostname
* init
* kill
* login
* ls
* man
* mount
* mv
* passwd
* ps
* rm
* sh
* shutdown
* su
* syslogd
* top
* uname
* whoami

Other utilities are exclusive to Hexagonix. Are they:

* clock (shows the current time in the console's top-right corner, refreshed every second. Meant to be run in the background, with `clock &`);
* fnt (changes the graphic font used by the console);
* hash (alternate shell);
* logind (daemon that manages the  virtual terminal;
* lshapp (reads and displays information from HAPP images);
* lshmod (reads and displays information from HBoot images).

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Login and user management infrastructure

<div align="justify">

Hexagonix keeps its user database in a single file, `/shadow`, with one record per line in the format `username:passwordhash:code:shell:theme`. The `code` field follows the same ID scheme used by Hexagon itself (555 for regular users, 777 for root), and the `theme` field ("dark" or "light") is applied by `login` right after a successful authentication, so each user can carry their own color preference instead of a single system-wide theme.

* `adduser`, restricted to the root user, interactively asks for the new username, the password (twice, with no echo on the console), the shell and the theme, then appends the corresponding record to `/shadow`, falling back to a default shell and theme if the user leaves those fields blank;
* `deluser` removes a user's record from `/shadow`, rewriting the file without the matching line;
* `passwd` lets the currently logged in user change their own password, updating only the hash field on their line;
* `login` and `su` both consult `/shadow` to validate the supplied credentials, and it is `login` that applies the authenticated user's theme;
* `logind` is the daemon responsible for setting up virtual terminal and manage other session settings.

Passwords are never written in plain text: the hash field in `/shadow` is computed from a DJB2 hash (implemented in `libasm` and shared by all of these utilities). It is worth noting that this is not a cryptographic hash, uses no salt, and is reversible by brute force, but it does keep the password from sitting in plain, readable text for anyone opening the file directly.

</div>
