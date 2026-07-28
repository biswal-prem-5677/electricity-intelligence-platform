# Mobile App SRS

## Purpose and scope

Provides customer and installer mobile flows for onboarding, monitoring, insight, support and privacy actions on supported iOS/Android versions.

## Architecture and interfaces

Mobile app ↔ OIDC identity ↔ versioned REST API; BLE is used only for time-bounded local commissioning. Push notifications are a delivery channel, never the sole record of an alert.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| MOB-001 | Provide account, site and role selection. | User can access only sites granted by backend policy. |
| MOB-002 | Guide installer through safety, Wi-Fi, CT mapping and verification steps. | Cannot activate without required checklist evidence or authorised override. |
| MOB-003 | Show data freshness, source, units and estimated-cost disclaimer. | Offline/stale state is visually distinct from zero consumption. |
| MOB-004 | Support circuit naming, tariff configuration and alert preferences. | Inputs validate and all changes show effective time/version. |
| MOB-005 | Provide accessible charts, export request and support diagnostics consent. | Screen-reader labels and contrast checks pass agreed QA suite. |
| MOB-006 | Store no long-lived device secret in the app. | Security review confirms use of platform secure storage/tokens only. |

## Non-functional requirements

App must degrade gracefully with poor network, cache only minimised/safe data, and preserve privacy on shared devices. Localisation begins with English; Indian-language rollout follows research, not unreviewed machine translation.

## Future extensions

Installer offline packets, household sharing, and enterprise work orders are deferred until role and security models are expanded.

