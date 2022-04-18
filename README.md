# Monarch OS

Monarch OS is an open source 6502 operating system. It is designed to be portable, so should be relatively easily ported between different computer systems. 

The OS will use a BIOS/CCP model, similar to CP/M and is nominally designed to fit in 64K of RAM, with 32K for user programs and (up to) 32K for ROM and I/O space. 

It is our goal to prototype the operating system on the Commander X16, but it should be modular enough to run on any 6502 system with at least 32K of RAM and 16K of 
ROM space. Other geometries are possible, including banking systems that allow for larger memory models.

## Memory 

The memory map is hardware-dependent. The baseline system looks like this:
* RAM from 0 to approx 32-48K
* I/O space
* 8-16KB of ROM space. (8K for systems with smart drives. 16K+banking for on board SD card storage.)

## BIOS

The system will feature a BIOS, much like CP/M and DOS computers. This is will handle:
* Displaying text on the screen
* Keyboard input
* Disk and tape drive access
* Memory management and banking (on >64K systems)
* Expect a machine monitor with the standard features

## MCP: Monarch Command Processor
The MCP, or Monarch Command Processor, will be the default user interface and is roughly similar to
COMMAND.COM on a DOS system.

* MCP will be located in ROM with the BIOS by default (but loadable from disk on small ROM systems)
* Run a program by typing the program name, with optional arguments: `BASIC HELLO` would load the BASIC interpreter and the `HELLO.BAS` program
* List files with DIR
* Delete files with DEL 
* Make and navigate subdirectires with CD, MD, and RD
* IF EXISTS *filename* *command* executes a command if a file or directory exists. 
* IF NOT EXISTS *filename* *command* executes a command if a file or directory does not exist.
* IF ERRORLEVEL *number* *command* executes a command if the last command set the return value >= *number*.
* Program files should have a PRG extension on file systems that do not have an intrinsic file type (ie: CBM-DOS)

## File System and I/O
* The file system will be based on the computer running the OS. IE: an Apple II will use the PRODOS file system, 
and a Commodore will use CBM's file system.
* Filenames can contain any character valid for the host file system. 
* Filenames with spaces must be enclosed with quotes. ie: README.TXT or "READ ME"
* If quotes are valid in the host file system, escape them with \ (£ on Commodore)
* Filename extensions (ie: .PRG, .TXT) are accepted, but do not have any specific meaning.

## Disk and I/O
* The file system and other I/O devices use distinct APIs for each interface. 
* A file-based interface will be provided for convenience. 
* The system will not use device numbers or the Commodore style secondary address. Where they are required for hardware
level access, these will be abstracted in the BIOS. Appropriate APIs will be provided for open-file, disk-command, etc.

## Character set and graphics
* The system will recognize 7-bit ASCII text. Character codes 128-255 are user definable and are not limited to any specific code page or PETSCII glyphs.
* Several character sets may be included, to allow for IBM PC (ie CP437), PETSCII, psuedo-PETSCII, Atari, and Tandy graphic character sets as font files.
* Line endings: the default is CR-only, but CR-LF and LF-only will be supported (probably via an escape sequence).
* Expect terminal codes for cursor and color control, probably a subset of VT-52 or VT-102. (VT-52 is faster and simpler to code. VT-102 is used much more widely.)

