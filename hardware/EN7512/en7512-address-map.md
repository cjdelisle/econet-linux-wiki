---
title: EN7512 Address Map
description: 
published: true
date: 2025-03-22T00:34:52.531Z
tags: 
editor: markdown
dateCreated: 2025-03-21T23:49:55.355Z
---

# EN7512 Address Map
| Address     | Size | Name         | Referece                                      |
|-------------|--------------|-----------------|-------------------------------------------|
| 0xBFA00000  | 0x00000000   | RBUS CORE     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L373Type)|
| 0xBFA60000  | 0x00000000   | SLM     | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L97C23-L97C33)|
| 0xBFA80000  | 0x00000000   | SIF_SLV     | [xhci-mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/usb/host/mtk_xhci/xhci-mtk.h)|
| 0xBFB00000  | 0x00000000   | SCU     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L982)|
| 0xBFB10000  | 0x00000000   | SMC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L973)|
| 0xBFB20000  | 0x00000000   | DMC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L382Type)|
| 0xBFB30000  | 0x00000000   | GDMA     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L423Type)|
| 0xBFB40000  | 0x00000000   | INTC     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L654)|
| 0xBFB54000  | 0x00000000   | QDMA1     | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L79C58-L79C68)|
| 0xBFB50000  | 0x00000000   | FE     | [fe_reg_en7512.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/fe/en7512/fe_reg_en7512.h#L61)
| 0xBFB55000  | 0x00000000   | QDMA2     | [qdma_reg.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/qdma/EN7512/qdma_reg.h#L79C58-L79C68)|
| 0xBFB58000  | 0x00000000   | GSW     | [fe_reg_en7512.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/fe/en7512/fe_reg_en7512.h#L418)
| 0xBFB60000  | 0x00000000   | ATM SAR     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L1071)|
| 0xBFB70000  | 0x00000000   | USB     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L997)|
| 0xBFB80000  | 0x00000000   | HOST BRIDGE | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L1063)|
| 0xBFB90000  | 0x00000000   | XHCI |[xhci-mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/usb/host/mtk_xhci/xhci-mtk.h)|
| 0xBFBC0000  | 0x00000000   | SPI     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L436)|
| 0xBFBF0000  | 0x00000000   | UART1     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L546)|
| 0xBFBF0100  | 0x00000000   | TIMER     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L813)|
| 0xBFBF0200  | 0x00000000   | GPIO     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L866)|
| 0xBFBF0300  | 0x00000000   | UART2     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L633)|
| 0xBFBF0400  | 0x00000000   | CPU TIMER     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L856)|
| 0xBFBF0500  | 0x00000000   | SIM     | [sim_hw_mtk.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/simcard_separation/sim_hw_mtk.h)|
| 0xBFBF8200  | 0x00000000   | EFUSE     | [tc3162.h](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/include/asm/tc3162/tc3162.h#L227Type)|
