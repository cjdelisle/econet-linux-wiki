---
title: UART
description: 
published: true
date: 2025-03-27T22:28:11.236Z
tags: 
editor: markdown
dateCreated: 2025-03-20T21:45:25.351Z
---

## UART

The EN751221 has 2 UART ports:

* UART1: `0xBFBF0000` - Wired to the accessible UART pins
* UART2: `0xBFBF0300`

This UART is identical to `ns16550` with a register width of 4 bytes, the only difference is that the speed converted into a clock rate using a formula that is not in `ns16550`.

Example DT for a EN751221 UART set to 115200:

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

There is a patch under review for [adding an EcoNet/Airoha UART driver](https://www.spinics.net/lists/devicetree/msg772090.html) which will correctly compute the clock in order to adjust the UART baud rate.
