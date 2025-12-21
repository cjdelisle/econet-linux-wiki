---
title: WED
description: Wifi / Ethernet dispatch
published: true
date: 2025-12-21T15:15:50.100Z
tags: 
editor: markdown
dateCreated: 2025-12-21T15:15:50.100Z
---

# Wireless Ethernet Dispatch
WED is a hardware flow offloading subsystem in the EN7523 SoC. The IP block seems to come from the Mediatek MT7622 SoC. The patch adding support for this in [linux](https://lwn.net/Articles/862864/) describes it as the following:

> This patch series adds hardware flow offloading for routing/NAT packets from
> ethernet to WLAN on MT7622 SoC, in combination with MT7915 WLAN devices
> This only offloads one direction, WLAN->Ethernet offload is not supported by
> MT7622 (but will be in newer SoC designs).
> 
> In order to make this work, the SoC contains a subsystem named Wireless
> Ethernet Dispatch. It intercepts access to the WLAN DMA register space and
> controls the DMA queues in order to be able to inject packets coming in from
> the packet processing engine (PPE). It also intercepts IRQs from PCIe.


The above information indicates that WED only supports hardware offloading in the tx direction (unverified) from the routers point of view.

## Block diagram
The below illustration describes the data path.

Any traffic that is destined to the WiFi needs to go over the pcie interface. An offloaded flow would then go from a PSE port to the GDM3 port and then via the WED interface injecting packets into the pcie DMA descriptor rings.

![frame_engine_en7523.drawio.png](/en7523/frame_engine_en7523.drawio.png)


## Available specification
The following document describes WDMA registers.
[MT7981](https://whycan.com/files/members/1979/MT7981_ReferenceManual_Part3.pdf)