# Cybersecurity SRS

## Purpose and scope

Protects device integrity, customer data, tenant boundaries and operations. Security requirements apply from manufacturing through end of support.

## Threat model and architecture

Primary threats: cloned device, stolen credential, rogue installer, malicious OTA, MQTT replay, insecure BLE commissioning, cloud cross-tenant access, supply-chain dependency vulnerability and physical tamper. Controls span hardware identity, secure boot, signed update, TLS, RBAC/MFA, secrets management, logging and incident response.

## Requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| SEC-001 | Each device has unique non-exportable or protected credential material. | Clone/credential extraction test follows approved threat test plan. |
| SEC-002 | Secure boot and signed OTA enforce authenticity and anti-rollback policy. | Invalid/obsolete image test fails safely. |
| SEC-003 | MQTT/API communication uses modern TLS and mutually authenticated device identity. | Network scan finds no plaintext telemetry/credential path. |
| SEC-004 | Commissioning uses proximity, time bound and user-visible verification. | Unauthorised nearby actor cannot take ownership in test. |
| SEC-005 | Enforce tenant isolation, RBAC, MFA for privileged roles and audit logs. | Automated authorization tests cover every sensitive endpoint. |
| SEC-006 | Maintain SBOM, vulnerability triage and coordinated disclosure process. | Release includes SBOM and no unaccepted critical vulnerability. |

## Reliability and operations

Keys are rotated/revoked with device recovery path. Incident severity, notification, evidence preservation and post-incident review are defined before pilot. Security telemetry must minimise customer behavioural data.

## Future extensions

Load control, third-party APIs and agentic tools need a new threat model and explicit user-authorisation design.

