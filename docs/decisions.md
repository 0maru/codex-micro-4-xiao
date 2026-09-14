# Decisions and open questions

## Confirmed decisions

| Date | Decision | Reason |
| --- | --- | --- |
| 2026-09-14 | Use 13 independent keys | Allows all purchased 1U keycaps to have separate actions |
| 2026-09-14 | Place encoder at top-left and joystick at top-right | Makes joystick operation easier for right-handed users |
| 2026-09-14 | Use XIAO nRF52840 non-Plus | Already owned and suitable for low-power BLE |
| 2026-09-14 | Use JezailFunder Mist switches | Purchased; quiet Choc V2-compatible linear switch |
| 2026-09-14 | Use LCK AI keycaps | Purchased for the intended command layout |
| 2026-09-14 | Write an independent firmware implementation | The Core2 project is a protocol observation reference only |
| 2026-09-14 | Validate BLE compatibility before PCB ordering | Host detection is the largest project risk |

## Current assumptions

| Assumption | Validation needed |
| --- | --- |
| The complete observed identity profile is sufficient for ChatGPT Desktop detection | Pair a physical XIAO and inspect settings availability |
| `ACT11` represents the second switch under the wide microphone key | Send press/release and inspect remapping behavior |
| A digital joystick can reproduce all ChatGPT direction functions | Verify the four fixed angle/distance pairs |
| The six Agent status updates can drive six local RGB LEDs | Decode `v.oai.thstatus` on current desktop version |

## Open decisions

- Digital or analog joystick
- Joystick push switch requirement
- Encoder part, detent count, shaft length, and knob
- Direct-solder or hot-swap switches
- Per-Agent RGB, underglow, both, or no RGB for the first PCB
- Battery capacity, connector, and physical placement
- Exposed-edge pins only or reverse-side XIAO pads
- Whether to add a separate conventional keyboard HID report
- PCB mounting, plate material, and enclosure process

## Decision rule

Do not convert an assumption to a confirmed decision based only on another compatibility implementation. Record the test hardware, host OS, ChatGPT Desktop version, and firmware commit that reproduced it.
