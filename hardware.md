---
title: Hardware
description: The specific EcoNet SoCs and their devices
published: true
date: 2025-03-21T22:54:57.891Z
tags: 
editor: markdown
dateCreated: 2025-03-20T00:16:36.117Z
---

# Hardware
EcoNet devices have some common hardware as well as some specific hardware to the sub-family. Below is a block diagram of the EcoNet SoCs, rounded objects are optional and square are always present.

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
[MIPS 34Kc / 1004Kc] --> [SIMCARD]
[SPI] --> [NOR / NAND Flash]
[MIPS 34Kc / 1004Kc] --> [PCM]
[PCM] --> (SLIC VoIP Port)
[MIPS 34Kc / 1004Kc] --> [PCIe]
[PCIe] --> [Wifi Chips]
[MIPS 34Kc / 1004Kc] --> [USB]
[MIPS 34Kc / 1004Kc] --> [I2C]

' Frame Engine stuff
[MIPS 34Kc / 1004Kc] <-right-> [Frame Engine]
[Frame Engine] -down-> [XPON/XDSL]
[Frame Engine] <-right-> (QDMA)
[Frame Engine] --> [Switch]
[Switch] --> [LAN Ports]

' Optional: Adjust layout for clarity
skinparam monochrome true
skinparam dpi 150
```

[Address Map](/hardware/EN7512/en7512-address-map)

* Common
  * ["High Precision Timer" CPU Timer](/hardware/econet-hpt)
  * Frame Engine
    * XPON
    * XDSL
    * Ethernet switch
    * QDMA
  * USB
  * PCIe
  * PCM
  * GPIO
  * GDMA
  * UART
  * Crypto
  * Memory
  * I2C
  * Timer
* [EN751221](/hardware/EN751221)
  * [EN751221 SPI Controller](/hardware/EN751221/en751221-spi)
  * [EN751221 Interrupt Controller](/hardware/EN751221/en751221-intc)
  * [EN7512 I2C Controller](/hardware/EN7512/en7512-i2c)
  * [EN7512 GDMA Controller](/hardware/EN7512/en7512-gdma)
  * [EN7512 Ethernet Switch](/hardware/EN7512/en7512-switch)
  * [EN7512 GPIO Controller](/hardware/EN7512/en7512-gpio)
  * [EN7512 Frame Engine](/hardware/EN7512/en7512-fe)
  * [EN7512 UART Controller](/hardware/EN7512/en7512-uart)
* EN751627
  * EN751627 SPI Controller
