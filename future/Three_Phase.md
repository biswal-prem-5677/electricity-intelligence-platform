# Future — Three-Phase Product (Site-3P)

**Status:** V2 feasibility programme. Highest-priority item in this folder.

## Why this is the most likely item to be unparked first

Version 1 deliberately targets **single-phase small commercial** sites. That segment has the payback economics that make the product sell (see [../10_bom_and_cost.md](../10_bom_and_cost.md)). But the same segment scales upward into three-phase service the moment a customer grows, and the highest-value sites — restaurants with large refrigeration, clinics, small manufacturing, multi-tenant buildings — are frequently three-phase already.

**Expect Site-3P to be pulled forward by paying customers, not by ambition.** That is the correct forcing function. When a V1 pilot customer says "I want this in my other shop and it's three-phase," that is a validated demand signal and this document becomes a project.

## The measurement problem it solves

V1 uses **one shared voltage reference**. Active power on a branch is computed from that branch's current waveform against that shared voltage waveform. This is correct only when the branch is fed from the same phase as the voltage reference.

If a branch is fed from a different phase, the voltage and current waveforms are displaced by 120°. The computed active power is not slightly wrong — it is grossly wrong, and can be negative. **There is no firmware workaround.** This is why V1 site-rejection criteria exclude sites with ambiguous or mixed phase topology, and why three-phase is a new hardware platform rather than a firmware option.

## Architecture requirements

| Requirement | Note |
|---|---|
| Three voltage reference channels | One per phase, each isolated |
| Phase-to-branch mapping | Every branch CT must be assigned to a known phase at commissioning, and the mapping must be verifiable, not merely declared |
| Simultaneous or synchronised sampling | Per-phase sampling must be phase-coherent; prove at DVT |
| Neutral current | Becomes far more useful in 3-phase (imbalance detection) |
| Topology support | 3-phase 4-wire LV is the baseline; delta and CT/VT-fed installations are separate decisions |
| Enclosure and DIN form factor | Commercial panels differ from residential DBs; separate mechanical programme |
| Certification | Does **not** inherit any V1 safety, EMC or accuracy evidence. New classification, new tests |

## What can be reused

Cloud data model, MQTT contract, device registry, OTA infrastructure, mobile app shell, rules engine, tariff engine, installer workflow, support tooling. The shared measurement contract in [../02_product_requirements.md](../02_product_requirements.md) was written to accommodate this — channel records already carry channel ID, direction and calibration version.

## What cannot be reused

Schematic, PCB, isolation design, enclosure, calibration fixture, safety evidence, EMC evidence, accuracy claim, installer training, or price. No SKU borrows another SKU's safety test, rating, certification claim or relay design without a fresh review.

## Unparking gate

Funded only when: ≥10 V1 pilot customers request it in writing, or ≥3 sites are lost per month to phase-topology rejection at commissioning, **and** V1 has passed its pilot stage gate.
