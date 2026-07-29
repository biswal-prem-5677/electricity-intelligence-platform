# 05 — Firmware SRS and Engineering Plan

**Document ID:** EIP-05 · **Version:** 1.0 · **Status:** Baseline · **Owner:** Embedded
**Supersedes:** Firmware SRS, Firmware Engineering Plan, Cybersecurity SRS (device portions), Performance SRS (firmware portions)

---

## 1. Scope

Embedded operation from boot through measurement, local persistence, communication, diagnostics, update and recovery for **Circuit-8**.

## 2. Architecture

```
Boot ROM → secure bootloader (signature verify) → signed application
                                                        │
  ┌────────────┬──────────────┬────────────┬────────────┼────────────┬──────────────┐
metrology   aggregation    local        provisioning  network/    device mgmt    diagnostics
driver      (1 s / 1 min   journal      (BLE, time-   MQTT+TLS    (desired/       & health
(AFE SPI)    / 5 min)      (packed      bounded)                  reported,       supervisor
                            binary)                               signed OTA)
```

**Execution model.** Use a small RTOS **only if** measured task-separation or latency needs exceed a cooperative design — decided at EVT with timing evidence, not upfront. Metrology acquisition timing is isolated from cloud retries and from UI/diagnostic work. Watchdog servicing is owned by a single health supervisor, never sprinkled across tasks.

## 3. State machines

```
RESET → secure-verify → self-test → uncommissioned ─┐
                                 └→ operational ────┼→ recovery
                                        provisioning ┘
Connectivity: disconnected → connecting → authenticated → online → backoff
OTA:          available → downloaded → verified → staged boot → healthy | rollback
Live stream:  idle → session-requested → streaming(1 s) → timeout → idle
```

Every transition records a reason and a monotonic timestamp. A failed self-test produces a visible fault state and a support route — it never implies electrical protection.

## 4. Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| FWR-001 | Verify the firmware signature before execution and before applying an update; enforce anti-rollback | An invalid or obsolete image cannot boot; the event is journaled |
| FWR-002 | Maintain monotonic energy counters and a timestamp-quality flag | A counter never decreases except under controlled factory erase; unsynchronised data is marked, not discarded |
| FWR-003 | Buffer telemetry through WAN loss and drain it without duplicate accounting | A 7-day configured-outage test preserves 1-minute aggregates and the event journal, then uploads ordered records with **zero** double counting |
| FWR-004 | Use watchdog, brownout handling and crash diagnostics | Fault injection recovers to the measurement state or to a safe update-recovery state |
| FWR-005 | Apply configuration atomically and report desired/reported versions | An interrupted configuration write cannot leave a mixed metrology channel mapping |
| FWR-006 | Provide authenticated commissioning through a short-lived, proximity-bound, user-visible flow | An unauthorised nearby actor cannot take ownership in test; attempts are logged |
| FWR-007 | Produce a quality-flagged 1-second record covering all nine channels in **one** bundled frame | A 24-hour HIL run yields ≥99.9% valid records, excluding declared outage |
| FWR-008 | Deliver a customer-visible live update within 2 s (P95) on a healthy connection, **only while a live session is active** | Time-synchronised device-to-app latency report; stream stops on session timeout |
| FWR-009 | Retain ≥7 days of 1-minute aggregates plus the event journal in local non-volatile storage | 7-day outage and recovery test; storage-budget calculation attached |
| FWR-010 | Store the journal in a **packed binary** format, not JSON | Storage budget below is met and verified on hardware |
| FWR-011 | Detect, timestamp and journal supply outages and voltage excursions, and reconstruct outage duration after power returns | Deliberate outage of known duration is reported within ±2 s |
| FWR-012 | Compute and publish the residual channel input (mains and branch sums) so the cloud can derive "Unmonitored" without ambiguity | Sum consistency verified against a known load configuration |
| FWR-013 | Set and discipline the real-time clock from a trusted network source; degrade the clock-quality flag when unsynchronised | Clock-loss test produces flagged records that the cloud accepts but marks |

**FWR-013 closes a gap:** the data model carried a `clockQuality` field with nothing specified to set the clock.

## 5. Storage budget — the calculation that makes FWR-009 pass or fail

| Item | Value |
|---|---|
| Retention | 7 days |
| Records | 7 × 1,440 min × 9 channels = **90,720** |
| Packed binary, ~32 B/record | **≈ 2.9 MB** ✅ fits alongside dual OTA slots on a 16 MB part |
| JSON, ~250 B/record | ≈ 22.7 MB ❌ **FWR-009 fails** |
| Event journal + power-quality log | Reserve 1 MB |
| Dual OTA slots | Reserve 2 × ~1.5–2 MB |
| **Minimum flash** | **16 MB** — a smaller part is not acceptable |

This is why FWR-010 is a normative requirement rather than an implementation detail.

## 6. Non-functional requirements

- **Priority:** measurement acquisition outranks telemetry, always. Cloud backpressure never stalls the AFE read.
- Memory limits, worst-case buffer capacity, boot time, update bandwidth and task timing are **controlled design inputs** with recorded values, not emergent properties.
- Wi-Fi reconnect uses exponential backoff. Local storage applies a quota and a documented downsampling policy before exhaustion — it never simply stops recording.
- Drivers have timeouts, bounded retries, versioned configuration and simulation seams for host-side testing.
- Every release produces an SBOM, reproducible build metadata and a signature record.

## 7. Security requirements (device side)

| ID | Requirement | Acceptance criterion |
|---|---|---|
| SEC-001 | Unique, non-exportable or hardware-protected device credential material | Clone and credential-extraction test per the approved threat plan |
| SEC-002 | Secure boot and signed OTA enforce authenticity and anti-rollback | Invalid and obsolete image tests fail safely and are logged |
| SEC-003 | MQTT uses modern TLS with a mutually authenticated device identity | A network scan finds no plaintext telemetry or credential path |
| SEC-004 | Commissioning is proximity-bound, time-bounded and user-visible | An unauthorised nearby actor cannot claim the device |
| SEC-006 | SBOM produced per release; vulnerability triage and coordinated disclosure process exist | Release includes an SBOM and no unaccepted critical vulnerability |

TLS trust anchors, device credentials and secrets are **never** logged. Keys are rotatable and revocable with a defined device-recovery path.

## 8. OTA

Reproducible signed builds, an SBOM, an immutable release manifest and a hardware-compatibility matrix. OTA validates signature, hash and size; writes to an inactive slot; boots with a trial flag; reports health; then commits — or rolls back automatically. Releases are staged by hardware revision and bootloader version, with an automatic halt on health regression.

## 9. Diagnostics

Export bounded, non-sensitive diagnostics: firmware and build ID, reset reason, radio state, metrology health, buffer depth, error counters. Unit-test calculation and parser boundaries on the host. Hardware-in-loop test: SPI faults, CT disconnection, clock loss, power interruption, MQTT replay, corrupt configuration, interrupted update. Production firmware is tagged to source, compiler, SBOM, signing key ID and test report.

## 10. Explicitly out of scope for V1 firmware

Any load-control code path · three-phase handling · local web server or local API · appliance inference · on-device ML · Bluetooth telemetry (BLE is commissioning-only, with **no** general telemetry service exposed) · over-the-air calibration changes without an authorised workflow.

Remote control would require a separate safety case and is excluded — see [future/Future_Hardware.md](future/Future_Hardware.md).
