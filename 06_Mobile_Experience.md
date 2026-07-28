# Phase 6 — Mobile Experience Plan

## Primary flows

1. Purchase/installer handoff: customer receives a clear site-readiness list, not a DIY mains guide.
2. Installer onboarding: authenticate → scan serial → safety acknowledgement → connect locally → Wi-Fi → CT mapping → live-load verification → handover.
3. Customer setup: name site/circuits, choose/enter tariff inputs, set notification preference, see data-freshness explanation.
4. Daily use: live total/circuits, daily/monthly history, cost estimate, alert evidence and action/dismiss/ask-electrician route.
5. Support/privacy: share diagnostics for bounded time, export, access sharing/revocation and delete request.

## UX rules

Show measured units before cost, mark estimated calculations, distinguish data gap from zero, explain that a circuit is not necessarily one appliance, and never use red “fault” language without a verified safety classification. Empty states tell users when the baseline will mature and what data is missing. Charts default to readable 5-minute/daily granularity; live data includes freshness.

## Notifications and offline

Notification categories: device health, unusual sustained load, after-hours deviation and weekly summary. Deduplicate, quiet hours and user opt-out apply except critical account/security notifications. App caches last authorised summaries with expiry; it cannot manufacture live values offline.

## Accessibility and research

Meet platform accessibility defaults: semantic labels, dynamic type, contrast, touch target, non-colour status and localisable strings. Test installer flows with gloves/poor panel lighting and customer flows with lower technical confidence. Validate terminology in launch languages with human review.

