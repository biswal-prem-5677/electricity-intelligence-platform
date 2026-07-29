# future/ — Parked, Not Cancelled

**Status:** This folder is outside the V1 critical path by design.
**Governed by:** [../00_execution_principles.md](../00_execution_principles.md) R8, R10, R18.

---

## Why this is a folder and not a roadmap document

It used to be `15_future_roadmap.md`. A "Future Roadmap" sitting in the main document list still reads as *something to keep reading*. A folder reads as *somewhere else*. That distinction is the entire point, and it is a real one: the parked list is larger and more exciting than the V1 list, and it will pull attention away from the work that ships unless it is physically separated.

**Nothing here is cancelled. Everything here is scheduled.** Each document keeps its rationale, its prerequisites and its unparking gate, so that when the moment arrives nobody has to reconstruct the thinking.

## The governing rule

> **If a document, feature or task does not directly help us build, test, validate or launch Version 1, it must not block Version 1.**

## The unparking rule

A capability leaves this folder only when **all five** are true:

1. **Gate 6 has passed** — the V1 pilot has proven the product pays back. Nothing unparks before that.
2. **Its own prerequisites are met** — the specific technical or data conditions stated in its document.
3. **A named owner exists** with capacity, not enthusiasm.
4. **It has an acceptance gate** — a measurable definition of working, written before work starts.
5. **A logged decision in [../14_decision_log.md](../14_decision_log.md) names what it removes or delays** (R9).

## Staged views — read these first

| | Horizon | Contains |
|---|---|---|
| [V2.md](V2.md) | Next, after Gate 6 | What is nearly ready and has a concrete trigger |
| [V3.md](V3.md) | Later | Needs a fleet, a dataset, or a different customer |
| [Moonshots.md](Moonshots.md) | Speculative | Needs a different company, or an unproven premise |

## Capability documents

| Document | Contains | Unparking trigger |
|---|---|---|
| [AI_V2.md](AI_V2.md) | Forecasting, budget tracking, billing verification, recommendations, ToD optimisation, anomaly detection, health trend, carbon, energy score, cohort benchmarking, appliance life, generative assistant, agentic capabilities | ≥100 devices × ≥90 days of quality data |
| [NILM.md](NILM.md) | Non-intrusive load monitoring / appliance disaggregation | Measured ground truth from a real fleet. Public datasets do not transfer to Indian loads |
| [Three_Phase.md](Three_Phase.md) | Site-3P platform, the 120° phase-error problem, reuse boundaries | ≥10 written customer requests **or** ≥3 sites/month lost to phase rejection (D-008) |
| [Future_Hardware.md](Future_Hardware.md) | Circuit-16, Plug-16, Switch-16, Site-3P, neutral/earth-imbalance channel, solar/battery/EV, and rejected hardware ideas with reasons | Per-product; the neutral/earth channel is the leading **V1.1** candidate |
| [Future_AI.md](Future_AI.md) | Long-horizon intelligence direction beyond AI_V2 | After AI_V2 capabilities are in production |
| [Enterprise.md](Enterprise.md) | Multi-site, SSO/SAML/SCIM, tenant delegation, open API, landlord/tenant, work orders, managed service, utility partnership | A paying multi-site customer asks, in writing |
| [Commercial.md](Commercial.md) | Volume manufacturing controls, certification programme, unit-economics discipline, cost-reduction roadmap, hiring order, fundraising against evidence | Gate 6 passed |
| [Research.md](Research.md) | Opportunity backlog, standing research questions, ideas permanently set aside | Ongoing at low intensity, never on the V1 critical path |

## Product sequence

| Version | Product | Adds | Precondition |
|---|---|---|---|
| **V1** | **Circuit-8** | 1 service + 8 branch CTs, single-phase, no relay, rules-only analytics | — |
| V1.1 | Circuit-8 rev B / Circuit-16 | Neutral or earth-imbalance channel (**insight only, never protection**); more channels if the survey demands it | Gate 6; DB survey evidence |
| V2 | Site-3P · Plug-16 · AI_V2 first wave | See [V2.md](V2.md) | Their own triggers |
| V3 | NILM · Switch-16 · enterprise platform | See [V3.md](V3.md) | Fleet and dataset |
| — | See [Moonshots.md](Moonshots.md) | | Probably never, and that is fine |

## Permanently set aside

Not parked — **decided against**, with reasons preserved in [Research.md](Research.md) and [Future_Hardware.md](Future_Hardware.md):

- **Appliance lifetime prediction from current signature alone.** The physics does not support the claim.
- **Revenue-grade / billing-authority metering.** A different product, regulatory regime and company.
- **Protection functionality of any kind.** A monitoring device that implies protection is a liability with a chart on it (R21).
- **DIY mains installation.** A safety decision, not a product one.
- **Hall-effect, shunt and direct-divider sensing**, and any non-pre-certified radio. See [../09_component_selection.md](../09_component_selection.md) §5.

## The honest summary

The parked list being larger than the V1 list is the correct shape. The V1 list is the part that 3–5 people can build in 12–18 months, sell, install safely, and verify pays for itself. Everything else is earned by shipping that.

**The fleet is the moat.** Every capability in this folder becomes possible because devices are measuring real Indian circuits — and impossible without them.
