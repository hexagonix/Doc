<!--

Copyright (c) 2015-2026 Felipe Miguel Nery Lunkes
Todos os direitos reservados.

-->

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

# HXBI, the Hexagon Boot Information block

<div align="center">

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/HexagonixSourceHeader.png">

</div>

HXBI is the format HBoot uses to describe the machine to Hexagon, and it is the only thing the kernel receives when it starts. Until Hexagon 1.8.0 the boot environment arrived in four registers: `CX` carried the amount of RAM HBoot had already measured, `BL` the drive the system booted from, `EBP` a pointer to the boot sector and `ESI` a pointer to HBoot's command line buffer. Everything else the kernel needed it obtained by itself, in real mode, before building its own GDT and switching to protected mode. Enabling the A20 line and asking the BIOS for the installed memory through function `E801h` were both done twice, once in HBoot and again in Hexagon.

The four registers were replaced by a single block with a header and a list of tags. HBoot assembles that block while it is still in real mode, where the BIOS is available and where it is already reading the disk anyway, switches the processor to 32 bit protected mode itself and jumps to the kernel with the address of the block in `EBX`. Hexagon no longer has real mode code of any kind, and makes no BIOS calls at any point of its execution.

The name comes from the signature at the start of the block. The current revision is 1, produced by `HBoot/hxbi.asm` and consumed by `kern/hxbi.asm`.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

## State of the machine at kernel entry

Three conditions hold when HBoot jumps to the kernel, and the kernel assumes nothing beyond them:

* The processor is in 32 bit protected mode.
* The A20 line is enabled.
* `EBX` holds the physical address of a valid HXBI block.

HBoot gets the processor that far with a transitional GDT, in `HBoot/pmode.asm`, built to survive the far jump and nothing more. Hexagon installs its own GDT and IDT before touching anything else, since the descriptors it expects (`08h` for code, `10h` and `18h` for data, all of them flat and based at zero) are the ones every other file in the kernel is written against. The stack HBoot leaves behind is its own real mode stack, read through a flat selector, so the kernel points `ESP` at its reserved area before the first call it makes.

The block itself lives inside HBoot's segment, at `0x1000:0000`, and that memory is not reserved from the kernel allocator. Anything Hexagon wants to keep from the block has to be copied out of it before the first disk access, which is what `kern.asm` does with the boot sector.

## The header

The block opens with sixteen bytes at fixed offsets. They are the only part of the format that can be reached without walking the list.

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `00h` | 4 | signature | The four characters `HXBI`. A kernel that does not find them refuses the block, instead of trusting whatever `EBX` happens to hold |
| `04h` | 4 | version | Revision of the format. Revision 1 is the only one that exists, and the kernel refuses any other, instead of guessing at the layout |
| `08h` | 4 | totalSize | Size of the whole block in bytes, this header included. The tag walk stops here, so a malformed list cannot run off the end |
| `0Ch` | 4 | flags | Bit 0 set means HBoot confirms the A20 line is enabled. The remaining bits are reserved and always zero |

The tag list starts at offset `10h`.

## The tag list

Each tag opens with eight bytes, a type and a size, and the size counts those eight bytes along with the payload. A reader that does not know a type can still move past it, because the size alone says where the next tag begins. Tags start on four byte boundaries, so walking means rounding the size up to a multiple of four before adding it. The list ends at a tag of type 0.

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `+00h` | 4 | type | What the payload holds, from the table below |
| `+04h` | 4 | size | Length of this tag, header and payload together |

The types HBoot writes today:

| Type | Tag | Size | Written when |
|:----:|:----|:----:|:-------------|
| 0 | End of list | 8 | Always, as the last tag |
| 1 | Loader information | 28 | Always |
| 2 | Memory map | 16 plus 20 per region | Always |
| 3 | Boot device | 524 | Always |
| 4 | Command line | 264 | Always |
| 5 | Framebuffer | 32 | Always, zeroed if no VBE mode was set |

Offsets in the tables below are counted from the start of the tag, so a payload field at `+08h` is the first byte after the tag header.

### Loader information, type 1

Says what loaded the kernel and which version of it did so. Hexagon skips this tag today, and it exists so a boot log can name the loader once there is somewhere to write it.

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `+08h` | 4 | loaderVersion | Packed version, `00010000h` for 1.0 |
| `+0Ch` | 16 | loaderName | Name of the loader in ASCII, padded with zeros. HBoot writes `HBoot` |

### Memory map, type 2

The region list the BIOS returns through function `E820h`, copied over as it is. HBoot collects up to 32 regions into `HBoot.Memory.E820.entries` and drops the four byte ACPI attribute `E820h` adds to each one, leaving the twenty bytes that matter.

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `+08h` | 4 | entrySize | Size of one region, 20 bytes in revision 1. A later revision can grow the region without moving any field before it |
| `+0Ch` | 4 | entryCount | Number of regions that follow |

