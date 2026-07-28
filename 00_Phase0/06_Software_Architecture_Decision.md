# Software Architecture Decision

## Recommended architecture

Use a device-first, event-driven backend: devices publish authenticated telemetry through MQTT; an ingestion service validates/enriches it; a time-series store serves charts; a relational store owns identities, sites and configuration; an asynchronous rules/analytics pipeline produces insights; REST APIs serve mobile/admin clients.

| Layer | Recommended baseline | Reason / trade-off |
|---|---|---|
| Device protocol | MQTT over TLS 1.2+ with per-device credential | Efficient offline-tolerant telemetry; needs broker operations |
| APIs | REST for mobile/admin V1 | Simple caching/versioning; GraphQL only if client composition justifies it |
| Identity | OIDC-compliant customer identity provider | MFA, recovery and audit ability; avoid custom password crypto |
| Time series | Managed time-series/PostgreSQL hypertables | Queryable, portable; evaluate cost after pilot |
| Relational data | PostgreSQL | Strong transactions for tenancy/configuration |
| Object storage | Encrypted regional bucket | Raw diagnostic packets, OTA artifacts and exports |
| Device management | Registry + desired/reported state | Supports inventory, config, update cohorts |
| Observability | Central structured logs, metrics, traces, alerting | Required for support and incident response |

## Data flow

    Device ──TLS MQTT──> Broker ──> Validator/normaliser ──> time-series store ──> API ──> app
                                              │                    │
                                              ├──> dead-letter queue └──> rules/analytics ──> notifications
                                              └──> device registry / audit log

## Key decisions

- Store high-resolution data locally for a bounded retention window; upload aggregated 5-minute records by default. Upload waveform diagnostics only after explicit support consent or an approved event policy.
- Make commands idempotent, versioned and acknowledged. Configuration changes use desired/reported state and audit trail.
- OTA is staged by hardware revision and bootloader version, signed, rollback-capable and halted automatically on health regression.
- A device must remain useful during cloud outage: local measurement, buffer, visible status and monotonic energy counter continue.

