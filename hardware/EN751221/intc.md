---
title: Interrupt Controller
description: The Interrupt Controller found on the EN751221 family
published: true
date: 2025-03-28T22:41:43.829Z
tags: 
editor: markdown
dateCreated: 2025-03-20T01:50:51.981Z
---

# Interrupt Controller
The EcoNet EN751221 (34Kc) uses a relatively simple 40 line intc. Each interrupt has one mask bit. However, the 34Kc supports SMP with 2 VPEs, and with only one bit per interrupt there is no way to mask an interrupt for one CPU and not the other. To address this, they introduce what we call *shadow-interrupts*.

## Shadow Interrupts
A shadow interrupt is a second IRQ number that is allocated for the purpose of controlling the second VPE’s view of a percpu interrupt. If for example int 31 is a percpu interrupt and int 30 is a shadow of int 31, then int 30 never fires but masking it will cause int 31 to stop firing on CPU#1.

## VI/EI Mode
This interrupt controller is designed to be used in VI/EI mode where the 7 normal CPU interrupts are re-routed to the interrupt controller, which then prioritizes them and sends them back to the CPU to be dispatched through a vector table. It *can* be run in legacy mode as long as `CPU_MIPSR2_IRQ_EI` and `CPU_MIPSR2_IRQ_VI` are NOT defined. In this mode, the interrupt controller is registered to the CPU under interrupt number 2, and it works as long as all other CPU interrupts are disabled (except for 0 and 1 which are used to control the software interrupts).

## Driver
A driver exists but is not yet merged to the Linux kernel: https://github.com/cjdelisle/openwrt/blob/new-platform-en75/target/linux/en75/files/drivers/irqchip/irq-en751221.c

## Register Layout
These are the actual registers on the interrupt controller itself. The controller is mapped to memory location `0xbfb40000` and is 104 bytes wide. "Value" is the value observed from dumping memory in the bootloader.
| Address     | Value | Meaning         | Notes                                      |
|-------------|--------------|-----------------|--------------------------------------------|
| BFB40000  | 0x00000000   | CR_INTC_ITR     | Type                                       |
| BFB40004    | 0x00200020   | CR_INTC_IMR     | Mask0 - Mask interrupts 0-31              |
| BFB40008    | 0x60000004   | CR_INTC_IPR     | Pending0 - Bitfield of pending interrupts 0-31 |
| BFB4000C    | 0x00000000   | CR_INTC_ISR     | Set0 (trigger)                            |
| BFB40010    | 0x1f091e1d   | CR_INTC_IPR0    | Priority - [Vendor code](https://github.com/copslock/test/blob/aea3a43562e8d3dc0335624202fde08d713a18c2/tclinux_phoenix/bootrom/bootram/init/irq.c#L108) |
| BFB40014    | 0x13161501   | CR_INTC_IPR1    | Priority                                  |
| BFB40018    | 0x1100080a   | CR_INTC_IPR2    | Priority                                  |
| BFB4001C    | 0x0b0c0d0f   | CR_INTC_IPR3    | Priority                                  |
| BFB40020    | 0x10060e07   | CR_INTC_IPR4    | Priority                                  |
| BFB40024    | 0x12030217   | CR_INTC_IPR5    | Priority                                  |
| BFB40028    | 0x18191a1b   | CR_INTC_IPR6    | Priority                                  |
| BFB4002C    | 0x1c050414   | CR_INTC_IPR7    | Priority                                  |
| BFB40030    | 0x00001000   | CR_INTC_IVSR0   | CPU Affinity - IRQ 30 set CPU1            |
| BFB40034    | 0x00000000   | CR_INTC_IVSR1   | CPU Affinity                              |
| BFB40038    | 0x00000000   | CR_INTC_IVSR2   | CPU Affinity                              |
| BFB4003C    | 0x00000000   | CR_INTC_IVSR3   | CPU Affinity                              |
| BFB40040    | 0x00000010   | CR_INTC_IVSR4   | CPU Affinity - IRQ 13 set CPU1            |
| BFB40044    | 0x00000000   | CR_INTC_IVSR5   | CPU Affinity                              |
| BFB40048    | 0x00000000   | CR_INTC_IVSR6   | CPU Affinity                              |
| BFB4004C    | 0x10100000   | CR_INTC_IVSR7   | CPU Affinity - IRQs 3 and 4 set CPU1      |
| BFB40050    | 0x00000000   | CR_INTC_IMR_1   | Mask1 - Mask interrupts >31               |
| BFB40054    | 0x00000000   | CR_INTC_IPR_1   | Pending1 - Bitfield of pending interrupts >31 |
| BFB40058    | 0x20212223   | CR_INTC_IPSR8   | Priority                                  |
| BFB4005C    | 0x24252627   | CR_INTC_IPSR9   | Priority                                  |
| BFB40060    | 0x00000000   | CR_INTC_IVSR8   | CPU Affinity                              |
| BFB40064    | 0x00000000   | CR_INTC_IVSR9   | CPU Affinity                              |

## GPIO Interrupts

The [gpio](/hardware/EN7512/en7512-gpio) pins can be configured to generate interrupts. The interrupts should be cascaded through the GPIO_INT signal.