Each region, repeated `entryCount` times from `+10h`:

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `+00h` | 8 | base | Physical address the region starts at |
| `+08h` | 8 | length | Length of the region in bytes |
| `+10h` | 4 | type | 1 usable, 2 reserved, 3 ACPI reclaimable, 4 ACPI NVS, 5 defective. The numbering is the one `E820h` uses, passed through without translation |

### Boot device, type 3

The drive the system booted from and the sector Saturno read the BPB out of. The sector travels inside the tag, and not as a pointer into memory HBoot is about to abandon.

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `+08h` | 1 | driveNumber | Index of the drive, matched against the disk table Hexagon builds for itself |
| `+09h` | 3 | reserved | Padding, always zero |
| `+0Ch` | 512 | bootSector | The boot sector, byte for byte |

### Command line, type 4

The parameters typed at the HBoot command line, from `HBoot.Parameters.readBuffer`. That buffer is 64 bytes, and the tag reserves 256 so the format is not what limits a parameter's length later on. The bytes past the first 64 are zero.

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `+08h` | 256 | text | Parameters in ASCII, terminated by a zero within the 256 bytes |

### Framebuffer, type 5

HBoot reads the wanted resolution from `/boot/boot.cfg`, in the same `key=value` form `/etc/rc` uses, sets the matching VBE mode and reads the mode information block back for the address, pitch and depth the card actually gave it. Hexagon builds its graphics console from this tag alone, and cannot set a video mode by itself. Changing the resolution means editing that line and rebooting, which is what the `config` utility does.

If `HBoot.VBE.setupVBE` finds no mode matching the requested resolution, the tag is written with every field zeroed.

| Offset | Size | Field | Meaning |
|:------:|:----:|:-----:|:--------|
| `+08h` | 8 | address | Physical address of the linear framebuffer. The high half is always zero, since the framebuffer sits below 4 GB |
| `+10h` | 4 | pitch | Bytes from the start of one scanline to the start of the next |
| `+14h` | 4 | width | Width in pixels |
| `+18h` | 4 | height | Height in pixels |
| `+1Ch` | 1 | bpp | Bits per pixel |

### End of list, type 0

Carries no payload and its size is always 8. A block holding nothing but a header and this tag is still valid.

## What Hexagon does with each tag

`Hexagon.HXBI.parse` runs before anything else in the kernel, right after the GDT goes in. It checks the signature and the revision, panics if either is wrong, and walks the list until the end tag or until the cursor reaches `totalSize`.

| Tag | What the kernel does with it |
|:----|:-----------------------------|
| 1 | Skipped |
| 2 | Adds up the length of every region of type 1 and stores the total in `Hexagon.Memory.totalMemory`. Only the low half of each length is read, since the kernel is 32 bit. Reserved regions are not consulted by the allocator yet |
| 3 | Stores the drive in `Hexagon.Dev.Disk.Control.bootDisk` and points `Hexagon.Memory.addressBPB` at the embedded sector. `kern.asm` then copies those 512 bytes into `Hexagon.Memory.BPBCopy` and repoints `addressBPB` at the copy, before any disk access, because the HXBI block is not reserved from the allocator and `Hexagon.Heap.DiskCache` reaches into it during a normal mount |
| 4 | Copies the whole 256 bytes into `Hexagon.Boot.Parameters.commandLine`. Nothing in the kernel reads that buffer yet |
| 5 | Stores the address, pitch, width, height and depth in `Hexagon.Boot.Framebuffer`, which the console and the graphics library read from then on |

A tag of any other type is skipped by its own size. A size smaller than the eight byte header would leave the cursor in place and walk the same tag forever, so the kernel treats it as a malformed block and panics.

## Adding a tag

A new tag is a new type number and a payload, appended before the end tag in `HBoot.HXBI.buildBlock`. Nothing else in the format moves, and a kernel built before the type existed walks past it on its size without noticing. This is the reason for a list of tags instead of a fixed structure: a structure would have to put each new field at some offset, and every pair of HBoot and Hexagon built before that point would read the wrong bytes there.

The same holds in the other direction. A loader that does not produce a tag simply leaves it out, and the kernel finds nothing where it would have been.

## What a UEFI loader would change

Nothing on the kernel side. A loader running under UEFI fills in the same header and the same tags, taking the regions from `GetMemoryMap()` instead of `E820h` and the framebuffer from GOP instead of VBE. The entry point and the list walk stay as they are.

<!-- Vai funcionar como <hr> -->

<img src="https://raw.githubusercontent.com/hexagonix/Doc/refs/heads/main/Img/hr.png" width="100%" height="2px" />

<details title="Hexagon license" align='left'>
<summary align='left'>Hexagon License</summary>
<br>

<div align="justify">

Please read the [license](https://github.com/hexagonix/Doc/blob/main/LICENSES/BSD-3) for more information about copyright, code ownership, and redistribution that applies to files available in this repository. Hexagonix is ​​fully licensed under [BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause). Always pay attention to the `LICENSE` file available in each repository to be aware of legal rights and obligations, as well as the list of project contributors.

</div>

</details>

</div>
