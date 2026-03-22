---
title: PON
description: 
published: true
date: 2026-03-22T14:59:15.208Z
tags: 
editor: markdown
dateCreated: 2026-03-22T14:59:15.208Z
---

# PON
PON is a complex technology with several components. This page will focus on GPON and how to setup a development envoronment that fits a desk.

## PON Introduction
The advantage of depoying PON vs AE(PtP) is that the CO side where the OLT is can feed a lot of customers in a 1:N mapping where the N is a power of to split in the range of 128-16. In a home use scenario several users share the available bandwidth. With PON this sharing occurs on the link layer and the signal splitting is passive.

### Link sharing
With a shared medium the traffic needs to be properly directed. This happens through various protocols and is handled by the PON MAC.

### PON versions
PON comes in several version. One of the most popular modes is GPON, it offeres 2.5G downsteam and 1.25G upstream. GPON has its heritage in ATM. There is another mode called EPON that uses Ethernet as its base structure. The Airoha familty of SoCs documented on the platform has support for both modes.