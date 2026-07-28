# Cloud Backend SRS

## Purpose and scope

Provides a tenant-safe platform for device identity, telemetry, configuration, alerts, history, support and data lifecycle.

## Architecture and interfaces

MQTT broker → schema validator → immutable ingest log → time-series and relational stores → rules/jobs → API/notifications. Interfaces are MQTT/TLS, REST/JSON, OIDC tokens, object storage and audited support tools.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| CLR-001 | Register each device with unique identity, hardware revision and ownership state. | Duplicate or orphan assignment is rejected/audited. |
| CLR-002 | Validate telemetry schema, sequence and tenant binding before persistence. | Malformed/cross-tenant packet reaches quarantine, not customer charts. |
| CLR-003 | Retain raw/aggregate data according to published policy and support export/delete workflow. | Policy job is tested against a sample tenant. |
| CLR-004 | Serve device desired/reported configuration and OTA cohorts. | Idempotent request with full audit and rollback record. |
| CLR-005 | Deliver alerts with deduplication, acknowledgement and quiet-hours policy. | Replayed event creates no duplicate notification. |
| CLR-006 | Expose health, ingest lag, error and security metrics. | On-call dashboard detects simulated broker/API failure. |

## Non-functional requirements

Availability and recovery targets are set by pilot SLO: 99.5% monthly API availability, RPO ≤24 hours for configuration/identity and RTO ≤8 hours, subject to provider validation. All data is encrypted in transit and at rest; least privilege and backup restore tests are mandatory.

## Future extensions

Add multi-region disaster recovery only when customer/contract requires it. Do not introduce multi-cloud complexity during MVP.

