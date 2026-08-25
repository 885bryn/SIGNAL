# SIGNAL — Product Definition

## Purpose

SIGNAL is a personal, offline-first multi-radio RF/environmental observation platform. It combines dedicated external radio sensors with an Android application that stores history, explains classifications, learns recurring environments, surfaces anomalies, and presents the live RF environment without pretending to provide capabilities the hardware cannot measure.

## User experience

The user connects a Flipper-based sensor assembly to an Android phone and opens SIGNAL. The phone is the primary interface and shows observations across:

- Wi-Fi access points;
- Wi-Fi client/station activity where supported;
- BLE advertisers where supported by the selected ESP32, with Android fallback only when explicitly required;
- passive 2.4 GHz nRF activity/spectrum observations;
- passive Sub-GHz observations.

The user can:

- view a live RSSI/activity-based radar visualization;
- inspect a master signal inventory and per-signal detail;
- see manufacturer/category candidates with confidence and supporting evidence;
- review first/last seen history and meaningful timeline events;
- compare the current environment with learned baselines;
- watch, trust, ignore, or label signals;
- inspect Wi-Fi/nRF/Sub-GHz channel or spectrum activity;
- see sensor health and connection state;
- map observation locations using phone GPS;
- export observations for later analysis.

## Product principles

1. Passive observation only.
2. Evidence before conclusions.
3. Explain every inferred classification.
4. Prefer historical/environmental context over treating every unknown transmitter as suspicious.
5. Reuse credible upstream engineering before writing new low-level radio or transport code.
6. Keep one authoritative owner for each RF domain and one final classifier/history store.
7. Make sensor failures visible.
8. Maintain licensing and upstream provenance.

## Primary screens

### Radar

A live RF environment visualization. Radial distance represents signal strength/proximity estimate. Angular placement is stable visualization only and must not be presented as measured direction.

Filters may include: ALL, WIFI, CLIENTS, BLE, NRF, SUBGHZ, NEW, UNKNOWN, PRIORITY, WATCHED.

### Geographic Observation Map

Uses phone GPS to show observer locations, routes, repeated sightings, strongest-observed areas, and heatmaps. It must describe observation positions rather than asserting exact transmitter positions.

### Signals

Master inventory of normalized signal identities and activity records.

### Signal Detail

Shows raw identifiers, sensor source, RSSI/activity, frequency/channel, manufacturer/category candidates, confidence, evidence, first/last seen, environments, persistence, timeline, anomaly reasons, and user actions.

### Timeline

Records meaningful state changes such as new/returned/disappeared signals, significant strength changes, classification changes, watched-signal appearances, Sub-GHz events, and environment deviations. It does not log every raw packet as a UI event.

### Spectrum

Separates actual channel/frequency activity from the radar metaphor. Covers Wi-Fi channels, nRF 2.4 GHz activity, and Sub-GHz activity where supported.

### Intel

Summarizes known/new/unknown/watched signals, infrastructure/camera-associated candidates, strong/persistent unknowns, and environment deviations with reasons.

### Sensor Array

Always shows actual hardware state, transport, active capabilities, record rate, drops/errors, and failures.

## Environment baselines

Android may learn environments such as HOME, OFFICE, CAR, HOTEL, and CUSTOM. Baselines answer:

- what is normally present;
- what appeared or disappeared;
- what returned;
- what became significantly stronger;
- what remains persistently unknown.

## Anomaly model

Start rule-based and explainable. Example rule families include:

- NewToEnvironment
- UnknownVendor
- UnknownCategory
- StrongSignal
- PersistentUnknown
- WatchedSignal
- InfrastructureSignature
- UnexpectedReturn
- RapidStrengthChange
- BaselineDeviation

Every anomaly score must include reasons and tunable weights.

## Exports

Planned Android-owned exports include CSV, JSON, GeoJSON, KML, WiGLE-compatible output where relevant, and a human-readable debrief.

## Explicit non-goals and prohibited capabilities

SIGNAL does not provide or claim:

- Wi-Fi deauthentication;
- Wi-Fi packet injection;
- jamming;
- BLE spam;
- MouseJack/injection functions;
- active interference features;
- exact RF ranging from RSSI;
- physical bearing/direction finding from the radar;
- exact transmitter location from observer GPS points;
- certainty that generic networking hardware belongs to a police vehicle or other specific organization;
- certainty that every camera or hidden device can be detected.

The target is a credible multi-radio environmental-intelligence platform, not a theatrical surveillance detector.
