# 06 — Cloud, Backend, Security and Deployment SRS

**Document ID:** EIP-06 · **Version:** 1.0 · **Status:** Baseline · **Owner:** Backend
**Supersedes:** Cloud Backend SRS, Cloud Engineering Plan, Cloud Interface Contract, Software Architecture Decision, Cybersecurity SRS (cloud portions), Deployment SRS

---

## 1. Architecture

```
Device ──MQTT/TLS──► Broker ──► schema validator ──► immutable ingest log
                                       │                    │
                          dead-letter queue          ┌───────┴────────┐
                                                time-series      relational
                                                  store            store
                                                     │                │
                                              rules engine ──► insight store
                                                     │                │
                                                     └──► REST API ◄──┘
                                                              │
                                                    mobile app · admin
```

| Layer | V1 baseline | Why |
|---|---|---|
| Device protocol | MQTT over TLS 1.2+, per-device credential | Efficient, offline-tolerant |
| APIs | REST/JSON, versioned | Simple caching and versioning; GraphQL only if client composition ever justifies it |
| Identity | OIDC-compliant provider | MFA, recovery, audit. **Never hand-roll password crypto** |
| Time series | PostgreSQL with hypertables (or a managed TS store) | Queryable and portable; re-evaluate cost after pilot |
| Relational | PostgreSQL | Transactions for tenancy and configuration |
| Object storage | Encrypted regional bucket | OTA artifacts, exports, exceptional diagnostics |
| Device management | Registry with desired/reported state | Inventory, configuration, OTA cohorts |
| Observability | Structured logs, metrics, traces, alerting | Required for support from day one |

**Keep it boring.** No multi-cloud, no multi-region, no microservice sprawl during V1. One region (India when available), one deployable backend, one broker.

## 2. Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| CLR-001 | Register each device with a unique identity, hardware revision and ownership state | Duplicate or orphan assignment is rejected and audited |
| CLR-002 | Validate telemetry schema, sequence and tenant binding before persistence | A malformed or cross-tenant packet reaches quarantine, never a customer chart |
| CLR-003 | Retain raw and aggregate data per published policy; support export and deletion workflows | Policy job tested against a sample tenant |
| CLR-004 | Serve device desired/reported configuration and OTA cohorts | Idempotent, versioned, acknowledged, with full audit and rollback record |
| CLR-005 | Deliver alerts with deduplication, acknowledgement and quiet hours | A replayed event creates no duplicate notification |
| CLR-006 | Expose health, ingest lag, error and security metrics | The on-call dashboard detects a simulated broker or API failure |
| CLR-007 | **Store 5-minute aggregates as the default cloud tier. Accept 1-second frames only while a live session is open, with a server-side session timeout** | A device with no active session publishes no 1-second data; verified by broker metrics |
| CLR-008 | Compute and serve the residual "Unmonitored" channel from mains minus branch sums | Residual is never negative in the API; a persistently negative raw residual raises a commissioning flag |
| CLR-009 | Ingest, store and serve the power-quality and outage log | Outage events survive device reboot and appear in the monthly summary |
| CLR-010 | Never invisibly repair measured data; any derived or corrected series carries lineage | Audit of a corrected series shows its source and transformation |

## 3. MQTT contract

Topics are logical examples; production identifiers are opaque and access-controlled. **A device is authorised only for its own paths. Wildcards are never granted to devices.**

| Direction | Topic | QoS / retention | Purpose |
|---|---|---|---|
| Device → cloud | `v1/devices/{id}/telemetry` | QoS 1, not retained | Bundled 5-minute aggregate frames |
| Device → cloud | `v1/devices/{id}/live` | QoS 0, not retained | Bundled 1-second frames — **session-gated** |
| Device → cloud | `v1/devices/{id}/events` | QoS 1, not retained | Outage, voltage excursion, fault, boot |
| Device → cloud | `v1/devices/{id}/health` | QoS 1, not retained | Heartbeat and diagnostics |
| Cloud → device | `v1/devices/{id}/desired` | QoS 1, retained | Versioned configuration, including live-session enable |
| Device → cloud | `v1/devices/{id}/reported` | QoS 1, retained | Applied configuration and versions |
| Cloud → device | `v1/devices/{id}/ota` | QoS 1, not retained | Signed OTA offer metadata |

Every payload carries `schemaVersion`, `deviceId`, `sequence`, `timestampUtc`, `clockQuality`, `firmwareVersion` and `calibrationVersion`.

### Bundled frame — all nine channels in one message

```json
{
  "schemaVersion": "1.0",
  "deviceId": "…",
  "sequence": 918273,
  "timestampUtc": "2026-07-29T12:00:00Z",
  "clockQuality": "synced",
  "intervalSeconds": 300,
  "firmwareVersion": "1.0.4",
  "calibrationVersion": "cal-2026-07-01",
  "voltageV": 231.8,
  "frequencyHz": 50.02,
  "channels": [
    { "id": "service", "currentA": 18.42, "activePowerW": 4118.6, "apparentPowerVA": 4270.1,
      "reactivePowerVAR": 1129.4, "powerFactor": 0.964, "activeEnergyWh": 812591.4,
      "direction": "import", "quality": ["calibrated"] },
    { "id": "branch-01", "currentA": 3.84, "activePowerW": 812.4, "apparentPowerVA": 890.1,
      "reactivePowerVAR": 364.8, "powerFactor": 0.913, "activeEnergyWh": 12591.4,
      "direction": "import", "quality": ["calibrated"] }
  ]
}
```

