# Future — Long-Horizon Intelligence Direction

**Status:** Parked. Governed by [../15_future_roadmap.md](../15_future_roadmap.md).
**Sits after:** [AI_V2.md](AI_V2.md), which covers the first wave of analytics. This document covers what comes after that.

---

## 1. Why this is a separate document

[AI_V2.md](AI_V2.md) is a costed list of capabilities with a concrete unparking gate (≥100 devices × ≥90 days). This document is deliberately vaguer, because it describes a direction rather than a backlog. **It should not be treated as a plan.** Nothing here has an owner, an estimate or a gate, and nothing here may influence a V1 decision.

## 2. The asset that makes any of it possible

A fleet of devices measuring real Indian circuits, at real sites, with known topology and installer-verified channel mapping. That is a dataset nobody else has, and it cannot be bought, scraped or simulated. Every direction below is a function of that dataset's size and quality — not of model architecture.

**Corollary:** the highest-leverage AI work available today is shipping hardware and keeping its data clean. Calibration versioning, quality flags, effective-dated channel mapping and honest data-gap handling are the AI roadmap, one layer down.

## 3. Directions worth revisiting later

| Direction | The idea | What would have to be true first |
|---|---|---|
| **Cross-site models** | Learn patterns across many sites of the same type (clinic, restaurant, workshop) rather than per-site baselines | Enough sites per segment that a cohort is statistically meaningful, with reliable segment labels |
| **Transfer to unmonitored circuits** | Use monitored circuits at similar sites to estimate what the residual channel probably contains | Measured ground truth from Plug-16 or comparable instrumentation |
| **Physics-informed models** | Constrain learned models with the electrical relationships we already know hold, instead of learning them from scratch | A validated metrology baseline — which is exactly what V1 produces |
| **Fault precursors** | Detect degradation signatures ahead of failure | Long time series **plus verified failure outcomes**. Without confirmed outcomes this is pattern-matching with a confident tone |
| **Tariff and demand optimisation** | Act on ToD structures and demand charges | Tariff data modelled properly, and a customer segment where the saving is worth the complexity |
| **Natural-language interface** | Ask questions of your own energy data | The underlying numbers must already be trusted. A conversational layer over unreliable data multiplies the damage |
| **Portfolio intelligence** | Compare, rank and benchmark across many sites for a multi-site owner | [Enterprise.md](Enterprise.md) capabilities, and multi-site customers |

## 4. Standing constraints, whenever this restarts

1. **Explainability is not optional.** Every output states its evidence window, its rule or model version, and its limitation. This standard is set in [AI_V2.md](AI_V2.md) and does not relax with model sophistication.
2. **A model may never imply a safety conclusion.** The device does not diagnose and does not protect. That boundary is architectural (F6) and does not move because a model became more confident.
3. **No claim without an evaluation dataset.** An impressive demo is not evidence.
4. **Suppress rather than guess.** When calibration, clock quality or measurement quality is invalid, output nothing — the rule from MET-005 applies to models exactly as it applies to rules.
5. **Cost per inference is a product constraint**, the same way telemetry cost is (D-004). A capability that cannot pay for its own compute is not ready.

## 5. What this document must never do

Justify a V1 feature. If an argument here starts to sound like a reason to add something to V1, that is the signal that the governing rule is being tested:

> If it does not directly help us build, test, validate or launch V1, it must not block V1.

Related: [AI_V2.md](AI_V2.md) · [NILM.md](NILM.md) · [Research.md](Research.md) · [Enterprise.md](Enterprise.md)
