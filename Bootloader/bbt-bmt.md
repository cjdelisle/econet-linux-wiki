---
title: NAND BBT/BMT (Bad Block Management)
description: The bad block management tables used by the EcoNet bootloader
published: true
date: 2025-03-21T18:26:45.788Z
tags: 
editor: markdown
dateCreated: 2025-03-21T18:26:45.788Z
---

# NAND BBT/BMT
On NAND based devices, the EcoNet bootloader and SDK factory OS reserves a certain number of blocks at the end of the flash device to be mapped in place of any blocks that fail.

Any OS which mounts partitions on this flash **must** implement the same block mapping table, or else there is a significant chance that either you overwrite something important to the bootloader, or that it overwrites something of yours.

TODO complete