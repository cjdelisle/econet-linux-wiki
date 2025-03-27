---
title: EN7512 Address Map
description: 
published: true
date: 2025-03-27T16:51:51.792Z
tags: 
editor: markdown
dateCreated: 2025-03-21T23:49:55.355Z
---

# EN7512 Address Map
| Address     | Size | Name | Reference  | Wiki Page |
|-------------|------|------|------------| --------- |
| 0xBF900000  | 0x00000000   | AFE     | [boot2.S](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/en7512_boot/boot2/init/boot2.S)| - |
| [0xBFA00000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=202532780#gid=202532780)  | 0x100   | RBUS CORE     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L373Type)| - |
| [0xBFA10000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=716018069#gid=716018069)  | 0x140   | SPI FLASH     | [newspiflash.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/mtd/chips/newspiflash.h)| [en751221-spi](/hardware/EN751221/en751221-spi)
| [0xBFA20000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=1122579885#gid=1122579885)  | 0x200   | CHIP SCU     | [boot2.S](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/en7512_boot/boot2/init/boot2.S)| - |
| [0xBFA30000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=1428020403#gid=1428020403)  | 0x00000000   | SRAM     | [boot2.S](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/en7512_boot/boot2/init/boot2.S)| - |
| [0xBFA60000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=1736905292#gid=1736905292)  | 0x00000000   | SLM (QDMA) | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L97C23-L97C33)| - |
| [0xBFA80000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=2114721873#gid=2114721873)  | 0x1000   | SIF_SLV (USB) | [xhci-mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/usb/host/mtk_xhci/xhci-mtk.h)| - |
| [0xBFAC0000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=943667332#gid=943667332)  | 0x1000 | PCIe1 PHY | [pcie-phy.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pcie-phy.c) | - |
| [0xBFAF0000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=706705987#gid=706705987) | 0x1000 | xPON PHY | [phy_init.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/xpon_phy/src/phy_init.c) | - |
| [0xBFAF1000](https://docs.google.com/spreadsheets/d/1oEOS7F2hzj64bRu7my8m8-sbhkW5HyM_phw7SuDU1gY/edit?gid=706705987#gid=706705987) | 0x1000 | PCIe0 PHY | [pcie-phy.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pcie-phy.c) | - |
| [0xBFB00000]()  | 0x1000   | SCU     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L985)
| 0xBFB10000  | 0x00000000   | SMC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L973)|
| 0xBFB20000  | 0x00000000   | DMC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L382Type)|
| 0xBFB30000  | 0x00000000   | GDMA     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L423Type)|
| 0xBFB40000  | 0x00000000   | INTC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L654)|
| 0xBFB50000  | 0x00000000   | FE     | [fe_reg_en7512.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/fe/en7512/fe_reg_en7512.h#L61)
| 0xBFB54000  | 0x00000000   | QDMA1     | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L79C58-L79C68)|
| 0xBFB55000  | 0x00000000   | QDMA2     | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L79C58-L79C68)|
| 0xBFB58000  | 0x00000000   | GSW     | [fe_reg_en7512.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/fe/en7512/fe_reg_en7512.h#L418)
| 0xBFB60000  | 0x00000000   | ATM SAR     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L1071)|
| 0xBFB70000  | 0x00000000   | USB     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L997)|
| 0xBFB80000  | 0x00000000   | PCIe | [pci-tc3162.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pci-tc3162.c)|
| 0xBFB90000  | 0xBFB9FFFF   | PCIe IO | [pci-tc3162.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pci-tc3162.c)|
| 0xBFBA0000  | 0xBFBCFFFF   | PCIe MEM| [pci-tc3162.c](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pci-tc3162.c)|
| 0xBFB90000  | 0x00000000   | XHCI |[xhci-mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/usb/host/mtk_xhci/xhci-mtk.h)|
| 0xBFBC0000  | 0x00000000   | SPI     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L436)|
| 0xBFBD0000  | 0x00000000   | PCM     | [pcmdriver.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/voip_2.6.36/DSP/MTK/pcm/pcmdriver.h#L112)|
| 0xBFBF0000  | 0x00000030   | UART1     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L546)|
| 0xBFBF0100  | 0x00000000   | TIMER     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L813)|
| 0xBFBF0200  | 0x00000000   | GPIO     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L866)|
| 0xBFBF0300  | 0x00000030   | UART2     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L633)|
| 0xBFBF0400  | 0x00000000   | CPU TIMER     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L856)|
| 0xBFBF0500  | 0x00000000   | SIM     | [sim_hw_mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/simcard_separation/sim_hw_mtk.h)|
| 0xBFBF8200  | 0x00000000   | EFUSE     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L227Type)|
