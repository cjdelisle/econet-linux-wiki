---
title: EcoNet Linux
description: A project to port mainline Linux to EcoNet MIPS devices
published: true
date: 2025-10-24T10:52:59.829Z
tags: 
editor: markdown
dateCreated: 2025-03-18T22:17:18.480Z
---

# EcoNet Linux
A project to port mainline Linux to EcoNet MIPS SoCs.

![econet_linux_logo.png](/econet_linux_logo.png)

<center>

This project is supported by **[NGI Zero Core](https://nlnet.nl/core/)**, a Horizon Europe funding vehicle administered by **[NLnet](https://nlnet.nl/project/Econet-Linux-mainline/)**.

| [![nlnet.svg](https://nlnet.nl/image/logo_nlnet.svg)](https://nlnet.nl/project/Econet-Linux-mainline/) | [<img src="https://nlnet.nl/image/logos/NGI0Core_tag.svg" width=220/>](https://nlnet.nl/project/Econet-Linux-mainline/) |
|---------:|:--------:|

**Check the [Project Plan](/plan) to see what's coming soon :rocket:**
</center>

## EcoNet Linux Support Status
See: [Hardware](/hardware)
* **[EN751221](/hardware/EN751221)** (EN7512, EN7513, EN7521, EN7526)
  * :white_check_mark: Ability to boot to a console
  * :white_check_mark: USB 2.0 support in OpenWRT
  * :hourglass: PCIe (Wifi) support in OpenWRT
  * :soon: Ethernet Support
  * :question: xPON (Fiber) Support (EN7521 & EN7526)
  * :question: xDSL Support (EN7512 & EN7513)
* **EN751627** :x: No support yet
  * EN7516 DSL
  * EN7527 Fiber
* **EN7528** Fiber :x: No support yet
* **EN7580** Fiber (10Gb GPON) :x: No support yet

## What are EcoNet SoCs?
EcoNet chips are used in DSL and XPON CPE. They are big endian MIPS, and come from the TrendChip TC3162 heritage. They mostly fall into two categories:
* [EN751221](/hardware/EN751221) based on 34Kc with a custom interrupt controller, and
* EN751627 based on MIPS 1004Kc with MIPS_GIC

They have SPI NAND or NOR flash, PCIe, USB, Ethernet, SLIC (VoIP), and XDSL or XPON depending on the chip.

## What is EcoNet?
EcoNet (HK) Limited was a subsidiary of MediaTek who produced broadband devices. In 2021 they were bought by Airoha, another MediaTek subsidiary who makes ARM based broadband chips. EcoNet and Airoha devices have a fair bit in common, but EcoNet devices are MIPS, so the core devices are all quite unique.

## I have an EcoNet device, what can I do with it?
Right now, there is basic support in OpenWRT build that can be run on EN751221 devices. It has rudimentary boot-to-a-console capability, but no drivers.

You can:

* Get the source from https://github.com/openwrt/openwrt/
* Get binaries from https://github.com/cjdelisle/OpenWRT-EN751221-Builds/releases/latest

**ETHERNET IS NOT SUPPORTED YET, YOU MUST HAVE UART PINS CONNECTED**

If you have a [SmartFiber XP8421-B](https://openwrt.org/inbox/toh/evaluation_boards/unbranded_boards/smartfiber_xp8421-b#installing_openwrt_from_the_bootloader_in_linux) or [TP-Link Archer VR1200v (v2)](https://openwrt.org/inbox/toh/tp-link/archer_vr1200v) then you can install from the relevant documentation. Otherwise you will only be able to load the generic image to memory, see the [VR1200v Debricking Guide](https://openwrt.org/inbox/toh/tp-link/archer_vr1200v#debricking) for instructions how to do that.

