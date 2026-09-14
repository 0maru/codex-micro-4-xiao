# Hardware design notes

## Fixed requirements

- Seeed Studio XIAO nRF52840, non-Plus model
- 13 independent Kailh Choc V2-compatible switches, all mounted in hot-swap sockets
- Top-left rotary encoder with push switch
- Top-right Alps Alpine SKRHABE010 four-direction switch with center push, placed for right-handed operation
- One independently addressable RGB LED under each of the 13 key switches
- No separate decorative underglow for v0.1
- Bluetooth Low Energy operation from a rechargeable LiPo battery
- USB-C access for charging, flashing, recovery, and development logs

## Mechanical layout

The key layout is defined in [`../layout/keyboard-layout.json`](../layout/keyboard-layout.json). It contains only the 13 keyboard switches. Mechanical center coordinates for the joystick and encoder will be added in KiCad after their exact parts are selected.

Use the purchased keycaps to confirm the final center-to-center pitch before locking PCB coordinates. KLE units alone are not manufacturing dimensions.

## Hot-swap mounting

Use 13 PCB-mounted hot-swap sockets for the purchased Mist switches. The sockets are soldered to the PCB; the key switches can then be removed without desoldering. This scope covers only the 13 keyboard switches, not the encoder, joystick, or XIAO.

The socket manufacturer and exact part number remain unselected. Before routing, verify the chosen socket and footprint against the actual Mist switch pins, fixing holes, PCB thickness, and manufacturer drawings. Check each socket together with its RGB LED, plate, case, and battery clearance. Do not assume every low-profile socket is compatible.

Include a small assembly trial before PCB ordering to practice socket soldering and supported switch insertion/removal. Plan access to support the socket during insertion and inspect for bent pins or lifted pads.

## Input matrix

The selected SKRHABE010 is a digital four-direction surface-mount switch with center push. The 13 keys, encoder push, four directions, and center push make 19 switch positions and fit in a 4×5 matrix. Encoder quadrature uses two more GPIOs and the addressable RGB chain uses one.

| Function | GPIO count |
| --- | ---: |
| 4×5 matrix | 9 |
| Encoder A/B | 2 |
| RGB data | 1 |
| Total | 12 |

The non-Plus XIAO exposes 11 edge GPIOs, so this baseline requires at least one reverse-side GPIO. A controllable LED power switch, battery sensing, or future features may require additional reverse-side pads or a revised circuit.

### SKRHABE010 constraints

- Part: Alps Alpine SKRHABE010
- Type: surface-mount, four digital directions with center push
- Body dimensions: 7.35×7.5×1.8mm
- Operating force: 1.23N direction, 2.35N center push
- Rated operating life: 200,000 cycles for each direction and center push
- Supply status: discontinued by the manufacturer

Secure multiple genuine parts before PCB ordering and verify the footprint against the manufacturer's land pattern. The enclosure must protect the small switch from excessive side load.

## Lighting priority

Place one independently addressable RGB LED under every one of the 13 key switches. The chain uses one serial data GPIO, and all keys act as a status-animation surface. ChatGPT Desktop status, BLE state, charging, low battery, and errors can be rendered across the chain. LED current can dominate the power budget.

Provide firmware controls for:

- global brightness limit
- automatic timeout
- complete LED power-off during deep sleep
- per-key status animation

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

- rotary encoder and knob
- exact hot-swap socket compatible with the purchased Mist switches
- switch diodes
- RGB LEDs
- LiPo capacity and connector
- power switch
- mounting system, plate thickness, and case construction
