# SIGNAL — Roadmap

The roadmap is dependency-ordered. Later phases must not bypass validation gates from earlier phases.

## Phase 0 — Hardware and upstream validation

Deliverables:

- exact Flipper firmware identified;
- exact ESP32 chip/board/pinout identified;
- exact nRF24 module/carrier and available UART/SPI pins identified;
- Android version and USB OTG behavior identified;
- candidate upstream projects cloned and pinned to exact commits;
- licenses/provenance recorded;
- relevant upstream projects built unchanged where practical;
- compatibility findings documented.

Gate: no integration assumptions become authoritative until validated here.

## Phase 1 — Transport vertical slice

Deliverable:

```text
ESP32 emits test observation
-> Flipper receives it
-> Flipper forwards it
-> Android displays it
```

Must prove 1000 synthetic records plus live ESP observations, framing recovery, disconnect/reconnect, acceptable throughput, and stable Flipper behavior.

## Phase 2 — Real ESP32 Wi-Fi

Replace synthetic data with passive Wi-Fi observations using the selected upstream base. Android displays AP/client information where available, RSSI, channel, and timestamps.

## Phase 3 — Android storage and identity

Add normalized observations, Room persistence, first/last seen tracking, signal inventory, and history.

## Phase 4 — nRF24 passive activity

Integrate only verified passive RF Lab-derived scanner functionality and stream nRF activity to Android.

## Phase 5 — Sub-GHz passive observation

Add passive Flipper Sub-GHz observations and stream them to Android.

## Phase 6 — BLE

Use ESP32 BLE if the selected hardware/firmware supports it cleanly. Add Android fallback only if hardware validation requires it.

## Phase 7 — Evidence classification

Import/normalize approved signature sources and implement candidate classification, confidence, and evidence explanations.

## Phase 8 — Environment intelligence

Add learned baselines, meaningful timeline state changes, anomaly rules/scores, and trust/watch/ignore workflows.

## Phase 9 — Primary UI

Build Radar, Signals, Signal Detail, Spectrum, Intel, and Sensor Array.

## Phase 10 — Geographic observation map

Attach phone GPS to observations and present observer locations, routes, repeated sightings, and heatmaps without asserting exact transmitter positions.

## Phase 11 — Exports, background behavior, and polish

Add exports, optional BLE phone transport, reconnect/crash recovery, performance work, and battery optimization.

## Current ordering rule

Do not start Phase 2+ work until the Phase 1 transport seam is proven. Do not build broad UI before transport and storage foundations exist.
