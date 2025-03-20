---
title: EN751221 SPI Flash Controller
description: The EN751221 / AN7523 SPI Controller
published: true
date: 2025-03-20T15:02:41.229Z
tags: 
editor: markdown
dateCreated: 2025-03-20T02:01:38.373Z
---

# EN751221 SPI Flash Controller
This SPI Controller has two modes, manual and auto. Auto mode is similar to DMA in that data is sent directly to a location in memory, but it is only for NOR flash. For NAND flash, this controller only supports manual mode, in which data is read and written one byte at a time by the CPU.

The EN751627 SFC is similar to this one, but also has DMA capability.

## Drivers
The manual mode driver is identical to [spi-en7523.c](https://github.com/openwrt/openwrt/blob/v24.10.0/target/linux/airoha/patches-6.6/300-spi-Add-support-for-the-Airoha-EN7523-SoC-SPI-contro.patch) used in the Airoha AN7523 ARM device, with the exception of the lost write quirk.

### Lost Write Quirk
On EN751221 devices, the default driver does not wait long enough after setting the chip select, causing NAND writes to be lost. This is mitigated in the vendor code by setting chip select twice, something which has been copied for the EN751221 SPI support.

```c
static void set_cs(int state)
{
	u32 cmd = state ? OP_CSH : OP_CSL;

	/* EN751221 drops writes if we don't send this twice. */
	opfifo_write(cmd, 1);
	opfifo_write(cmd, 1);
}
```

TODO: It is possible that this may be solved more cleanly by adjusting `CSHEXT` / `CSLEXT`.

### Vendor Drivers
There are two main vendor drivers:
* spi_controller.c - Like spi-en7523.c, manual mode PIO
* newspiflash.c - Automatic mode, NOR flash only

## Register Layout
These are the registers used in the SFC, uncommented registers are unused in the drivers and vendor code, and are presumed to be for self-testing or else not connected. Value (boot) is a read value of the register from the bootloader of an EN7526G. Name is based on what is used in vendor GPL code and/or Airoha AN7523 driver. A number of registers are only accessed in the bootloader during self-tests.

| Address     | Value (boot) | Name                   | Notes                                                                 | In EN7523 driver | In vendor driver | In vendor bootloader |
|-------------|--------------|------------------------|----------------------------------------------------------------------|---------------------------|---------------------|--------------------------|
| 0xBFA10000  | 0x00000000   | READ_MODE              | Speed selection for Auto Mode                                        |                           |                     | :white_check_mark:       |
| 0xBFA10004  | 0x00000000   | READ_IDLE_EN           | Detect auto mode is idle                                             | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10008  | 0x00000000   | SIDLY                  | Number of delay cells of read data timing delay                      |                           |                     | :white_check_mark:       |
| 0xBFA1000C  | 0x00000005   | CSHEXT                 | Delay for OP_CSH in auto mode (0-7, 5=reset)                         |                           |                     | :white_check_mark:       |
| 0xBFA10010  | 0x00000001   | CSLEXT                 | Delay after OP_CSL in auto mode (0-7, 1=reset)                       |                           |                     | :white_check_mark:       |
| 0xBFA10014  | 0x00000009   | MTX_MODE_TOG           | Routing to SPI subsystems. Auto mode = 0, Manual = 9                 | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10018  | 0x00000000   | ENSPI_RDCTL_FSM        | Auto Read Status, 0 = idle                                           | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA1001C  | 0x00000001   | MACMUX_SEL             | Set to 1 by the bootloader, leave alone. 0 is some undocumented self-test. |                     | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10020  | 0x00000001   | MANUAL_EN              | Manual Mode Enable, 0 = auto, 1 = manual                             | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10024  | 0x00000001   | OPFIFO_EMPTY           | 1 when all operations have been processed                            | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10028  | 0x00000405   | OPFIFO_WDATA           | Write SPI opcode + data length                                       | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA1002C  | 0x00000000   | OPFIFO_FULL            | 1 when op fifo is full, wait before next write                       | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10030  | 0x00000000   | OPFIFO_WR              | Write 1 to transfer value of OPFIFO_WDATA to queue                   | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10034  | 0x00000000   | DFIFO_FULL             | 1 when data fifo is full, pause writing                              | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10038  | 0x00000000   | DFIFO_WDATA            | Write 1 byte of data at a time here                                  | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA1003C  | 0x00000000   | DFIFO_EMPTY            | Used when reading to know that DFIFO_RDATA is ready                  | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10040  | 0x00000000   | DFIFO_RD               | Write 1 after reading DFIFO_RDATA to load next byte                  | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10044  | 0x000000ff   | DFIFO_RDATA            | Read 1 byte from data fifo                                           | :white_check_mark:        | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10048  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA1004C  | 0x00000055   |                        |                                                                      |                           |                     |                          |
| 0xBFA10050  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10054  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10058  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA1005C  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10060  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10064  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10068  | 0x00002504   |                        |                                                                      |                           |                     |                          |
| 0xBFA1006C  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10070  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10074  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA10078  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA1007C  | 0x00000001   |                        |                                                                      |                           |                     |                          |
| 0xBFA10080  | 0x00000001   | DUMMY                  | Auto mode, 0-15 dummy cycles to add after addressing, set in bootloader, left alone after | |                     | :white_check_mark:       |
| 0xBFA10084  | 0x00000000   | ADDR_3B4B              | Auto mode, 1 = use 4 byte addressing                                 |                           | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10088  | 0x00000000   | PROBE_SEL              | Undocumented debugging probe, only set and read back in self-test    |                           |                     | :white_check_mark:       |
| 0xBFA1008C  | 0x00000000   | SF_CFG3B4B_EN          | 1 = enable configuration of ADDR_3B4B                                |                           | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10090  | 0x00000000   | ENSPI_IER, INTERRUPT   | read 1 = auto/manual at same time error, write 1 = acknowledge      | :white_check_mark: (clears) | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10094  | 0x00000000   | INTERRUPT_EN           | 1 = enable auto/manual error interrupt                               |                           | :white_check_mark:  | :white_check_mark:       |
| 0xBFA10098  | 0x00000000   | CPOL                   | Set SPI flash clock default value (0|1)                              |                           |                     |                          |
| 0xBFA1009C  | 0x00000009   | SI_CK_SEL              | (1<<3) = set MSB first for flash output ORed with: 0-5 = delay cycles to latch flash data, set by bootloader left alone after | |                     | :white_check_mark:       |
| 0xBFA100A0  | 0x00000ee1   |                        |                                                                      |                           |                     |                          |
| 0xBFA100A4  | 0x00000fe0   |                        |                                                                      |                           |                     |                          |
| 0xBFA100A8  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA100AC  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA100B0  | 0x000024cc   |                        |                                                                      |                           |                     |                          |
| 0xBFA100B4  | 0x000024cc   |                        |                                                                      |                           |                     |                          |
| 0xBFA100B8  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA100BC  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA100C0  | 0x000024cc   |                        |                                                                      |                           |                     |                          |
| 0xBFA100C4  | 0x000024cc   |                        |                                                                      |                           |                     |                          |
| 0xBFA100C8  | 0x0007a0ad   |                        |                                                                      |                           |                     |                          |
| 0xBFA100CC  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA100D0  | 0x0008c098   |                        |                                                                      |                           |                     |                          |
| 0xBFA100D4  | 0x0008c098   |                        |                                                                      |                           |                     |                          |
| 0xBFA100D8  | 0x0007a0ad   |                        |                                                                      |                           |                     |                          |
| 0xBFA100DC  | 0x0007a0ad   |                        |                                                                      |                           |                     |                          |
| 0xBFA100E0  | 0x00000000   |                        |                                                                      |                           |                     |                          |
| 0xBFA100E4  | 0x00000000   | SLAVE_SEL              | 0 = select device 0, 1 = select device 1                            |                           |                     |                          |
| 0xBFA100E8  | 0x0007a120   | PLDCTL_RSTTIMER        | NAND BOOT - reset wait timer, for early loader code from SPI NAND    |                           |                     | :white_check_mark:       |
| 0xBFA100EC  | 0xc0c00fff   | PLDCTL_CFG0            | NAND BOOT - reset config register, only used in test                 |                           |                     | :white_check_mark:       |
| 0xBFA100F0  | 0x00000013   | PLDCTL_CFG1            | NAND BOOT - reset config register, only used in test                 |                           |                     | :white_check_mark:       |
| 0xBFA100F4  | 0x00000000   | PLDCTL_SWEN            | NAND BOOT - set 1 to trigger automatic preload                       |                           |                     | :white_check_mark:       |
| 0xBFA100F8  | 0x0003d090   | PLDCTL_CHKST_TIMER     | NAND BOOT - timeout timer, 32 bits wide                              |                           |                     |                          |
| 0xBFA100FC  | 0x00000001   | PLDCTL_STS             | NAND BOOT - write 1 = clear counter, read 1 = counter active         |                           |                     | :white_check_mark:       |
| 0xBFA10100  | 0x00000101   | PLDCTL_FSM             | NAND BOOT - state, 0 = idle                                          |                           |                     |                          |
| 0xBFA10104  | 0x00000000   | SW_CFGSPITYPE_VAL      | If enabled, 0 = NOR, 1 = NAND                                        |                           |                     | :white_check_mark:       |
| 0xBFA10108  | 0x00000000   | SW_CFGSPITYPE_EN       | 1 = enable SW_CFGSPITYPE_VAL, 0 = hardware detection                |                           |                     | :white_check_mark:       |
| 0xBFA1010C  | 0x00000000   | SW_CFGNANDADDR_VAL     | If enabled, 1 = append dummy after address                           |                           |                     | :white_check_mark:       |
| 0xBFA10110  | 0x00000000   | SW_CFGNANDADDR_EN      | 1 = enable SW_CFGNANDADDR_EN, 0 = hardware detection                |                           |                     | :white_check_mark:       |
| 0xBFA10114  | 0x00000006   | SFC_STRAP              | Test strapping configuration                                         |                           |                     | :white_check_mark:       |
| 0xBFA10118  | 0x00000052   |                        |                                                                      |                           |                     |                          |
| 0xBFA1011C  | 0x00000041   |                        |                                                                      |                           |                     |                          |
| 0xBFA10120  | 0x55555555   |                        |                                                                      |                           |                     |                          |
| 0xBFA10124  | 0x55555555   |                        |                                                                      |                           |                     |                          |
| 0xBFA10128  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA1012C  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA10130  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA10134  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA10138  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |
| 0xBFA1013C  | 0xdeadbeef   |                        |                                                                      |                           |                     |                          |