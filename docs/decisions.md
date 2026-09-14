# Decisions and open questions

## Confirmed decisions

| Date | Decision | Reason |
| --- | --- | --- |
| 2026-09-14 | Use 13 independent keys | Allows all purchased 1U keycaps to have separate actions |
| 2026-09-14 | Place encoder at top-left and joystick at top-right | Makes joystick operation easier for right-handed users |
| 2026-09-14 | Use XIAO nRF52840 non-Plus | Already owned and suitable for low-power BLE |
| 2026-09-14 | Use JezailFunder Mist switches | Purchased; quiet Choc V2-compatible linear switch |
| 2026-09-14 | Use LCK AI keycaps | Purchased for the intended command layout |
| 2026-09-14 | Use hot-swap sockets for all 13 key switches | Enables switch replacement and provides assembly practice for future keyboard projects; socket part now fixed as CPG135001S30; physical compatibility remains to be validated |
| 2026-09-14 | Use Alps Alpine SKRHABE010 for the top-right control | Provides four digital directions and a center push without analog calibration |
| 2026-09-14 | Place one addressable RGB LED under every key switch | Uses all 13 keys as a status-animation surface while consuming one data GPIO |
| 2026-09-14 | Do not add separate decorative underglow to v0.1 | Avoids redundant LEDs and reduces battery load |
| 2026-09-14 | Write an independent firmware implementation | The Core2 project is a protocol observation reference only |
| 2026-09-14 | Validate BLE compatibility before PCB ordering | Host detection is the largest project risk |

## Additional confirmed selections

- Use Kailh CPG135001S30 sockets for all 13 switches; self-assemble the sockets. Selection is fixed, with Mist/PCB/RGB fit testing still pending.
- Use OPSCO SK6803MINI-E-001 LEDs for all 13 keys (JLCPCB C5242955); outsource their assembly. Exact electrical specifications and placement validation remain pending.
- Directly solder the owned XIAO nRF52840 to the PCB and use reverse-side GPIO pads. Connection method and recovery access still need design verification.
- Adopt Bourns PEC11R-4215F-S0024 on the basis of outsourced assembly. Sourcing and manufacturer-compatible assembly process must be confirmed before ordering.
- Outsource difficult parts to JLCPCB rather than require a fully assembled device. Remaining assembly scope is decided per part.
- Allow the encoder knob to protrude. The builder will choose its appearance and purchase it later; the maximum mechanical envelope is still a layout requirement.

## Current assumptions

| Assumption | Validation needed |
| --- | --- |
| The complete observed identity profile is sufficient for ChatGPT Desktop detection | Pair a physical XIAO and inspect settings availability |
| `ACT11` represents the second switch under the wide microphone key | Send press/release and inspect remapping behavior |
| SKRHABE010 can reproduce all ChatGPT direction functions | Verify the four fixed angle/distance pairs and center-push mapping |
| The six Agent status updates can drive patterns across all 13 RGB LEDs | Decode `v.oai.thstatus` on current desktop version |
| Enough genuine SKRHABE010 stock can be secured for prototypes and repairs | The manufacturer marks this part discontinued; buy and inspect spares before PCB ordering |

## Open decisions

- Knob mechanical envelope (appearance/purchase deferred)
- Verified footprints and fit for CPG135001S30, Mist, SK6803MINI-E-001, and PEC11R-4215F-S0024
- LED electrical specifications and power-control circuit
- Battery capacity, connector, and physical placement
- Exact XIAO reverse-side GPIO allocation and soldering/access method
- Whether to add a separate conventional keyboard HID report
- PCB mounting, plate material, and enclosure process

## Decision rule

Do not convert an assumption to a confirmed decision based only on another compatibility implementation. Record the test hardware, host OS, ChatGPT Desktop version, and firmware commit that reproduced it.
