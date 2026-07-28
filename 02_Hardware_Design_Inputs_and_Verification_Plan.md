# Hardware Design Inputs and Verification Plan

**Document ID:** EIP-HW-DIVP-001  
**Version:** 0.1  
**Status:** Design input; all performance values require verification before external use  
**Owner:** Hardware and Metrology  
**Applies to:** Home-8, single-phase V1

## Authoritative V1 topology

Home-8 measures one service-live conductor and up to eight labelled branch-live conductors. Each CT surrounds one conductor only; it must not surround the live-neutral pair. One protected, isolated voltage-reference path provides the voltage waveform used for mains and branch power calculations. A branch’s active power is calculated from its CT waveform and the shared voltage reference; a branch label is electrician-configured, not an appliance-identity claim.

The primary AC domain contains only the rated AC entry/protection, isolated supply and selected isolated voltage-reference component. The metering AFE, MCU, Wi-Fi/BLE radio, debug interface, LED, button and all customer-accessible service connections are SELV. Any alternative that transfers a hazardous-domain analogue signal must be rejected unless it has a written isolation architecture, component ratings, fault analysis and safety-review approval.

## Controlled operating inputs

| Parameter | Pre-DVT input | Evidence required before it becomes a published specification |
|---|---|---|
| Nominal line | 230 V AC, 50 Hz, single phase | Panel survey and final compliance plan |
| Verification voltage/frequency | 180–260 V AC; 45–65 Hz | Traceable bench report |
| Service measurement | One 63 A nominal service CT | Selected CT data sheet, thermal/linearity result and installation mock-up |
| Branch measurement | Eight separately keyed and mapped CT inputs | Harness/connector review and commissioning test |
| Operating/storage environment | 0–60 °C / −20–70 °C planning target; indoor distribution-board installation only | Enclosure/environment definition and laboratory plan |
| Accuracy | Engineering target: system active-energy error no greater than ±1% only in the approved test envelope after calibration | DVT report with CT, firmware, temperature, current and PF traceability |
| Availability | Seven days of local one-minute aggregates plus event journal during WAN loss | Storage budget and seven-day outage/recovery test |
| Customer latency | P95 end-to-end live update no greater than two seconds on a healthy connection | Time-synchronised device-to-app test |

The approved accuracy envelope must explicitly define service and branch CT part numbers, burden/protection circuit, voltage, at least six current points, power-factor points, phase direction, temperature points, calibration state, reference instrument and acceptance statistic. “±1%” may not appear in sales material before this envelope and report are approved.

## Metrology and calibration requirements

| ID | Requirement | Verification evidence |
|---|---|---|
| MET-001 | The design shall retain a unique calibration version, source fixture ID, reference-instrument ID and calibration date for every unit. | Unit genealogy query and manufacturing record |
| MET-002 | Calibration coefficients shall be CRC/signature protected and atomic under power interruption. | Fault-injection test and production-fixture result |
| MET-003 | A measurement record shall carry channel ID, direction, quality flag, calibration version, sequence and clock-quality state. | Schema conformance and HIL report |
| MET-004 | A CT mapping/orientation error shall cause an installer-visible warning or an explicit confirmation rather than silently changing historical raw data. | Commissioning test with reversed/mis-mapped CT |
| MET-005 | The product shall suppress metrology-dependent advice when calibration, timestamp or measurement-quality status is invalid. | Rules-engine integration test |

## Electrical safety design inputs

The schematic review must document line/fault paths, fuse and protection ratings, isolation barriers, creepage/clearance by layer, component voltage/temperature rating, enclosure material/flame rating, ingress assumption, terminal/harness retention, CT open-circuit behaviour, service access, ESD/surge paths, thermal derating and abnormal-power recovery. These are design inputs, not a declaration of compliance.

No engineering build may be installed in a customer panel until the hazard review has approved the panel type, installation method, enclosure, line protection, installer PPE/work instruction and rejection criteria.

## Core DVP&R

| Requirement | Test / method | Minimum evidence | Gate |
|---|---|---|---|
| HWR-001 | Line voltage/frequency and current measurement across approved envelope | Traceable reference report, wiring photo, raw data and verdict | DVT |
| HWR-002 / MET-004 | CT keying, mapping and orientation error simulation | Installer-flow recording and warning result | EVT |
| HWR-003 | Active-energy accuracy at approved voltage/current/PF/temperature points | Calibrated load/reference report with uncertainty | DVT |
| HWR-004 | Schematic/BOM/netlist review for prohibited load-control path | Signed independent review | EVT |
| HWR-005 | Isolation, surge, ESD, thermal, enclosure and pre-compliance plan | Hazard review plus test/lab reports | DVT |
| HWR-006 / MET-001 | Calibration corruption and unit-genealogy tests | Fixture result and device record | EVT |
| FWR-003 / FWR-007–009 | Seven-day WAN outage, restart and ordered recovery | Device logs, cloud reconciliation and no-double-count verdict | DVT |
| PRD-001 / FWR-008 | Device-to-app latency under defined pilot load | P50/P95/P99 timing report | Pilot |
| SEC-001–006 | Secure boot, OTA, commissioning and tenant-bound communication | Security test report and release sign-off | DVT |

Each row will receive a test-case ID, sample size, fixture/environment, owner, evidence location, result and waiver status in the release DVP&R. A requirement without current evidence remains open; passing an informal bench demonstration does not close it.

## EVT exit package

Before authorising DVT, the team must release: reviewed schematic and PCB layout; selected CT/voltage-reference/PSU candidates and alternates; preliminary BOM with lifecycle and lead-time data; isolation and thermal calculations; metrology test report; manufacturing-test concept; installation mock-up; updated FMEA; compliance applicability decision; and costed DVT build plan.
