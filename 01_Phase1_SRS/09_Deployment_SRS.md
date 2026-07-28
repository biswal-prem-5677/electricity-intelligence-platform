# Deployment SRS

## Purpose and scope

Defines production release, installer deployment, device activation, monitoring, rollback and continuity procedures.

## Architecture and interfaces

CI/CD creates signed immutable artifacts; infrastructure is declared and reviewed; OTA service distributes approved firmware in cohorts; installer app activates authorised inventory; operations monitors fleet and cloud SLOs.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| OPS-DEP-001 | Use separate dev, staging and production with change approval. | Production change is traceable to reviewed artifact and ticket. |
| OPS-DEP-002 | Release backend/app by progressive rollout with rollback. | Deliberate failure reverts without data-schema loss. |
| OPS-DEP-003 | Release firmware by hardware-compatible cohort with health stop rule. | Error/boot regression triggers automatic cohort halt. |
| OPS-DEP-004 | Activate device only after inventory and installer checks pass. | Reused/unknown serial cannot be claimed. |
| OPS-DEP-005 | Maintain backup, restore and incident runbooks. | Restore exercise meets stated RPO/RTO target. |

## Non-functional requirements

Production access is time-bound and audited. Deployments avoid destructive schema changes without tested migration/backfill and rollback plan. Pilot change windows and customer communication are proportionate to impact.

## Future extensions

Regional expansion requires a data-residency, support-hours, tax and connectivity review before enabling a new production tenancy.

