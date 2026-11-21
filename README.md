# LineCraft GRBL

Firmware for **LineCraft**, a low-cost 2D CNC plotter (150 × 150 mm, under $50) that draws with a pen or in sand.

Project page: https://ochilovrustam.github.io/cnc-plotter.html

This is GRBL 0.9j, using the Costycnc modification that drives **28BYJ-48 stepper motors through ULN2003 boards** directly, plus an RC servo for lifting the pen. Standard GRBL expects step/direction stepper drivers; this version switches the four motor coils itself in a half-step sequence.

## Hardware

- Arduino Nano (ATmega328P)
- 2 × 28BYJ-48 stepper motors with ULN2003 driver boards
- MG90S servo for pen / stylus up and down
- 5 V, 2 A power supply
- No limit switches: the machine is zeroed by hand before each drawing

## Pin map

| Arduino Nano | Connects to |
|---|---|
| D5, D4, D3, D2 | X-axis ULN2003: IN1, IN2, IN3, IN4 |
| A3, A2, A1, A0 | Y-axis ULN2003: IN1, IN2, IN3, IN4 |
| D11 | Servo signal |
| 5V, GND | Both ULN2003 boards and the servo |

## Pen control

The servo is driven through GRBL's spindle PWM output on D11:

- `M3 S90` puts the pen down
- `M5` lifts the pen

## Flashing

1. Copy the `grbl` folder into your Arduino `libraries` folder.
2. In the Arduino IDE, open **File → Examples → grbl → grblUpload**.
3. Select **Arduino Nano** and the right port, then upload.
4. Connect with Universal G-code Sender at 115200 baud.

## Calibration

The default steps per mm (`$100`, `$101`) do not match this machine. We calibrated each axis by commanding moves, measuring how far the pen really travelled, and correcting the value.

Our calibrated values:

| Setting | Value |
|---|---|
| `$100` X steps/mm | ≈ 126 |
| `$101` Y steps/mm | ≈ 132 |
| Max reliable feed rate | 350 mm/min |

For comparison, the theoretical value for a 28BYJ-48 in half-step mode (≈ 4096 steps per turn) with a 20-tooth GT2 pulley (40 mm per turn) is about 102 steps/mm.

## Credits and license

- [GRBL](https://github.com/gnea/grbl) by Sungeun K. Jeon and Simen Svale Skogsrud, licensed under GPLv3 (see `COPYING`).
- 28BYJ-48 / ULN2003 modification by Costycnc.
- Mechanical design based on the plotter in [this video](https://www.youtube.com/watch?v=d8ZynbxfBDo), with parts modified for our components.

Like GRBL itself, this firmware is distributed under the GNU General Public License v3.
