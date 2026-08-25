# SIGNAL — Architecture

## Status

Approved architecture baseline. Hardware-specific compatibility and upstream implementation details remain subject to Phase 0 validation.

## System topology

```text
ANDROID PHONE
  UI / Radar / Map / Room DB / Classification / Baselines / Anomaly
  GPS / Timeline / Exports / Alerts / Sensor Control
        |
        | USB OTG primary
        | BLE secondary after USB is proven
        v
FLIPPER ZERO — SIGNAL Node
  Sensor gateway / status / forwarding
    | UART            | SPI              | built-in CC1101
    v                 v                  v
  ESP32              nRF24             Sub-GHz
  Wi-Fi/BLE          2.4 GHz activity   passive observation
```

## Component ownership

| Capability | Authoritative component |
|---|---|
| Wi-Fi AP observation | ESP32 |
| Wi-Fi client/station observation | ESP32 |
| Probe/management frame observation | ESP32 |
| Wi-Fi channel statistics | ESP32 |
| Environmental BLE | ESP32 where hardware supports practical operation |
| BLE fallback | Android only when ESP hardware cannot provide it or for approved diagnostics/testing |
| nRF24 spectrum/activity | nRF24 + Flipper |
| Sub-GHz observation | Flipper CC1101 |
| GPS | Android phone |
| Final classification | Android |
| Signature database | Android |
| Signal identity/fingerprinting | Android |
| Persistent history | Android |
| Environment baseline | Android |
| Anomaly scoring | Android |
| Radar | Android |
| Geographic map | Android |
| Exports | Android |
| Notifications | Android |
| Flipper↔phone transport | Reused/adapted from supported Flipper transport mechanisms where practical |

This table is an architectural invariant unless deliberately changed through architecture review.

## Android responsibilities

Android is the central compute, storage, GPS, analysis, control, and UI unit. It normally does not perform environmental Wi-Fi scanning. Android Wi-Fi scanning may later exist as diagnostic fallback, disconnected-hardware mode, or comparison/testing mode, but remains off during normal external-sensor operation.

Suggested package structure inside one Android application module:

```text
transport/{flipper,usb,ble,protocol}
observation/{raw,normalized,identity}
classification/{evidence,signatures,vendor,category}
persistence/{room,repository}
environment/{baseline,comparison}
anomaly/{rules,scoring}
feature/{radar,map,signals,signaldetail,timeline,spectrum,intelligence,sensors,environments,settings}
export/{csv,json,geojson,kml,wigle,debrief}
```

Do not adopt a large multi-module architecture merely because an upstream project uses one.

## Processing pipeline

```text
Flipper transport
  -> wire decoder
  -> RawObservation
  -> observation normalizer
  -> identity / fingerprinting
  -> evidence classifier
  -> signal repository
      -> timeline
      -> environment baseline
      -> anomaly engine
      -> UI state
```

Radio hardware emits observations and raw evidence. Android produces final human-facing identity/category/confidence.

## Core observation model

Minimum observation families:

- `WifiApObservation`
- `WifiClientObservation`
- `WifiActivityObservation`
- `BleAdvertisementObservation`
- `NrfSpectrumObservation`
- `SubGhzObservation`
- `SensorStatusObservation`

Common metadata should include source, timestamp, RSSI/activity metric where meaningful, frequency/channel, raw identifier where available, and raw metadata. Android attaches phone GPS at observation time.

## Classification model

Classification is evidence-based:

```text
Evidence[] -> classifier -> candidate classification -> confidence
```

A sensor may report evidence such as an OUI hit, SSID pattern, frame behavior, service UUID, or activity characteristics. Android combines that evidence and must preserve the reasons shown to the user.

Do not encode simplistic mappings such as `MAC -> police car`.

## ESP32 responsibility

ESP32 owns environmental Wi-Fi observation. Depending on the exact chip and verified firmware capabilities, it may provide:

- 2.4 GHz AP observation;
- management-frame/client/probe activity;
- channel statistics;
- passive frame metadata;
- 5 GHz observation where the selected hardware/firmware actually supports it;
- environmental BLE where supported and practical.

Do not duplicate continuous Android scanning merely because Android exposes related APIs.

Hardware-specific firmware selection remains a Phase 0 decision:

- ESP32-C5: evaluate WatchFlock/Marauder C5 as preferred candidates.
- classic ESP32 / ESP32-S3: evaluate Marauder passive components plus compatible companion telemetry.
- ESP32-S2: verify lack of BLE for the exact board/toolchain; if confirmed, use an approved BLE fallback or hardware change rather than silent duplication.

