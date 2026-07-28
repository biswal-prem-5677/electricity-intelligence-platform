# Product Requirements SRS

## Purpose and scope

Defines the V1 customer outcome: safe circuit-level monitoring for authorised single-phase sites. It applies to Home-8 hardware, firmware, cloud, mobile app, installation and support.

## Architecture and interfaces

Customer ↔ mobile app ↔ authenticated API ↔ telemetry/rules platform ↔ MQTT device ↔ labelled CTs. Installer uses the same app with elevated, audited role. The utility meter and electrical protection devices are external systems and remain authoritative.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| PRD-001 | Show live mains and labelled branch power with freshness state. | P95 healthy-connection end-to-end live update latency is <=2 s; stale state appears after 15 min without telemetry. |
| PRD-002 | Store 5-minute history and daily/monthly cost estimate. | Recomputed view matches source records; estimator identifies tariff version. |
| PRD-003 | Support circuit labels, category and CT orientation setup. | Change is audited and reflected in future records without rewriting raw data. |
| PRD-004 | Alert only on documented rule/model evidence. | Alert shows circuit, interval, baseline, action and limitation. |
| PRD-005 | Support qualified installer commissioning and site rejection. | Mandatory checklist is completed before activation; rejection records reason. |
| PRD-006 | Permit customer export and deletion request. | Export is tenant-scoped; deletion workflow is auditable and policy-bound. |

## Non-functional requirements

Usability: first successful installer commissioning target ≤45 minutes excluding panel preparation. Reliability: V1 operates in monitoring-only safe state through cloud loss. Performance: 95% app history queries under 3 seconds at pilot scale. Security: roles, audit records and per-device identity are mandatory. Scalability: no customer may read another tenant’s records.

## Future extensions

Home-16, three-phase, solar, enterprise multi-site, and integrations are separate product requirements; none may weaken the V1 safety boundary.
