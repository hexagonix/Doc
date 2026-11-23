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

<!-- Em revisão para corrigir erros de inglês-->

# Hexagon system calls

Hexagon provides a series of well-documented system calls for developing utilities and applications. System calls can be accessed in the user environment through a software interrupt. When an application or utility wants to request a service provided by Hexagon, it invokes a system call, which interrupts the execution of the process, transfers execution control back to Hexagon, which performs privileged operations, and transfers execution back to the utility. This mechanism for transferring execution from the utility to the kernel is called [context switching](https://en.wikipedia.org/wiki/Context_switch). A context switch is essential for the utility to request privileged operations from the kernel, such as performing I/O operations on devices or starting a new process, as well as file manipulation operations. It is worth noting that utilities are not authorized to perform I/O operations directly in user mode. Furthermore, all the logic for handling files and disks/volumes is in the kernel. In Hexagon, the functions made available in the system call can be requested by software interrupt number `128` in decimal or `80h` in hexadecimal, the same vector traditionally used by Unix-like systems for system calls. Notably, the most used notation is hexadecimal, for system calls in various operating systems. When requesting a system call, you must also supply parameters that identify the chosen function, as well as the required parameters for it. You can find the required parameters for them in the table below.

An example of how to request a system call:

```assembly

    push function_number

    mov eax, parameter1
    mov ebx, parameter2
    mov ecx, parameter3
    mov edx, parameter4
    mov esi, parameter5
    mov edi, parameter6

    int 80h

```

Go to [table of functions](#table-of-functions-provides-by-hexagon) provided by Hexagon for more information about each one of them. You can also view a [sample code](#sample-code) in `Assembly x86`.

The number and parameters of a function in the system call are always kept within a version of Hexagonix. In this way, any application developed targeting the H2 version should work within all revisions and releases of the version. A change could occur, however, in a future version such as H3. The Hexagonix ABI and API have a lifecycle that is modeled on their FreeBSD lifecycle.

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Descriptors

Every process holds up to 16 descriptors, small numbers naming what it opened, like a Unix file descriptor. A process starts with a copy of its creator's descriptors, and the ones still open are closed when it ends.

Descriptors 0, 1 and 2 name the controlling terminal of the process, the virtual terminal it is attached to, which makes them its standard input, output and error. They follow the process when it claims another virtual terminal through `hx.ioctl` command 3.

Opening a device with `hx.open` takes the lowest free descriptor, which `hx.read`, `hx.write`, `hx.ioctl` and `hx.close` receive in EAX. Opening a common file loads it whole at the address given in EDI and takes no descriptor.

`hx.dup2` points one descriptor at what another names, as `dup2` in BSD. Since `hx.spawn` copies the descriptors of its caller, a process can start a child on another device by pointing its own 0, 1 and 2 there, spawning, and putting them back, which is how a program is started on a [pseudoterminal](TERMINAL.en.md#pseudoterminals).

A descriptor can carry two flags, set through `hx.ioctl` commands that every descriptor takes, whatever device it names, with ECX = 1 to set the flag and 0 to clear it. A copy made by `hx.dup2` starts with neither.

| Command | Flag | Effect |
|:-------:|:----:|:------:|
| `100h` | Non-blocking | `hx.read` and `hx.write` return at once instead of waiting, with CF set and EAX = `0Bh` when there was nothing to read or no room at all, as `O_NONBLOCK` in BSD. A line being typed in canonical mode is kept for the next read |
| `101h` | Close on spawn | A process created by `hx.spawn` or `hx.exec` gets the descriptor free instead of a copy, as `FD_CLOEXEC` in BSD |

The `hx.setNonBlocking` and `hx.setCloseOnSpawn` macros of `hexagon.s` in libasm send them.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Table of functions provides by Hexagon

Now, a table with the Hexagonix system call functions, with every function gathered into a single table and sorted by category through the Group column.

> Remember that a table of functions, standardized according to the functions available in Version 7 UNIX, is being developed. In this case, there is no purpose of function number pairing with UNIX, but conformity in function names. For example, `allocateMemory` would become `malloc`, and `returnVersion`, `uname`. In the future, both nomenclatures will be available to allow migration of applications and utilities. Come back to this file later to check for updates.

| Function number | Name | Group | Input | Output | Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:-----------:|
| 1 | hx.malloc | Memory and Process Management | EBX = Requested memory size, in bytes | EAX = 0 on failure, nonzero otherwise; EBX = Pointer to allocated memory | Allocate memory for the process|
| 2 | hx.free | Memory and Process Management | EBX = Pointer previously returned by hx.malloc; ECX = Ignored, kept so callers written against the older contract still assemble | No output | Frees previously allocated memory|
| 3 | hx.exec | Memory and Process Management | ESI = Program name; EDI = Arguments; EAX = 0 if no arguments are passed | CF set on error or image not found | Loads and executes image present in the volume, with a copy of the caller's [descriptors](#descriptors) apart from those marked to close on spawn. The caller blocks until it ends, and the child becomes the foreground process of their virtual terminal, if the caller was, or if that terminal had none|
| 4 | hx.exit | Memory and Process Management | EAX = Error code, if any; EBX = 0 if just terminate execution; EBX = 0x1234 to keep resident | No output | Finalizes the execution of a process |
| 5 | hx.pid | Memory and Process Management | No input | EAX = PID of current process | Get the PID of the running process |
| 6 | hx.spawn | Memory and Process Management | ESI = Name of the file to execute; EDI = Arguments; EAX = 0 if no arguments are passed | EAX = PID of the new process; CF set on error (process limit reached, image not found or incompatible, or out of memory) | Creates a new process without blocking the calling process, which keeps running immediately after the spawn. The new process gets a copy of the caller's [descriptors](#descriptors), apart from those marked to close on spawn|
| 7 | hx.kill | Memory and Process Management | EBX = Target PID | CF set if no process exists with the given PID; EAX = 05h | Terminates an arbitrary process by PID, callable by any process against any other (unlike terminating the foreground process via the special key)|
| 8 | hx.memoryUsage | Memory and Process Management | No input | EAX = Used memory, in bytes; EBX = Total memory available for use, in bytes; ECX = Total memory available for use, in Mbytes (less accurate); EDX = Memory reserved for the Hexagon, in bytes; ESI = Total allocated memory (reserved+processes), in Kbytes| Get Detailed System Memory Usage|
| 9 | hx.getProcesses | Memory and Process Management | ESI = Caller's own buffer; ECX = Size of that buffer, in bytes | EAX = Number of records written, with CF clear; EAX = Buffer size needed, in bytes, with CF set, in which case nothing was written. The record size leads the buffer, in a dword, and is 78 bytes in this version. Each record: `+0` PID; `+4` Parent PID (0 when started by the kernel); `+8` State; `+9` Name, NUL terminated, at most 64 bytes; `+73` Virtual terminal the process is attached to; `+74` Memory the process holds, in bytes | Get running processes into a buffer owned by the caller|
| 10 | hx.getErrorCode | Memory and Process Management | No input | EAX = Error code (0 for no error)| Get the code returned by the last running process |
| 11 | hx.getenv | Memory and Process Management | ESI = Name of the environment variable | ESI = Pointer to the value; CF set if the variable is not defined | Reads an environment variable from the calling process|
| 12 | hx.setenv | Memory and Process Management | ESI = Name of the environment variable; EDI = Value | CF set if the environment does not have enough room left for the new entry | Defines or replaces an environment variable of the calling process|
| 13 | hx.unsetenv | Memory and Process Management | ESI = Name of the environment variable | CF set if the variable is not defined | Removes an environment variable from the calling process|
| 14 | hx.environ | Memory and Process Management | No input | ESI = Pointer to the raw, double NUL terminated environment block; EAX = Number of variables defined | Get every environment variable of the calling process|
| 15 | hx.open | File and Device Management | ESI = Pointer to the device name, or to the path of the file to open (a leading `/` is absolute, otherwise relative to the current directory); EDI = Loading address, in case of file; ECX = Byte cap on a file read, 0 for the whole file | EAX = Descriptor, for a device; CF set on error, with EAX holding its code (`0Ah` when the process has no free descriptor) | Opens a device and returns a [descriptor](#descriptors) for it, or loads a common file present on disk at the given address (devices and disks are treated as files)|
| 16 | hx.write | File and Device Management | EAX = Descriptor; ESI = Pointer to the data; ECX = Number of bytes | EAX = Bytes written; CF set on error, with EAX holding its code (`09h` when the descriptor is not open, `0Bh` when it may not wait and nothing fit) | Writes bytes to the device behind a descriptor. A [non-blocking](#descriptors) descriptor on a pty writes what fits. Written to a tty, they go through the [console terminal](TERMINAL.en.md)|
| 17 | hx.close | File and Device Management | EAX = Descriptor | CF set if the descriptor is not open | Closes a descriptor|
| 18 | hx.create | File and Device Management | ESI = Pointer to the path of the file to create (absolute or relative to the current directory); EDI = Pointer to the content; EAX = File Size | CF defined in case of error, file already present, or a directory in the path not existing (EAX = `IO.pathNotFound`) | Save a file on the mounted volume|
| 19 | hx.touch | File and Device Management | ESI = Pointer to the path of the file to create (absolute or relative to the current directory) | CF set if the file already exists, a directory in the path does not exist, or the filename is longer than 12 characters | Creates a new, empty file on the mounted volume, unlike `hx.create` above, which writes content in one step. A file created by root comes out protected, through the SYSTEM attribute of FAT16B. See `hx.chown`|
| 20 | hx.unlink | File and Device Management | ESI = Pointer to the path of the file to remove | CF defined in case of error, non-existing file, or removing a protected file (see `hx.chown`) as a non-root user | Remove a file on the mounted volume |
| 21 | hx.rename | File and Device Management | ESI = Pointer to the path of the file to rename; EDI = Pointer to the new name (only its last component is used; renaming into a different directory is not supported) | CF defined in case of error, error in name update, or renaming a protected file (see `hx.chown`) as a non-root user | Updates the name of a file on the mounted volume |
| 22 | hx.listFiles | File and Device Management | ESI = Caller's own buffer; ECX = Size of that buffer, in bytes | EAX = Number of entries written, with CF clear; EAX = Buffer size needed, in bytes, with CF set, in which case nothing was written; EAX = 0 with CF set reports a filesystem error instead. Each entry ends in a file or directory separator byte, with the whole list NUL terminated | Get list of files present in the current directory, into a buffer owned by the caller|
| 23 | hx.stat | File and Device Management | ESI = Pointer to the path of the file to check (absolute or relative to the current directory) | EAX = File size; EBX = `Hexagon.Dev.DeviceClasses` value, 0 when the path is not a device node; ECX = Attribute byte (bit `10h` marks a directory); EDX = Device major (DH) and minor (DL), 0:0 when not a device node; CF set if the file, or a directory in the path, does not exist | Check if a file exists on the volume, and whether it is a device node|
| 24 | hx.mkdir | File and Device Management | ESI = Path of the directory to create (absolute or relative to the current directory) | CF defined in case of error; EAX = `IO.pathNotFound` if a directory in the path does not exist | Creates a new, empty directory on the mounted volume. A directory created by root comes out protected, through the SYSTEM attribute of FAT16B. See `hx.chown`|
| 25 | hx.rmdir | File and Device Management | ESI = Path of the directory to remove (absolute or relative to the current directory) | CF defined in case of error; EAX = `IO.operationDenied` for a non-root user removing a protected directory (see `hx.chown`), `IO.directoryNotEmpty` if the directory still has entries other than `.` and `..` | Removes an empty directory from the mounted volume|
| 26 | hx.changeDirectory | File and Device Management | ESI = Path of the directory to change to (absolute or relative, may have several `/`-separated components) | CF set if the path is empty or any component is not found or is invalid. On error the current directory is left unchanged | Changes the current working directory|
| 27 | hx.getCurrentDirectory | File and Device Management | EDI = Caller's own buffer, at least 64 bytes | Buffer filled with the current directory path, NUL-terminated | Copies the calling process's own current directory into a caller-supplied buffer|
| 28 | hx.mount | File and Device Management | ESI = Device path, resolved through a devfs mount, such as `/dev/hd1` for a whole disk or `/dev/hd1p0` for one of its up to 4 MBR primary partitions. The reserved name `devfs` creates a devfs mount instead, with no device behind it; EDI = Mount point path, which must already exist as a directory | CF set on error; EAX = `01h` permission denied (root only), `02h` device not found, not a block device, or none of its partitions supported, `03h` mount point does not exist, is not a directory, or its path is too long, `04h` no free mount slots, `05h` failed to read the BPB of the partition, `06h` mount point already in use by another device. Mounting a device at a mount point that already holds it succeeds without doing anything, so re-reading /etc/fstab stays idempotent | Mounts a device or one of its partitions at an existing directory|
| 29 | hx.unmount | File and Device Management | ESI = Mount point path | CF set on error; EAX = `01h` permission denied (root only), `02h` nothing mounted at that path, `03h` refusing to unmount `/` | Unmounts a previously mounted device|
| 30 | hx.getMounts | File and Device Management | ESI = Caller's own buffer, sized for up to 8 records of 109 bytes each (`+0` mount point path, NUL terminated, 64 bytes; `+64` device path, NUL terminated, 32 bytes; `+96` filesystem type code, 1 byte; `+97` volume label, NUL terminated, 12 bytes) | EAX = Number of records written | Lists every mount point currently in use, into a buffer owned by the caller|
| 31 | hx.lock | User Management and Permissions | No input | No output | Block foreground process termination signal by special key|
| 32 | hx.unlock | User Management and Permissions | No input | No output | Enable foreground process termination signal by special key use|
| 33 | hx.setUser | User Management and Permissions | EAX = User ID | No output | Registers the id of the user now logged in, supplied by the login manager. The kernel neither stores nor resolves usernames; that mapping lives in userland, in the `/etc/shadow` database|
| 34 | hx.getUser | User Management and Permissions | No input | EAX = User ID | Get the logged in user's id for the current session |
| 35 | hx.uname | Hexagon Services | EDI = Caller's own buffer, at least 96 bytes | EAX = Version number; EBX = Subversion number; ECX = Revision number; EDX = Architecture; EDI = Preserved. The kernel name is written at the start of the buffer and its build at offset 48, both NUL terminated | Returns Hexagon version for applications|
| 36 | hx.sleep | Hexagon Services | ECX = Time in count units to cause delay | No output | Causes a delay in operations |
| 37 | hx.installISR | Hexagon Services | EAX = Interrupt number; ESI = Pointer to handler | No output | Install interrupt service routine|
| 38 | hx.restart | Power Management | No input | No output | Request device restart|
| 39 | hx.shutdown | Power Management | No input | No output | Prompts for device shutdown|
| 40 | hx.print | Video and Graphics Services | EAX = Numerical content, if this is the case, respecting the designated formats. The formats must be informed; ESI = Pointer to the string to be printed, if this is the case; EBX = Input type (01h - decimal integer; 02h - hexadecimal integer; 03h - binary integer; 04h - string; 05h - single character, taken from AL and drawn at the DL column and DH line, leaving the cursor where it is)| No output | Sends a defined content to an output device|
| 41 | hx.clearConsole | Video and Graphics Services | No input | No output | Clear current console|
| 42 | hx.clearLine | Video and Graphics Services | AL = Line number | No output | Clears a specific line in the console|
| 43 | hx.scrollConsole | Video and Graphics Services | No input | No output | Scrolls the console down one line|
| 44 | hx.setCursor | Video and Graphics Services | DL = position on the X axis; DH = position on the Y axis | No output | Sets the cursor at a specific position |
| 45 | hx.drawCharacter | Video and Graphics Services | EAX = position on the X axis; EBX = position on the Y axis; EDX = Color in hexadecimal | No output | Puts a pixel on the console|
| 46 | hx.drawBlock | Video and Graphics Services | EAX = position on the X axis; EBX = position on the Y axis; ESI = Length; EDI = Height; EDX = Color in hexadecimal | No output | Draw a specific color block|
| 47 | hx.printCharacter | Video and Graphics Services | AL = Character; EBX = 01h to reposition cursor | No output | Print character in console at cursor position|
| 48 | hx.setColor | Video and Graphics Services | EAX = Font color (RGB in hexadecimal); EBX = Background color (RGB in hexadecimal); ECX=1234h to change the default theme to the requested values; In text mode, only black and white are allowed | No output | Set background and foreground color|
| 49 | hx.getColor | Video and Graphics Services | No input | EAX = Font color (RGB in hexadecimal); EBX = Background color (RGB in hexadecimal); ECX=1234h to change the default theme to the requested values; In text mode, only black and white are allowed | Get background and foreground color|
| 50 | hx.getConsoleInfo | Video and Graphics Services | No input | EAX = Resolution X (bits 0..15), Y (bits 16..31); EBX = Columns (bit 0..7), Rows (8..15), Bits per pixel (16..23); EDX = Address of the beginning of the video frame; CF defined in case of text mode | Get current console info |
| 51 | hx.updateScreen | Video and Graphics Services | No input | No output | Updates the primary console with content from the secondary (double buffering) console|
| 52 | hx.getResolution | Video and Graphics Services | No input | EAX = Number relative to the resolution to be used (1 = Resolution of 800x600 pixels; 2 - Resolution of 1024x768 pixels) | They have the resolution used by the main console|
| 53 | hx.getCursor | Video and Graphics Services | No input | DL = X axis; DH = Y axis | Get cursor position|
| 54 | hx.waitKeyboard | PS/2 Device Services | No input | AL = Character; AH - Scancode | Waits for a keypress on the keyboard|
| 55 | Retired | PS/2 Device Services | No input | CF set | Was hx.getString. A line is read from the tty with `hx.read`, in [canonical mode](TERMINAL.en.md#reading-a-tty), or edited with `readLine` from [`tty.s`](https://github.com/hexagonix/lib/blob/main/fasm/tty.s). The number is not reused|
| 56 | hx.getKeyState | PS/2 Device Services | No input | EAX = Status of special keys (bit 0: Control key; bit 1: Shift key; bit 2-31: Reserved) | Get the state of special keys such as Control and Shift|
| 57 | hx.changeConsoleFont | Video and Graphics Services | ESI = Pointer to the buffer containing the name of the file containing the Hexagonix compatible font | CF defined in case of file not found or incompatible | Change the default system display font|
| 58 | hx.changeLayout | PS/2 Device Services | ESI = File containing a valid keyboard layout | CF defined in case of file not found or incompatible | Change keyboard layout|
| 59 | hx.waitMouse | PS/2 Device Services | No input | EAX = Position on the X axis; EBX = Position on the Y axis; EDX = Buttons | Wait for mouse event|
| 60 | hx.getMouse | PS/2 Device Services | No input | EAX = Position on the X axis; EBX = Position on the Y axis; EDX = Buttons (bit 0 the left one, bit 1 the right one, bit 2 the middle one) | Get current mouse position and button state|
| 61 | hx.setMouse | PS/2 Device Services | EAX = Position on the X axis; EBX = Position on the Y axis | No output | Set new mouse position|
| 62 | hx.emitSound | Sound Output Services | AX = Frequency to play | No output | Plays a tone on the computer's internal speaker|
| 63 | hx.turnOffSound | Sound Output Services| No input | No output | Turns off the computer's internal speaker, stopping any sound in progress|
| 64 | hx.sendMessageHexagon | Messaging Service | ESI = Message; EAX = Error code, if any; EBX = Priority | No output | Sends a high priority message from Hexagon|
| 65 | hx.date | Real Time Clock Services | EAX = Day, in ASCII; EBX = Month, in ASCII; ECX = Century, in ASCII; EDX = Year, in ASCII | No output | Returns real-time clock information in ASCII (String) format. Conversion to number may be required|
| 66 | hx.time | Real Time Clock Services | EAX = Time, in ASCII; EBX = Minute, in ASCII; ECX = Second, in ASCII | No output | Returns real-time clock information in ASCII (String) format. Conversion to number may be required|
| 67 | hx.ioctl | Device Control Service | EAX = Descriptor; EBX = Command; ECX = Argument for the command | CF set if the descriptor is not open, or its class has no handler for this command | Device-specific control that does not fit the byte stream of read and write, dispatched to the class of the device behind the descriptor. For a tty, command 1 selects the console the process draws on (ECX = 0 for the main one, 1 for the secondary one presented by hx.updateScreen), command 2 activates that virtual terminal and command 3 claims it as the caller's own, command 4 returns its [mode](TERMINAL.en.md#reading-a-tty) in EAX and command 5 sets it from ECX, command 6 returns the [size of its window](TERMINAL.en.md#size-of-the-window), columns in EAX and rows in EBX, and on a pty command 7 sets it, columns in ECX and rows in EDX, command 8 holds a virtual terminal for the caller alone with ECX = 1, which only its foreground process may do, as X does in BSD with `KD_GRAPHICS`: calls that draw on it and bytes written to it by any other process are dropped until the holder lets go with ECX = 0 or ends. Commands `100h` and `101h` are taken for every descriptor, whatever its device, as [above](#descriptors)|
| 68 | hx.chown | File and Device Management | ESI = Pointer to the path; EBX = Requested mode (only bit `0002h`, write for others, is inspected) | CF set on error; EAX = `01h` permission denied (root only), `02h` path not found | Applies a Unix-style mode to a path. Only root may call it. What the mode changes is up to the filesystem: FAT16B has no owner and no real permission bits, so it only derives whether the path becomes root-only-write, through its SYSTEM attribute, the same way the msdosfs driver of FreeBSD reduces chmod to the read-only DOS attribute. DEVFS ignores the call, without error|
| 69 | hx.getTTY | Process Management Service | No input | EAX = VT index (0-7) | Tells the calling process which virtual terminal it is attached to, like ttyname() on Unix|
| 70 | hx.sysctl | Kernel Diagnostics Service | EBX = Function number (only 1 exists: read the kernel log buffer); ESI = Caller's buffer; ECX = Caller's buffer size | EAX = Bytes copied; CF set on an unknown function number | Reads kernel diagnostic data into a buffer owned by the caller. Function 1 returns the log buffer of the kernel itself, without userland messages|
| 71 | hx.read | File and Device Management | EAX = Descriptor; EDI = Caller's buffer; ECX = Maximum bytes | EAX = Bytes read; CF set on error, with EAX holding its code | Reads bytes from the device behind a descriptor, blocking until there is at least one, or, on a [non-blocking](#descriptors) descriptor, returning at once with CF set and EAX = `0Bh`. Zero bytes read is the end of the file. Only a tty can be read for now, either side of a pty among them, and a virtual terminal only by its own processes. Whether it returns lines or keys depends on the [mode of the tty](TERMINAL.en.md#reading-a-tty)|
| 72 | hx.dup2 | File and Device Management | EAX = Descriptor to copy; EBX = Descriptor it is copied to | EAX = EBX; CF set if EAX is not open or either is out of range, with EAX = `09h` | Points the descriptor in EBX at what the one in EAX names, closing it first if it was open. The same descriptor in both does nothing|

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Sample code

Below, we have an example of a text mode application that uses a series of functions provided by Hexagon. You'll see the HAPP header assembled by the [libasm](https://github.com/hexagonix/lib/blob/main/fasm/HAPP.s) `headerHAPP` macro, the system call and console macro includes, clearing the console, printing a message, waiting for a key and terminating the process. These macros can be found and studied directly in the [libasm](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s) repository. The example below was written in `x86 Assembly` with Intel syntax, targeting the fasm (flat assembler) assembler.

```assembly
format binary as "app" ;; Specifies the file format and extension

use32

include "HAPP.s" ;; Macro that assembles the HAPP header

;; Architecture | Version | Subversion | Entry point | Image type | BSS size
headerHAPP HAPP.Architectures.i386, 1, 8, applicationStart, 01h, HAPP.bss.none

;;*************************************************************

include "hexagon.s" ;; The hx.syscall macro and hx.* constants
include "console.s" ;; Console output macros (fputs, puts...)
include "macros.s"  ;; finishProcess and other general purpose macros

;;*************************************************************

;; Variables and constants

message: db 10, 10, "This is a sample HAPP application for Hexagonix!", 10, 0

;;*************************************************************

applicationStart:

;; Clear the current console and print the greeting message

    hx.syscall hx.clearConsole

    puts message ;; fputs, if you don't want the trailing newline

;; Wait for a keypress before exiting

    hx.syscall hx.waitKeyboard

;; Terminate the process (error code 0, do not stay resident)

    finishProcess 0, 0
```

Beyond this example, you can study real applications that use the Hexagon system call directly in the Hexagonix source tree, in the [Unix-Apps](https://github.com/hexagonix/Unix-Apps) repository. The `testa` and `testb` utilities, in particular, are small diagnostic applications (not part of the distribution) written to exercise `hx.spawn`: `testa` requests the non-blocking creation of `testb` and logs, through `log.s`, that it keeps running right after the spawn, proving that the call does not block the calling process.

</div>
