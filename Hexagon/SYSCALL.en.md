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
[![](https://img.shields.io/twitter/follow/hexagonixOS.svg?style=social&label=Follow%20%40HexagonixOS)](https://twitter.com/hexagonixOS)

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

<div align="justify">

<!-- Em revisão para corrigir erros de inglês-->

# Hexagon system calls

Hexagon provides a series of well-documented system calls for developing utilities and applications. System calls can be accessed in the user environment through a software interrupt. When an application or utility wants to request a service provided by Hexagon, it invokes a system call, which interrupts the execution of the process, transfers execution control back to Hexagon, which performs privileged operations, and transfers execution back to the utility. This mechanism for transferring execution from the utility to the kernel is called [context switching](https://en.wikipedia.org/wiki/Context_switch). A context switch is essential for the utility to request privileged operations from the kernel, such as performing I/O operations on devices or starting a new process, as well as file manipulation operations. It is worth noting that utilities are not authorized to perform I/O operations directly in user mode. Furthermore, all the logic for handling files and disks/volumes is in the kernel. In Hexagon, the functions made available in the system call can be requested by software interrupt number `105` in decimal or `69h` in hexadecimal. Notably, the most used notation is hexadecimal, for system calls in various operating systems. When requesting a system call, you must also supply parameters that identify the chosen function, as well as the required parameters for it. You can find the required parameters for them in the table below.

An example of how to request a system call:

```assembly

    push function_number

    mov eax, parameter1
    mov ebx, parameter2
    mov ecx, parameter3
    mov edx, parameter4
    mov esi, parameter5
    mov edi, parameter6

    int 69h

```

Go to [table of functions](#table-of-functions-provides-by-hexagon) provided by Hexagon for more information about each one of them.

The number and parameters of a function in the system call are always kept within a version of Hexagonix. In this way, any application developed targeting the H2 version should work within all revisions and releases of the version. A change could occur, however, in a future version such as H3. The Hexagonix ABI and API have a lifecycle that is modeled on their FreeBSD lifecycle.

</div>

<!-- Vai funcionar como <hr> -->

<img src="https://github.com/hexagonix/Doc/blob/main/Img/hr.png" width="100%" height="2px" />

## Table of functions provides by Hexagon

Now, a table with the Hexagonix system call functions. Functions are sorted into categories. Functions were also classified as `Unix-like`, when analogous versions exist on UNIX or similar systems, and `Hexagonix`, when the implemented function does not have a similar version on other UNIX or Unix-like systems.

> Remember that a table of functions, standardized according to the functions available in Version 7 UNIX, is being developed. In this case, there is no purpose of function number pairing with UNIX, but conformity in function names. For example, `allocateMemory` would become `free`, and `returnVersion`, `uname`. In the future, both nomenclatures will be available to allow migration of applications and utilities. Come back to this file later to check for updates.

### Memory and process management functions

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 1 | allocateMemory | Memory and Process Management | EAX = Requested memory size, in bytes | EBX = Pointer to allocated memory | Unix-like| Allocate memory for the process|
| 2 | freeMemory | Memory and Process Management | EBX = Pointer to allocated memory; ECX = Size of allocated memory | No output | Unix-like | Frees previously allocated memory|
| 3 | startProcess | Memory and Process Management | ESI = Program name; EDI = Arguments; EAX = 0 if no arguments are passed | CF set on error or image not found | Unix-like | Loads and executes image present in the volume|
| 4 | closeProcess | Memory and Process Management | EAX = Error code, if any; EBX = 0 if just terminate execution; EBX = 0x1234 to keep resident | No output | Unix-like | Finalizes the execution of a process |
| 5 | getPID | Memory and Process Management | No input | EAX = PID of current process | Unix-like | Get the PID of the running process |
| 6 | memoryUse | Memory and Process Management | No input | EAX = Used memory, in bytes; EBX = Total memory available for use, in bytes; ECX = Total memory available for use, in Mbytes (less accurate); EDX = Memory reserved for the Hexagon®, in bytes; ESI = Total allocated memory (reserved+processes), in Kbytes| Unix-like | Get Detailed System Memory Usage|
| 7 | getProcesses | Memory and Process Management | No input | ESI = List of processes; EAX = Number of running processes | Unix-like | Get running processes |
| 8 | getErrorCode | Memory and Process Management | No input | EAX = Error code (0 for no error)| Hexagonix | Get the code returned by the last running process |

### File and Device Management

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 9 | open | File and Device Management | ESI = Pointer to the buffer containing the agreed name; EDI = Loading address, in case of file| CF set when device name is invalid or file does not exist | Unix-like | Opens a read/write channel on a requested device or common file present on disk (devices and disks are treated as files). In case of file on disk, a load address must be provided |
| 10 | write | File and Device Management | ESI = Pointer to the buffer containing the data | CF set on error or no device open | Unix-like | Send data to open device|
| 11 | close | File and Device Management | No input | No output | Unix-like | Closes the last device opened by the current process|
| 13 | saveFile | File and Device Management | ESI = Pointer to file name; EDI = Pointer to the content; EAX = File Size | CF defined in case of error or file already present | Unix-like | Save a file on the mounted volume|
| 14 | deleteFile | File and Device Management | ESI = Pointer to filename | CF defined in case of error or non-existing file | Unix-like | Remove a file on the mounted volume |
| 15 | listFiles | File and Device Management | No input | ESI = Pointer to list of files; EAX = Total Files | Unix-like | Get list of files present on volume |
| 16 | fileExists | File and Device Management | ESI = File name to check | EAX = File Size; CF set if file does not exist | Hexagonix | Check if a file exists on the volume |
| 17 | getDisk | File and Device Management | No input | ESI = Device name; EDI = Used volume label | Hexagonix | Get information from the mounted disk at `/`|

</details>

### User management roles and permissions

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 18 | lock | User Management and Permissions | No input | No output | Unix-like | Block foreground process termination signal by special key|
| 19 | unlock | User Management and Permissions | No input | No output | Unix-like | Enable foreground process termination signal by special key use|
| 20 | setUser | User Management and Permissions | EAX = Group ID; ESI = Username | No output | Hexagonix | Define a user for the current session|
| 21 | getUser | User Management and Permissions | No input | EAX = Group ID; ESI = Username| Hexagonix | Get logged in user data for current session |

### Hexagon Services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 22 | returnVersion | Hexagon Services | No input | EAX = Version number; EBX = Subversion number; CH = Review character; EDX = Architecture; ESI = Kernel Name; EDI = Kernel Build| Unix-like | Returns Hexagon version for applications|
| 23 | getRandom | Hexagon Services | EAX = Maximum | EAX = Number | Hexagonix | Get a random number|
| 24 | feedRandom | Hexagon Services | EAX - Number to create entropy | No output | Hexagonix | Feed Entropy to Kernel Random Number Generator|
| 25 | causeDelay | Hexagon Services | ECX = Time in count units to cause delay | No output | Hexagonix | Causes a delay in operations |
| 26 | installISR | Hexagon Services | EAX = Interrupt number; ESI = Pointer to handler | No output | Hexagonix | Install interrupt service routine|

### Power Management Services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 27 | restartPC | Power Management | No input | No output | Unix-like | Request device restart|
| 28 | shutdownPC | Power Management | No input | No output | Unix-like | Prompts for device shutdown|

</details>

### Video and graphics services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 29 | print | Video and Graphics Services | EAX = Numerical content, if this is the case, respecting the designated formats. The formats must be informed; ESI = Pointer to the string to be printed, if this is the case; EBX = Input type (01h - decimal integer; 02h - hexadecimal integer; 03h - binary integer; 04h - string)| No output | Hexagonix | Sends a defined content to an output device
| 30 | clearScreen | Video and Graphics Services | No input | No output | Hexagonix | Clear current console|
| 31 | clearLine | Video and Graphics Services | AL = Line number | No output | Hexagonix | Clears a specific line in the console|
| 33 | scrollScreen | Video and Graphics Services | No input | No output | Hexagonix | Scrolls the console down one line|
| 34 | setCursor | Video and Graphics Services | DL = position on the X axis; DH = position on the Y axis | No output | Hexagonix | Sets the cursor at a specific position |
| 35 | drawCharacter | Video and Graphics Services | EAX = position on the X axis; EBX = position on the Y axis; EDX = Color in hexadecimal | No output | Hexagonix | Puts a pixel on the console|
| 36 | drawBlock | Video and Graphics Services | EAX = position on the X axis; EBX = position on the Y axis; ESI = Length; EDI = Height; EDX = Color in hexadecimal | No output | Hexagonix | Draw a specific color block|
| 37 | printCharacter | Video and Graphics Services | AL = Character; EBX = 01h to reposition cursor | No output | Hexagonix | Print character in console at cursor position|
| 38 | setColor | Video and Graphics Services | EAX = Font color (RGB in hexadecimal); EBX = Background color (RGB in hexadecimal); ECX=1234h to change the default theme to the requested values; In text mode, only black and white are allowed | No output | Hexagonix | Set background and foreground color|
| 39 | getColor | Video and Graphics Services | No input | EAX = Font color (RGB in hexadecimal); EBX = Background color (RGB in hexadecimal); ECX=1234h to change the default theme to the requested values; In text mode, only black and white are allowed | Hexagonix | Get background and foreground color|
| 40 | getScreenInfo | Video and Graphics Services | No input | EAX = Resolution X (bits 0..15), Y (bits 16..31); EBX = Columns (bit 0..7), Rows (8..15), Bits per pixel (16..23); EDX = Address of the beginning of the video frame; CF defined in case of text mode | Hexagonix | Get current console info |
| 41 | refreshScreen | Video and Graphics Services | No input | No output | Hexagonix | Updates the primary console with content from the first virtual console|
| 42 | setResolution | Video and Graphics Services | EAX = Number relative to the resolution to be used (1 = Resolution of 800x600 pixels; 2 - Resolution of 1024x768 pixels; 3 - Change to text mode)| No output | Hexagonix | Sets the main console resolution|
| 43 | getResolution | Video and Graphics Services | No input | EAX = Number relative to the resolution to be used (1 = Resolution of 800x600 pixels; 2 - Resolution of 1024x768 pixels) | Hexagonix | They have the resolution used by the main console|
| 44 | getCursor | Video and Graphics Services | No input | DL = X axis; DH = Y axis | Hexagonix | Get cursor position|

### PS/2 Keyboard Handling Services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 45 | awaitKeyboard | PS/2 Keyboard Handling Services | No input | AL = Character; AH - Scancode | Hexagonix | Waits for a keypress on the keyboard|
| 46 | getString | PS/2 Keyboard Handling Services | AL = Maximum characters to get | EBX = Presence or absence of echo during typing (1234h for no echo and any value to activate); ESI = String | Hexagonix | Get a string from the keyboard|
| 47 | getKeyState | PS/2 Keyboard Handling Services | No input | EAX = Status of special keys (bit 0: Control key; bit 1: Shift key; bit 2-31: Reserved) | Hexagonix | Get the state of special keys such as Control and Shift|
| 48 | changeFont | PS/2 Keyboard Handling Services | ESI = Pointer to the buffer containing the name of the file containing the Hexagonix compatible font | CF defined in case of file not found or incompatible | Hexagonix | Change the default system display font|
| 49 | changeLayout | PS/2 Keyboard Handling Services | ESI = File containing a valid keyboard layout | CF defined in case of file not found or incompatible | Hexagonix | Change keyboard layout|

### PS/2 Mouse Handling Services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 50 | waitMouse | PS/2 Mouse Handling Services | No input | EAX = Position on the X axis; EBX = Position on the Y axis; EDX = Buttons | Hexagonix | Wait for mouse event|
| 51 | getMouse | PS/2 Mouse Handling Services | No input | EAX = Position on the X axis; EBX = Position on the Y axis; EDX = Buttons | Hexagonix | Get current mouse position and button state|
| 52 | setMouse | PS/2 Mouse Handling Services | EAX = Position on the X axis; EBX = Position on the Y axis | No output | Hexagonix | Set new mouse position|

### Data manipulation and conversion services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 53 | compareStringWords | Data manipulation and conversion services | ESI = First string; EDI = Second string | CF set if equal | Hexagonix | Compare first words of two strings |
| 54 | removeCharacterString | Data manipulation and conversion services | ESI = String; EAX = Character Position | No output | Hexagonix | Removes a character at a specific position from a string|
| 55 | insertCharacter | Data manipulation and conversion services | ESI = String; EDX = Position; AL = Character to insert | No output | Hexagonix | Inserts a character at a specific position in the string |
| 56 | stringSize | Data manipulation and conversion services | ESI = String | EAX = Length of string | Hexagonix | Get the length of a string|
| 57 | compareString | Data manipulation and conversion services | ESI = First string; EDI = Second string | CF set if both are equal | Hexagonix | Compare all characters in a string are equal|
| 58 | stringToUppercase | Data manipulation and conversion services | ESI = String | Converted String | Hexagonix | Convert a string to uppercase characters|
| 59 | stringToLowercase | Data manipulation and conversion services | ESI = String | Converted String | Hexagonix | Convert a string to lowercase characters|
| 60 | cutString | Data manipulation and conversion services | ESI = String | String cut | Hexagonix | Remove whitespace from string|
| 61 | findCharacter | Data manipulation and conversion services | ESI = String, AL = Character to find | EAX = Number of occurrences of the character; CF set if character not found | Hexagonix | Find specific character in string|
| 62 | stringToInt | Data manipulation and conversion services | ESI = String | EAX = Integer; CF defined in case of invalid number | Hexagonix | Convert a string number to integer |
| 63 | toString | Data manipulation and conversion services | EAX = Integer to be converted | ESI = Pointer to the buffer containing the string | Hexagonix | Converts an integer to a string|

### Sound output services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 64 | emitSound | Sound Output Services | AX = Frequency to play | No output | Hexagonix | Plays a tone on the computer's internal speaker|
| 65 | offSound | Sound Output Services| No input | No output | Hexagonix | Turns off the computer's internal speaker, stopping any sound in progress|

### Message service

| Function number | Name | Group | Input | Output | Function family| Description |
|:----------------:|:----:|:-------:|:------:|:----:|:----------------:|:---------:|
| 66 | sendMessageHexagon | Messaging Service | ESI = Message; EAX = Error code, if any; EBX = Priority | No output | Hexagonix | Sends a high priority message from Hexagon|

### Real-time clock services

| Function number | Name | Group | Input | Output | Function family| Description |
|:---------------:|:----:|:-----:|:-----:|:------:|:--------------:|:-----------:|
| 67 | returnDate | Real Time Clock Services | EAX = Day, in ASCII; EBX = Month, in ASCII; ECX = Century, in ASCII; EDX = Year, in ASCII | No output | Hexagonix | Returns real-time clock information in ASCII (String) format. Conversion to number may be required|
| 68 | returnTime | Real Time Clock Services | EAX = Time, in ASCII; EBX = Minute, in ASCII; ECX = Second, in ASCII | No output | Hexagonix | Returns real-time clock information in ASCII (String) format. Conversion to number may be required|

</div>
