---
title: I2C
description: 
published: true
date: 2025-03-27T21:40:16.959Z
tags: 
editor: markdown
dateCreated: 2025-03-20T19:31:04.127Z
---

## I2C

The I2C driver is present in the linux kernel tree. The same driver as for the MT7621 should work.
[Source code](https://web.git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/drivers/i2c/busses/i2c-mt7621.c?h=v6.13.7)

What is missing is to be able to select the driver when compiling the ECONET target.

### Related Specifications
[Chapter 2.7, page 49](http://gw.stasoft.net/share/nts/datasheets/MT7621_ProgrammingGuide_Preliminary_Platform.pdf) 
