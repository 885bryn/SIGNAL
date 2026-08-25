# SIGNAL System Baseline Design

Date: 2026-08-25
Status: APPROVED DESIGN — awaiting repository commit/push verification

## Goal

Establish the initial authoritative design baseline for SIGNAL before feature implementation begins.

## Product boundary

SIGNAL is a passive, offline-first multi-radio environmental observation system. Android is the primary interface, compute, storage, GPS, analysis, and control unit. External hardware supplies observations through a Flipper gateway. The system emphasizes sensor fusion, history, evidence-based classification, baselines, anomaly reasoning, and explainable UX.

## Architecture summary

- Android owns UI, persistence, GPS attachment, final identity/classification, baselines, anomaly scoring, notifications, and exports.
- ESP32 owns normal environmental Wi-Fi observation and BLE where selected hardware can provide it cleanly.
- Flipper acts as gateway/controller and Sub-GHz source with a minimal local status interface.
- nRF24 contributes passive 2.4 GHz activity/spectrum measurements rather than duplicating Wi-Fi scanning.
- USB OTG is the primary first transport; BLE follows only after USB is proven.
- Radio sensors emit evidence; Android makes final human-facing conclusions.
- No active interference/attack capabilities are part of SIGNAL.
- No exact range, bearing, or transmitter-location claims are derived from RSSI/observer positions.

## Reuse strategy

Prefer mature upstream radio, transport, parser, and signature implementations where licensing and compatibility are verified. Keep original engineering concentrated in Android-side sensor fusion, persistence, identity, classification confidence, environment intelligence, and UX.

Candidate upstreams are documented in `third_party/UPSTREAMS.md`; exact commits, licenses, builds, and compatibility remain Phase 0 work.

## Critical implementation gate

Do not build the broad Android product before proving:

```text
ESP32 -> Flipper -> USB -> Android
```

The first transport proof must exercise synthetic and live telemetry, framing recovery, reconnect, throughput, and stability.

## Repository/document structure

The initial baseline consists of:

- `README.md`
- `AGENTS.md`
- `docs/PRODUCT.md`
- `docs/ARCHITECTURE.md`
- `docs/ROADMAP.md`
- `docs/DECISIONS.md`
- `docs/STATUS.md`
- `docs/superpowers/specs/2026-08-25-signal-system-baseline-design.md`
- `third_party/UPSTREAMS.md`
- `third_party/NOTICE.md`
- `third_party/LICENSES/` reserved for verified upstream license texts

Feature/task files are created only when work is approved and ready for decomposition. Source component directories are created when implementation tasks require them rather than as empty scaffolding.

## Success criteria for initialization

Initialization is complete only when:

1. these baseline documents are added to the SIGNAL repository;
2. Codex reports the actual Git branch/status;
3. the baseline is committed and pushed;
4. the resulting commit hash is known;
5. GitHub reflects the pushed baseline;
6. Sol reviews the committed baseline for consistency before F001 begins.

## Next feature after initialization

F001 — Hardware and Upstream Validation.

Its purpose is to replace unverified assumptions with measured hardware facts, pinned upstream commits, verified licensing, and baseline upstream build results before integration work begins.
