---
title: PON
description: 
published: true
date: 2026-03-22T15:33:38.469Z
tags: 
editor: markdown
dateCreated: 2026-03-22T14:59:15.208Z
---

# PON
PON is a complex technology with several components. This page will focus on GPON and how to setup a development envoronment that fits a desk.

## PON Introduction
The advantage of depoying PON vs AE(PtP) is that the CO side where the OLT is can feed a lot of customers in a 1:N mapping where the N is a power of to split in the range of 128-16. In a home use scenario several users share the available bandwidth. With PON this sharing occurs on the link layer and the signal splitting is passive.

### Link sharing
With a shared medium the traffic needs to be properly directed. This happens through various protocols and is handled by the PON MAC.

With a downstream channel having 1 optical transmitter and N receivers the ONTs can always listen on the optical signal and sort out the data that is directed to the specific ONT.

The upstream channel is more complex. As the OLT can only listen to one transmitter at a time the channel is split in the time domain. Ie ONT 1 transmits during time slot 1 and ONT 2 transmits during time slot 2.

### PON versions
PON comes in several version. One of the most popular modes is GPON, it offeres 2.5G downsteam and 1.25G upstream. GPON has its heritage in ATM. There is another mode called EPON that uses Ethernet as its base structure. The Airoha familty of SoCs documented on the platform has support for both modes.

## ONT GPON Components
To create a GPON link you need several components. The layer 1 optical signal is analogue, this signal is translated to digital via a laser driver. The SoC <-> laser driver data path is a LVDS-link driven by the PON-phy serdes. The PON-phy data path is then connected to the PON-MAC.

### Laser driver EN7570 and EN7571
These components handles optical to LVDS signaling. It is always listening but only transmitting during its allocated time slot.

The laser driver is controlled via I2C on address 0x50 via 16-bit addressing. The laser driver is non-linear and needs to load calibration from flash to operate properly.

The vendor driver supports calibration. An upstream driver does not need to support that. The prefered model of an upstream driver is to emulate the SFP-8472 apis with regards to DDM data. The eeprom on address 0x80 can be emulated with the Linux kernel I2C slave mode EEPROM simulator