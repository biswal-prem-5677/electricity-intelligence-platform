# Initial Requirements Traceability Matrix

**Document ID:** EIP-SRS-RTM-001  
**Version:** 0.1  
**Status:** Planning matrix; rows close only with signed test evidence  
**Owner:** QA and Systems Engineering

## Control rule

Every normative requirement has one or more planned verification methods and an evidence location. Verification methods are I = inspection/review, A = analysis, T = test, and D = demonstration. This matrix is the initial coverage map; test-case IDs, sample sizes, equipment, environment, result, waiver and evidence link must be added before a release gate closes.

| Requirement group | Primary verification | Required evidence | Gate |
|---|---|---|---|
| PRD-001–006 Product outcome, data, alerts, commissioning and privacy actions | T, D | End-to-end app/site test, audit log and export/delete workflow result | Pilot |
| HWR-001–006 Electrical topology, CT mapping, accuracy, no switching, safety and calibration identity | I, A, T | Schematic/BOM review, metrology report, hazard review and unit-genealogy record | DVT |
| HWR-007–008 Published-envelope and sampling evidence | A, T | Measurement architecture analysis and DVT metrology report | DVT |
| FWR-001–006 Secure boot, energy counter, buffer, recovery, configuration and commissioning | T | HIL fault-injection, seven-day outage/recovery and security test records | DVT |
| FWR-007–009 Live record, latency and local-retention performance | T | Time-synchronised latency report and storage/recovery report | Pilot |
| CLR-001–007 Device registry, telemetry validation, lifecycle, OTA, alerting and cloud retention | T, I | API/MQTT contract tests, tenant-isolation tests, restore exercise and observability dashboard | Pilot |
| AIR-001–005 Baseline, tariff, explanation, feedback and rule/model rollback | T, A | Versioned rule card, offline evaluation and canary/rollback record | Pilot |
| MOB-001–006 Tenant-scoped access, commissioning, freshness, settings, accessibility and secret handling | T, D, I | Mobile QA report, accessibility results and mobile security review | Pilot |
| ADM-001–005 Admin scope, fleet health, OTA, installers and consent | T, I | RBAC test suite, audit export and staged-OTA simulation | DVT |
| TST-001–005 Test traceability, metrology, system testing, compliance testing and pilot UAT | I, T | Signed DVP&R, calibrated-instrument register, test reports and pilot report | Pilot |
| OPS-DEP-001–005 Environments, rollout, OTA, activation and recovery | T, D | Deployment records, rollback exercise, inventory test and restore exercise | DVT |
| SEC-001–006 Device identity, boot/update, communication, commissioning, RBAC and SBOM | T, I | Threat model, penetration/abuse tests, SBOM and vulnerability disposition | DVT |
| MFG-001–005 Revision control, genealogy, end-of-line test, calibration and quality | I, T | Manufacturing traveller, fixture data, sample audit and CAPA records | PVT |
| OPS-001–006 Installer eligibility, support, RMA, data requests and safety wording | D, I | Installer records, tabletop exercise, support scripts and monthly quality review | Pilot |

## Evidence convention

Evidence is stored by release and artefact type, for example: artefacts/testing/DVT/TC-HWR-003-metrology-report.pdf. A failed test opens a defect; a waiver requires the risk ID, compensating control, approver, expiry and affected versions. Requirements must not be silently deleted to make a matrix appear complete.
