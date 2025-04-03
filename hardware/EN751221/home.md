---
title: EN751221
description: The EN751221 family, based on 34Kc processor
published: true
date: 2025-04-03T19:06:57.360Z
tags: 
editor: markdown
dateCreated: 2025-03-20T00:40:26.266Z
---

# EcoNet EN751221
The EN751221 family consists of the following devices:

| | DSL | xPON |
| - | - | - |
| **Original** | [EN7512](/hardware/EN751221/EN7512) @ 700Mhz | EN7521 @ 750Mhz |
| **Updated** | EN7513 @ 900Mhz | EN7526 @ 900Mhz |

From the perspective of the software, they are almost almost identical, with the major differences between the DSL and XPON chips being in the GPIO and PINMUX assignments.

## Peripherals
* System Controller
  * [PINMUX](/hardware/EN751221/pinmux)
* [Crypto Engine](/hardware/crypto-eip93)
* Timer
* [High Precision Timer](/hardware/econet-hpt)
* [Interrupt Controller](/hardware/EN751221/intc)
* [UART](/hardware/EN751221/uart)
* [Network (Frame Engine)](/hardware/EN751221/frame-engine)
  * QDMA
* [MT7530 Gigabit Switch](/hardware/EN751221/switch-mt7530)
* [GPIO Controller](/hardware/EN751221/gpio)
* [SPI Flash Controller](/hardware/EN751221/spi-flash)
* SIM Card Reader
* PCM (VoIP)
* xPON (Fiber Optic)
* xDSL
* PCIe
* USB
* [I2C](/hardware/EN751221/i2c)

