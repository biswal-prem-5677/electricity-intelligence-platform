# Phase 5 — Cloud Engineering Plan

## Device registry and MQTT

Register immutable serial/hardware revision, credential state, ownership, firmware, calibration reference and desired/reported configuration. MQTT topics are tenant-isolated and device-specific; broker policy permits a device only to publish telemetry and receive its own authorised commands. Validate schema version, packet sequence, clock quality and size before persistence; retain a dead-letter path for diagnosable rejects.

## Data and analytics pipeline

| Data class | Store / retention starting point | Rationale |
|---|---|---|
| 5-minute energy/power | Time-series, customer-visible retention policy | Charts, bills and baselines |
| Site/device/config | Relational store | Transactional ownership and audit |
| Raw diagnostic/waveform | Encrypted object store, short, consent/event-bound | Avoid unnecessary behavioural-data capture |
| Audit/security logs | Restricted log store, defined minimum retention | Forensics and access accountability |

Normalise units, quality flags and tariff versions before analytics. Never “repair” measured data invisibly; derived/corrected series carry lineage.

## APIs, auth and authorisation

Use OIDC/OAuth issuer, short-lived tokens, MFA for privileged roles and server-side tenant/role checks. APIs are versioned, rate-limited, schema-validated and audited for sensitive operations. Service-to-service access uses workload identities/secrets manager rather than static credentials in code.

## Reliability, monitoring and recovery

Monitor broker connections, valid packets, ingest lag, queue depth, API latency/error, DB saturation, notification delivery, OTA health and anomalous auth. Backup relational/configuration data, test restore, and document RPO/RTO. Scale ingestion horizontally behind queues; scale database only after query/cost evidence. A cloud outage must not erase device local energy counters.

## Release practice

Infrastructure as code, peer review, environment separation, dependency scanning, secrets rotation, migration plan and progressive deploy are required. A runbook exists for packet storm, certificate revocation, data deletion request, deployment rollback, loss of region/provider and compromised device cohort.

