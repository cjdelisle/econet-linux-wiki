---
title: EN7512 MT7530
description: 
published: true
date: 2025-03-20T23:29:03.980Z
tags: 
editor: markdown
dateCreated: 2025-03-20T19:53:00.382Z
---

## Switch

The internal switch is MT7530-based and the same code for the AN7581 should work. The switch also provide the MDIO-bus.
[Source code](https://web.git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/drivers/net/dsa/mt7530-mmio.c)

On the mdio bus 4 ge-phys are connected on mdio address 9,10,11,12. A driver implementing the calibration logic is needed.
[SDK driver code](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/tcphy/tcetherphy_7512.c#L8569)

Controlling the phy leds can be done the same way as in this code:
[MT7988 code](https://web.git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/drivers/net/phy/mediatek/mtk-ge-soc.c?h=v6.13.7#n1207)

### GSW_BASE
[0xBFB58000](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/ether/en7512/eth_en7512.h#L68C9-L68C17)

### Related Specifications
[Specifications for MT7531](https://drive.google.com/file/d/1aVdQz3rbKWjkvdga8-LQ-VFXjmHR8yf9/view)

