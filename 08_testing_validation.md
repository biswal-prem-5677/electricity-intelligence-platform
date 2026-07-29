# 08 — Testing, Certification and Field Validation

**Document ID:** EIP-08 · **Version:** 1.0 · **Status:** Baseline · **Owner:** Quality and Test
**Supersedes:** Testing & Certification Plan, Reliability/Compliance SRS, Field Validation Plan

---

## 1. Principle

**A requirement without current evidence is open.** An informal bench demonstration does not close it. Every requirement in [02](02_product_requirements.md), [04](04_hardware_srs.md), [05](05_firmware_srs.md), [06](06_cloud_backend_srs.md) and [07](07_mobile_app_srs.md) maps to at least one test case with a sample size, fixture, environment, owner, evidence location and verdict.

## 2. Test levels

| Level | What it proves | Runs when |
|---|---|---|
| Unit / host | Calculation, parser and state-machine correctness | Every commit |
| Integration | AFE→MCU→cloud→app path with schema conformance | Every merge to main |
| Hardware-in-loop (HIL) | Fault injection: SPI fault, CT disconnect, clock loss, brownout, MQTT replay, corrupt config, interrupted OTA | Nightly on a rack of ≥3 units |
| Metrology | Accuracy against a traceable reference across the envelope | EVT, DVT, and every calibration-method change |
| Safety / EMC pre-compliance | Isolation, surge, ESD, emissions, immunity, thermal | Before tooling, and after any layout change |
| Soak | 30-day continuous operation without memory leak, counter drift or reconnect failure | DVT and PVT |
| Field | Real panels, real installers, real Wi-Fi | Pilot |

## 3. Metrology test plan — the one that decides the product

**Reference:** a traceable reference standard or calibrated reference meter with a stated uncertainty **at least 4× better** than the claimed accuracy. Budget and lead time in [10_bom_and_cost.md](10_bom_and_cost.md).

**Matrix:** ≥6 current points spanning the approved range including a low-current point · ≥3 power-factor points (1.0, 0.8 lagging, 0.5 lagging) · ≥3 temperature points · nominal and range-limit voltage. Repeat on ≥5 units with ≥3 CT samples each to expose unit-to-unit spread.

**Report contents:** raw data, reference instrument ID and calibration date, ambient conditions, wiring photograph, computed error per point, stated measurement uncertainty, and a pass/fail verdict against HWR-003a. HWR-003b is reported **only** with its envelope printed beside it.

**Real-load test cases** (each with expected profile): ceiling fan · iron · air conditioner · refrigerator (duty-cycled) · water heater · laptop charger (low PF, low current — the hard one) · LED lighting bank · idle site · near-maximum load. The low-current and low-PF cases are where a CT-based system fails; run them first, not last.

## 4. Safety and certification

| Item | Position for V1 |
|---|---|
| Product classification opinion | **A Phase 0 deliverable.** Obtain a written opinion from a competent authority or consultant *before* PCB tooling. Classification drives everything downstream |
| Radio authorisation | Avoided by design — pre-certified module with valid Indian ETA (F7) |
| Electrical safety testing | Applicable standard set determined by the classification opinion; pre-compliance before tooling, full test before commercial sale |
| EMC | Pre-scan emissions and immunity before tooling; re-scan after any layout or enclosure change |
| Product liability insurance | Required before the first customer-site installation, not before pilot sign-off |
| Utility/billing claims | **None.** The product is not a revenue-grade meter and no document, screen or advertisement may imply it is |

**The no-waiver gate (HWR-005 / §10 of [04](04_hardware_srs.md)):** no engineering build enters a customer panel until the hazard review has approved panel type, installation method, enclosure, line protection, installer PPE, work instruction and site rejection criteria.

## 5. Field validation (pilot)

**Target:** 20–30 units across a mix of small commercial sites (the beachhead segment per D-002) and a small number of homes as a control.

| Question the pilot must answer | Measurement | Pass |
|---|---|---|
| Does it install safely and repeatably? | Commissioning time, rework rate, site-rejection rate | Median ≤90 min; rework <10% |
| Is the data trustworthy? | Spot comparison against a reference clamp meter at each site; residual sanity | Within the HWR-003a envelope at every site |
| Does it stay online? | Uptime, buffer depth, reconnect count | ≥95% of expected 5-minute intervals delivered |
| Do the rules produce useful, non-embarrassing insights? | Insight acceptance / dismissal / "this is wrong" rate | <10% flagged wrong |
| **Does it pay back?** | Customer-reported or measured action taken and resulting bill change | ≥1 verified action per site within 60 days |
| Would they pay again? | Willingness-to-pay interview and renewal intent | ≥60% would recommend at the tested price |

**The pilot fails honestly if these are not met.** A pilot that produces no verified saving is a product answer, not a marketing problem.

## 6. Reliability

Define MTBF expectation, warranty reserve assumption and failure-return process **before** shipping. Every field failure gets a returned unit, a recorded root cause and a corrective action. Track: infant mortality rate, CT failures, PSU failures, flash wear, connectivity dropouts. Flash wear budget is calculated from the FWR-010 write pattern, not assumed.

## 7. Release criteria for V1 commercial

All of the following, with no open critical or high defect and no unapproved waiver:

1. HWR-003a met on ≥5 units with a traceable report
2. Safety and EMC evidence complete for the determined classification
3. 30-day soak passed with zero data-integrity defects
4. 7-day WAN-outage recovery with zero double counting
5. Security test report signed (SEC-001…008)
6. Pilot targets in §5 met
7. Support runbooks, RMA process and installer training material in place
8. Every published claim passes the HWR-007 claim-review checklist
