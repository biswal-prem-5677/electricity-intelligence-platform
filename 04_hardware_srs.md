# 04 — Hardware SRS, Design Inputs and Verification Plan

**Document ID:** EIP-04 · **Version:** 1.0 · **Status:** Design input — no value here is a published specification until verified · **Owner:** Hardware and Metrology
**Supersedes:** Hardware SRS, Hardware Engineering Plan, Hardware Design Inputs & Verification Plan, PCB Design Plan, Performance/Data/Interface SRS (hardware portions)

---

## 1. Scope

Electrical, mechanical, metrology, PCB and production requirements for **Circuit-8**, single-phase V1. This is **not** a certified utility-meter design and must never be represented as one.

## 2. Requirements

| ID | Requirement | Acceptance criterion | Gate |
|---|---|---|---|
| HWR-001 | Measure 230 V / 50 Hz single-phase mains via one service-live CT and a shared isolated voltage reference; 63 A nominal service current; define overload and derating | Traceable bench report covering 180–260 V, 45–65 Hz and the approved current/PF/temperature envelope | DVT |
| HWR-002 | Eight branch CT inputs with keyed, orientation-controlled harnessing | No normal installation permits a wrong branch mapping without explicit app confirmation | EVT |
| HWR-003a | **System active-energy error ≤ ±2.0%** across the full approved envelope after calibration | Traceable reference comparison at ≥6 current points, ≥3 PF points, ≥3 temperature points | DVT |
| HWR-003b | Stretch: ≤ ±1.0% within a narrower published envelope | Same report; envelope printed alongside the figure | DVT |
| HWR-004 | **Contain no load-switching path** | Independent schematic, BOM and netlist review confirms no relay, contactor or triac output | EVT |
| HWR-005 | Provide isolation, protection, creepage/clearance and enclosure design appropriate to the final classification | Independent safety design review plus applicable pre-compliance evidence | DVT |
| HWR-006 | Retain calibration and hardware-revision identity through power loss | Unit reads a signed/CRC-protected record after power cycling and brownout injection | EVT |
| HWR-007 | Every published range/accuracy claim identifies topology, CT part, calibration method, envelope and reference standard | Claim-review checklist signed before any external use | Pilot |
| HWR-008 | Sample at a rate justified by the metrology architecture, with proven alias control | DVT metrology report for the selected AFE/ADC | DVT |
| HWR-009 | Detect and flag a plausibility failure indicating CT mis-mapping or reversal (e.g. persistent negative residual, branch > mains) | Commissioning test with a deliberately reversed and a deliberately mis-mapped CT | EVT |
| HWR-010 | Enclosure fits the approved Indian distribution-board types identified in the Phase 0 survey, with strain relief and service access | Physical fit verified in ≥5 real DBs of each approved type | DVT |
| **HWR-011** | **Device self-consumption ≤ 3.0 W typical at the AC input, ≤ 5.0 W peak during radio transmit.** Stretch: ≤ 2.0 W typical | Measured at the AC input with a reference power analyser, over 24 h with the radio connected and a live session opened, at 180 V / 230 V / 260 V | DVT |
| **HWR-012** | **Operating ambient 0 to +50 °C, storage −20 to +70 °C, 5–95% RH non-condensing**, at the enclosure exterior inside a closed distribution board. Accuracy per HWR-003a holds across this range | Chamber test at ≥3 temperature points, plus the modelled and thermocouple-validated internal rise required by §7 | DVT |

