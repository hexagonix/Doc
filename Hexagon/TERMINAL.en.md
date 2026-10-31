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
| `CSI row ; column H` | Cursor to that row and column, also `f` |
| `CSI J` | Clears from the cursor to the end of the screen |
| `CSI 2 J` | Clears the whole screen, also `3 J` |
| `CSI K` | Clears from the cursor to the end of the line |
| `CSI 1 K` | Clears from the start of the line to the cursor |
| `CSI 2 K` | Clears the whole line |
| `CSI ... m` | Colors, as below |

Clearing never moves the cursor. An unknown sequence is consumed whole and draws nothing, and so are private ones such as `CSI ? 25 l` and those with an intermediate byte such as `ESC ( B`.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Colors

| Parameter of `CSI ... m` | Effect |
|:------------------------:|:------:|
| `0` | Back to the colors of the theme |
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

A tty starts with both, the cooked mode of BSD. The [`tty.s`](https://github.com/hexagonix/lib/blob/main/fasm/tty.s) library of libasm has `ttyGetMode` and `ttySetMode` for them.

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

A read returns what is typed as it comes, at least one byte. Ctrl with a letter gives its control character, `01h` to `1Ah`, except Ctrl+C, which ends the foreground process. Keys with no character of their own become the sequences a terminal sends for them, while the numeric keypad types its digits.

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

</div>
