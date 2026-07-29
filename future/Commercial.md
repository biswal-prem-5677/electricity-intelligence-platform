# Future — Manufacturing Scale-Up, Certification Programme and Commercial Operations

**Status:** Parked until the V1 pilot stage gate passes. Applies from PVT onward.

## Why parked

The full manufacturing, certification and commercial machinery described here is correct for a company shipping thousands of units. Applying it to a 20-unit engineering build stops the build. V1 carries only the minimum controls listed in [../08_testing_validation.md](../08_testing_validation.md) — revision control, serial genealogy, calibration record, end-of-line functional test. Everything below is the version of those controls needed at volume.

## Manufacturing at volume

Approved vendor list → incoming inspection → SMT/PCBA → programming and calibration → functional and safety test → final inspection → serialisation → packing → shipment/RMA feedback, with an MES or controlled production record tying serial to BOM revision, firmware, calibration, operator and test result.

| Control | Volume requirement |
|---|---|
| Build authority | Only released BOM/PCB/firmware revisions; the traveller blocks undocumented substitution |
| Genealogy | Serial query returns lot, supplier and date code, calibration record and final-test result |
| End-of-line test | Every unit: identity, metrology sanity, radio, safety checks, firmware signature. Failures quarantined |
| Calibration | Controlled fixture and reference; coefficients CRC/signature protected; sample audit against the accuracy spec |
| Quality system | Incoming inspection, ESD control, workmanship standard, nonconformance and CAPA with an SLA |
| Process | Capability study, yield tracking, rework policy, burn-in decision and sampling plan — all established from pilot data, not assumed |
| Substitution | No safety-critical or metrology-critical alternate without engineering **and** compliance approval |
| Tooling | Injection moulding for the enclosure; expect two revisions before the tool is right |

## Certification programme

V1 pre-compliance is scoped in [../08_testing_validation.md](../08_testing_validation.md). The full programme below begins at DVT exit.

| Workstream | Owner | Note |
|---|---|---|
| Product classification opinion | Compliance consultant | Monitor vs. meter boundary, permitted claims, labelling. **This is a Phase 0 deliverable — it is the one certification item that cannot wait** |
| Electrical safety | Electrical safety engineer + accredited lab | Standard selection follows classification. Expect 6–14 week lab queues |
| EMC/EMI | Accredited lab | Pre-scan per PCB revision; formal test after layout freeze. Any layout change reopens it |
| Radio authorisation | Compliance lead | Avoidable cost: use a pre-certified module with valid Indian ETA rather than laying out your own RF |
| Metrology claim | Metrology lead | Accuracy language, reference standard, calibration method, report format |
| Privacy / DPDP | Privacy lead | Data inventory, notice, consent, processor contracts, retention/deletion, incident process |
| E-waste, packaging, consumer obligations | Operations/Legal | EPR, packaging, pre-packaged commodity rules, warranty wording |
| Product liability insurance | Founder/Legal | Mains-connected consumer device. Required before the first customer install |

## Commercial operations at scale

**Go-to-market.** One or two cities, trained electricians, a narrow set of approved panel types. Sell an installed outcome with published exclusions — never an unmanaged DIY kit.

**Unit economics — track separately, never blended.** Landed COGS, installer payment, warranty reserve, shipping, payment fees, cloud cost per device per month, support cost per device per month, CAC, refund rate. *A hardware gross margin computed without warranty, support and cloud is not a gross margin.*

**Levers.** Price hardware-only and installed-package separately and never conceal installation cost. Distribute through an installer-led channel plus direct lead generation. Publish warranty coverage, exclusions, replacement process and electrician escalation. Track first-contact resolution, RMA rate, repeat truck rolls and CSAT. Keep controlled builds — do not finance wide inventory before field yield stabilises.

**Cost-reduction roadmap, in order.** (1) Never reduce protection, isolation, CT qualification or production test. (2) Consolidate SKU components after field evidence. (3) Dual-source harnesses and CTs. (4) Automate calibration and final functional test. (5) Negotiate blanket POs only after yield is stable.

## Hiring, in the order the work appears

Product/field operations → power and electronics engineer → embedded engineer → backend/mobile engineer → QA/compliance → supply chain → support. **Add data science only after quality telemetry and labelled field problems exist**, not before.

## Fundraising

Raise against evidence, not narrative: panel-fit survey data, a signed safety design review, measured unit cost, measured installation time, field reliability, paid demand, and a written regulatory plan. A generic AI story raises nothing in hardware.

## Strategic boundaries

Utility/DISCOM contracts, insurance products, demand response, load switching, enterprise hospital and industrial deployments, and white-label projects each require their own compliance, liability, sales-cycle and support review. **None is a prerequisite for the V1 launch.**
