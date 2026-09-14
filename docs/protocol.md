# Observed Codex Micro compatibility protocol

This document records compatibility behavior observed by third-party implementations. It is not an official protocol specification.

## Confidence labels

- **Observed:** present in a publicly available implementation reported to work with physical hardware and ChatGPT Desktop.
- **Inferred:** a likely meaning or value that still needs confirmation on this project.
- **Verified:** reproduced by this project on XIAO nRF52840. Nothing is Verified yet.

## BLE identity

The following complete profile was reported as compatible. The smallest mandatory subset has not been isolated, so the PoC should initially reproduce the complete profile.

| Field | Value | Confidence |
| --- | --- | --- |
| BLE device name | `Codex Micro` | Observed |
| Manufacturer string | `Work Louder` | Observed |
| PnP source | `0x02` | Observed |
| Vendor ID | `0x303A` | Observed |
| Product ID | `0x8360` | Observed |
| Product release | `0x0101` | Observed |
| Pairing | Bonding, no input/output (`Just Works`) | Observed |
| Appearance | Generic HID | Observed |

The firmware should make these constants easy to replace. They are compatibility values and must not be used as evidence that the device is official hardware.

## HID report descriptor

One vendor-defined application collection is used.

| Field | Value |
| --- | --- |
| Usage Page | Vendor Defined `0xFF00` |
| Application Usage | `0x01` |
| Report ID | `6` |
| Logical range | `0` to `255` |
| Report size | 8 bits |
| Input report count | 63 bytes |
| Output report count | 63 bytes |

Observed descriptor bytes:

```text
06 00 FF 09 01 A1 01 85 06 15 00 26 FF 00 75 08
95 3F 09 01 81 02 95 3F 09 02 91 02 C0
```

On BLE HOGP the Report ID normally selects the report characteristic and is not included in the 63-byte characteristic value. The receive path may defensively accept a leading `0x06` as well.

## Report framing

Each 63-byte report body uses this structure:

| Offset | Size | Meaning |
| ---: | ---: | --- |
| 0 | 1 | Message type, observed value `2` |
| 1 | 1 | Payload length, `0..61` |
| 2 | 0..61 | UTF-8 JSON fragment |
| remaining | variable | Zero padding |

Outgoing JSON is terminated by `\n` and split into fragments of at most 61 bytes. The implementation must serialize complete messages through a single queue so fragments from different messages cannot interleave.

The receive accumulator must have a fixed maximum size and a timeout. Invalid length, malformed UTF-8, malformed JSON, overflow, or a new top-level message during incomplete reception must reset the accumulator safely.

## Device-to-host events

Messages resemble JSON-RPC notifications but do not include a `jsonrpc` member.

### Keys and encoder

```json
{"method":"v.oai.hid","params":{"k":"AG00","act":1,"ag":0}}
```

| Parameter | Meaning |
| --- | --- |
| `k` | Physical/control identifier |
| `act: 0` | Release |
| `act: 1` | Press |
| `act: 2` | One encoder step |
| `ag` | Optional Agent slot index `0..5` |

Observed identifiers:

- Agent keys: `AG00` through `AG05`
- Command keys: `ACT06`, `ACT07`, `ACT08`, `ACT09`, `ACT10`, `ACT12`
- Encoder: `ENC_CC`, `ENC_CW`, `ENC`

`ACT11` is an **inferred** identifier for the second physical switch under the wide microphone key. OpenAI documents that the two microphone switches can be assigned independently, but this identifier has not yet been verified.

### Joystick

```json
{"method":"v.oai.rad","params":{"a":0.75,"d":1.0}}
```

Angles are normalized turns:

| Direction | `a` | Press `d` | Release `d` |
| --- | ---: | ---: | ---: |
| Right | `0.00` | `1.0` | `0.0` |
| Down | `0.25` | `1.0` | `0.0` |
| Left | `0.50` | `1.0` | `0.0` |
| Up | `0.75` | `1.0` | `0.0` |

A digital four-direction control can emit these endpoint values. A true analog stick can calculate continuous angle and distance, while ChatGPT Desktop ultimately resolves movement into four configurable directions.

## Host-to-device requests

Host requests include `method`, usually `params`, and `id`. Responses echo `id` and contain either `result` or `error`.

| Method | Required response or effect | Confidence |
| --- | --- | --- |
| `sys.version` | Return a firmware version | Observed |
| `device.status` | Return version, profile, layer, battery, charging | Observed |
| `v.oai.thstatus` | Update one or more Agent status lights | Observed |
| `v.oai.rgbcfg` | Store/apply ambient and key lighting settings | Observed |
| `lights.preview` | Apply preview if supported; otherwise acknowledge | Observed |
| `host.focused_app` | Store/apply app context if supported; otherwise acknowledge | Observed |

Suggested status response shape for the PoC:

```json
{
  "id": 1,
  "result": {
    "version": "0.1.0-xiao",
    "profile_index": 0,
    "layer_index": 1,
    "battery": 100,
    "is_charging": false
  }
}
```

Unknown methods should receive an error compatible with JSON-RPC method-not-found semantics:

```json
{"id":1,"error":{"code":-32601,"message":"Method not found"}}
```

## Agent status payload

Each `v.oai.thstatus` array member has been observed with these compact fields:

| Field | Meaning |
| --- | --- |
| `id` | Agent slot `0..5` |
| `c` | 24-bit RGB color |
| `b` | Brightness multiplier |
| `e` | Effect such as `off` or `breath` |
| `s` | Effect speed |

Validate all indexes and numeric ranges before writing shared state.

## PoC acceptance criteria

- macOS pairs with the device and ChatGPT Desktop exposes Codex Micro settings.
- The device answers `sys.version` and `device.status` without reconnect loops.
- `AG00` press/release selects an Agent slot.
- One known Command key executes its configured action.
- `ACT11` behavior is recorded as accepted, ignored, or rejected.
- Encoder clockwise/counterclockwise and press are recognized.
- All four joystick directions press and release cleanly.
- Agent status updates are received and decoded.
- Disconnect during a held control leaves no stuck state after reconnect.

Record the macOS version, ChatGPT Desktop version, firmware commit, and HID descriptor hash for every run.

## Sources

- [OpenAI Codex Micro documentation](https://learn.chatgpt.com/docs/features/codex-micro)
- [imliubo/codex-micro-4-core2 technical notes](https://github.com/imliubo/codex-micro-4-core2/blob/main/docs/TECHNICAL.md)
