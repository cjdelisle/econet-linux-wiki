---
title: EcoNet Linux
description: A project to port mainline Linux to EcoNet MIPS devices
published: true
date: 2026-01-05T09:14:15.051Z
tags: 
editor: markdown
dateCreated: 2025-03-18T22:17:18.480Z
---

# EcoNet Linux
A project to port mainline Linux to EcoNet MIPS SoCs.

![econet_linux_logo.png](/econet_linux_logo.png)

<center>

This project is supported by **[NGI Zero Core](https://nlnet.nl/core/)**, a Horizon Europe funding vehicle administered by **[NLnet](https://nlnet.nl/project/Econet-Linux-mainline/)**.

| [![nlnet.svg](https://nlnet.nl/image/logo_nlnet.svg)](https://nlnet.nl/project/Econet-Linux-mainline/) | [<img src="https://nlnet.nl/image/logos/NGI0Core_tag.svg" width=220/>](https://nlnet.nl/core/) |
|---------:|:--------:|

**Check the [Project Plan](/plan) to see what's coming soon :rocket:**
</center>

## EcoNet Linux Support Status
See: [Hardware](/hardware)
* **[EN751221](/hardware/EN751221)** (EN7512, EN7513, EN7521, EN7526)
  * :white_check_mark: Ability to boot to a console
  * :white_check_mark: USB 2.0 support in OpenWRT
  * :hourglass: PCIe (Wifi) support in OpenWRT
  * :white_check_mark: Basic Ethernet Support
  * :question: xPON (Fiber) Support (EN7521 & EN7526)
  * :question: xDSL Support (EN7512 & EN7513)
* **[EN751627](/hardware/EN751627)** :x: No support yet
  * EN7516 DSL
  * EN7527 Fiber
* **EN7528** (Fiber)
  * :white_check_mark: Ability to boot to a console
* **EN7580** Fiber (10Gb GPON) :x: No support yet
* **[EN7523](/hardware/EN7523)** (EN7523, EN7529, EN7562)
  * Airoha ARM devices which are similar to EcoNet and also documented here
### Current Open Patches and Pull Requests
* **OpenWrt**
  * [EN7528 Basic Support](https://github.com/openwrt/openwrt/pull/21326)
* **Linux Kernel**
  * Nothing right now :tada:
## What are EcoNet SoCs?
EcoNet chips are used in DSL and XPON CPE. They are MIPS, and come from the TrendChip TC3162 heritage. They mostly fall into three categories:
* [EN751221](/hardware/EN751221) based on 34Kc with a custom interrupt controller, and
* EN751627 based on MIPS 1004Kc with MIPS_GIC
* EN7528 mostly the same as EN751627 except little endian
* [EN7523](/hardware/EN7523) - Airoha chipset which is ARM based, but otherwise similar to EcoNet chipsets

They have SPI NAND or NOR flash, PCIe, USB, Ethernet, SLIC (VoIP), and XDSL or XPON depending on the chip.

## What is EcoNet?
EcoNet (HK) Limited was a subsidiary of MediaTek who produced broadband devices. In 2021 they were bought by Airoha, another MediaTek subsidiary who makes ARM based broadband chips. EcoNet and Airoha devices have a fair bit in common, but EcoNet devices are MIPS, so the core devices are all quite unique.

## I have an EcoNet device, what can I do with it?
Right now, there is basic support in OpenWRT build that can be run on certain EN751221 and EN7528 devices, including:

* EN751221
  * [Zyxel PMG5617GA](https://openwrt.org/inbox/toh/zyxel/zyxel_pmg5617ga)
  * [TP-Link Archer VR1200v (v2)](https://openwrt.org/inbox/toh/tp-link/archer_vr1200v)
  * [SmartFiber XP8421-B](https://openwrt.org/inbox/toh/evaluation_boards/unbranded_boards/smartfiber_xp8421-b)
  * [Nokia G-240G-E](https://openwrt.org/inbox/toh/bt/g-240g-e_1)
* EN7528
  * **DASAN H660GM-A**

You can:

* Get the source from https://github.com/openwrt/openwrt/
* Get snapshot binaries:
  * **EN751221** --> https://github.com/cjdelisle/OpenWrt-EN751221-Builds/releases/latest
  * **EN7528** --> https://github.com/cjdelisle/OpenWrt-EN7528-Builds/releases/latest

<div style="color:red">

**SUPPORT IS STILL EXPERIMENTAL, YOU MUST HAVE UART PINS CONNECTED**

</div>

If you have an EN751221 or EN7528 which is not on the list, you can still potentially load the generic kernel into memory and jump to it from the bootloader, see the [VR1200v Debricking Guide](https://openwrt.org/inbox/toh/tp-link/archer_vr1200v#debricking) for instructions how to do that.