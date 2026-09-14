# Repository instructions

## Project boundaries

- Implement the firmware and hardware design independently.
- Use `imliubo/codex-micro-4-core2` only as a record of observed protocol behavior. Do not copy its implementation.
- Keep verified observations, inferences, and open questions clearly separated.
- Do not describe this project or its hardware as official OpenAI or Work Louder hardware.

## Hardware constraints

- Target Seeed Studio XIAO nRF52840, non-Plus model.
- Preserve the 13-key KLE layout unless a design decision explicitly changes it.
- The top-left control is a rotary encoder and the top-right control is an Alps Alpine SKRHABE010 four-direction switch with center push.
- Key switches are Kailh Choc V2-compatible JezailFunder Mist switches.
- All 13 key switches must use hot-swap sockets. Socket soldering to the PCB is required; exact socket selection and compatibility validation remain open. This decision does not make the XIAO, encoder, or joystick socketed.
- Place one independently addressable RGB LED under each of the 13 key switches. Do not add separate decorative underglow to v0.1.
- Optimize for BLE battery life. Status lighting must be dimmable and able to turn off completely.
- Do not finalize footprints before the exact encoder, socket, RGB LED, and battery connector parts are selected. Verify the SKRHABE010 footprint against the manufacturer land pattern.

## Firmware constraints

- Prefer a small dedicated Zephyr application over modifying ZMK internals.
- Keep matrix scanning, input mapping, protocol framing, BLE transport, and lighting as separate modules.
- Serialize all outgoing HID messages through one queue.
- Bound receive buffers and recover from malformed or incomplete messages.
- Pair every press with a release, including disconnect and sleep recovery paths.
- Never log account data or complete host messages by default.

## Validation

- Test protocol behavior on physical XIAO nRF52840 hardware and current ChatGPT Desktop.
- Record OS and ChatGPT Desktop versions with each compatibility result.
- Re-pair after changing the HID descriptor or device identity.
- Verify reconnect, sleep/wake, battery reporting, encoder steps, joystick release, and all 13 keys before PCB ordering.
