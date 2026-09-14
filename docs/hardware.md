# Hardware design notes

## Fixed requirements

- Seeed Studio XIAO nRF52840, non-Plus model, directly soldered to the PCB with reverse-side GPIO connections
- 13 independent Kailh Choc V2-compatible switches, all mounted in Kailh CPG135001S30 hot-swap sockets
- Top-left Bourns PEC11R-4215F-S0024 rotary encoder with push switch, planned for outsourced assembly
- Top-right Alps Alpine SKRHABE010 four-direction switch with center push, placed for right-handed operation
- One OPSCO SK6803MINI-E-001 addressable RGB LED under each of the 13 key switches
- No separate decorative underglow for v0.1
- Bluetooth Low Energy operation from a rechargeable LiPo battery
- USB-C access for charging, flashing, recovery, and development logs

## Mechanical layout

The key layout is defined in [`../layout/keyboard-layout.json`](../layout/keyboard-layout.json). It contains only the 13 keyboard switches. Mechanical center coordinates for the selected joystick and encoder will be added in KiCad after their dimensions and operating clearances are verified.

Use the purchased keycaps to confirm the final center-to-center pitch before locking PCB coordinates. KLE units alone are not manufacturing dimensions.

## Hot-swap mounting

Use 13 PCB-mounted hot-swap sockets for the purchased Mist switches. The sockets are soldered to the PCB; the key switches can then be removed without desoldering. This scope covers only the 13 keyboard switches, not the encoder, joystick, or XIAO.

Kailh CPG135001S30 is selected, quantity 13 excluding practice parts and spares. Selection does not establish verified compatibility with Mist. Before routing, verify the chosen socket and footprint against the actual Mist switch pins, fixing holes, PCB thickness, and manufacturer drawings. Check each socket together with its RGB LED, plate, case, and battery clearance. Do not assume every low-profile socket is compatible.

Include a small assembly trial before PCB ordering to practice socket soldering and supported switch insertion/removal. Plan access to support the socket during insertion and inspect for bent pins or lifted pads.

## XIAO and assembly split

The builder will directly solder the owned XIAO and connect its reverse-side GPIO pads. Define the pad access, insulation, wire routing, and assembly sequence before placement is finalized; preserve reset, USB, and SWD recovery access.

The builder will solder CPG135001S30 sockets and insert the purchased key switches. Difficult components, including SK6803MINI-E-001 LEDs and the PEC11R-4215F-S0024 encoder, are planned for JLCPCB assembly. Decide the remaining part-by-part split after sourcing and process review. A library listing does not guarantee stock or acceptance of the proposed mounting orientation. Verify that the encoder assembly process meets its manufacturer's conditions; outsourcing alone does not resolve the hand-soldering concern.

The knob may protrude above the low-profile keys. Its appearance and purchase are deferred to the builder. Reserve maximum diameter, shaft engagement, push travel, and adjacent-key clearance before freezing PCB and case dimensions.

## Input matrix

The selected SKRHABE010 is a digital four-direction surface-mount switch with center push. The 13 keys, encoder push, four directions, and center push make 19 switch positions and fit in a 4×5 matrix. Encoder quadrature uses two more GPIOs and the addressable RGB chain uses one.

| Function | GPIO count |
| --- | ---: |
| 4×5 matrix | 9 |
| Encoder A/B | 2 |
| RGB data | 1 |
| Total | 12 |

The non-Plus XIAO exposes 11 edge GPIOs, so this baseline requires at least one reverse-side GPIO. LED power-enable control should be budgeted separately: a dedicated enable adds one GPIO, giving 13 before any additional external sensing/control. Final allocation and battery measurement remain unverified.

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

### Selected LED and pending validation

- Manufacturer: OPSCO Optoelectronics
- Exact part: SK6803MINI-E-001; quantity: 13
- JLCPCB identifier: C5242955
- Reference: [JLCPCB part page](https://jlcpcb.com/partdetail/5951765-SK6803MINI_E001/C5242955)
- Assembly: outsource to JLCPCB; the previously inspected listing offered Economic and Standard assembly, to be reconfirmed for the actual order.

Part selection is confirmed, not electrical or mechanical compatibility. Obtain and verify the exact part datasheet: supply range, input-high threshold, timing, RGB order, channel current, quiescent current, pinout, package dimensions, recommended lands, aperture, and soldering orientation. The listing's 3mA optical test current is not the maximum total LED current. Do not copy supply or standby-current values from SK6812MINI-E.

Do not assume direct 3.3V supply, LiPo supply, or 3.3V data compatibility. Determine whether regulation/boost and level conversion are needed. Provide genuine LED power isolation during sleep, prevent back-power through DIN, and verify the disabled regulator's leakage path. Bypass capacitors, bulk capacitance, brightness/current limits, and power-control parts remain open.

Validate one complete Mist + CPG135001S30 + SK6803MINI-E-001 key position before repeating it across the layout. Check optical clearance as well as solder-pad, socket, and case interference.

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

- encoder knob (appearance/purchase deferred; mechanical envelope required before layout freeze)
- switch diodes
- LED power-control and any required regulator/level-conversion parts
- LiPo capacity and connector
- power switch
- mounting system, plate thickness, and case construction
