---
title: EN751221 Interrupt Controller
description: The Interrupt Controller found on the EN751221 family
published: true
date: 2025-03-21T22:50:52.295Z
tags: 
editor: markdown
dateCreated: 2025-03-20T01:50:51.981Z
---

# EN751221 Interrupt Controller
The EcoNet EN751221 (34Kc) uses a relatively simple 40 line intc. Each interrupt has one mask bit. However, the 34Kc supports SMP with 2 VPEs, and with only one bit per interrupt there is no way to mask an interrupt for one CPU and not the other. To address this, they introduce what we call *shadow-interrupts*.

## Shadow Interrupts
A shadow interrupt is a second IRQ number that is allocated for the purpose of controlling the second VPE’s view of a percpu interrupt. If for example int 31 is a percpu interrupt and int 30 is a shadow of int 31, then int 30 never fires but masking it will cause int 31 to stop firing on CPU#1.

## VI/EI Mode
This interrupt controller is designed to be used in VI/EI mode where the 7 normal CPU interrupts are re-routed to the interrupt controller, which then prioritizes them and sends them back to the CPU to be dispatched through a vector table. It *can* be run in legacy mode as long as `CPU_MIPSR2_IRQ_EI` and `CPU_MIPSR2_IRQ_VI` are NOT defined. In this mode, the interrupt controller is registered to the CPU under interrupt number 2, and it works as long as all other CPU interrupts are disabled (except for 0 and 1 which are used to control the software interrupts).

## Driver
A driver exists but is not yet merged to the Linux kernel: https://github.com/cjdelisle/openwrt/blob/new-platform-en75/target/linux/en75/files/drivers/irqchip/irq-en751221.c

## Interrupts
These are what are believed to be the interrupts wired on EN751221 chips. They are marked confirmed if they have been successfully used in an actual driver. "Name" is the symbol used to reference that interrupt in various GPL sources online.

| Confirmed | Interrupt # | Name | Description                        |
|---|----|------|------------------------------------|
|:white_check_mark:| 0  | UART_INT | UART interrupt |
| | 1  | PTM_B0_INT | DRAM illegal access interrupt |
|:white_check_mark:| 2  | SI_SWINT1_INT0 | Shadow of interrupt 7 |
|:white_check_mark:| 3  | SI_SWINT1_INT1 | Shadow of interrupt 8 |
| | 4  | TIMER0_INT | Timer 0 interrupt |
| | 5  | TIMER1_INT | Timer 1 interrupt |
| | 6  | TIMER2_INT | Timer 2 interrupt |
|:white_check_mark:| 7  | SI_SWINT_INT0 | MIPS34K software interrupt 0 |
|:white_check_mark:| 8  | SI_SWINT_INT1 | MIPS34K software interrupt 1 |
| | 9  | TIMER5_INT | Timer 3 interrupt |
| | 10 | GPIO_INT | GPIO controller interrupt |
| | 11 | PCM1_INT | PCM1 interrupt |
| | 12 | SI_PC1_INT | Shadow of interrupt 13 |
| | 13 | SI_PC_INT | MIPS34K performance counter interrupt |
| | 14 | APB_DMA0_INT | GDMA controller interrupt |
| | 15 | MAC1_INT, ESW_INT | Giga switch interrupt |
| | 16 | HSUART_INT | UART2 interrupt |
| | 17 | IRQ_RT3XXX_USB | USB host controller interrupt     |
| | 18 | DYINGGASP_INT | Dying Gasp interrupt              |
| | 19 | DMT_INT | xDSL DMT interrupt                |
| | 20 | USB20_INT, UNUSED0_INT | USB 2.0               |
| | 21 | MAC_INT, FE_MAC_INT | Frame engine interrupt            |
| | 22 | SAR_INT | SAR (ATM)                    |
| | 23 | PCIE_0_INT, USB11_INT | PCIe Port0 interrupt             |
| | 24 | PCIE_A_INT | PCIe Port1 interrupt              |
| | 25 | PCIE_SERR_INT | PCIe Error interrupt |
| | 26 | XSLV0_INT, PTM_B1_INT | Unknown / Unused |
| | 27 | XSLV1_INT, SPI_MC_INT | Unknown / Unused |
| | 28 | XSLV2_INT, CRYPTO_INT, USB_INT | Crypto interrupt |
| :white_check_mark: | 29 | SI_TIMER1_INT | Shadow of interrupt 30          |
| :white_check_mark: | 30 | SI_TIMER_INT | [HPT Timer](/hardware/econet-hpt) |
| | 31 | SWR_INT | Pbus timeout interrupt            |
| | 32 | BUS_TOUT_INT | PCM2 interrupt                    |
| | 33 | RESERVE_A_INT, PCM2_INT | Reserved                          |
| | 34 | RESERVE_B_INT | SLM interrupt                     |
| | 35 | RESERVE_C_INT | SPI controller interrupt          |
| | 36 | AUTO_MANUAL_INT | CPU FDC interrupt0                |
| | 37 |              | CPU FDC interrupt1                |
| | 38 |             | Reserved                          |

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