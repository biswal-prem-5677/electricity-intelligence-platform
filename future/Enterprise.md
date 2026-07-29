# Future — Enterprise, Multi-Site and Platform

**Status:** Parked. V2+ at the earliest. No V1 work may be shaped around these requirements.

## Why parked

Enterprise features are a tax on a product that has not yet proven single-site value. Every one of them multiplies the permission model, the support obligation and the sales cycle. The V1 admin surface is deliberately support-grade only: enough to operate a fleet of ≤100 devices, and nothing more.

## Parked capabilities

| Capability | Description | Unpark when |
|---|---|---|
| Multi-site dashboard | Compare, roll up and export across a customer's sites | A single customer owns ≥5 sites and asks for it |
| Enterprise SSO / SAML / SCIM | Corporate identity integration | A procurement process demands it in writing |
| Tenant delegation | A customer administers their own users and roles, rather than support doing it | ≥20 sites under one customer |
| Role expansion | Facility manager, auditor, finance viewer | Driven by an actual signed contract, not anticipation |
| Open developer API | Third-party integrators build automation on tenant-scoped data | Data contract stable for ≥2 releases; support model funded |
| Landlord / tenant transparency | Time-stamped circuit evidence for shared-supply disputes | Tenancy/permission model reviewed legally |
| Work-order integration | Push a fault candidate into a maintenance system | Fault detection itself is validated ([AI_V2.md](AI_V2.md)) |
| Managed energy service | We operate the monitoring and report outcomes | Pilot unit economics prove support cost per site |
| Utility / DISCOM partnership | Data or deployment collaboration | Separate compliance, procurement and liability programme |
| Hospital / industrial deployment | High-reliability sites with SLA | Requires certified 3-phase product and 24×7 operations |

## Admin/fleet capability held back from V1

The original Admin Dashboard SRS specified five roles, quarterly access reviews, tamper-evident audit export, and staged-OTA approval workflows. These are correct requirements for a fleet of 10,000 devices and premature for a fleet of 50.

**V1 keeps only:** authorised device/site lookup, fleet last-seen and firmware view, staged OTA with a manual stop, installer eligibility flag, and an immutable audit log of privileged actions. Everything else in the original ADM-001…005 set moves here.

**Held for V2:** role granularity beyond support/operator/approver, quarterly access review automation, self-service consent dashboards, tenant-delegated administration, and enterprise audit export formats.

## Guardrails that survive to any enterprise release

- Tenant boundary is enforced server-side, always, without exception.
- Privileged action requires a stated reason and produces an immutable audit event.
- Internal support roles are never reused as customer-facing enterprise roles.
- OEM/white-label projects get separate serial ranges, separate configuration signing keys, and never silently share customer telemetry.
