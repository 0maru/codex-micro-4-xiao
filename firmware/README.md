# Firmware

This directory will contain an independent Zephyr application for Seeed Studio XIAO nRF52840.

The first milestone is a protocol PoC that exposes the observed vendor-defined BLE HID report and can be driven without the final PCB. Matrix, encoder, joystick, and lighting support will be added after host detection succeeds.

## Intended structure

```text
firmware/
  CMakeLists.txt
  prj.conf
  app.overlay
  src/
    main.c
    protocol.c
    protocol.h
    transport.c
    transport.h
    controls.c
    controls.h
```

The exact Zephyr or nRF Connect SDK release is intentionally not pinned yet. Pin it when the first physical XIAO build succeeds, then add CI using the same toolchain version.

See [`../docs/protocol.md`](../docs/protocol.md) for the observed compatibility profile and PoC acceptance criteria.
