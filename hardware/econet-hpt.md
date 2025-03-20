---
title: High Performance Timer
description: The timer device found on EcoNet chips
published: true
date: 2025-03-20T09:06:21.101Z
tags: 
editor: markdown
dateCreated: 2025-03-20T00:32:52.921Z
---

# High Precision Timer
The HPT is a per-cpu timer which is present on EcoNet MIPS devices and it is necessary on these platforms because `CEVT_R4K` interrupt is missing.

## What is in a name?

The name “High Precision Timer” comes from the old name for the R4K timer. [Early vendor code for the HPT](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/ralink/time2.c#L545) was copied from `mips/kernel/time.c` back in the 2.6 era when it contained the log line [Using XXX.XX MHz high precision timer](https://github.com/torvalds/linux/blob/v2.6.12/arch/mips/kernel/time.c#L674). Subsequent revisions copied and updated the code, preserving the log line long after it had been removed from the kernel proper.

Years later, integrators and other 3rd party SDK developers began referring to the timer as “HPT” and the log line became its calling card.

## Driver

There is currently no driver for the HPT in Linux proper, but one can be found here: https://github.com/cjdelisle/openwrt/blob/new-platform-en75/target/linux/en75/files/drivers/clocksource/timer-en75-hpt.c

In EN751221 platforms, there is one HPT with 2 count registers, one for each VPE. On EN751627 platforms, there are 2 HPTs since 1004Kc processors have 2 cores with 2 VPEs each.

## Register Layout
This is the register layout of the timer device. The addresses given are for timer0/1 which is located at `0xBFBF0400`, timer2/3 (EN751627 MIPS 1004Kc only) has the same layout but is based at `0xBFBE0000`.
| Address    | Value (boot) | Name           | Notes                         |
|------------|--------------|----------------|-------------------------------|
| 0xBFBF0400   | 0x00000000   | CR_CPUTMR_CTL  | Enable and check if pending   |
| 0xBFBF0404   | 0xffffffff   | CR_CPUTMR_CMR0 | Compare register, VPE timer 0 |
| 0xBFBF0408   | 0x00000000   | CR_CPUTMR_CNT0 | Count register, VPE timer 0   |
| 0xBFBF040C   | 0xffffffff   | CR_CPUTMR_CMR1 | Compare register, VPE timer 1 |
| 0xBFBF0410   | 0x00000000   | CR_CPUTMR_CNT1 | Count register, VPE timer 1   |