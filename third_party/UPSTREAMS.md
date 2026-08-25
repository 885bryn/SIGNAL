# SIGNAL — Candidate Upstreams

This file records intended upstream sources before Phase 0 verification. Do not treat repository availability, commit SHA, license, build status, or compatibility as verified until Phase 0 records evidence.

For every upstream actually reused, record:

- Repository
- Upstream URL
- Commit SHA
- License
- Files/components imported
- Why they are used
- Local modifications
- Last synchronization date
- Build/compatibility notes

## Candidate: ESP32 Marauder

- Repository: `justcallmekoko/ESP32Marauder`
- Intended use: ESP32 board support, passive Wi-Fi scanning, AP/client/probe/channel handling, passive frame parsing, and hardware compatibility work.
- Intended exclusions: deauthentication, injection, beacon spam, jamming, and other active/attack functions.
- License: **UNVERIFIED IN REPOSITORY — verify during Phase 0**
- Commit SHA: **UNPINNED**

## Candidate: WatchFlock

- Repository: `JakeSwiz/WatchFlock`
- Intended use: evaluate as ESP32-C5/passive telemetry base or reference, including UART telemetry and passive detection structure.
- License: **UNVERIFIED IN REPOSITORY — verify during Phase 0**
- Commit SHA: **UNPINNED**

## Candidate: PINGEQUA RF Lab

- Repository: `pingequalab/rf-lab`
- Intended use: passive nRF24 driver/scanner, register definitions, activity/max-hold/dwell concepts, board detection, chip arbitration.
- Intended exclusions: jammer/TX functionality.
- License: **UNVERIFIED IN REPOSITORY — verify during Phase 0**
- Commit SHA: **UNPINNED**

## Candidate: Official Flipper Android App

- Repository: `flipperdevices/Flipper-Android-App`
- Intended use: narrowly reuse/adapt USB/BLE transport, connection state, device abstraction, RPC/protobuf helpers, and reconnection behavior where suitable.
- Do not import the entire application architecture.
- License: **UNVERIFIED IN REPOSITORY — verify during Phase 0**
- Commit SHA: **UNPINNED**

## Candidate: FlipDeFlock

- Repository: `ReconGrunt/FlipDeFlock`
- Intended use: primary Flipper conceptual/base candidate for ESP link/parser, sensor control, evidence/signature concepts, session/hardware handling, and companion protocol concepts.
- Avoid duplicating its full UI/report surface because Android owns those capabilities.
- Potential licensing boundary: user handoff identifies GPL-3.0-or-later; **verify exact repository license during Phase 0 before reuse**.
- Commit SHA: **UNPINNED**

## Candidate: Flipper WiFi Marauder Companion

- Repository: `0xchocolate/flipperzero-wifi-marauder`
- Intended use: compatibility/transport reference, especially UART/unified serial/PCAP concepts if needed later.
- Not intended as a second simultaneous architectural base.
- License: **UNVERIFIED IN REPOSITORY — verify during Phase 0**
- Commit SHA: **UNPINNED**

## Reference candidate: wardriver_rev3

- Repository: `JosephHewitt/wardriver_rev3`
- Intended use: reference for GPS logging, Wi-Fi/BLE wardriving, WiGLE conventions, and ESP32 logging patterns only where needed.
- License: **UNVERIFIED IN REPOSITORY — verify during Phase 0**
- Commit SHA: **UNPINNED**
