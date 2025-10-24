---
title: Project Plan
description: Plan for the EcoNet Linux project
published: true
date: 2025-10-24T12:41:37.248Z
tags: 
editor: markdown
dateCreated: 2025-10-24T12:03:51.502Z
---

# Project Plan

<center>

This project is supported by **[NGI Zero Core](https://nlnet.nl/core/)**, a Horizon Europe funding vehicle administered by **[NLnet](https://nlnet.nl/project/Econet-Linux-mainline/)**.

| [![nlnet.svg](https://nlnet.nl/image/logo_nlnet.svg)](https://nlnet.nl/project/Econet-Linux-mainline/) | [<img src="https://nlnet.nl/image/logos/NGI0Core_tag.svg" width=220/>](https://nlnet.nl/core/) |
|---------:|:--------:|

**This is the agreed-upon plan**
</center>

While EcoNet Linux is a long term vision for *all* EcoNet MIPS devices, this specific project has a one-year duration and a defined plan for specific chipsets. The goal of this project is to get PCI (Wifi), USB, Ethernet, and PON (Optical) support in the EN751221 processor family - that is, *everything*, with the exception of DSL and VoIP.

For the EN761627 family, the goal is just to reach minimal "boots to a console" support which will provide a jumping-off point for future developments.

Parts of this plan are very risky, particularly the xPON development. If this task proves to be unrealistic then the backup plan will be to propose replacing that task with more complete support for EN751627 and possibly some initial support EN7628, and/or EN7580.

## PCIe and Wifi Support
Before starting the project, there was a [demonstration with legacy code](https://github.com/cjdelisle/openwrt/commits/econet-pci-vendor). [New code](https://github.com/cjdelisle/openwrt/commits/econet-pci-experiment-sept20-25) based on upstream mediatek PCI and (mis)using Airoha CLK driver works for one card, PHY is not tuning properly so the other card fails.

**Objective**: PCIe with working PHY and correct CLK driver, submitted to Linux upstream and OpenWRT. Wifi drivers added to OpenWRT builds.

- :soon: PCIe PHY developed and submitted to the Linux kernel
- :soon: Clock developed and submitted to the Linux kernel
- :soon: PCIe finalized and submitted to the Linux kernel
- :hourglass: Wifi drivers added to OpenWRT and builds
  * Wifi support based on dirty vendor PCI driver to be published soon
- :soon: Patchset submitted to OpenWRT
- :soon: Reviews addressed and code accepted in Linux kernel

## USB 2.0 & 3.0 Support
[USB 2.0 works in OpenWRT](https://github.com/openwrt/openwrt/commit/c43925313e7179aec7a93aa24a03532f0c1fbaea) without any PHY tuning, and most EN751221 devices only use USB 2.0. However the chipset does have pins for a single USB 3.0 port and [vendor code does control the PHY](https://github.com/cjdelisle/kernel-49/blob/master/arch/mips/tc3262/uphy.c), so it would be inappropriate to attempt upstreaming without demonstrating USB 3.0 capability.

**Objective**: Proof of USB 3.0 capability on a device that supports it (which most likely requires PHY tuning), and submission of PHY and USB support to upstream kernel and OpenWRT.

- :soon: USB PHY developed and submitted to the Linux kernel
- :soon: USB support submitted to the Linux kernel
- :soon: Demonstration of working USB 3.0
- :soon: Patchset submitted to OpenWRT
- :soon: Reviews addressed and code accepted in Linux kernel

## Gigabit Ethernet Support
Currently there is no Ethernet support whatsoever.

**Objective**: Fully functioning Ethernet on EN751221 with support for controlling the MT7530 switch that is embedded on-chip.

- :hourglass: Minimal demonstrator based on ported legacy code (no switch support)
  * A repo has been started with vendor networking code copied from U-Boot (GPL), does not yet build
- :soon: Quality ethernet driver developed and contributed to OpenWRT
- :soon: Finalized ethernet driver with MT7530 switch contributed to Linux kernel
- :soon: Reviews addressed and code accepted in Linux kernel

## First Class EN751221 OpenWRT
Currently there are OpenWRT builds of main branch, but not releases. The filesystem is currently read-only so as not to disrupt the factory OS image which also resides on the devices. There is an experimental USB/SD-Card based root-fs implementation. There has been no testing with the OpenWRT image-builder, and there is no update process. Installation of packages has been tested but causes conflicts and there is no EN751221 repository.

**Objective**: First-class OpenWRT support with release version, package repository, data storage partition (SD-Card or UBI), and working update process.

- :hourglass: Support added to Zyxel PMG5617GA (EN7526), incl. OpenWRT wiki update
  * Got a device hooked up to serial, support should be submitted soon
- :soon: OpenWRT package for USB / SD-Card root-fs
- :soon: OpenWRT optional UBI root-fs overlay support
- :soon: OpenWRT package repository delivered and installation issues resolved
- :soon: Branches for two release versions, upgrade path validated
- :soon: Support EcoNet in OpenWRT image builder

## Minimal Support for EN751627 Family

Currently we haven’t tested on EN751627 because the only device we have has a password-locked bootloader. The EN751627 platform consists of the EN7516 DSL chip and the rare EN7527 xPON chip. Ordinarily this would not be a very interesting task because it basically only opens up one chipset and that chipset is DSL. However, the EN751627 is based on the MIPS 1004Kc processor which is the same as the EN7528 and EN7580. The EN7528 and EN7580 are still significantly different from the EN751627 because they are little endian, but because they’re the same processor revision (and same interrupt controller), EN751627 is a necessary stepping-stone to unlocking EN7528 and EN7580.

**Objective**: Bring the EN7516 up to the level of "boots to a console" which is the current (as of beginning the project) status of the EN751221 in Linux upstream. This allows a jumping-off point for future development of the EN7516, the EN7528 and the EN7580.

- :soon: EN7516 Publish Unlocked Bootloader & register maps documented
- :soon: Bootable PoC added to OpenWRT (PoC interrupt controller)
- :soon: GIC interrupt controller driver, submitted to Linux and OpenWRT
- :soon: DMA based flash driver (using Airoha's driver)
- :soon: Reviews addressed and code accepted in Linux kernel

## Cleanup and upstreaming
As of the beginning of the project, there have been a few things that are not ideal for long term maintainability because getting them cleaned up will require some consensus building with the Linux dev community. Firstly, all of the EcoNet devices use a [bespoke bad-block remapping algorithm](/en/bootloader/bbt-bmt) which layers over the MTD of the NAND chip. This is already [implemented and merged in OpenWRT](https://github.com/openwrt/openwrt/blob/master/target/linux/econet/files/drivers/mtd/nand/en75_bmt.c), but it cannot be upstreamed to Linux because it depends on a [MediaTek Bad Block Table framework](https://github.com/openwrt/openwrt/blob/c3c75d0e685b90fdcb9f1cf79a6e2c6949cce9ac/target/linux/generic/files/drivers/mtd/nand/mtk_bmt.h#L4) that uses some [messy hooks to attach itself into the MTD subsystem](https://github.com/openwrt/openwrt/blob/master/target/linux/econet/patches-6.12/902-snand-mtk-bmt-support.patch). Secondly, the EN751221 is based on a MIPS 34Kc which has 2 threads available, but currently it is only running in single core mode because [enabling SMP requires enabling a form of vectored interrupts](https://lwn.net/ml/all/7307e611-1cc6-425e-a066-478794878d8e@cjdns.fr/) which is not currently supported in Linux.

**Objectives**: Firstly we would like to write a generic MTD→MTD mapping driver, allowing us to upstream the Bad Block Table to the Linux kernel so that it does not need to remain as a patch in OpenWRT. Secondly we would like to add a support code to /arch/mips for gracefully dealing with 34Kc vectored interrupts so that we can turn on both threads on EN751221. There is also a miscellaneous task to add automated testing on real devices.

- :soon: Drafting and submission of an RFC PoC of a MTD passthrough driver 
- :soon: Consensus achieved and final version delivered of MTD passthrough
- :soon: Submission of EcoNet BBT/BMT code to Linux upstream as MTD passthrough
- :soon: Drafting and submission of an RFC PoC of a MIPS 34Kc intc dispatcher
- :soon: Consensus achieved and final version of MIPS 34Kc intc dispatcher
- :soon: Submission of multi-core support for EN751221 (using intc dispatcher)
- :soon: Automated testing harness for real devices + CI delivered

## xPON Fiber Optic Driver Development

Nobody has attempted to do anything with the optical port. A prerequisite of getting the port to send or receive anything is going to be working ethernet, because the optical port registers itself into the ethernet system. This is a very risky sub-project, so I’ve broken it down into small phases.

**Objective**: Ethernet over xPON protocol accomplished and submitted to Linux kernel. We do not know at this time whether it will be better to develop for EPON or GPON protocol, it will depend on what type of test equipment we are able to build, lease, or borrow. EPON is 1.25Gb/s and shares coding with gigabit optical ethernet protocol, while GPON is 2.4Gb/s and uses a totally unique line coding protocol. EPON protocol only acts as plain Ethernet, but GPON is a complex protocol offering voice, TV, and data. The objective of this project is to achieve only equivalent features to a classical Ethernet connection, whether EPON or GPON based.

- :soon: First demo of OLT being recognized by modem + attempting to negotiate
- :soon: Modem able to receive traffic from OLT which contains recognizable data
- :soon: Able to demo tx bursts (confirm the laser is sending a burst)
- :soon: Able to receive traffic from modem by OLT
- :soon: Establish time synchronization between OLT and modem
- :soon: xPON driver communicating with configured OLT
- :soon: Modem capable of IP over xPON, using minimum TWO different OLTs
- :soon: Demo quality but usable PON driver sent to OpenWRT
- :soon: xPON driver cleaned up and sent to Linux kernel
- :soon: Reviews addressed and PON driver accepted