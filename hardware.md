---
title: Hardware
description: The specific EcoNet SoCs and their devices
published: true
date: 2025-03-20T15:39:17.057Z
tags: 
editor: markdown
dateCreated: 2025-03-20T00:16:36.117Z
---

# Hardware
EcoNet devices have some common hardware as well as some specific hardware to the sub-family.

```plantuml
' Internal stuff
[MIPS 34Kc / 1004Kc] -up-> [INTC]
[MIPS 34Kc / 1004Kc] -up-> [CPU Timer]
[MIPS 34Kc / 1004Kc] -up-> [Memory]
[Memory] -up-> [DRAM]
[MIPS 34Kc / 1004Kc] -up-> [Crypto]

[MIPS 34Kc / 1004Kc] -left-> [UART]

' Interfaces
[MIPS 34Kc / 1004Kc] --> [GPIO]
[MIPS 34Kc / 1004Kc] --> [SPI]
[SPI] --> [NOR / NAND Flash]
[MIPS 34Kc / 1004Kc] --> [PCM]
[PCM] --> (SLIC VoIP Port)
[MIPS 34Kc / 1004Kc] --> [PCIe]
[PCIe] --> [Wifi Chips]
[MIPS 34Kc / 1004Kc] --> [USB]
[MIPS 34Kc / 1004Kc] --> (XDSL)

' Frame Engine stuff
[MIPS 34Kc / 1004Kc] -right-> [Frame Engine]
[Frame Engine] -up-> (XPON)
[Frame Engine] --> [Switch]
[Switch] --> [LAN Ports]

' Optional: Adjust layout for clarity
skinparam monochrome true
skinparam dpi 150
```

* Common
  * ["High Precision Timer" CPU Timer](/hardware/econet-hpt)
  * Frame Engine
    * XPON
    * Ethernet
    * Switch
  * XDSL
  * USB
  * PCIe
  * PCM
  * GPIO
  * UART
  * Crypto
  * Memory
* [EN751221](/hardware/EN751221)
  * [EN751221 SPI Controller](/hardware/EN751221/en751221-spi)
  * [EN751221 Interrupt Controller](/hardware/EN751221/en751221-intc)
* EN751627
  * EN751627 SPI Controller
