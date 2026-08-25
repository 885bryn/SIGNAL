# SIGNAL

SIGNAL is a personal, offline-first RF/environmental observation system built around an Android phone, a Flipper Zero sensor gateway, an ESP32 Wi-Fi/BLE sensor, an nRF24L01-class passive 2.4 GHz activity sensor, and the Flipper's Sub-GHz radio.

The product is intentionally passive. Its original engineering focus is Android-side sensor fusion, evidence-based classification, history, environmental baselines, anomaly analysis, and user experience. Mature open-source radio, transport, and parser implementations should be reused or minimally adapted where legally and technically appropriate.

## Authoritative project documents

Read these before implementation:

1. `docs/PRODUCT.md`
2. `docs/ARCHITECTURE.md`
3. `docs/DECISIONS.md`
4. `docs/ROADMAP.md`
5. `docs/STATUS.md`
6. Approved files in `docs/features/` and `docs/tasks/`
7. `AGENTS.md` for Codex execution rules

GitHub `main` is the source of truth for reviewed code. Chat history is supporting context only.

## Current state

The architecture baseline is approved, but implementation and hardware/upstream compatibility have not yet been validated. Phase 0 must audit exact hardware, pin upstream repositories to concrete commits, verify licensing, and build relevant upstream projects unchanged before integration begins.
