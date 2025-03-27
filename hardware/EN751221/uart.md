---
title: UART
description: 
published: true
date: 2025-03-27T22:23:11.011Z
tags: 
editor: markdown
dateCreated: 2025-03-20T21:45:25.351Z
---

## UART

The EN751221 has 2 UART ports. For booting purposes, the port used is the one at `0xBFBF0000` and it can be treated as an `ns16550` with a register width of 4 bytes.

Example DT for a EN751221 UART set to 115200.

```
	serial@1fbf0000 {
		compatible = "ns16550";
		reg = <0x1fbf0000 0x30>;
		reg-io-width = <4>;
		reg-shift = <2>;
		interrupt-parent = <&intc>;
		interrupts = <0>;
		clock-frequency = <1843200>;
	};
```

Setting an arbitrary baud rate is non-trivial because the baud rate must be converted into a clock divisor. There is a patch under review for [adding an EcoNet/Airoha UART driver](https://www.spinics.net/lists/devicetree/msg772090.html) which will correctly compute the clock in order to adjust the UART baud rate.
