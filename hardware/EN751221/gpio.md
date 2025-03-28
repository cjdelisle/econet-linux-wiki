---
title: GPIO
description: 
published: true
date: 2025-03-28T22:22:51.991Z
tags: 
editor: markdown
dateCreated: 2025-03-20T20:14:09.970Z
---

## GPIO

The EN751221 supports up to 64 GPIO lines. This means almost every physical pin on the chip can be assigned as a GPIO. Of course this won't happen in practice because they're needed for other types of IO.

### Drivers

Basic read/write operation of the GPIOs is implemented in the [airoha,en7523-gpio](https://web.git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/drivers/gpio/gpio-en7523.c?h=v6.13.8) driver. The configuration for `EN751221` devices is the same as for `AN7523`:

```
  /* GPIOs 0-31 */
	gpio0: gpio@1fbf0200 {
		compatible = "airoha,en7523-gpio";
		reg = <0x1fbf0204 0x4>, /*CR_GPIO_DATA*/
		      <0x1fbf0200 0x4>, /*CR_GPIO_CTRL*/
		      <0x1fbf0220 0x4>, /*CR_GPIO_CTRL1*/
		      <0x1fbf0214 0x4>; /*CR_GPIO_ODRAIN*/
		gpio-controller;
		#gpio-cells = <2>;
	};

  /* GPIOs 32-63 */
	gpio1: gpio@1fbf0270 {
		compatible = "airoha,en7523-gpio";
		reg = <0x1fbf0270 0x4>, /*CR_GPIO_DATA1*/
		      <0x1fbf0260 0x4>, /*CR_GPIO_CTRL2*/
		      <0x1fbf0264 0x4>, /*CR_GPIO_CTRL3*/
		      <0x1fbf0278 0x4>; /*CR_GPIO_ODRAIN1*/
		gpio-controller;
		#gpio-cells = <2>;
	};
```

A more full-featured driver providing pinctrl, gpio and gpio-interrupt support is WIP. It is based on [pinctrl-airoha.c](https://web.git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/drivers/pinctrl/mediatek/pinctrl-airoha.c?h=v6.13.7).

## Serial GPIO (s0-s16)

In addition to normal GPIO, this controller also has an additional 17 serial GPIO lines which are output-only and which are multiplexed over a couple (unknown how many or which) physical pins to a [74HC595](https://www.ti.com/lit/ds/symlink/sn74hc595.pdf) or [74HC164](https://www.ti.com/lit/ds/symlink/sn74hc164.pdf) shift register chip which outputs the 17 lines.

There is currently no driver available for SGPIO, but the implementation is trivial, with a single register [CR_SGPIO_DATA](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/tc3162l2hp2h/ledctrl.c#L501) where lines 0-16 can be updated by setting bits 0-16 whenever bit 31 becomes 0.

## GPIO Interrupt (0-15)
While not implemented in `airoha,en7523-gpio`, the first 16 GPIOs can also be configured to trigger an interrupt on IRQ line 10. Upon receiving an interrupt, the lower 16 bits of `CR_GPIO_INTS` will tell you which GPIOs are pending (upper 16 bits are unused). Configuring GPIOs to interrupt is done with two registers: `CR_GPIO_EDET` and `CR_GPIO_LDET`. Each GPIO occupies 2 bits on each register.

* `CR_GPIO_EDET`
  * 00 -> No edge triggered interrupt
  * 01 -> Rising edge, when pin goes low to high
  * 10 -> Falling edge, when pin goes high to low
  * 11 -> Both
* `CR_GPIO_LDET`
  * 00 -> No level triggered interrupt
  * 01 -> Trigger when GPIO pin is high
  * 10 -> Trigger when GPIO pin is low
  * 11 -> Unused

## Simple Blink Mode (0-15)
GPIOs 0-15 support a simple LED blinking mode with only a single defined clock and a per-GPIO clock divider.

First set `0xBFBF0218` to the clock speed you want to blink at, then each GPIO can be set to blink by setting its 2 bits in `0xBFBF021C` to:

0. Disabled
1. Full speed
2. 1/2 speed
3. 1/4 speed

## Blinkenlights Mode (excl: 32-36, 52-63)
GPIOs can be configured to blink in hardware. When blink mode is enabled, the GPIO line will blink whenever it is switched on. Blink mode is enabled for the lower 16 GPIOs via the low 16 bits of `CR_GPIO_FLAMOD`. For GPIOs 16-31 and 37-51 it can be enabled via the 32 bits of `CR_GPIO_FLAMOD_EXT`. For serial GPIOs, bit 1 of `CR_SGPIO_MODE` enables blink for all SGPIOs.

### Configurable Blink Patterns (0-15, s0-s16)
You can configure up to 8 unique blink-patterns via registers `CR_GPIO_FLAP0`, `CR_GPIO_FLAP1`, `CR_GPIO_FLAP2`, `CR_GPIO_FLAP3`. each blink pattern has 2 values: time-high and time-low. Each time value is 8 bits wide and supports values of 0-127. Each blink pattern register specifies 2 blink patterns as follows:

| Bits: | 31 | 30:24 | 23 | 22:16 | 15 | 14:8 | 7 | 6:0 |
| :- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| Desc: | R | T-Lo1 | R | T-Hi1 | R | T-Lo0 | R | T-Hi0 |

* R: Reserved
* T-Lo - Time low
* T-Hi - Time high

Once you have created your blink patterns, you assign them to various LEDs using `CR_GPIO_FMAP0`, `CR_GPIO_FMAP1`, `CR_SGPIO_FMAP0`, `CR_SGPIO_FMAP1`, and `CR_SGPIO_FMAP2`. Each register has the same 4 bit structure repeated:

| Bits: | 3 | 2:0 |
| - | - |
| Desc: | Enable (must be set) | Select pattern 0-7 |

16-31 and 37-51 do not have blink pattern capability.

## Register Table

| Address | Name | GPIOs |  Desc |
| --- | --- | --- | --- |
| 0xBFBF0200 | CR_GPIO_CTRL | 0-15 | Input/Output, 2 bits/pin, 01=output |
| 0xBFBF0204 | CR_GPIO_DATA | 0-31 | Value (R/W) |
| 0xBFBF0208 | CR_GPIO_INTS | 0-15 | 16 bits, 1 = interrupt pending |
| 0xBFBF020C | CR_GPIO_EDET | 0-15 | Edge triggering, 2 bits/pin |
| 0xBFBF0210 | CR_GPIO_LDET | 0-15 | Level triggering, 2 bits/pin |
| 0xBFBF0214 | CR_GPIO_ODRAIN | 0-31 | 1 = Output enabled |
| 0xBFBF0218 | ??? | - | Simple blinker clock |
| 0xBFBF021C | ??? | 0-15 | 2 bits/pin, simple blink mode |
| 0xBFBF0220 | CR_GPIO_CTRL1 | 16-31 | Input/Output, 2 bits/pin, 1=output |
| 0xBFBF0224 | CR_SGPIO_DATA | s0-s15 | Low 17 bits 1 = high, only write when bit-31 is 0 |
| 0xBFBF0228 | CR_SGPIO_CDIV | s(all) | Clock divider: speeds: 0,1,2,3 |
| 0xBFBF022C | CR_SGPIO_CDLY | s(all) | Clock delay: 0..16 |
| 0xBFBF0230 | CR_SGPIO_MODE | s(all) | Bits 0,1,2 enable blink & shift register chip type |
| 0xBFBF0234 | CR_GPIO_FLAMOD | 0-15 | Low 16 bits 1 = blink enabled |
| 0xBFBF0238 | CR_GPIO_IMME | 0-15 | Low 16 bits 1 = fast response |
| 0xBFBF023C | CR_GPIO_FLAP0 | - | Blink pattern timing: 0, 1 |
| 0xBFBF0240 | CR_GPIO_FLAP1 | - | Blink pattern timing: 2, 3 |
| 0xBFBF0244 | CR_GPIO_FLAP2 | - | Blink pattern timing: 4, 5 |
| 0xBFBF0248 | CR_GPIO_FLAP3 | - | Blink pattern timing: 6, 7 |
| 0xBFBF024C | CR_GPIO_FMAP0 | 0-7 | Select blink pattern, 4 bits/pin |
| 0xBFBF0250 | CR_GPIO_FMAP1 | 8-15 | Select blink pattern, 4 bits/pin |
| 0xBFBF0254 | CR_SGPIO_FMAP0 | s0-s7 | Select blink pattern, 4 bits/pin |
| 0xBFBF0258 | CR_SGPIO_FMAP1 | s8-s15 | Select blink pattern, 4 bits/pin |
| 0xBFBF025C | CR_SGPIO_FMAP2 | s16 | Select blink pattern, only low 4 bits used |
| 0xBFBF0260 | CR_GPIO_CTRL2 | 32-47 | Input/Output, 2 bits/pin, 01=output |
| 0xBFBF0264 | CR_GPIO_CTRL3 | 48-63 | Input/Output, 2 bits/pin, 01=output |
| 0xBFBF0268 | CR_GPIO_FLAMOD_EXT | 16-31,37-51 | 1 = Blink enabled |
| 0xBFBF026C | ??? | - | Unused |
| 0xBFBF0270 | CR_GPIO_DATA1 | 32-63 | Value (R/W) |
| 0xBFBF0274 | ??? | - | Unused |
| 0xBFBF0278 | CR_GPIO_ODRAIN1 | 32-63 | 1 = Output enabled |

## Typical GPIOs
The wiring of any given GPIO is the decision of the board integrator, however there are some GPIO numbers which *typically* have certain roles.

| GPIO | EN7512 | EN7521 |
|------|:--------:|:--------:|
| 2    | [LED_DSL_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512) | [LED_XPON_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L150) |
| 3    | ? | [GE_LED](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/drivers/serial/tc3162_uart2.c#L478) |
| 4    | [LED_INTERNET_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512#L107) | [LED_INTERNET_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L119)        |
| 6    | [Reset PCI bonding slave](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/pci/pci-tc3162u.c#L1329)          ||
| 7 | ? | [LAN Port 3](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/tc3162l2hp2h/ledctrl.c#L5109)
| 8 | ? | [LAN Port 2](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/tc3162l2hp2h/ledctrl.c#L5109)
| 9 | ? | [LAN Port 1](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/tc3162l2hp2h/ledctrl.c#L5109)
| 10 | ? | [LAN Port 0](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/tc3162l2hp2h/ledctrl.c#L5109)
| 11   | [LED_USB_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512#L128) | [LED_USB_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L150)             |
| 15   | [LED_PIN_PON](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/bootram/init/main.c#L1978)                    ||
| 16   | | [LED_PHY_TX_POWER_DISABLE](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L128)   |
| 17   | [LED_PIN_ALARM](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/bootram/init/main.c#L1978)                  ||
| 19   | [LED_PIN_FXS1](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/bootram/init/main.c#L1978) ||
| 25   | [MDC_GPIO_DEF](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/ralink/ex_mdio_api.c#L33) ||
| 26   | [MDIO_GPIO_DEF](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/linux-2.6.36/arch/mips/ralink/ex_mdio_api.c#L33) ||
| 42   | [LED_PIN_LAN1](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/bootram/init/main.c#L1978)               ||
| 43   | [LED_PIN_LAN2](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/bootrom/bootram/init/main.c#L1978)               ||

## Wifi GPIO (64-81)
Unrelated to the EN751221 GPIO system, but there is another GPIO system which uses pins on the WLAN chip, this provides up to 17 additional GPIO lines and requests are forwarded over. This GPIO system is implemented in [EcoNet / MT7603 GPIO](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/wifi/MT7603/os/linux/bb_soc.c#L380) and in [EcoNet / MT7601 GPIO](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/wifi/MT7601E_LinuxAP_20130305_DPA/os/linux/bb_soc.c#L290). Wifi GPIO pins can be read and written, but do not trigger an interrupt directly. The vendor implementation does not use interrupts at all, instead it polls once per 2 seconds to check if a button is being pressed.

### Typical Wifi GPIOs
As with the other GPIOs, these board-specific, but there are some values which are more common.

| GPIO | Wifi GPIO | EN7512 | EN7521 |
|------|---|:--------:|:--------:|
| 65   | 1 | [LED_WLAN_WPS_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512#L128)    | [LED_WLAN_WPS_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L129)    |
| 67   | 3 | [LED_VOIP_HOOK2_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512#L124) | - |
| 77   | 13 | [LED_WLAN_WPS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512#L132) | [LED_WLAN_WPS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L144) (button) |
| 78   | 14 | [LED_VOIP_HOOK1_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512#L123C4-L123C6) | [LED_VOIP_HOOK1_STATUS](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L135)      |
| 79   | 15 | [LED_WLAN_RADIO](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7512#L131) | [LED_WLAN_RADIO](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/apps/private/led_conf/led.conf_en7521#L143) (poorly named reset button) |
