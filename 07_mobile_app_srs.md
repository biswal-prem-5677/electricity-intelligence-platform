# 07 — Mobile App SRS and Experience Plan

**Document ID:** EIP-07 · **Version:** 1.0 · **Status:** Baseline · **Owner:** Mobile
**Supersedes:** Mobile App SRS, Mobile Experience Plan, Admin Dashboard SRS (V1 subset)

---

## 1. Scope

One app, two roles: **customer** and **installer**. The installer role is the same app with elevated, audited permissions — not a separate build. Supported: current iOS and Android with a stated minimum version.

## 2. The five flows

**1 — Purchase and site handoff.** The customer receives a site-readiness list (single-phase supply, panel access, Wi-Fi at the DB). **Never a DIY mains guide.**

**2 — Installer commissioning.** Authenticate → scan device serial → safety acknowledgement → connect locally over BLE → configure Wi-Fi → map CTs to channels → **verify against a known load** → photograph the installation → hand over. Any step may end in a recorded site rejection.

**3 — Customer setup.** Name the site and circuits, enter tariff details, set business/closing hours, choose notification preferences, read the data-freshness explanation.

**4 — Daily use.** Live total and per-circuit power including the **Unmonitored residual** · daily and monthly history · cost estimate · power-quality and outage log · alerts with evidence and an act/dismiss/ask-an-electrician route.

**5 — Support and privacy.** Time-bounded diagnostic sharing, data export, access sharing and revocation, deletion request.

## 3. Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| MOB-001 | Account, site and role selection | A user reaches only the sites granted by backend policy |
| MOB-002 | Guide the installer through safety, Wi-Fi, CT mapping and live verification | Activation is impossible without the checklist evidence or an authorised override |
| MOB-003 | Show data freshness, source, units and an estimated-cost disclaimer | **A stale or offline state is visually distinct from zero consumption** |
| MOB-004 | Circuit naming, category, tariff configuration and alert preferences | Inputs validate; every change shows its effective time and version |
| MOB-005 | Accessible charts, export request, support-diagnostics consent | Screen-reader labels and contrast pass the agreed QA suite |
| MOB-006 | Store no long-lived device secret in the app | Security review confirms platform secure storage and short-lived tokens only |
| MOB-007 | Display the **Unmonitored** residual channel with an explanation of what it is | Sum of displayed circuits plus residual equals the mains reading |
| MOB-008 | Present the power-quality and outage log as a first-class screen | Monthly view shows outage count, total duration and voltage excursions |
| MOB-009 | Open a live session explicitly and close it on navigation-away or timeout | Backend confirms the 1-second stream stops; the app does not hold it open in the background |

**MOB-009 is a cost requirement disguised as a UX requirement.** The live stream is expensive; the app must not leak sessions.

## 4. UX rules

- Show **measured units before cost.** Cost is derived and clearly marked as an estimate.
- Distinguish a **data gap** from **zero consumption**. This is the single most important honesty rule in the app.
- Explain that a circuit is not necessarily one appliance.
- Never use red "fault" language without a verified safety classification. The device does not diagnose.
- Empty states say when the baseline will mature and what data is missing — never a blank chart.
- Charts default to readable 5-minute and daily granularity. Live data always shows freshness.
- The "ask an electrician" route is always one tap from any alert.

## 5. Notifications

Categories: device health · unusual sustained load · after-hours deviation · voltage/outage event · weekly summary. Deduplicated, quiet-hours aware, individually opt-out-able — except critical account and security notifications. **Push is a delivery channel, never the sole record of an alert;** every alert persists in the app.

## 6. Offline behaviour

The app caches the last authorised summaries with an explicit expiry. **It never manufactures a live value offline.** Installer commissioning should complete without connectivity and sync afterwards (Should-have, S4).

## 7. Accessibility and localisation

Platform accessibility defaults: semantic labels, dynamic type, contrast, touch-target size, non-colour status indication, localisable strings. Test the installer flow **with gloves, in poor panel lighting**. Test the customer flow with users of lower technical confidence. English first; Indian-language rollout follows human review — never unreviewed machine translation.

## 8. Admin web surface — V1 minimum

A separate small web client for internal use only. Roles: support, fleet operator, release approver. MFA mandatory. Every privileged action requires a stated reason and emits an immutable audit event.

| ID | Requirement |
|---|---|
| ADM-001 | Device and site lookup within an authorised scope; support cannot enumerate unrelated tenants |
| ADM-002 | Fleet health: last-seen, firmware version, error state, buffer depth |
| ADM-003 | Staged OTA with cohort selection, a manual stop and rollback |
| ADM-004 | Installer eligibility flag; an expired installer cannot activate a new site |
| ADM-005 | Privacy-safe support-diagnostics request with recorded customer consent and revocation |

Everything beyond this list is in [future/Enterprise.md](future/Enterprise.md).

## 9. Out of scope for V1

Multi-site dashboards · household sharing beyond a simple invite (Could-have, C4) · enterprise work orders · in-app purchase or subscription · third-party integrations · appliance-level UI · any control affordance whatsoever. **There is no relay, so there is no switch in the UI.**
