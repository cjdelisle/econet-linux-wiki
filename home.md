---
title: Main
description: A project to port mainline Linux to EcoNet MIPS devices
published: true
date: 2025-03-20T02:07:24.559Z
tags: 
editor: markdown
dateCreated: 2025-03-18T22:17:18.480Z
---

# EcoNet Linux
This is a project to port mainline Linux to EcoNet MIPS SoCs.

## What are EcoNet SoCs?
EcoNet chips are used in DSL and XPON CPE. They are big endian MIPS, and come from the TrendChip TC3162 heritage. They mostly fall into two categories:
* [EN751221](/hardware/EN751221) based on 34Kc with a custom interrupt controller, and
* EN751627 based on MIPS 1004Kc with MIPS_GIC

They have SPI NAND or NOR flash, PCIe, USB, Ethernet, SLIC (VoIP), and XDSL or XPON depending on the chip.

## What is EcoNet?
EcoNet (HK) Limited was a subsidiary of MediaTek who produced broadband devices. In 2021 they were bought by Airoha, another MediaTek subsidiary who makes ARM based broadband chips. EcoNet and Airoha devices have a fair bit in common, but EcoNet devices are MIPS, so the core devices are all quite unique.

## What chips are there?
See: [Hardware](/hardware)
* [EN751221](/hardware/EN751221) chips
  * [EN7512](EN7512)
  * EN7513
  * EN7521
  * EN7526
  * EN7520 (partially compatible)
* EN751627
  * EN7516
  * EN7527
  * EN7528 (partially)
  * EN7580 (partially)

## I have an EcoNet device, what can I do with it?
Right now, there is an out-of-tree Openwrt build that can be run on EN751221 devices. It has rudimentary boot-to-a-console capability, but no drivers.

You can:

* Get the source from https://github.com/cjdelisle/openwrt/tree/new-platform-en75
* Get binaries from https://github.com/cjdelisle/OpenWRT-EN751221-Builds/releases/latest

**ETHERNET IS NOT SUPPORTED YET, YOU MUST HAVE UART PINS CONNECTED**

If you have a [SmartFiber XP8421-B](https://openwrt.org/inbox/toh/evaluation_boards/unbranded_boards/smartfiber_xp8421-b#installing_openwrt_from_the_bootloader_in_linux) or [TP-Link Archer VR1200v (v2)](https://openwrt.org/inbox/toh/tp-link/archer_vr1200v) then you can install from the relevant documentation. Otherwise you will only be able to load the generic image to memory, see the [VR1200v Debricking Guide](https://openwrt.org/inbox/toh/tp-link/archer_vr1200v#debricking) for instructions how to do that.