**HWR-011 and HWR-012 were added during Phase 1** ([D-017](14_decision_log.md#d-017)). Both were gaps, not changes: HWR-001 and HWR-003a already referred to "the approved temperature envelope" without anywhere defining it, and self-consumption had no requirement at all.

**Why self-consumption is a requirement and not a detail.** The device is always on and it is sold on the basis of reducing an electricity bill. At 3 W it draws ~26 kWh/year, roughly **₹210–265/year** at commercial tariff, against a claimed saving of ₹14,400–43,200/year — under 2% of the benefit. That arithmetic is fine, and it must be *available on demand*, because the question "your energy saver consumes energy?" will be asked by a customer, a competitor or a journalist, and the only good answer is a measured number. It also constrains component selection directly: an AFE's supply current, the isolated voltage reference's burden and the AC/DC module's efficiency at a few watts are now scored, not assumed.

## 3. Metrology and calibration

| ID | Requirement | Evidence |
|---|---|---|
| MET-001 | Every unit retains a unique calibration version, fixture ID, reference-instrument ID and calibration date | Unit genealogy query + manufacturing record |
| MET-002 | Calibration coefficients are CRC/signature protected and written atomically under power interruption | Fault-injection test + production-fixture result |
| MET-003 | Every measurement record carries channel ID, direction, quality flag, calibration version, sequence and clock-quality state | Schema conformance + HIL report |
| MET-004 | A CT mapping or orientation error produces an installer-visible warning or an explicit confirmation — it never silently alters historical raw data | Commissioning test with reversed / mis-mapped CT |
| MET-005 | Metrology-dependent output is suppressed when calibration, timestamp or measurement quality is invalid | Rules-engine integration test |
| MET-006 | **Field CT replacement has a defined workflow**: replacing a CT either uses a pre-characterised matched part, or triggers a guided in-app recalibration, or explicitly downgrades that channel's accuracy claim with a visible flag | Documented procedure + a test replacement performed end-to-end |

**MET-006 closes a real gap:** per-CT calibration is required to meet the accuracy spec, but CTs are field-replaceable consumables. One of the three paths above must be chosen at component selection — it is a sourcing decision as much as a firmware one.

## 4. Electrical design requirements

**Voltage sensing.** An isolated potential transformer or equivalently qualified isolated voltage-reference path, with primary fuse and protection, secondary protection and anti-aliasing, phase characterisation, and validated fault and thermal derating. A direct high-impedance divider into a hazardous-domain AFE is **not** the V1 baseline.

**CT inputs.** Keyed connectors, burden and protection network, common-mode and ESD design, input-open detection where technically viable, and per-CT phase and gain calibration. *CT open-circuit behaviour must be analysed and documented — prefer CTs with an integrated burden.*

**Power.** Fused line entry, surge protection sized by the final test plan, certified isolated AC/DC module, SELV rails, brownout detection. **No reliance on protective earth for basic function.**

**Isolation.** Document functional/basic/reinforced boundaries and creepage/clearance per layer after pollution degree, material group and altitude are frozen. Copper, slots and fasteners may not violate a boundary.

**Relay.** None. Any future switching requires contact rating, weld detection, manual override, abnormal-state analysis and a separate safety case — see [future/Future_Hardware.md](future/Future_Hardware.md).

## 5. PCB requirements

**Stack-up:** 4-layer FR-4 default with manufacturer-controlled dielectric data. Layer assignment is a design output, not a rule of thumb — protect metrology return paths, give RF a solid reference, keep switching return currents compact.

**Partitioning:** AC entry/protection · isolated power · voltage sensing · CT channels · metering AFE · MCU/radio/secure element · programming and debug · indicators · manufacturing test. Every connector pin, net class and domain boundary is labelled.

| Area | Rule |
|---|---|
| Hazardous input | Fuse, protection and AC connector grouped; minimise unprotected trace length |
| Isolation barrier | Calculated clearance and creepage on **every** layer; no silkscreen or copper shortcut; slots documented and production-validated |
| Metering analogue | CT and voltage paths short and symmetric where required; isolated from switching-supply and RF return currents |
| RF | Follow the module's reference keep-out, antenna clearance and matching guidance; no metal or enclosure obstruction without a test |
| Power | Traces and vias sized for fault and continuous current; no thermal relief where safety current requires a solid connection |
| Ground | Intentional return paths; hazardous and SELV grounds joined only through the approved architecture — normally not at all |
| Test | Labelled test points for rails, reset, programming, metrology reference, CT and voltage inputs, production UART and fixture ground. Unsafe-domain points carry guarded fixture instructions |

**Release checklist:** ERC/DRC, 3D interference check, stack-up signoff, creepage/clearance audit, thermal review, assembly review, test-fixture review, and an **independent** schematic and layout safety review. No routing exception is accepted verbally — each has a drawing location, calculated rationale, approver and revalidation trigger.

**Manufacturing outputs:** controlled Gerber/ODB++, drill, fab drawing, assembly drawing, centroid, BOM, schematic PDF, pick-and-place, programming image and revision manifest. Panelisation, fiducials, tooling holes, paste apertures, spacing and polarity marking agreed with the EMS. AOI/X-ray specified for packages where visual inspection is inadequate. First articles get 100% visual inspection plus electrical fixture test.

## 6. Thermal, EMC and mechanical

Model worst-case enclosure temperature at high line, radio transmit and target ambient; validate with thermocouples; derate components. Pre-scan conducted and radiated emissions and immunity **before tooling**; cure root causes at PCB and enclosure level, never with unsupported component substitutions. Flame-rated, electrician-accessible polymer enclosure with strain relief, validated against real Indian DB geometry.

## 7. Failure modes

| Failure mode | Detection / mitigation |
|---|---|
| CT unplugged, reversed or on the wrong circuit | Keyed harness, commissioning verification against a known load, plausibility flag (HWR-009), installer photo checklist |
| Overvoltage / surge | Fuse and protection network, qualified PSU, defined test plan |
| Supply brownout or reset | Supervisor, local journal, watchdog, diagnostic counter |
| Calibration corruption | CRC/versioned coefficients, manufacturing record, recalibration workflow (MET-006) |
| Wi-Fi loss | Local buffer, reconnect backoff, clear status indication |
| Enclosure heat or terminal stress | Thermal and mechanical validation, documented installation exclusions |
| CT open circuit | Integrated-burden CT selection; behaviour analysed and documented |

## 8. Design Verification Plan and Report (DVP&R)

| Requirement | Method | Minimum evidence | Gate |
|---|---|---|---|
| HWR-001 | Voltage/frequency/current across the approved envelope | Traceable reference report, wiring photo, raw data, verdict | DVT |
| HWR-002 / MET-004 / HWR-009 | CT keying, mapping and orientation-error simulation | Installer-flow recording + warning result | EVT |
| HWR-003a/b | Active-energy accuracy at all approved V/I/PF/temperature points | Calibrated load and reference report with stated uncertainty | DVT |
| HWR-004 | Schematic/BOM/netlist review for any prohibited control path | Signed independent review | EVT |
| HWR-005 | Isolation, surge, ESD, thermal, enclosure, pre-compliance | Hazard review + laboratory reports | DVT |
| HWR-006 / MET-001/002 | Calibration corruption and unit-genealogy test | Fixture result + device record | EVT |
| HWR-010 | Physical fit in approved DB types | Photographed fit test, ≥5 DBs per type | DVT |
| MET-006 | Field CT replacement workflow | End-to-end replacement performed and accuracy re-verified | DVT |
| FWR-003 / 007–009 | 7-day WAN outage, restart, ordered recovery | Device logs, cloud reconciliation, no-double-count verdict | DVT |
| SEC-001–006 | Secure boot, OTA, commissioning, tenant binding | Security test report + release sign-off | DVT |

Each row gets a test-case ID, sample size, fixture, environment, owner, evidence location, result and waiver status. **A requirement without current evidence stays open. An informal bench demonstration does not close it.**

## 9. EVT exit package

Before DVT is authorised, release: reviewed schematic and PCB layout · selected CT, voltage-reference and PSU candidates plus alternates · preliminary BOM with lifecycle and lead-time data · isolation and thermal calculations · metrology test report · manufacturing-test concept · installation mock-up in a real DB · updated FMEA · compliance applicability decision · costed DVT build plan.

## 10. Safety gate

**No engineering build may be installed in a customer panel until** the hazard review has approved the panel type, installation method, enclosure, line protection, installer PPE and work instruction, and the site rejection criteria. This gate has no waiver path.
