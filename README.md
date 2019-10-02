# Commander X16 BASIC/KERNAL/DOS/GEOS ROM

This is the Commander X16 ROM containing BASIC, KERNAL, DOS and GEOS. BASIC and KERNAL are derived from the [Commodore 64 versions](https://github.com/mist64/c64rom). GEOS is derived from the [C64/C128 version](https://github.com/mist64/geos).

* BASIC is fully compatible with Commodore BASIC V2.
* KERNAL
	* supports the complete $FF81+ API.
	* has the same zero page and $0200-$033C memory layout as the C64.
	* does not support tape (device 1).
* GEOS is fully compatible with the C64 version.

## Releases and Building

Each [release of the X16 emulator](https://github.com/commanderx16/x16-emulator/releases) includes a compatible build of `rom.bin`. If you wish to build this yourself (perhaps because you're also building the emulator) see [`BUILD.md`](BUILD.md).

> __WARNING:__ The emulator will work only with a contemporary version
> of `rom.bin`; eariler or later versions are likely to fail.

## New Features

* F-keys:
	F1: `LIST`
	F2: `MONITOR`
	F3: `RUN`
	F4: &lt;switch 40/80&gt;
	F5: `LOAD`
	F6: `SAVE"`
	F7: `DOS"$`
	F8: `DOS`
* New BASIC instructions
	* `MONITOR`: see below.
	* `DOS`:
	no argument: read disk status.
	"8" or "9" as an argument: switch default drive.
	"$" as an argument: show directory.
	all other arguments: send DOS command
	* `VPEEK`(bank, offset), `VPOKE` bank, offset, value to access video memory. "offset" is 16 bits, "bank" is bits 16-19 of the linear address.
	Note that the tokens for the new BASIC commands have not been finalized yet, so loading a BASIC program that uses the new keywords in a future version of the ROM will break!
* Support for `$` and `%` in BASIC expressions for hex and binary
* `LOAD` prints the start and end(+1) addresses
* Integrated Monitor derived from the [Final Cartridge III](https://github.com/mist64/final_cartridge).
	* `O00`..`OFF` to switch ROM and RAM banks
	* `OV0`..`OV4` to switch to video address space
* FAT32-formatted SD card as drive 8 as a full IEC (TALK/LISTEN & CBM DOS) compatible device:
	* read directory
	* load file
	* send "I" command
	* read status
	* everything else is unimplemented
* Some new KERNAL APIs (to be documented)

## Big TODOs

* DOS needs more features.
* BASIC needs more features.
* RS232 and IEC are not working.
* PS/2 and SD have issues on real hardware.

## ROM Map

|Bank|Name   |Description                                            |
|----|-------|-------------------------------------------------------|
|0   |BASIC  |BASIC interpreter                                      |
|1-3 |–      |*[Currently unused]*                                   |
|4   |GEOS   |GEOS KERNAL                                            |
|5   |CBDOS  |The computer-based CBM-DOS for FAT32 SD cards          |
|6   |KEYMAP |Keyboard layout tables                                 |
|7   |KERNAL |character sets (uploaded into VRAM), MONITOR, KERNAL   |

## RAM Map

* fixed RAM:
	* $0000-$0400 KERNAL/BASIC/DOS system variables
	* $0400-$0800 currently unused
	* $0800-$9F00 BASIC RAM
* banked RAM:
	* banks 0-254: free for applications
	* bank 255: DOS buffers and variables


## Credits

KERNAL, BASIC and GEOS additions, DOS: Michael Steil, [www.pagetable.com](https://www.pagetable.com/); 2-clause BSD license

FAT32 and SD card drivers: Copyright (c) 2018 Thomas Woinke, Marko Lauke, [www.steckschein.de](https://steckschwein.de); MIT License

## Release Notes

### Release 31

* correct ROM banking:
	* BASIC and KERNAL now live on separate 16 KB banks ($C000-$FFFF)
	* BASIC "PEEK" will always access KERNAL ROM
	* BASIC "SYS" will have BASIC ROM enabled
* added GEOS
* added OLD statement to recover deleted BASIC program after NEW or RESET
* removed software RS-232, will be replaced by VERA UART later
* Full ISO mode support in Monitor

### Release 31

* switched to VERA 0.8 register layout; character ROM is uploaded on startup
* ISO mode: ISO-8859-15 character set, standard ASCII keyboard
* keyboard
	* completed US and UK keymaps so all C64 characters are reachable
	* support for AltGr
	* support for F9-F12
* allow hex and binary numbers in DATA statements [Frank Buss]
* switched SD card from VIA SPI to VERA SPI (works on real hardware!)
* fix: VPEEK overwriting POKER ($14/$15)
* fix: STOP sometimes not registering in BASIC programs

### Release 30

* support for 13 keyboard layouts; cycle through them using F9
* GETJOY call will fall back to keyboard (cursor/Ctrl/Alt/Space/Return), see Programmer's Reference Guide on how to use it
* startup message now shows ROM revision
* $FF80 contains the prerelease revision (negated)
* the 60 Hz IRQ is now generated by VERA VSYNC
* fix: VPEEK tokenization
* fix: CBDOS was not correctly preserving the RAM bank
* fix: KERNAL no longer uses zero page $FC-$FE
