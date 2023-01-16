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

# Hexagon system calls

Hexagon provides a series of well-documented system calls for developing utilities and applications. System calls can be accessed through software interrupt number `69h`. When requesting a system call, you must also supply parameters that identify the chosen function, as well as the required parameters for it. You can find the required parameters for them in the table below.

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

Now, a table with the Hexagonix system call functions. `The table is formatted as a file containing x86 Assembly code`:

> Remember that a table of functions, standardized according to the functions available in Version 7 UNIX, is being developed. In this case, there is no purpose of function number pairing with UNIX, but conformity in function names. For example, `allocateMemory` would become `free`, and `returnVersion`, `uname`. In the future, both nomenclatures will be available to allow migration of applications and utilities. Come back to this file later to check for updates.


```assembly

;;************************************************************************************
;;
;; Hexagonix® memory and process management services
;;
;;************************************************************************************

allocateMemory = 1     ;; Allocate memory
                       ;; Input: EAX  - Requested memory size, in bytes
                       ;; Output: EBX - Pointer to allocated memory

releaseMemory = 2      ;; Free memory
                       ;; Input: EBX - Pointer to allocated memory
                       ;;        ECX - Size of allocated memory

startProcess = 3       ;; Load program from disk and run it
                       ;; Input: ESI - Program name; EDI - Arguments; EAX = 0 if no arguments are passed
                       ;; Output: CF defined in case of error or program not found

killProcess = 4        ;; Terminate the currently running process
                       ;; Input: EAX - Error code, if any
                       ;; EBX = 0 if just terminate execution; EBX = 0x1234 to keep resident

getPID = 5             ;; Returns the identifier of the running process
                       ;; Output: EAX - Process PID

memoryUse = 6          ;; Returns usage statistics for main memory
                       ;; Output: EAX - Used memory, in bytes
                       ;;         EBX - Total memory available for use, in bytes
                       ;;         ECX - Total memory available for use, in Mbytes (less accurate)
                       ;;         EDX - Memory reserved for the Hexagon®, in bytes
                       ;;         ESI - Total allocated memory (reserved+processes), in kbytes
                                  
getProcesses = 7       ;; Get the running processes
                       ;; Output: ESI - List of processes; EAX - Number of processes running

getErrorCode = 8       ;; Gets the code returned by the last running process.
                       ;; Output: EAX - Error code (0 for no error/normal output)

;;************************************************************************************
;;
;; Hexagonix® File and Device Management Services
;;
;;************************************************************************************

open = 9               ;; Opens a read/write channel with certain requested device or
                       ;; ordinary file present on disk (devices and disks are treated as
                       ;; files). In case of file on disk, a load address must be
                       ;; provided.
                       ;; Input: ESI - Pointer to the buffer containing the agreed name
                       ;; EDI - Loading address, in case of file
                       ;; CF set when device name is invalid or file does not exist

write = 10             ;; Send data to open device
                       ;; Input: ESI - Pointer to the buffer containing the data
                       ;; Output: CF set on error or no device open
                       ;; Notice! In the future, it will be used to save files.

close = 11             ;; Closes the last opened device

;;************************************************************************************
;;
;; Hexagonix® File System and Volume Management Services
;;
;;************************************************************************************

saveFile = 13          ;; Save a file to disk
                       ;; Input: ESI - Pointer to file name; EDI - Pointer to the content
                       ;; Input: EAX - File Size
                       ;; Output: CF defined in case of error or file already present
                         
deleteFile = 14        ;; Remove a file from disk
                       ;; Input: ESI - Pointer to file name
                       ;; Output: CF defined in case of error or file not existing

listFiles = 15         ;; Get file list
                       ;; Output: ESI - Pointer to list of files; EAX - Total files
                                  
fileExists = 16        ;; Check if a file exists on disk
                       ;; Input: ESI - Name of file to check
                       ;; Output: EAX - File Size
                       ;; CF set if file does not exist

getDisk = 17           ;; Get the disk used by the system
                       ;; Output: ESI - Device Name
                       ;;         EDI - Volume label

;;************************************************************************************
;;
;; Hexagonix® User Management Services
;;
;;************************************************************************************

lock = 18              ;; Blocks the foreground process, preventing it from being terminated
                       ;; by the user using a special key or combination.
                       ;; The F1 key is the "Kill process" key on Hexagonix®.
                       ;; This key is analog to CTRL-C in Unix-like operating systems

unlock = 19            ;; Enables the user to kill the running process by pressing a special key
                       ;; or key combination. The "Kill process" key (F1) becomes enabled.                
                                   
defineUser = 20        ;; Defines a user for the session.
                       ;; Input: EAX - Group ID; ESI - User name

getUser = 21           ;; Get data from the user logged into the session
                       ;; Output: EAX - Group ID; ESI - User name

;;************************************************************************************
;;
;; Services offered by Hexagonix®
;;
;;************************************************************************************

returnVersion = 22     ;; Returns the system version
                       ;; Output: EAX - Version number; EBX - Subversion number
                       ;;         CH - Review character; EDX - Architecture
                       ;;         ESI - Kernel Name
                       ;;         EDI - Kernel Build

getRandom = 23 ;       ;; Get a random number
                       ;; Entry: EAX - Maximum
                       ;; Output: EAX - Number

feddRandom = 24        ;; Feed the number generator
                       ;; Entry: EAX - Number

causeDelay = 25        ;; Used to cause a delay, used to adapt operations
                       ;; of memory, disk operations and enable screen reading by user
                       ;; Input: ECX - Time in counting units to cause delay
 
installISR = 26        ;; Install interrupt service routine
                       ;; Input: EAX - Interrupt number; ESI - Pointer to handler

;;************************************************************************************
;;
;; Hexagonix® Power Management Services
;;
;;************************************************************************************

restartPC = 27         ;; Restart the computer
                    
shutdownPC = 28        ;; Shut down the computer

;;************************************************************************************
;;
;; Hexagonix® video and graphics output services
;;
;;************************************************************************************


print = 29             ;; Print defined content to an output device 
                       ;;
                       ;; Input:
                       ;;
                       ;; EAX - Numerical content, if this is the case, respecting the formats designated below. Formats must be informed!
                       ;; ESI - Pointer to the string to be printed, if this is the case.
                       ;; EBX - Input type, which can be:
                       ;; 01h - Decimal integer
                       ;; 02h - hexadecimal integer
                       ;; 03h - Binary integer
                       ;; 04h - String
                       ;; Tip! Use the macros at the end of the file to use this function

clearScreen = 30       ;; Clear the current console
                        
clearLine = 31         ;; Clears a specific line on the console
                       ;; Input: AL - Line number
                        
NULL = 32              ;; Null function, no return or function
                       ;; Kept for compatibility

scrollScreen = 33      ;; Scroll down one line

setCursor = 34         ;; Set cursor at a specific position
                       ;; Input: DL - X; DH - Y

drawCharacter = 35     ;; Put a pixel on the screen
                       ;; Input: EAX - X; EBX-Y; EDX - Color in hexadecimal

drawBlock = 36         ;; Draw a specific color block
                       ;; Input: EAX - X; EBX-Y; ESI - Length
                       ;; Input: EDI - Height; EDX - Color in hexadecimal


printCharacter = 37    ;; Print character at cursor position
                       ;; Input: AL - Character; EBX - 01h to position cursor

setColor = 38          ;; Set background and foreground color
                       ;; Input: EAX - Font color (RGB in hexadecimal)
                       ;;        EBX - Background color (RGB in hexadecimal)
                       ;;        ECX - 1234h to change to default theme
                       ;; In text mode, only black and white are allowed
                         
getColor = 39          ;; Get background and foreground color
                       ;; Output: EAX - Background (RGB in hexadecimal)
                       ;; EBX - Background (RGB in hexadecimal)
                       ;; ECX - Default font color, in current theme
                       ;; EDX - Default background color, in current theme
                       ;; In text mode, only black and white are allowed

getScreenInfo = 40     ;; Get screen info
                       ;; Output: EAX - Resolution X (bits 0..15), Y (bits 16..31),
                       ;;         EBX - Columns (bit 0..7), Rows (8..15), Bits per pixel (16..23),
                       ;;         EDX - Address of start of video frame
                       ;;         CF defined in case of text mode
                                        
updateScreen = 41      ;; Update video memory with buffer contents

defineResolution = 42  ;; Used to define the resolution to be used in the video
                       ;; Input: EAX - Number related to the resolution to be used
                       ;; 1 - Resolution of 800x600 pixels
                       ;; 2 - Resolution of 1024x768 pixels
                       ;; 3 - Change to text mode

getResolution = 43     ;; Used to obtain the code related to the used resolution
                       ;; in standard video
                       ;; Output: EAX - Number related to the currently used resolution
                       ;; 1 - Resolution of 800x600 pixels
                       ;; 2 - Resolution of 1024x768 pixels
                                   
getCursor = 44         ;; Get cursor position
                       ;; Output: DL - X, DH - Y
                                            
;;************************************************************************************
;;
;; Hexagonix® PS/2 Keyboard Handling Services
;;
;;************************************************************************************

awaitKeyboard = 45     ;; Wait for a key press on the keyboard
                       ;; Output: AL - Character; AH - Scan code
                              
getString = 46         ;; Get a string from keyboard 
                       ;; Input: AL - Maximum characters to receive
                       ;;        EBX - Presence or not of echo during typing - 1234h to disable echo and <> 1234h to enable
                       ;; Output: ESI - String


getKeyState = 47       ;; Get status of special keys
                       ;; Output: EAX - Status of special keys
                       ;;
                       ;; Format:
                       ;;
                       ;; bit 0: Control key
                       ;; bit 1: Shift key
                       ;; bit 2-31: Reserved

changeSource = 48      ;; Change the default system display font
                       ;; Input: ESI - Pointer to buffer containing file name that contain the font compatible with the Hexagonix® Operating System
                       ;; Output: CF defined in case of file not found

change Layout = 49     ;; Change keyboard layout
                       ;; Input: ESI - File containing a valid keyboard layout

;;************************************************************************************
;;
;; Hexagonix® PS/2 Mouse Handling Services
;;
;;************************************************************************************

waitMouse = 50         ;; Wait for mouse event
                       ;; Output: EAX - X; EBX-Y; EDX - Buttons

getMouse = 51          ;; Get current mouse position and state of buttons
                       ;; Output: EAX - X; EBX-Y; EDX - Buttons

setMouse = 52          ;; Set new mouse position
                       ;; Input: EAX - X; EBX Y

;;************************************************************************************
;;
;; Hexagonix® data manipulation and conversion services
;;
;;************************************************************************************

compareWordsInString = 53   ;; Compare first words of two strings
                            ;; Input: ESI - First string; EDI - Second string
                            ;; Output: CF set if equal

removeCharacterString = 54  ;; Remove a character from a specific position in the string
                            ;; Input: ESI - String; EAX - Character position

insertCharacter = 55     ;; Insert a character at a specific position in the string
                         ;; Input: ESI - String; EDX - Character to insert; AL - Character to insert

sizeString = 56          ;; Get the length of a string
                         ;; Input: ESI - String.
                         ;; Output: AX - String Size

compareString = 57       ;; Compare two strings
                         ;; Input: ESI - First string; EDI - Second string
                         ;; Output: CF set if both are equal

stringToUppercase = 58   ;; Convert string to uppercase
                         ;; Input: ESI - String

stringTolowercase = 59   ;; Convert string to lowercase
                         ;; Input: ESI - String

cutString = 60           ;; Remove whitespace from string
                         ;; Input: ESI - String.

findCharacter = 61       ;; Find specific character in string
                         ;; Input: ESI - String, AL - character to find
                         ;; Output: EAX - Number of occurrences of the character
                         ;; CF set if character not found
                              
stringToInt = 62         ;; Convert a number in an string to integer
                         ;; Input: ESI - String
                         ;; Output: EAX - Integer
                         ;; CF set in case and invalid number

intToString = 63         ;; Converts an integer to a string
                         ;; Input: EAX - Integer to be converted
                         ;; Output: ESI - Pointer to the buffer containing the contents

;;************************************************************************************
;;
;; Hexagonix® Sound Output Services
;;
;;************************************************************************************

soundOn = 64           ;; Plays a tone from the computer's internal speaker
                       ;; Input: AX - Frequency to be reproduced

soundOff = 65          ;; Turns off the computer's internal speaker, interrupting
                       ;; any sound in progress

;;************************************************************************************
;;
;; Hexagonix® Messaging Services
;;
;;************************************************************************************

sendMessageHexagon = 66    ;; Sends a high priority message from Hexagon
                           ;; Entry: ESI - Message
                           ;;        EAX - Error code, if any
                           ;;        EBX - Priority                                                 
```

</div>