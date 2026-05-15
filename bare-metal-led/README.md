# bare-metal-led

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**About:** LED driver on STM32H753ZI — RCC + GPIO register writes, no HAL. First project in the bare-metal DSP firmware portfolio.

Bare-metal firmware on the STM32H753ZI — no HAL, no CubeMX, register-level only. Every peripheral configured directly from RM0433. This project establishes the compile → flash → debug workflow before any DSP work begins.

---

## Goal

Bring up the STM32H753ZI in pure bare-metal: prove GPIO control via direct register writes, establish the OpenOCD + GDB debug workflow, and verify every configuration step through GDB register inspection.

This project has no library dependencies — not even CMSIS. Every address and every bit is explicit.

---

## Hardware

| Component | Details |
|---|---|
| MCU | STM32H753ZI — Cortex-M7 @ 64 MHz (HSI default, no PLL) |
| Board | Nucleo-144 |
| LED | LD1 green — PB0 |
| Debugger | ST-Link V3 onboard — SWD |
| Host | Arch Linux — arm-none-eabi-gcc, OpenOCD, GDB |

---

## How to Build

```bash
cd bare-metal-led
make
```

Produces `build/firmware.elf` and `build/firmware.bin`.

> Requires `arm-none-eabi-gcc` and `arm-none-eabi-binutils` installed.
> See `docs/toolchain-setup.md` at repo root for one-time setup.

---

## How to Flash and Debug

```bash
# Terminal 1 — start OpenOCD
openocd -f interface/stlink.cfg -f target/stm32h7x.cfg

# Terminal 2 — connect GDB
arm-none-eabi-gdb build/firmware.elf
(gdb) target extended-remote :3333
(gdb) monitor reset halt
(gdb) load
(gdb) continue
```

LED LD1 (green, PB0) toggles at approximately 1 Hz.

---

## Verification

GDB register inspection confirms correct configuration:

```
(gdb) x/xw 0x580244E0
0x580244e0: 0x00000002     // RCC_AHB4ENR: bit 1 set — GPIOB clock enabled

(gdb) x/xw 0x58020400
0x58020400: 0xXXXXXXX1     // GPIOB_MODER: bits [1:0] = 01 — PB0 output

(gdb) x/xw 0x58020414
0x58020414: 0x00000001     // GPIOB_ODR: PB0 high — LED on
```

Full register-level decisions and GDB verification in `docs/design.md`.

---

## Project Structure

```
bare-metal-led/
├── README.md
├── docs/
│   ├── design.md      # register decisions, BSRR vs ODR rationale, GDB verification
│   └── performance.md # build size, DWT methodology (sparse — grows from P03 onward)
├── src/
│   └── main.c
├── startup.s          # custom vector table and Reset_Handler
├── linker.ld          # FLASH + DTCM memory regions
└── Makefile
```

---

## Key References

- RM0433 Rev 8 §11 — GPIO registers
- RM0433 Rev 8 §8.7.40 — RCC_AHB4ENR
- DS12117 Rev 10 — Table 12 (PB0 pin assignment)
- PM0253 — Cortex-M7 boot sequence

---

## Release

`v0.1.0` — see repo-level [CHANGELOG.md](../CHANGELOG.md)
