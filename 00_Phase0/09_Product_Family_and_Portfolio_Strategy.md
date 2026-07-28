# Product Family and Portfolio Strategy

## Purpose

This document restores the four-product portfolio identified in the founding plan while preventing a risky attempt to build all four products simultaneously. All products share identity, data model, cloud, mobile experience and installation standards; their mains, switching and certification requirements remain independent.

## Portfolio definition

| SKU | Working name | Customer / job | Electrical baseline | V1 portfolio status | Decision rationale |
|---|---|---|---|---|---|
| A | Plug-16 | Appliance-level visibility for movable 6/10/16 A loads | 230 V, 50 Hz; 16 A max; measured plug-through; optional future relay | Discovery prototype after Home-8 EVT | Simple user story but high thermal, socket-quality and switching risk |
| B | Switch-16 | Hidden light/fan/socket circuit insight | Behind-switch module; 6/10/16 A variants, no universal claim | Research only | Installation depth, neutral availability, relay safety and local controls vary widely |
| C | Home-8 / DIN-8 | Circuit-level home/small-shop monitoring | 230 V, 50 Hz; mains + eight CT channels | **Commercial MVP** | Best deterministic attribution-to-risk ratio; no load interruption or V1 switching |
| D | Site-3P | Commercial/industrial feeder and circuit monitoring | 3-phase topology, CT/VT option, DIN/panel mount | V2 feasibility programme | Valuable but demands phase topology, EMC, installer and support capability beyond home MVP |

Product C is the first shipped product. This is a portfolio sequencing decision, not a declaration that products A, B and D have been cancelled.

## Shared measurement contract

Every SKU shall measure, where topology permits: RMS voltage/current, active, apparent and reactive power, power factor, frequency and accumulated active energy. Measurements carry sample interval, quality, calibration version, topology and unit metadata. “Appliance-level” means direct plug/switch measurement; “circuit-level” means an electrician-labelled branch circuit; neither wording implies a guaranteed appliance identity.

## SKU-specific design constraints

| Constraint | Plug-16 | Switch-16 | Home/DIN-8 | Site-3P |
|---|---|---|---|---|
| Internal sampling | ≥4 kHz candidate; final rate proven by metrology | ≥4 kHz candidate | AFE/ADC architecture proven at EVT; thousands of waveform samples/s where metrology requires | Per phase simultaneously sampled or synchronised, proven at DVT |
| Customer-visible live update | ≤2 s | ≤2 s | ≤2 s | ≤2 s |
| Local buffering | ≥7 days of 1-minute aggregates; event journal | Same | ≥7 days of 1-minute aggregates; 1-second live ring buffer | Sized by channel count, ≥7 days aggregates |
| Switching | Deferred; relay must be separately qualified | Requires fail-safe local control and relay safety case | Not present in MVP | External contactor integration only after separate programme |
| Installation | Qualified installer recommended until product and local rules permit otherwise | Qualified electrician only | Qualified electrician only | Qualified commercial electrician only |

The prior 5-minute cloud aggregation remains a cost-optimisation default; it does not replace a one-second live measurement/update path or seven-day local aggregate buffer.

## Stage-gate portfolio governance

| Gate | Evidence required | Exit decision |
|---|---|---|
| G0: problem/fit | ≥20 customer interviews, panel/socket survey, preliminary safety classification | Continue, pause or kill SKU |
| G1: architecture | Hazard analysis, component availability, BOM range, certification route, install workflow | Approve EVT |
| G2: EVT | Metrology proof, thermal fault tests, secure boot/connectivity prototype | Approve DVT or redesign |
| G3: DVT | Design verification, pre-compliance, field-install simulation, manufacturing test concept | Approve controlled pilot |
| G4: pilot | Safety record, reliability, accuracy, installation time, willingness-to-pay evidence | Approve PVT/launch |
| G5: commercial | Production yield, service/warranty model, compliance evidence, cybersecurity release review | Launch or hold |

No SKU borrows another SKU’s safety test, rating, certification claim or relay design without review.

## Explicit V1 exceptions

Home-8 exposes no relay status because it has no relay. It reports enclosure/board temperature only if a qualified sensor is fitted and calibrated; otherwise the application reports “temperature not instrumented” rather than inventing a health value. Hardware reset and pairing are physical/authorised operations, not customer-facing emergency controls.

