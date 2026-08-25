# SIGNAL — Project Status

## Stage

INITIALIZATION

## Architecture

Approved baseline. Architecture status: GREEN for the documented target design.

## Repository state

Baseline package prepared for repository initialization. The GitHub repository has been verified empty; the local initialization commit and push remain pending.

## Implementation state

No SIGNAL implementation is considered validated yet.

Do not infer that any of the following currently exist or work:

- Android application;
- SIGNAL Node FAP;
- ESP32 sensor firmware;
- nRF24 integration;
- Sub-GHz streaming;
- Android↔Flipper USB transport;
- BLE transport;
- database/schema;
- classifier/signature database;
- baseline/anomaly engine;
- production UI.

## Immediate next milestone

Complete project initialization by committing/pushing the approved baseline documentation and verifying GitHub reflects it.

After that, prepare the first feature specification: **F001 — Hardware and Upstream Validation**.

## Known unverified facts to resolve in Phase 0

- exact Flipper firmware;
- exact ESP32 chip and board;
- ESP32 pinout and verified radio capabilities;
- exact nRF24 module/carrier;
- available UART/SPI pins;
- phone Android version;
- phone USB OTG behavior;
- current upstream repository availability;
- exact upstream commit SHAs;
- exact license texts/obligations for selected versions;
- unchanged upstream build status;
- actual compatibility of proposed reuse components;
- practical USB telemetry path for an external FAP.
