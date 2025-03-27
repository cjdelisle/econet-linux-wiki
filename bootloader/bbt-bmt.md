---
title: NAND BBT/BMT (Bad Block Management)
description: The bad block management tables used by the EcoNet bootloader
published: true
date: 2025-03-27T15:19:07.107Z
tags: 
editor: markdown
dateCreated: 2025-03-21T18:26:45.788Z
---

# NAND BBT/BMT

On NAND-based devices using the EcoNet bootloader, there is a bad block management system to map out defective NAND flash blocks. This system reserves a number of blocks at the end of the flash device to replace any blocks that fail over time or are defective from the factory. The system uses two key tables: the **Bad Block Table (BBT)** for factory defects and the **Block Mapping Table (BMT)** for blocks that wear out during use.

Any operating system mounting partitions on this flash **must** implement the same block mapping scheme to avoid conflicts with the bootloader. The **BBT** changes the address of any block after an identified bad block, so an OS which does not implement this can't know where the bootloader believes is important data like the A/B dual partition boot flag, let alone update its kernel in a way that the bootloader can load.

## Overview

The EcoNet NAND flash is divided into two main regions:
- **User Area**: Contains usable data blocks, reduced by factory bad blocks (BBT) and worn blocks (mapped via BMT).
- **Reserve Area**: A pool of blocks at the end of the flash used for remapping worn blocks and storing the BBT and BMT tables.

### Reserve area size
The reserve area size is determined by counting backwards from the end of the flash until 8% of the total blocks have been counted. However, if a block in the reserve area appears to be bad, it doesn't count, so the beginning of the reserve area may change over time. Some versions of the firmware specify a limit on the size of the reserve area, bad blocks notwithstanding. This has to be checked on a per-device basis.

### Bad Block Table (BBT)
The very beginning of the reserve area is where the BBT is placed (on the first non-bad block).

- **Purpose**: Tracks blocks marked as bad at the factory.
- **Behavior**: Factory bad blocks (FBBs) are permanently excluded from the user area. When a block is added to the BBT, all subsequent blocks shift up by one, reducing the usable space.
- **Constraints**: The BBT cannot be modified after initialization without loss of data because it alters the offsets across the entire flash.
- **Storage**: Located at the start of the reserve area (lowest usable block).
- **Size**: Up to 1000 entries (MAX_BBT_SIZE), stored in ascending order with a 16-bit checksum.

The format of the bad block table is as follows:

```c
struct bbt_table_header {
	/* "RAWB" */
	char signature[4];
	/* bbt_checksum() (only 16 bits used) */
	u32 checksum;
	/* 1 */
	u8 version;
	/* Number of bad blocks in table */
	u8 size;
	/* Unused (ffff) */
	u8 reserved[2];
};
```

Following this header is 1000 `u16` entries which are the indexes of the bad blocks. The list is sorted ascending, but zero-padded following the last meaningful entry. The checksum covers all 1000 entries, so unused entries must be zeroed.

### Block Mapping Table (BMT)
- **Purpose**: Manages blocks that wear out over time by mapping them to reserve blocks.
- **Behavior**: Worn blocks are replaced with free blocks from the reserve pool, maintaining usable space without shifting offsets like the BBT.
- **Storage**: Located at the end of the reserve area (highest usable block).
- **Size**: Up to 256 mappings (MAX_BMT_SIZE), with an 8-bit checksum.

### Layout Example
The layout of the flash is something like this:

```
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|D D D D F D D D D D D D W D D D D D D D D F D D|B M M 0 0 0 0 0 T|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     User Area                 |   Reserve Area  |
```

The letter-identified blocks are defined as follows:

- **D**: Data block
- **F**: Factory bad block (BBT)
- **W**: Worn block (mapped via BMT)
- **B**: BBT block
- **M**: Mapped block (replacing a worn block)
- **0**: Free block
- **T**: BMT block

In this example, the user area has 22 usable blocks (excluding two factory bad blocks), reporting a capacity of 2816 KiB (assuming 128 KiB blocks). The reserve area dynamically adjusts its starting point (and therefore the location of the BBT) to ensure a minimum number of good blocks (8% of total blocks).

## Operational Mechanics

### Table Reconstruction
- **BMT Reconstruction**: Achieved by scanning the reserve area for blocks with out-of-band (OOB) data containing back-references to mapped user blocks.
- **BBT Reconstruction**: After BMT reconstruction, remaining unmapped blocks are scanned. Blocks which are marked bad (typically 0x00 in OOB) or which fail to read are assumed to be factory defects. This is obviously error-prone since freshly worn blocks, or transient errors will cause a block to be assumed a factory defect and all offsets after it will be adjusted.

### Remapping Process
- When a block fails to read, or shows sufficient ECC corrected errors to be worth replacing:
  1. A free block is selected from the reserve pool.
  2. Data is copied (if possible) to the new block.
  3. The block is marked in it's OOB data area with a back-reference to the worn out block (allowing reconstruction of the BMT).
  4. The BMT is updated with the new mapping.
  5. The worn block is marked bad.
- Remapping can occur during reads or writes if enabled, but if the copy fails, data will be silently lost, the user will suddenly begin reading the newly erased remap block.

## Drivers
There is a driver available out of tree in [en75_bmt.c](https://github.com/cjdelisle/openwrt/blob/new-platform-en75/target/linux/en75/files/drivers/mtd/nand/en75_bmt.c) based on the MediaTek BMT framework.

This driver attempts to mitigate some of the most dangerous bugs, but still is incomplete:
* Rewriting of the BBT is disabled by default unless flagged in the Device Tree. This prevents the amount of damage that can be done, but remember, the bootloader will happily rewrite the BBT.
* Remapping must be enabled in the Device Tree, so you can choose which partitions it will remap. Generally speaking, you should not enable remapping on a partition unless:
  1. Your OS can write to it. Read only partitions like the bootloader should never be remapped. Or if they should, it's not your business to do it.
  2. There is no other (better) remapping system in place. Partitions like UBI should not enable remapping because the UBI system will naturally do it much more effectively than this driver.
* Worn blocks are marked bad by setting the mark to `0x55` rather than `0x00`. To this vendor implementation there is no difference, but in case of rebuilding the BBT, these blocks will not be confused with factory bad blocks even if the mapping block goes bad or disappears.

But this driver still has bugs and limitations:
* If a block is remapped during read and the copy operation fails, the user will read a newly erased block instead of getting what they expected *or* getting an error.
* If the user stores data at the end of the user space and then a block wears out in the reserve space, the user data will become inaccessible after the reserve area boundary is moved.
* If run on a device where the bootloader has a hardcoded maximum reserve area size, this driver may create mappings or store the BBT out of the range where the bootloader can find them. This might happen by surprise when a block in the reserve area wears out.
* 8% of the entire disk is reserved for block mappings, even if the majority of the disk is UBI and therefore has its own mapping system.


