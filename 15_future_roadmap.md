# 15 — Future Roadmap (the governor for `future/`)

**Document ID:** EIP-15 · **Version:** 1.0 · **Status:** Baseline · **Owner:** Product
**Supersedes:** AI Roadmap, Future Opportunity Backlog, Product Family & Portfolio Strategy (post-V1 portions)

---

## 1. What this document is

An index and a **gatekeeper**. Everything worth building that is not V1 lives in [future/](future/). Nothing in there is cancelled; it is scheduled. This document defines the only way something leaves that folder.

## 2. The governing rule

> **If a document, feature or task does not directly help us build, test, validate or launch Version 1, it must not block Version 1.**

Applied in reverse, it also protects the ideas: parking something is not rejecting it. A parked capability keeps its rationale, its prerequisites and its unparking gate, so that when the moment arrives nobody has to reconstruct the thinking.

## 3. The unparking rule

A capability leaves `future/` only when **all five** are true:

1. **The V1 pilot gate (Gate 6) has passed.** Nothing unparks before the product is proven to pay back.
2. **Its own prerequisites are met** — the specific technical or data conditions stated in its `future/` document.
3. **A named owner exists** with capacity, not enthusiasm.
4. **It has an acceptance gate** — a measurable definition of working, written before the work starts.
5. **A logged decision in [14](14_decision_log.md) names what it removes or delays.** Additions without subtractions are unadmitted schedule slips.

## 4. The folder

| Document | Contains | Unparking trigger |
|---|---|---|
| [future/AI_V2.md](future/AI_V2.md) | Forecasting, budget tracking, billing verification, recommendations, ToD optimisation, anomaly detection, health trend, carbon, energy score, cohort benchmarking, appliance life, generative assistant, agentic capabilities | ≥100 devices × ≥90 days of quality data, plus the five conditions above |
| [future/NILM.md](future/NILM.md) | Non-intrusive load monitoring / appliance disaggregation | Measured ground truth from a real fleet. Public datasets do not transfer to Indian loads |
| [future/Three_Phase.md](future/Three_Phase.md) | Site-3P platform, the 120° phase-error problem, reuse boundaries | ≥10 written customer requests **or** ≥3 sites/month lost to phase rejection (D-008) |
| [future/Future_Hardware.md](future/Future_Hardware.md) | Circuit-16, Plug-16, Switch-16, Site-3P, neutral/earth-imbalance channel, solar/battery/EV, and the table of rejected hardware ideas with reasons | Per-product; the neutral/earth-imbalance channel is the leading **V1.1** candidate |
| [future/Future_AI.md](future/Future_AI.md) | Longer-horizon intelligence direction beyond AI_V2 | After AI_V2 capabilities are in production |
| [future/Enterprise.md](future/Enterprise.md) | Multi-site, SSO/SAML/SCIM, tenant delegation, open API, landlord/tenant, work orders, managed service, utility partnership | A paying multi-site customer asks, in writing |
| [future/Commercial.md](future/Commercial.md) | Volume manufacturing controls, the certification programme, unit economics discipline, cost-reduction roadmap, hiring order, fundraising against evidence | Gate 6 passed |
| [future/Research.md](future/Research.md) | Opportunity backlog, standing research questions, ideas permanently set aside | Ongoing, at low intensity, never on the V1 critical path |

## 5. Product sequence

| Version | Product | Adds | Precondition |
|---|---|---|---|
| **V1** | **Circuit-8** | 1 service + 8 branch CTs, single-phase, no relay, rules-only analytics | — |
| V1.1 | Circuit-8 rev B / Circuit-16 | Neutral or earth-imbalance monitoring channel (**insight only, never protection**); more channels if the survey demands it | Gate 6 passed; DB survey evidence |
| V2 | Site-3P | Three-phase platform with per-phase voltage references | D-008 trigger met |
| V2 | Plug-16 | Plug-level companion for appliance-specific ground truth | A commercial reason beyond curiosity |
| V2+ | AI_V2 capabilities | Forecasting, benchmarking, anomaly detection | ≥100 devices × ≥90 days |
| V3+ | NILM, Switch-16, enterprise platform | | Their own gates |

## 6. What is permanently set aside

Not parked — **decided against**, with reasons preserved in [future/Research.md](future/Research.md) and [future/Future_Hardware.md](future/Future_Hardware.md):

- **Appliance lifetime prediction from current signature alone.** The physics does not support the claim.
- **Revenue-grade / billing-authority metering.** A different product, a different regulatory regime, a different company.
- **Protection functionality of any kind.** A monitoring device that implies protection is a liability with a chart on it.
- **DIY mains installation.** Not a product decision — a safety one.
- **Hall-effect, shunt and direct-divider sensing**, and any non-pre-certified radio. See [09](09_component_selection.md) §5.

## 7. The honest summary

The parked list is larger and more exciting than the V1 list. That is the correct shape. The V1 list is the part that can be built by 3–5 people in 12–18 months, sold, installed safely, and verified to pay for itself. Everything else is earned by shipping that.

**The fleet is the moat.** Every capability in `future/` becomes possible because devices are measuring real Indian circuits — and impossible without them.
