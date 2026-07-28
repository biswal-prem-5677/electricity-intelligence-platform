# Admin Dashboard SRS

## Purpose and scope

Provides restricted operational tools for support, fleet health, installer administration, OTA release management and audit investigation. It is not a consumer dashboard.

## Architecture and interfaces

Admin web client ↔ OIDC with MFA ↔ admin API ↔ audited services. Roles are support, installer manager, fleet operator, security auditor and release approver; privileged action requires a reason and immutable audit event.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| ADM-001 | Search devices/sites using authorised scope. | Support agent cannot enumerate unrelated tenants. |
| ADM-002 | View fleet health, firmware, last-seen and error state. | Test device change appears within defined telemetry lag. |
| ADM-003 | Run staged OTA with approval, cohort, stop and rollback controls. | Pilot simulation blocks unauthorised release and records all actions. |
| ADM-004 | Manage installer eligibility and training status. | Expired installer cannot activate new site. |
| ADM-005 | Provide privacy-safe support diagnostics request. | Customer consent/revocation is preserved in audit log. |

## Non-functional requirements

MFA, session control, least privilege, sensitive-field masking, tamper-evident audit export and quarterly access review are mandatory. No production command may be hidden in a generic support console.

## Future extensions

Enterprise customer administration is a separate tenant-delegation feature, not a reuse of internal support roles.

