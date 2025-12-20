---
title: Frame Enigne
description: 
published: true
date: 2025-12-20T21:49:54.498Z
tags: 
editor: markdown
dateCreated: 2025-12-20T21:49:54.498Z
---

# Frame Engine
The EcoNet Ethernet / xPON driver is known as the "Frame Engine", it consists of:
* Two **QDMA** engines which handle QoS and which transport data to and from the CPU.
* One **TDMA** enigne (very similar to Mediatek PDMA) which can handle transport of data to and from the CPU.
* Two **GDM** ports which control the two physical ports, one of which goes to an internal MT7530 switch, and the other which goes to either an xPON subsystem, or a WAN ethernet port (in the DSL case).
* A Packet Processing Engine (**PPE**) which is able to do hardware NAT, packet encapsulation and forwarding.

![frame_engine_en7523.drawio.png](/en7523/frame_engine_en7523.drawio.png)