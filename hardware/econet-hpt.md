---
title: High Performance Timer
description: The timer device found on EcoNet chips
published: true
date: 2025-03-20T00:32:52.921Z
tags: 
editor: markdown
dateCreated: 2025-03-20T00:32:52.921Z
---

# High Performance Timer
The HPT is a per-cpu timer which is present on EcoNet MIPS devices and it is necessary on these platforms because CEVT_R4K interrupt is missing.

The name “High Performance Timer” comes from the old name for the R4K timer. Early vendor code for this timer was a patch to `mips/kernel/time.c` back when it contained the log line `printk(" Using %u.%03u MHz high precision timer.\n"`. Subsequent revisions copied and updated the patched code, preserving the log line long after it had been removed from the kernel proper.

Years later, integrators and other 3rd party SDK developers began referring to the timer as “HPT” and the log line became its calling card.

There is currently no driver for the HPT in Linux proper, but one can be found here: https://github.com/cjdelisle/openwrt/blob/new-platform-en75/target/linux/en75/files/drivers/clocksource/timer-en75-hpt.c

In EN751221 platforms, there is one HPT with 2 count registers, one for each VPE. On EN751627 platforms, there are 2 HPTs since 1004Kc processors have 2 cores with 2 VPEs each.