> **This bundling is not a style preference.** One frame per interval instead of nine is the difference between ~8,640 and ~77,760 messages per device per month. See [03_hardware_architecture.md](03_hardware_architecture.md) §7.

## 4. Data model

| Resource | Required fields | Key rule |
|---|---|---|
| Tenant | ID, region, retention policy | Boundary enforced server-side, without exception |
| User role | user ID, tenant/site role, MFA state | Least privilege, audited |
| Site | timezone, tariff version, topology, business hours | No utility billing authority implied |
| Device | serial, hardware revision, credential state, owner | Ownership transfer is audited |
| Channel | mapping, label, category, sensor type, orientation, calibration version | Mapping changes are **effective-dated**; history is never rewritten |
| Measurement aggregate | interval, values, quality, source version | Raw and derived data never overwrite each other invisibly |
| Event | type, start, end, severity, evidence | Outages and excursions are first-class, not log lines |
| Insight | evidence window, rule version, action, limitation | Explainable and dismissible |
| OTA release | compatibility, signature, cohort, rollback state | Dual approval for production promotion |

**Initial REST endpoints:** `POST /v1/sites` · `GET /v1/sites/{id}/measurements` · `GET /v1/sites/{id}/events` · `PATCH /v1/devices/{id}/configuration` · `POST /v1/devices/{id}/live-session` · `GET /v1/insights` · `POST /v1/exports` · `POST /v1/support-consents`. Schemas, error codes, pagination and rate limits are defined in an OpenAPI artifact **before** implementation.

## 5. Non-functional requirements

| Area | V1 target |
|---|---|
| API availability | 99.5% monthly, subject to provider validation |
| RPO / RTO | ≤24 h / ≤8 h for configuration and identity data |
| Encryption | In transit and at rest, everywhere |
| Retention | 5-minute aggregates per published policy; 1-second data not retained by default (it is occupancy-revealing); raw waveform exceptional and consent-bound |
| Data residency | India by default. **"Stored in India" is a risk control, not a completed compliance position** — DPDP Rules 2025 obligations are assessed against the actual pilot date and data flows |

## 6. Security requirements (cloud side)

| ID | Requirement | Acceptance criterion |
|---|---|---|
| SEC-005 | Enforce tenant isolation, RBAC, MFA for privileged roles, and audit logs | Automated authorisation tests cover **every** sensitive endpoint |
| SEC-007 | Service-to-service auth uses workload identity or a secrets manager, never static credentials in code | Repository and image scan finds no embedded secret |
| SEC-008 | Privileged support action requires a stated reason and emits an immutable audit event | Audit export reviewed in a tabletop exercise |

**Threat model:** cloned device · stolen credential · rogue installer · malicious OTA · MQTT replay · insecure BLE commissioning · cross-tenant access · dependency vulnerability · physical tamper. Incident severity, notification, evidence preservation and post-incident review are defined **before** the first pilot telemetry arrives.

## 7. Deployment

| ID | Requirement | Acceptance criterion |
|---|---|---|
| OPS-DEP-001 | Separate dev, staging and production with change approval | Every production change traces to a reviewed artifact and ticket |
| OPS-DEP-002 | Progressive backend/app rollout with rollback | A deliberate failure reverts without data-schema loss |
| OPS-DEP-003 | Firmware released by hardware-compatible cohort with a health stop rule | An error or boot regression halts the cohort automatically |
| OPS-DEP-004 | Activate a device only after inventory and installer checks pass | A reused or unknown serial cannot be claimed |
| OPS-DEP-005 | Maintain backup, restore and incident runbooks | A restore exercise meets the stated RPO/RTO |

Infrastructure as code, peer review, environment separation, dependency scanning, secrets rotation, tested migrations. Production access is time-bound and audited. **Runbooks required before pilot:** packet storm · certificate revocation · data-deletion request · deployment rollback · loss of region · compromised device cohort.

## 8. The rule that outranks the rest

**A cloud outage must never erase or corrupt a device's local energy counters, and must never cause silent data loss.** The device keeps measuring, keeps buffering, and reconciles on reconnect. If the cloud and the device disagree about energy, the device's monotonic counter wins.

## 9. Admin surface — deliberately minimal for V1

Only: authorised device/site lookup, fleet last-seen and firmware view, staged OTA with a manual stop, installer eligibility flag, immutable audit log. Everything else (role granularity, quarterly access-review automation, tenant delegation, enterprise audit export) is in [future/Enterprise.md](future/Enterprise.md). You are operating ≤100 devices, not 10,000.
