# Electricity Intelligence Platform — Engineering Documentation Suite

**Status:** Pre-build architecture baseline; controlled artefacts in preparation  
**Version:** 0.3  
**Prepared:** 29 July 2026  
**Primary market:** India  
**Document owner:** Product and Engineering

This repository is the controlled documentation baseline for an affordable, electrician-installed electricity intelligence platform. It is intentionally not a utility billing meter specification. The V1 product is a **behind-the-meter monitoring appliance** that measures mains and selected circuits, presents reliable energy intelligence, and never controls a load by default.

## Product decision in one page

| Decision | Baseline | Reason |
|---|---|---|
| Initial customer | Urban Indian homes and small commercial sites | Shortest installation and validation loop; clear bill and uptime pain |
| Measurement approach | One service-live CT plus eight branch-live CTs | Deterministic circuit attribution is more useful and dependable than appliance inference |
| Electrical topology | Clamp-on CT inputs; isolated low-voltage electronics | No interruption of the customer conductor; safer retrofit path |
| Metering claim | Monitoring-grade, target ±1% system energy after field calibration | Do not make revenue-grade or utility-billing claims before product-specific certification |
| Connectivity | Wi-Fi 2.4 GHz; BLE only for commissioning | Lowest cost and broadest consumer availability |
| Cloud | MQTT ingestion, time-series store, rules first | Robust offline operation and explainable MVP recommendations |
| AI position | Augment measured data, never replace it | Reduces unsafe or unprovable claims |
| Installation | Qualified electrician, inside/outside approved distribution-board enclosure | Mains work is not DIY |
| Control | No contactor/relay in V1 | Removes a high-risk failure mode; monitoring evidence comes first |

## Reading order

1. [00_Phase0/00_Project_Overview.md](00_Phase0/00_Project_Overview.md) through [00_Phase0/08_MVP_Scope.md](00_Phase0/08_MVP_Scope.md)
2. [01_Phase1_SRS/](01_Phase1_SRS/)
3. [02_Hardware_Engineering.md](02_Hardware_Engineering.md) through [09_Commercialization.md](09_Commercialization.md)
4. [10_Assumptions_Traceability_and_References.md](10_Assumptions_Traceability_and_References.md)
5. [00_Phase0/12_Prebuild_Governance_Compliance_and_Risk_Plan.md](00_Phase0/12_Prebuild_Governance_Compliance_and_Risk_Plan.md), [02_Hardware_Design_Inputs_and_Verification_Plan.md](02_Hardware_Design_Inputs_and_Verification_Plan.md), and [01_Phase1_SRS/14_Requirements_Traceability_Matrix.md](01_Phase1_SRS/14_Requirements_Traceability_Matrix.md)

## Product boundaries

The device may show estimated tariff cost, but the utility invoice and the utility meter remain authoritative. It is not an emergency protection device, breaker, MCB/RCCB replacement, medical device, or a substitute for statutory energy accounting. A recommended action must be explainable from measured data and must never claim a guaranteed saving.

## Baseline release gates

No pilot shipment is permitted until: electrical safety design review is signed off; the compliance applicability matrix is approved; metrology accuracy is demonstrated against a traceable reference across the stated operating envelope; secure-boot and signed-OTA tests pass; field-installation checklist is approved; and product-liability/warranty terms are reviewed locally.