## Address Map
| Address     | Size | Name | Reference  | Wiki Page |
|-------------|------|------|------------| --------- |
| 0xBF900000  | 0x00000000   | AFE     | [boot2.S](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/en7512_boot/boot2/init/boot2.S)| - |
| [0xBFA00000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=202532780#gid=202532780)  | 0x100   | RBUS CORE     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L373Type)| - |
| [0xBFA10000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=716018069#gid=716018069)  | 0x140   | SPI FLASH     | [spi_controller.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/mtd/chips/spi_controller.c)| [SPI Flash](/hardware/EN751221/spi-flash)
| [0xBFA20000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1122579885#gid=1122579885)  | 0x200   | CHIP SCU     | [boot2.S](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/en7512_boot/boot2/init/boot2.S)| - |
| [0xBFA30000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1428020403#gid=1428020403)  | 0x00000000   | SRAM     | [boot2.S](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/en7512_boot/boot2/init/boot2.S)| - |
| [0xBFA60000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1736905292#gid=1736905292)  | 0x00000000   | SLM (QDMA) | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L97C23-L97C33)| - |
| [0xBFA80000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=2114721873#gid=2114721873)  | 0x1000   | SIF_SLV (USB) | [xhci-mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/usb/host/mtk_xhci/xhci-mtk.h)| - |
| [0xBFAC0000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=943667332#gid=943667332)  | 0x1000 | PCIe1 PHY | [pcie-phy.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pcie-phy.c) | - |
| [0xBFAF0000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=706705987#gid=706705987) | 0x1000 | xPON PHY | [phy_init.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/xpon_phy/src/phy_init.c) | - |
| [0xBFAF1000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=706705987#gid=706705987) | 0x1000 | PCIe0 PHY | [pcie-phy.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pcie-phy.c) | - |
| [0xBFB00000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1750048098#gid=1750048098)  | 0x1000   | SCU     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L985) | - |
| [0xBFB10000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=129170564#gid=129170564)  | 0x10   | SMC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L973)| - |
| [0xBFB20000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=848628768#gid=848628768)  | 0x650   | DRAMC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L382Type)| - |
| [0xBFB30000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1667295257#gid=1667295257)  | 0x300   | GDMA     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L423Type)| [GDMA](/hardware/EN751221/gdma) |
| [0xBFB40000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=857008869#gid=857008869)  | 0x100   | INTC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L654)| [Interrupt Controller](/hardware/EN751221/intc) |
| [0xBFB50000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1957572195#gid=1957572195)  | 0x4000   | FE     | [fe_reg_en7512.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/fe/en7512/fe_reg_en7512.h#L61) | [Frame Engine](/hardware/EN751221/frame-engine) |
| [0xBFB54000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1957572195#gid=1957572195)  | 0x1000   | QDMA1     | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L79C58-L79C68)| - |
| [0xBFB55000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1957572195#gid=1957572195)  | 0x1000   | QDMA2     | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L79C58-L79C68)| - |
| [0xBFB58000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1957572195#gid=1957572195)  | 0x8000   | GSW     | [fe_reg_en7512.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/fe/en7512/fe_reg_en7512.h#L418) | [MT7530 Giga-Switch](/hardware/EN751221/switch-mt7530) |
| 0xBFB64000  | 0x2000   | GPON MAC     | [gpon_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/xpon/inc/gpon/gpon_reg.h#L8)| - |
| 0xBFB66000  | 0x2000   | EPON MAC     | [epon_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/xpon/inc/epon/epon_reg.h#L189)| - |
| [0xBFB70000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=2051134624#gid=2051134624)  | 0x10000   | CRYPTO     | [rt_mmap.h](https://github.com/cjdelisle/kernel-49/blob/master/arch/mips/include/asm/tc3162/rt_mmap.h#L21)| [Crypto Engine](/hardware/crypto-eip93) |
| [0xBFB80000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=332323451#gid=332323451)  | 0x4000   | PCIe | [pci-7512api.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pci-7512api.c)| - |
| 0xBFBA0000  | 0x20000   | XHCI |[xhci-mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/usb/host/mtk_xhci/xhci-mtk.h)| - |
| 0xBFBD0000  | 0x10000   | PCM     | [pcmdriver.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/voip_2.6.36/DSP/MTK/pcm/pcmdriver.h#L112)| - |
| [0xBFBF0000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460)  | 0x30   | UART1     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L546)| [UART](/hardware/EN751221/uart) |
| [0xBFBF0100](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460)  | 0x3C | TIMER     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L813)| - |
| [0xBFBF0200](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460)  | 0x7C   | GPIO     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L866)| [GPIO](/hardware/EN751221/gpio) |
| [0xBFBF0300](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460)  | 0x30   | UART2     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L633)| [UART](/hardware/EN751221/uart) |
| [0xBFBF0400](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460)  | 0x100   | CPU TIMER     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L856)| [High Precision Timer](/hardware/econet-hpt)
| [0xBFBF0500](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460)  | 0x84   | SIM     | [sim_hw_mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/simcard_separation/sim_hw_mtk.h)| - |
| [0xBFBF8000](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460) | 0x100 | I2C | [rt_mmap.h](https://github.com/cjdelisle/kernel-49/blob/55aac5e7e66370260fe514baff3dd0337e728173/arch/mips/include/asm/tc3162/rt_mmap.h#L14) | [I2C](/hardware/EN751221/i2c) |
| [0xBFBF8200](https://docs.google.com/spreadsheets/d/1kzdVxYqu4cTpe6ascNshn5PoCmxrbTkr3bAuWkqKkeg/edit?gid=1947557460#gid=1947557460)  | 0x100   | EFUSE     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L227Type)| - |

## Interrupts

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
| :white_check_mark: | 9  | TIMER5_INT | Timer 3 (watchdog) interrupt |
| | 10 | GPIO_INT | [GPIO](/hardware/EN751221/gpio) controller interrupt |
| :white_check_mark: | 11 | PCM1_INT | PCM1 interrupt |
| | 12 | SI_PC1_INT | Shadow of interrupt 13 |
| | 13 | SI_PC_INT | MIPS34K performance counter interrupt |
| | 14 | APB_DMA0_INT | GDMA controller interrupt |
| | 15 | MAC1_INT, ESW_INT | Giga switch interrupt |
| | 16 | HSUART_INT | UART2 interrupt |
| :white_check_mark: | 17 | IRQ_RT3XXX_USB | USB host controller interrupt     |
| :white_check_mark: | 18 | DYINGGASP_INT | Dying Gasp interrupt              |
| | 19 | DMT_INT | xDSL DMT interrupt                |
| | 20 | USB20_INT, UNUSED0_INT | USB 2.0               |
| :white_check_mark: | 21 | MAC_INT, FE_MAC_INT, CONFIG_QDMA_IRQ | QDMA LAN |
| :white_check_mark: | 22 | CONFIG_QDMA_IRQ | QDMA WAN |
| :white_check_mark: | 23 | PCIE_0_INT | PCIe Port0 interrupt |
| :white_check_mark: | 24 | PCIE_A_INT | PCIe Port1 interrupt |
| | 25 | PCIE_SERR_INT | PCIe Error interrupt |
| | 26 | XSLV0_INT, PTM_B1_INT | Unknown / Unused |
| | 27 | XSLV1_INT, SPI_MC_INT | Unknown / Unused |
| | 28 | XSLV2_INT, CRYPTO_INT, USB_INT | Crypto interrupt |
| :white_check_mark: | 29 | SI_TIMER1_INT | Shadow of interrupt 30 |
| :white_check_mark: | 30 | SI_TIMER_INT | [High Precision Timer](/hardware/econet-hpt) |
| | 31 | SWR_INT | Unknown / Unused |
| :white_check_mark: | 32 | BUS_TOUT_INT | Pbus timeout interrupt |
| | 33 | RESERVE_A_INT, PCM2_INT | Reserved                          |
| | 34 | RESERVE_B_INT | SLM interrupt                     |
| | 35 | RESERVE_C_INT | SPI controller interrupt          |
| | 36 | AUTO_MANUAL_INT | CPU FDC interrupt0                |
| | 37 |              | CPU FDC interrupt1                |
| | 38 |             | Reserved                          |