## Flipper responsibility

The Flipper is primarily a sensor gateway and radio controller, not the main UI. The SIGNAL Node FAP should provide:

- ESP32 UART control/telemetry;
- nRF24 control/telemetry;
- passive Sub-GHz observation;
- sensor health/status;
- forwarding to Android;
- a minimal local status screen for debugging without the phone.

It should not duplicate Android radar, map, anomaly engine, signal inventory, or polished report generation.

## nRF24 responsibility

nRF24 provides passive 2.4 GHz activity/spectrum measurements such as channel, activity/persistence, max-hold/activity level, and timestamps. It is not a second Wi-Fi scanner.

## Sub-GHz responsibility

Use the Flipper's built-in Sub-GHz hardware first. Do not add another CC1101 unless hardware testing demonstrates a concrete advantage.

## Sensor protocol

Start from compatible upstream line-oriented telemetry concepts where practical and extend rather than redesign without cause.

Conceptual record families include:

```text
S / STAT  sensor/status
D         detection/evidence
BLE       BLE observation
W         Wi-Fi AP observation
WC        Wi-Fi client observation
WF        Wi-Fi frame/activity summary
NRF       nRF24 activity observation
SUB       Sub-GHz observation
ERR       sensor error
CAP       capability announcement
```

The final schema must be versioned. Every observation should directly or indirectly carry protocol version, source, time, identifier where available, RSSI/activity, channel/frequency, raw evidence, and metadata.

Avoid bulk raw-frame/PCAP streaming in the initial version. Raw capture is a later optional capability.

## Android↔Flipper transport

USB OTG / serial-style transport is the first implementation target because it is easier to debug and should provide deterministic development behavior and adequate throughput.

BLE is secondary and should follow only after USB end-to-end telemetry is reliable.

Before building the full application, prove this seam:

```text
ESP32 -> Flipper FAP -> USB -> Android test app
```

The transport spike must demonstrate at least 1000 synthetic telemetry records plus live ESP Wi-Fi observations, framing recovery, disconnect/reconnect behavior, acceptable throughput, and no Flipper crash.

If a stock external-app path cannot expose the required USB stream cleanly, evaluate in this order:

1. supported CLI/application bridge;
2. supported RPC facility;
3. minimal Flipper firmware extension;
4. custom BLE profile only as a last architectural change.

Any step that changes the approved transport architecture requires explicit review.

## Upstream/reuse architecture

Candidate upstream projects are recorded in `third_party/UPSTREAMS.md`. Their exact commits, licenses, build status, and compatibility are not considered verified until Phase 0 completes.

Intended separation:

- Android application: original SIGNAL code plus narrowly reused MIT-compatible transport components where verified.
- SIGNAL Node FAP: may become a GPL-compatible derivative if directly based on FlipDeFlock; keep this licensing boundary isolated.
- ESP32 sensor: preferably MIT-derived Marauder/WatchFlock passive subset where verified.
- nRF24 scanner: reuse/adapt passive RF Lab pieces where verified.

Do not copy GPL-derived implementation into Android without deliberate licensing review.

## Repository layout

Target monorepo shape:

```text
android-app/
flipper-fap/
esp32-sensor/
protocol/{specification,fixtures,compatibility-tests}
vendor/{android-flipper-transport,pingequa-rf-scanner}
tools/{telemetry-simulator,log-parser,signature-tools}
docs/
third_party/{UPSTREAMS.md,LICENSES/,NOTICE.md}
```

Do not create empty source trees merely to satisfy this diagram; create them when approved implementation tasks require them.

## Architecture invariants

1. Reuse before implementation.
2. One authoritative sensor per RF domain.
3. One final classifier: Android.
4. One persistent history: Android.
5. Hardware emits evidence, not final conclusions.
6. Every inferred label has confidence and supporting evidence.
7. Do not fabricate direction or exact distance.
8. Observation-map points are observer locations, not guaranteed transmitter locations.
9. Do not silently fall back to a second radio.
10. Sensor failure is visible in Sensor Array status.
11. Radio acquisition remains passive.
12. Maintain upstream attribution and commit provenance.
13. Modify upstream code minimally until end-to-end functionality exists.
14. Do not prematurely redesign mature upstream radio code.
15. If upstream functionality meets the requirement, wrap it rather than rewrite it.
