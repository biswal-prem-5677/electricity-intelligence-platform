# 12 — Progress Tracker

**Document ID:** EIP-12 · **Version:** 1.0 · **Status:** Living document — update weekly · **Owner:** Founder / Execution lead

---

## 1. How to use this

This is the **only** status document. Update it every week. A task is `Done` only when its completion criterion in [11_project_plan.md](11_project_plan.md) is met with evidence — not when it feels finished. **Percentages are banned.** A task is Not started, In progress, Blocked, or Done.

## 2. Overall position

| | |
|---|---|
| **Current phase** | Phase 0 — Architecture Freeze and Reality Check |
| **Next gate** | 🚦 Gate 0 |
| **Gate 0 target date** | _set at kickoff_ |
| **Spend to date** | ₹0 of ₹41–94 L |
| **Top risk right now** | **R-13 — will 9 CTs physically fit in a real Indian distribution board?** |
| **Architecture status** | 🔒 Frozen ([03](03_hardware_architecture.md)) |

## 3. Gate status

| Gate | Meaning | Status | Evidence location |
|---|---|---|---|
| 🚦 G0 | Reality check passed; architecture frozen | ⬜ Not started | |
| 🚦 G1 | Components selected from measured data | ⬜ | |
| 🚦 G2 | EVT hardware works | ⬜ | |
| 🚦 G3 | Firmware feature-complete | ⬜ | |
| 🚦 G4 | End-to-end pilot-ready | ⬜ | |
| 🚦 G5 | **Accuracy and safety proven** | ⬜ | |
| 🚦 G6 | **Pilot proves payback** | ⬜ | |
| 🚦 G7 | Commercial release criteria met | ⬜ | |

Legend: ⬜ Not started · 🟡 In progress · 🔴 Blocked · ✅ Passed

## 4. Phase 0 task board

| # | Task | Owner | Status | Evidence | Note |
|---|---|---|---|---|---|
| 0.1 | DB survey, ≥30 boards | PM, HW | ⬜ | | Highest-value task in the project |
| 0.2 | 15–20 segment interviews | PM | ⬜ | | |
| 0.3 | Regulatory classification opinion | PM, EXT | ⬜ | | Longest lead time — **start on day 1** |
| 0.4 | Metrology bench decision | HW, PM | ⬜ | | |
| 0.5 | Off-the-shelf bench proof | HW | ⬜ | | |
| 0.6 | Freeze architecture and scope | PM, HW | ⬜ | | |

### Gate 0 checklist

- [ ] ≥30 DBs surveyed, ≥70% assessed installable
- [ ] ≥5 businesses state a bill problem and a price
- [ ] Classification opinion received; certification path ≤ ₹10 L
- [ ] Bench proof ≤ ±5% on ≥3 load types
- [ ] Committed path to a traceable reference, with dates
- [ ] [03](03_hardware_architecture.md) and [02](02_product_requirements.md) signed

## 5. Later-phase boards

Phases 1–7 task boards are created **when the preceding gate passes**, copied from [11_project_plan.md](11_project_plan.md). Maintaining eight empty boards is documentation work, not execution work.

## 6. Requirement coverage

| Group | Count | Implemented | Verified |
|---|---|---|---|
| Must-have (M1–M16) | 16 | 0 | 0 |
| Hardware (HWR-001…010) | 10 | 0 | 0 |
| Metrology (MET-001…006) | 6 | 0 | 0 |
| Firmware (FWR-001…013) | 13 | 0 | 0 |
| Cloud (CLR-001…010) | 10 | 0 | 0 |
| Mobile (MOB-001…009) | 9 | 0 | 0 |
| Admin (ADM-001…005) | 5 | 0 | 0 |
| Security (SEC-001…008) | 8 | 0 | 0 |
| Rules/analytics (AIR-001…008) | 8 | 0 | 0 |
| **Total** | **85** | **0** | **0** |

**Implemented ≠ Verified.** Verified means a test case in [08](08_testing_validation.md) passed with evidence recorded.

## 7. Open decisions awaiting closure

| ID | Decision | Needed by | Owner |
|---|---|---|---|
| OPEN-1 | Metering AFE part selection | Gate 1 | HW |
| OPEN-2 | CT part and second source | Gate 1 | HW |
| OPEN-3 | MET-006 field CT replacement path | Gate 1 | HW, PM |
| OPEN-4 | Metrology bench: buy vs rent vs lab hire | Gate 0 | HW, PM |
| OPEN-5 | RTOS vs cooperative scheduler | Gate 2 | FW |
| OPEN-6 | Time-series store: PostgreSQL hypertables vs managed | Gate 4 | BE |
| OPEN-7 | Final V1 price point | Gate 6 | PM |
| OPEN-8 | 100 A claim — evidence or permanent deferral | Gate 5 | HW |

Closed decisions move to [14_decision_log.md](14_decision_log.md).

## 8. Weekly update format

```
Week of ____
Phase: __        Gate: __        Status: on track | at risk | slipped
Done this week:
Blocked:
Decisions needed from me:
Spend this week: ₹____    Cumulative: ₹____
Top risk change:
```

## 9. Scope-change log

Any addition to V1 scope is recorded here **with what it removes.** An addition with no removal is a schedule slip that has not been admitted yet.

| Date | Added | Removed | Approver | Schedule impact |
|---|---|---|---|---|
| — | — | — | — | — |
