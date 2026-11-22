<p align="center">
<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/banner.png">
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
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

<div align="justify">


# Hexagon console terminal

Bytes written to a tty through `hx.write` go through a small terminal emulator in the kernel, the role `teken` plays in the `vt(4)` console of FreeBSD. A program moves the cursor, clears the console and changes colors by writing escape sequences, instead of calling a function of the kernel for each one, so the same program works wherever its output goes. The macros of [`console.s`](https://github.com/hexagonix/lib/blob/main/fasm/console.s) in libasm, such as `fputs`, `gotoxy` and `setConsoleColor`, already write this way.

Every virtual terminal has its own cursor, colors and sequence in progress, so a sequence split between two writes is still understood. A process may write to the tty of another process, which draws on that virtual terminal, visible or not.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Control characters

| Byte | Name | Effect |
|:----:|:----:|:------:|
| `0Ah` | LF | Next line, at its first column |
| `0Dh` | CR | First column of the current line |
| `08h` | BS | One column back, without erasing |
| `09h` | TAB | One space |
| `1Bh` | ESC | Starts an escape sequence |

Any other control character, BEL and DEL among them, draws nothing.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Escape sequences

CSI stands for ESC followed by `[`. Parameters are decimal numbers separated by `;`, and an empty one takes its default. Rows and columns count from 1.

| Sequence | Effect |
|:--------:|:------:|
| `CSI n A` | Cursor up n rows, 1 by default, stopping at the edge |
| `CSI n B` | Cursor down n rows |
| `CSI n C` | Cursor forward n columns |
| `CSI n D` | Cursor back n columns |
| `CSI n G` | Cursor to column n of its row |
| `CSI row ; column H` | Cursor to that row and column, also `f` |
| `CSI J` | Clears from the cursor to the end of the screen |
| `CSI 2 J` | Clears the whole screen, also `3 J` |
| `CSI K` | Clears from the cursor to the end of the line |
| `CSI 1 K` | Clears from the start of the line to the cursor |
| `CSI 2 K` | Clears the whole line |
| `CSI ... m` | Colors, as below |
| `ESC 7` | Saves the position of the cursor |
| `ESC 8` | Puts the cursor back where `ESC 7` saved it, at the top left corner if nothing was saved |

Clearing never moves the cursor. An unknown sequence is consumed whole and draws nothing, and so are private ones such as `CSI ? 25 l` and those with an intermediate byte such as `ESC ( B`.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Colors

| Parameter of `CSI ... m` | Effect |
|:------------------------:|:------:|
| `0` | Back to the colors of the theme |
| `7` | Font and background swapped, until `27` or `0` |
| `27` | Font and background back in place |
| `30` to `37` | Font color, from the palette below |
| `90` to `97` | Bright font color |
| `40` to `47` | Background color |
| `100` to `107` | Bright background color |
| `38 ; 2 ; r ; g ; b` | Font color given in RGB |
| `48 ; 2 ; r ; g ; b` | Background color given in RGB |
| `39` | Font color of the theme |
| `49` | Background color of the theme |

The palette, in order: black, red, green, yellow, blue, magenta, cyan and white, as `000000h`, `AA0000h`, `00AA00h`, `AA5500h`, `0000AAh`, `AA00AAh`, `00AAAAh` and `AAAAAAh`. The bright ones are `555555h`, `FF5555h`, `55FF55h`, `FFFF55h`, `5555FFh`, `FF55FFh`, `55FFFFh` and `FFFFFFh`.

A sequence holds at most 5 parameters, so a font and a background color given in RGB go in two sequences.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Reading a tty

What `hx.read` returns from a tty depends on the mode of that tty, kept by the tty and not by its processes, in the manner of the `c_lflag` of `termios`. `hx.ioctl` command 4 returns it in EAX and command 5 sets it from ECX:

| Bit | Name | Effect |
|:---:|:----:|:------:|
| `01h` | Canonical | Lines, gathered and edited by the kernel |
| `02h` | Echo | What is typed is shown as it is typed |
| `04h` | ISIG | Ctrl+C ends what runs on the terminal |

A tty starts with all three, the cooked mode of BSD. The [`tty.s`](https://github.com/hexagonix/lib/blob/main/fasm/tty.s) library of libasm has `ttyGetMode` and `ttySetMode` for them, and `readKey`, which waits for a single key in raw mode and puts the mode back.

With ISIG, Ctrl+C on a virtual terminal ends its foreground process, as `SIGINT` would in BSD, though there are no signals yet. Without it, Ctrl+C reaches the program as the byte `03h`, which lets a program in raw mode, such as a terminal emulator, pass it on. `readLine` turns ISIG off while it reads, and drops the line on `03h`, showing `^C` as the shells of BSD do, so Ctrl+C at a prompt does not end the shell.

### Canonical mode

A read waits for a whole line and returns it with its `0Ah`. A line longer than the buffer of a read is returned over the next reads. A line holds 255 bytes at most, its end included.

| Key | Byte | Effect |
|:---:|:----:|:------:|
| Enter | `0Ah` | Ends the line |
| Backspace | `08h` or `7Fh` | Erases the last character |
| Ctrl+W | `17h` | Erases the last word |
| Ctrl+U | `15h` | Erases the whole line |
| Ctrl+D | `04h` | Returns what was typed, with no end of line. On an empty line the read returns 0, the end of the file |

The arrows, Home, End and Delete do nothing in a line. Editing anywhere in it is up to the program, as `readLine` in `tty.s` does over the raw mode.

### Raw mode

A read returns what is typed as it comes, at least one byte. Ctrl with a letter gives its control character, `01h` to `1Ah`, Ctrl+C included when ISIG is off. Keys with no character of their own become the sequences a terminal sends for them, while the numeric keypad types its digits.

| Key | Bytes |
|:---:|:-----:|
| Up | `ESC [ A` |
| Down | `ESC [ B` |
| Right | `ESC [ C` |
| Left | `ESC [ D` |
| Home | `ESC [ H` |
| End | `ESC [ F` |
| Delete | `ESC [ 3 ~` |
| Esc | `1Bh` |
| Enter | `0Ah` |
| Backspace | `08h` |

When a sequence does not fit in the buffer of a read, the rest is returned by the next one.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Size of the window

`hx.ioctl` command 6 returns the size of the window of a tty, columns in EAX and rows in EBX, in the manner of `TIOCGWINSZ` in BSD. On a virtual terminal it is the size of the console. On a pty it is what the manager set with command 7, columns in ECX and rows in EDX, and 80 by 25 until it does. `ttyGetSize` in `tty.s` reads it for descriptor 0.

A program that lines up text by the size of its window, and moves only through the sequences above, shows the same on a virtual terminal and on a pty.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Pseudoterminals

A pty is a pair of devices, as `pty(4)` in BSD. The subsidiary side, `ttyp0` to `ttyp7`, is a tty like any virtual terminal, with its own mode, line and echo, where a program such as `sh` runs. The manager side, `ptyp0` to `ptyp7`, belongs to whatever plays the terminal, such as an emulator, and takes the place of the keyboard and the console:

| Operation | Effect |
|:---------:|:------:|
| Writing to the manager | The bytes reach the subsidiary as if typed |
| Reading the subsidiary | Through the mode of the subsidiary, as on a virtual terminal |
| Writing to the subsidiary | The bytes, with the echo, wait for the manager |
| Reading the manager | What the subsidiary wrote, at least one byte |

Each side has a ring of 1 KB, allocated when the manager is opened. A read of an empty ring and a write to a full one wait until the other side acts. A manager already open refuses another `hx.open`, and so does a pair whose last session still has its subsidiary open, so a manager that opens is always a free pair. A subsidiary opens only while its manager is open. `hx.ioctl` commands 4 and 5 work on either side, acting on the mode of the subsidiary.

Once no descriptor holds the manager, reading the subsidiary returns 0, the end of the file. Once no descriptor holds the subsidiary and what it wrote was read, reading the manager returns 0. This is how an emulator learns that its shell is gone. The rings are freed when no descriptor holds either side.

A program runs on a pty because its descriptors 0, 1 and 2 point to the subsidiary, which its creator arranges with `hx.dup2` before `hx.spawn`. Its virtual terminal stays the one it came from, which `ps` shows.

The byte `03h` written to the manager, while the subsidiary has ISIG, is not delivered. It ends the processes that read the subsidiary through descriptor 0, apart from those waiting on a child, as a shell outlives the command it ran, and drops the input not read yet.

The [`script`](https://github.com/hexagonix/Unix-Apps/tree/main/script) utility runs a shell on a pty and records the session into a file, the way an emulator would drive it. [`raava`](https://github.com/hexagonix/Unix-Apps/tree/main/raava) shows several shells in windows on a virtual terminal of its own, each on a pty, keeping what each one drew with the `term.s` library of libasm, which interprets bytes as this terminal does.

</div>
