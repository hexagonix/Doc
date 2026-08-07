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

## Table of functions provides by Hexagon

Now, a table with the Hexagonix system call functions, with every function gathered into a single table and sorted by category through the Group column.

> Remember that a table of functions, standardized according to the functions available in Version 7 UNIX, is being developed. In this case, there is no purpose of function number pairing with UNIX, but conformity in function names. For example, `allocateMemory` would become `malloc`, and `returnVersion`, `uname`. In the future, both nomenclatures will be available to allow migration of applications and utilities. Come back to this file later to check for updates.

| Function number | Name | Group | Input | Output | Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:-----------:|
| 1 | hx.malloc | Memory and Process Management | EAX = Requested memory size, in bytes | EBX = Pointer to allocated memory | Allocate memory for the process|
| 2 | hx.free | Memory and Process Management | EBX = Pointer to allocated memory; ECX = Size of allocated memory | No output | Frees previously allocated memory|
| 3 | hx.exec | Memory and Process Management | ESI = Program name; EDI = Arguments; EAX = 0 if no arguments are passed | CF set on error or image not found | Loads and executes image present in the volume|
| 4 | hx.exit | Memory and Process Management | EAX = Error code, if any; EBX = 0 if just terminate execution; EBX = 0x1234 to keep resident | No output | Finalizes the execution of a process |
| 5 | hx.pid | Memory and Process Management | No input | EAX = PID of current process | Get the PID of the running process |
| 6 | hx.memoryUsage | Memory and Process Management | No input | EAX = Used memory, in bytes; EBX = Total memory available for use, in bytes; ECX = Total memory available for use, in Mbytes (less accurate); EDX = Memory reserved for the Hexagon, in bytes; ESI = Total allocated memory (reserved+processes), in Kbytes| Get Detailed System Memory Usage|
| 7 | hx.getProcesses | Memory and Process Management | No input | ESI = List of processes; EAX = Number of running processes | Get running processes |
| 8 | hx.getErrorCode | Memory and Process Management | No input | EAX = Error code (0 for no error)| Get the code returned by the last running process |
| 9 | hx.open | File and Device Management | ESI = Pointer to the device name, or to the path of the file to open (a leading `/` is absolute, otherwise relative to the current directory); EDI = Loading address, in case of file| CF set when device name is invalid or file does not exist | Opens a read/write channel on a requested device or common file present on disk (devices and disks are treated as files). In case of file on disk, a load address must be provided |
| 10 | hx.write | File and Device Management | ESI = Pointer to the buffer containing the data | CF set on error or no device open | Send data to open device|
| 11 | hx.close | File and Device Management | No input | No output | Closes the last device opened by the current process|
| 13 | hx.create | File and Device Management | ESI = Pointer to the path of the file to create (absolute or relative to the current directory); EDI = Pointer to the content; EAX = File Size | CF defined in case of error, file already present, or a directory in the path not existing (EAX = `IO.pathNotFound`) | Save a file on the mounted volume|
| 14 | hx.unlink | File and Device Management | ESI = Pointer to the path of the file to remove | CF defined in case of error or non-existing file | Remove a file on the mounted volume |
| 15 | hx.rename | File and Device management | ESI = Pointer to the path of the file to rename; EDI = Pointer to the new name (only its last component is used; renaming into a different directory is not supported) | CF defined in case of error or error in name update | Updates the name of a file on the mounted volume |
| 16 | hx.listFiles | File and Device Management | No input | ESI = Pointer to list of files; EAX = Total Files | Get list of files present in the current directory |
| 17 | hx.fileExists | File and Device Management | ESI = Pointer to the path of the file to check (absolute or relative to the current directory) | EAX = File Size; CF set if the file does not exist or a directory in the path does not exist | Check if a file exists on the volume |
| 18 | hx.getVolume | File and Device Management | No input | ESI = Device name; EDI = Used volume label | Get information from the mounted disk at `/`|
| 19 | hx.lock | User Management and Permissions | No input | No output | Block foreground process termination signal by special key|
| 20 | hx.unlock | User Management and Permissions | No input | No output | Enable foreground process termination signal by special key use|
| 21 | hx.setUser | User Management and Permissions | EAX = User ID; ESI = Username | No output | Define a user for the current session, supplied by the login manager|
| 22 | hx.getUser | User Management and Permissions | No input | EAX = User ID; ESI = Username| Get logged in user data for current session |
| 23 | hx.uname | Hexagon Services | No input | EAX = Version number; EBX = Subversion number; ECX = Revision number; EDX = Architecture; ESI = Kernel name; EDI = Kernel build date/time| Returns Hexagon version for applications|
| 24 | hx.getRandom | Hexagon Services | EAX = Maximum | EAX = Number | Get a random number|
| 25 | hx.feedRandom | Hexagon Services | EAX - Number to create entropy | No output | Feed Entropy to kernel random number generator|
| 26 | hx.sleep | Hexagon Services | ECX = Time in count units to cause delay | No output | Causes a delay in operations |
| 27 | hx.installISR | Hexagon Services | EAX = Interrupt number; ESI = Pointer to handler | No output | Install interrupt service routine|
| 28 | hx.restart | Power Management | No input | No output | Request device restart|
| 29 | hx.shutdown | Power Management | No input | No output | Prompts for device shutdown|
| 30 | hx.print | Video and Graphics Services | EAX = Numerical content, if this is the case, respecting the designated formats. The formats must be informed; ESI = Pointer to the string to be printed, if this is the case; EBX = Input type (01h - decimal integer; 02h - hexadecimal integer; 03h - binary integer; 04h - string)| No output | Sends a defined content to an output device|
| 31 | hx.clearConsole | Video and Graphics Services | No input | No output | Clear current console|
| 32 | hx.clearLine | Video and Graphics Services | AL = Line number | No output | Clears a specific line in the console|
| 33 | hx.scrollConsole | Video and Graphics Services | No input | No output | Scrolls the console down one line|
| 34 | hx.setCursor | Video and Graphics Services | DL = position on the X axis; DH = position on the Y axis | No output | Sets the cursor at a specific position |
| 35 | hx.drawCharacter | Video and Graphics Services | EAX = position on the X axis; EBX = position on the Y axis; EDX = Color in hexadecimal | No output | Puts a pixel on the console|
| 36 | hx.drawBlock | Video and Graphics Services | EAX = position on the X axis; EBX = position on the Y axis; ESI = Length; EDI = Height; EDX = Color in hexadecimal | No output | Draw a specific color block|
| 37 | hx.printCharacter | Video and Graphics Services | AL = Character; EBX = 01h to reposition cursor | No output | Print character in console at cursor position|
| 38 | hx.setColor | Video and Graphics Services | EAX = Font color (RGB in hexadecimal); EBX = Background color (RGB in hexadecimal); ECX=1234h to change the default theme to the requested values; In text mode, only black and white are allowed | No output | Set background and foreground color|
| 39 | hx.getColor | Video and Graphics Services | No input | EAX = Font color (RGB in hexadecimal); EBX = Background color (RGB in hexadecimal); ECX=1234h to change the default theme to the requested values; In text mode, only black and white are allowed | Get background and foreground color|
| 40 | hx.getConsoleInfo | Video and Graphics Services | No input | EAX = Resolution X (bits 0..15), Y (bits 16..31); EBX = Columns (bit 0..7), Rows (8..15), Bits per pixel (16..23); EDX = Address of the beginning of the video frame; CF defined in case of text mode | Get current console info |
| 41 | hx.updateScreen | Video and Graphics Services | No input | No output | Updates the primary console with content from the first virtual console|
| 42 | hx.setResolution | Video and Graphics Services | EAX = Number relative to the resolution to be used (1 = Resolution of 800x600 pixels; 2 - Resolution of 1024x768 pixels; 3 - Change to text mode)| No output | Sets the main console resolution|
| 43 | hx.getResolution | Video and Graphics Services | No input | EAX = Number relative to the resolution to be used (1 = Resolution of 800x600 pixels; 2 - Resolution of 1024x768 pixels) | They have the resolution used by the main console|
| 44 | hx.getCursor | Video and Graphics Services | No input | DL = X axis; DH = Y axis | Get cursor position|
| 45 | hx.waitKeyboard | PS/2 Device Services | No input | AL = Character; AH - Scancode | Waits for a keypress on the keyboard|
| 46 | hx.getString | PS/2 Device Services | AL = Maximum characters to get | EBX = Presence or absence of echo during typing (1234h for no echo and any value to activate); ESI = String | Get a string from the keyboard|
| 47 | hx.getKeyState | PS/2 Device Services | No input | EAX = Status of special keys (bit 0: Control key; bit 1: Shift key; bit 2-31: Reserved) | Get the state of special keys such as Control and Shift|
| 48 | hx.changeConsoleFont | Video and Graphics Services | ESI = Pointer to the buffer containing the name of the file containing the Hexagonix compatible font | CF defined in case of file not found or incompatible | Change the default system display font|
| 49 | hx.changeLayout | PS/2 Device Services | ESI = File containing a valid keyboard layout | CF defined in case of file not found or incompatible | Change keyboard layout|
| 50 | hx.waitMouse | PS/2 Device Services | No input | EAX = Position on the X axis; EBX = Position on the Y axis; EDX = Buttons | Wait for mouse event|
| 51 | hx.getMouse | PS/2 Device Services | No input | EAX = Position on the X axis; EBX = Position on the Y axis; EDX = Buttons | Get current mouse position and button state|
| 52 | hx.setMouse | PS/2 Device Services | EAX = Position on the X axis; EBX = Position on the Y axis | No output | Set new mouse position|
| 53 | hx.compareWordsString | Data manipulation and conversion services | ESI = First string; EDI = Second string | CF set if equal | Compare first words of two strings |
| 54 | hx.removeCharacterString | Data manipulation and conversion services | ESI = String; EAX = Character Position | No output | Removes a character at a specific position from a string|
| 55 | hx.insertCharacter | Data manipulation and conversion services | ESI = String; EDX = Position; AL = Character to insert | No output | Inserts a character at a specific position in the string |
| 56 | hx.stringSize | Data manipulation and conversion services | ESI = String | EAX = Length of string | Get the length of a string|
| 57 | hx.compareString | Data manipulation and conversion services | ESI = First string; EDI = Second string | CF set if both are equal | Compare all characters in a string are equal|
| 58 | hx.stringToUppercase | Data manipulation and conversion services | ESI = String | Converted String | Convert a string to uppercase characters|
| 59 | hx.stringToLowercase | Data manipulation and conversion services | ESI = String | Converted String | Convert a string to lowercase characters|
| 60 | hx.trimString | Data manipulation and conversion services | ESI = String | String cut | Remove whitespace from string|
| 61 | hx.findCharacter | Data manipulation and conversion services | ESI = String, AL = Character to find | EAX = Number of occurrences of the character; CF set if character not found | Find specific character in string|
| 62 | hx.stringToInt | Data manipulation and conversion services | ESI = String | EAX = Integer; CF defined in case of invalid number | Convert a string number to integer |
| 63 | hx.toString | Data manipulation and conversion services | EAX = Integer to be converted | ESI = Pointer to the buffer containing the string | Converts an integer to a string|
| 64 | hx.emitSound | Sound Output Services | AX = Frequency to play | No output | Plays a tone on the computer's internal speaker|
| 65 | hx.turnOffSound | Sound Output Services| No input | No output | Turns off the computer's internal speaker, stopping any sound in progress|
| 66 | hx.sendMessageHexagon | Messaging Service | ESI = Message; EAX = Error code, if any; EBX = Priority | No output | Sends a high priority message from Hexagon|
| 67 | hx.date | Real Time Clock Services | EAX = Day, in ASCII; EBX = Month, in ASCII; ECX = Century, in ASCII; EDX = Year, in ASCII | No output | Returns real-time clock information in ASCII (String) format. Conversion to number may be required|
| 68 | hx.time | Real Time Clock Services | EAX = Time, in ASCII; EBX = Minute, in ASCII; ECX = Second, in ASCII | No output | Returns real-time clock information in ASCII (String) format. Conversion to number may be required|
| 69 | hx.changeDirectory | File and Device Management | ESI = Path of the directory to change to (absolute or relative, may have several `/`-separated components) | CF set if the path is empty or any component is not found or is invalid. On error the current directory is left unchanged | Changes the current working directory|
| 70 | hx.spawn | Memory and Process Management | ESI = Name of the file to execute | EAX = PID of the new process; CF set on error (process limit reached, image not found or incompatible, or out of memory) | Creates a new process without blocking the calling process, which keeps running immediately after the spawn. Does not accept arguments yet, unlike hx.exec|
| 71 | hx.kill | Memory and Process Management | EBX = Target PID | CF set if no process exists with the given PID; EAX = 05h | Terminates an arbitrary process by PID, callable by any process against any other (unlike terminating the foreground process via the special key)|
| 72 | hx.mkdir | File and Device Management | ESI = Path of the directory to create (absolute or relative to the current directory) | CF defined in case of error; EAX = `IO.operationDenied` for a non-privileged user, `IO.pathNotFound` if a directory in the path does not exist | Creates a new, empty directory on the mounted volume|
| 73 | hx.rmdir | File and Device Management | ESI = Path of the directory to remove (absolute or relative to the current directory) | CF defined in case of error; EAX = `IO.operationDenied` for a non-privileged user, `IO.directoryNotEmpty` if the directory still has entries other than `.` and `..` | Removes an empty directory from the mounted volume|

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## Sample code

Below, we have an example of a text mode application that uses a series of functions provided by Hexagon. You'll see the HAPP header assembled by the [libasm](https://github.com/hexagonix/lib/blob/main/fasm/HAPP.s) `appHeader` macro, the system call and console macro includes, clearing the console, printing a message, waiting for a key and terminating the process. These macros can be found and studied directly in the [libasm](https://github.com/hexagonix/lib/blob/main/fasm/hexagon.s) repository. The example below was written in `x86 Assembly` with Intel syntax, targeting the fasm (flat assembler) assembler.

```assembly
format binary as "app" ;; Specifies the file format and extension

use32

include "HAPP.s" ;; Struc that assembles the HAPP header

;; Instance | Structure | Architecture | Version | Subversion | Entry point | Image type
appHeader headerHAPP HAPP.Architectures.i386, 1, 6, applicationStart, 01h

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
