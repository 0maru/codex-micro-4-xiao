# Hardware design notes

## Fixed requirements

- Seeed Studio XIAO nRF52840, non-Plus model
- 13 independent Kailh Choc V2-compatible switches
- Top-left rotary encoder with push switch
- Top-right joystick, placed for right-handed operation
- Bluetooth Low Energy operation from a rechargeable LiPo battery
- USB-C access for charging, flashing, recovery, and development logs

## Mechanical layout

The key layout is defined in [`../layout/keyboard-layout.json`](../layout/keyboard-layout.json). It contains only the 13 keyboard switches. Mechanical center coordinates for the joystick and encoder will be added in KiCad after their exact parts are selected.

Use the purchased keycaps to confirm the final center-to-center pitch before locking PCB coordinates. KLE units alone are not manufacturing dimensions.

## Input matrix options

### Digital four-direction joystick

The 13 keys, encoder push, four directions, and optional joystick push fit in a 4×5 matrix: up to 19 switch positions with nine GPIOs. Encoder quadrature uses two more GPIOs.

| Function | GPIO count |
| --- | ---: |
| 4×5 matrix | 9 |
| Encoder A/B | 2 |
| Total | 11 |

This consumes all edge GPIOs on the non-Plus XIAO. Lighting, battery sensing, or future features would require reverse-side pads, an I/O expander, or a different matrix/driver design.

### Analog joystick

The 13 keys and encoder push fit in a 4×4 matrix. Joystick X/Y require two ADC inputs and encoder quadrature requires two digital inputs.

| Function | GPIO count |
| --- | ---: |
| 4×4 matrix | 8 |
| Encoder A/B | 2 |
| Joystick X/Y | 2 ADC |
| Total | 12 |

At least one reverse-side GPIO is therefore required before adding lighting. The analog stick's potentiometers also draw current continuously unless their supply is switched. Exact current depends on the selected part.

## Lighting priority

ChatGPT Desktop supplies status for six Agent slots. If LEDs are included, prioritize one independently addressable RGB LED per Agent key. A single serial data line is enough for addressable LEDs, but LED current can dominate the power budget.

Provide firmware controls for:

- global brightness limit
- automatic timeout
- complete LED power-off during deep sleep
- optional per-key status animation

The LED part, voltage compatibility, bypass capacitors, bulk capacitance, and data-level requirements remain open.

## Power

The XIAO nRF52840 has battery pads and an onboard charging path. The final design still needs:

- a protected 3.7V LiPo cell sized after current measurement
- an accessible hard power switch
- battery connector selection and polarity documentation
- a verified method for battery-level measurement
- sleep/wake behavior that does not lose bonding unexpectedly

Do not estimate runtime until the BLE PoC, scan loop, joystick, and LEDs have been measured together.

## PCB constraints

- Keep copper, ground pours, batteries, metal encoder bodies, and mounting hardware out of the antenna keep-out area.
- Keep the XIAO USB-C connector accessible after assembly.
- Add labelled test pads for SWD, power, ground, matrix rows/columns, and the vendor HID PoC.
- Place reset/recovery access where it remains usable after installing the case.
- Confirm Choc V2 switch and socket footprints from manufacturer drawings before routing.
- Check encoder and joystick body height against the low-profile keycaps in a side-view mechanical drawing.

## Open part selections

- joystick type and exact part number
- rotary encoder and knob
- Choc V2 hot-swap socket or direct soldering
- switch diodes
- RGB LEDs
- LiPo capacity and connector
- power switch
- mounting system, plate thickness, and case construction
