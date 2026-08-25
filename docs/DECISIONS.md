# SIGNAL — Architecture and Product Decisions

These decisions are approved unless later hardware validation proves a concrete incompatibility or Sol proposes and the user approves an intentional architecture change.

## D001 — Android is the central unit

**Decision:** Android owns the primary UI, persistent database, GPS attachment, signal identity, final classification, environment baselines, anomaly scoring, timeline, notifications, exports, and sensor control.

**Reason:** Centralizing analysis and persistence prevents duplicated state and conflicting classifications across constrained sensor devices.

## D002 — ESP32 owns normal environmental Wi-Fi observation

**Decision:** Normal Wi-Fi environmental scanning is performed by the ESP32. Android Wi-Fi scanning is off in normal external-sensor mode and may only be added for explicit diagnostic, fallback, disconnected-hardware, or comparison/testing modes.

## D003 — One authoritative sensor per RF domain

**Decision:** Avoid continuous duplicate collection across multiple radios. Environmental BLE belongs to ESP32 where practical; Android is fallback only when required and explicitly selected.

## D004 — Flipper is a gateway, not the main UI

**Decision:** SIGNAL Node provides sensor control, forwarding, health, Sub-GHz observation, and a minimal local status view. Radar, map, inventory, anomaly analysis, and polished reports remain on Android.

## D005 — nRF24 is passive 2.4 GHz activity/spectrum, not Wi-Fi

**Decision:** nRF24 reports channel/activity/persistence-style measurements and later protocol-specific decoding only where justified.

## D006 — Use Flipper built-in Sub-GHz hardware first

**Decision:** Do not add another CC1101 unless hardware testing demonstrates a real advantage.

## D007 — USB before BLE for Android↔Flipper transport

**Decision:** Prove USB OTG / serial-style end-to-end telemetry first. BLE follows after USB is reliable.

## D008 — Prove the transport seam before full Android application development

**Decision:** The first integration proof is ESP32 → Flipper → USB → Android test app, including synthetic and live telemetry, recovery, reconnect, throughput, and stability checks.

## D009 — Classification is evidence-based and finalized on Android

**Decision:** Sensors emit observations/evidence. Android combines evidence into candidate vendor/category labels with confidence and explanations.

## D010 — Passive-only acquisition

**Decision:** SIGNAL excludes Wi-Fi deauthentication/injection, jamming, beacon spam, BLE spam, MouseJack/injection, and other active interference or attack functions, even if upstream projects contain them.

## D011 — No fabricated RF capabilities

**Decision:** RSSI may support proximity-strength visualization but not exact range. Radar angular placement is visual stability, not measured direction. GPS observation points are observer locations, not exact transmitter positions.

## D012 — Reuse mature upstream implementations before writing low-level code

**Decision:** Do not write new radio drivers, scanners, parsers, transport layers, signature databases, or protocol implementations when a credible legally reusable upstream implementation meets the requirement or can be minimally extended.

## D013 — Monorepo with explicit component/licensing boundaries

**Decision:** Android, Flipper FAP, ESP32 firmware, protocol, tooling, vendored upstream components, and third-party provenance live in one repository while preserving license boundaries between components.

## D014 — Preserve GPL isolation if FlipDeFlock source is reused directly

**Decision:** If SIGNAL Node becomes a direct FlipDeFlock derivative, keep the Flipper component GPL-compatible and avoid unnecessarily copying GPL-derived source into the Android application.

## D015 — Versioned telemetry protocol

**Decision:** The sensor protocol must be versioned and carry enough source/time/channel/frequency/evidence metadata to recover from framing errors and support normalization without pretending sensors know final classifications.

## D016 — Rule-based anomaly engine first

**Decision:** Begin with tunable explainable rules and reasoned scores. Do not market the initial anomaly system as AI.
