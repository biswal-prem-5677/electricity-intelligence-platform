# Performance, Data and Interface SRS

## Purpose and scope

Defines the shared, measurable operating contract for Plug-16, Switch-16, Home/DIN-8 and Site-3P. A SKU may omit a field only when the field is explicitly marked Not Applicable in its interface-control document.

## Electrical operating envelope

| Requirement | Home/DIN-8 baseline | Plug-16 / Switch-16 baseline | Site-3P baseline |
|---|---|---|---|
| Nominal voltage/frequency | 230 V, 50 Hz | 230 V, 50 Hz | 3-phase LV topology to be frozen |
| Verified operating range | 180–260 V AC, 45–65 Hz | 180–260 V AC, 45–65 Hz | Per selected topology/standard |
| Current range | CT-specific; 63 A nominal / 100 A stated max for mains | 0.05–16 A, 16 A maximum only after thermal validation | CT/VT and rating specific |
| Operating/storage temperature | 0–60 °C / −20–70 °C target | 0–60 °C / −20–70 °C target | Defined by commercial enclosure/environment class |
| Accuracy | Target ≤±1% system energy in approved envelope | Target ≤±1% system energy in approved envelope | Product-specific class target, not inherited |

HWR-007: every published range and accuracy claim shall identify topology, CT/sensor, calibration method, PF/current/temperature envelope and reference standard. “99% accurate” is prohibited because it is not a metrology specification.

## Timing, buffering and response

| ID | Requirement | Acceptance criterion |
|---|---|---|
| HWR-008 | Acquire electrical waveform samples at a rate justified by the metrology architecture; target thousands of samples/second internally. | DVT metrology report proves accuracy and alias control for selected AFE/ADC. |
| FWR-007 | Produce a quality-flagged 1-second live record. | 24-hour HIL test has ≥99.9% valid records excluding declared outage. |
| FWR-008 | Provide customer-visible live update within 2 seconds during healthy connection. | P95 end-to-end latency ≤2 seconds under pilot load. |
| FWR-009 | Retain at least 7 days of one-minute aggregates and event journal through WAN loss. | 7-day outage/recovery test uploads ordered records without double counting. |
| CLR-007 | Store cost-efficient 5-minute cloud aggregates while retaining 1-second live path separately. | Chart/archive policy does not affect live view or monotonic energy. |

## Canonical device data

| Field | Cadence | Applies to | Notes |
|---|---|---|---|
| Timestamp, sequence, clock-quality | every record | all | UTC and quality flag |
| Voltage, current, active/apparent/reactive power, PF, frequency, energy | 1-second live; aggregate archive | all where topology allows | SI units and schema version mandatory |
| Channel ID, direction and calibration version | every measured record | all | Direction supports solar/import later |
| Enclosure/board temperature | 1-minute minimum if fitted | all | “not instrumented” if no qualified sensor |
| Relay command/reported state | on change + heartbeat | relay SKUs only | Home/DIN-8 V1: N/A |
| RSSI/network state, storage depth, reset reason, firmware | heartbeat | all | Diagnostic access is role/consent controlled |

## Interfaces

- Physical: CT/voltage/AC inputs, status LED and authorised reset/provisioning interface are revision-controlled.
- Local: BLE is commissioning-only, time-limited and mutually authorised; no general telemetry service is exposed.
- Cloud: MQTT over TLS with device identity; HTTPS for signed OTA metadata/artifacts.
- App/admin: versioned REST API; no app holds device root credential.

## Reliability target

The five-year MTBF aspiration is a reliability-programme goal, not a proven commercial claim. It becomes publishable only after a defined prediction method, component stress/thermal data, field-return data and approval record exist.

