# Pre-build Governance, Compliance and Risk Plan

**Document ID:** EIP-P0-GCR-001  
**Version:** 0.1  
**Status:** Working baseline; no approval or certification claim  
**Owner:** Product and Engineering  
**Approval required before:** EVT build release

## Purpose

This document converts the architecture baseline into a controlled pre-build programme. It is authoritative where it resolves an ambiguity in an earlier planning document. It does not substitute for an electrical safety engineer, accredited laboratory, legal counsel, privacy counsel, or a certification decision.

## Architecture decision log

| ID | Decision | Rationale | Owner | Exit evidence |
|---|---|---|---|---|
| ADR-001 | Home-8 measures one service-live conductor and eight labelled branch-live conductors. | A normal single-phase installation has one live service conductor to measure; the former “two mains CT” wording was ambiguous. | Hardware | Single-line diagram, connector map and installation mock-up |
| ADR-002 | CTs must never be installed around live and neutral together. | Their opposing magnetic fields cancel and do not provide valid branch-current measurement. | Hardware / Operations | Installer training and commissioning-photo check |
| ADR-003 | V1 uses a shared isolated voltage-reference path; the MCU, radio and customer-accessible interfaces remain SELV. | Keeps hazardous-domain design out of the app/service interface and makes the isolation boundary reviewable. | Hardware | Schematic, insulation-coordination calculation and independent safety review |
| ADR-004 | One-second records support live experience; one-minute aggregates and event journal must survive seven days offline; five-minute aggregates are the default cloud-history tier. | Separates customer responsiveness from storage cost. | Firmware / Cloud | Seven-day outage and recovery report |
| ADR-005 | No revenue-meter, protection-device, fault-diagnosis, or guaranteed-savings claim is permitted for V1. | The evidence and certification route have not yet been established. | Product / Compliance | Claim-review checklist |

No architecture decision becomes final merely because it appears in a document. Each decision needs the stated evidence, named approver, revision impact assessment and release date.

## Compliance applicability workstream

The project must determine applicability before it selects a laboratory, schedule, label or marketing claim. The Bureau of Indian Standards material for IS 16444 concerns static watt-hour smart meters; it is useful context, not proof that a behind-the-meter CT monitor is either exempt from or compliant with that route. The Central Electricity Authority’s 2026 material must be cited only from its final official/Gazette status, not inferred from a consultation or archive title. [BIS Scheme I](https://www.bis.gov.in/product-certification/products-under-compulsory-certification/scheme-i-mark-scheme/?lang=en) · [CEA meter regulations](https://cea.nic.in/regulations-category/metering-regulations/?lang=en)

| Workstream | Required decision/evidence | Accountable owner | Gate |
|---|---|---|---|
| Product classification and claims | Written opinion covering monitor vs meter, utility boundary, labels and prohibited claims | Compliance lead | Before DVT scope |
| Electrical safety | Applicable standards, insulation class, installation environment, markings, test laboratory and sample plan | Electrical safety engineer | Before EVT schematic release |
| EMC/EMI and radio | Applicable emissions/immunity tests and Wi-Fi/radio authorisation route, including WPC/ETA applicability | Compliance lead | Before DVT |
| Metering performance | Allowed accuracy language, reference standard, calibration method and report format | Metrology lead | Before pilot claims |
| Privacy and data governance | Data inventory, purpose/notice, consent where needed, processor contracts, retention/deletion and incident process | Privacy lead | Before first pilot telemetry |
| E-waste, packaging and consumer obligations | EPR/packaging applicability, warranty wording, pre-packaged/consumer-information requirements and service escalation | Operations / Legal | Before PVT |
| Site installation | Electrician qualification, rejection criteria, local work practices, handover and insurance requirements | Operations lead | Before first field install |

The Digital Personal Data Protection Rules, 2025 and the related enforcement timeline must be reviewed against the actual pilot date, customer type and data flows; “store in India” is a risk-control assumption, not complete compliance. [MeitY DPDP Rules 2025](https://www.meity.gov.in/documents/act-and-policies/digital-personal-data-protection-rules-2025-gDOxUjMtQWa)

## Risk register

| ID | Risk | Initial level | Mitigation / evidence required | Owner | Release blocker |
|---|---|---|---|---|---|
| R-01 | Wrong CT location/orientation produces misleading insight | High | Keyed harness, photo checklist, known-load verification and quality flag | Hardware / Operations | Pilot |
| R-02 | Insulation, surge or enclosure failure exposes hazardous voltage | Critical | Hazard analysis, schematic/layout review, rated components, pre-compliance and lab plan | Electrical safety | EVT |
| R-03 | CT phase/gain error prevents stated accuracy at low current or poor PF | High | CT selection study, calibration plan and traceable metrology test | Metrology | DVT |
| R-04 | Panel geometry or unsafe conductors prevent installation | High | Survey at 20+ representative sites and explicit site rejection flow | Product / Operations | Pilot |
| R-05 | Heat, radio transmission or supply faults reduce reliability | High | Thermal model/test, brownout test, RF coexistence test and derating | Hardware / Firmware | DVT |
| R-06 | Compliance scope or laboratory lead time invalidates launch schedule | High | Written classification plan, laboratory quote and standards matrix | Compliance | PVT |
| R-07 | Cross-tenant access or unnecessary behavioural data harms customers | Critical | Threat model, access tests, retention policy and incident runbook | Security / Privacy | Pilot |
| R-08 | Cloud/support/warranty costs consume apparent hardware margin | High | Full unit-economics model, RMA reserve and support time study | Finance / Operations | PVT |
| R-09 | Wi-Fi failure leaves customer with unusable experience | Medium | Pre-install survey, seven-day buffer, reconnect tests and clear freshness UX | Firmware / Operations | Pilot |
| R-10 | AI alerts cause unsafe or unhelpful action | High | Explainability, suppression on poor data, technician escalation and precision monitoring | Product / AI | Pilot |

Risks are reviewed weekly in EVT/DVT and before every pilot cohort. A waived blocker requires an executive owner, compensating control, expiry date and affected-unit list.

## Document control and evidence rules

1. Store this documentation in Git before collaborative engineering begins. Protect the main branch and use reviewed pull requests for normative changes.
2. Every released artefact has a document ID, version, owner, approver, status, effective date and linked evidence.
3. Requirements, risks, decisions, tests, defects, waivers and production changes use immutable IDs; a superseded requirement remains traceable.
4. Store released evidence under controlled folders named artefacts/hardware, artefacts/firmware, artefacts/cloud, artefacts/testing, artefacts/compliance, and artefacts/suppliers. Supplier quotations and personal data are access-restricted.
5. A review may approve a design only against current controlled inputs. A prose document never replaces a schematic, calculation, laboratory report, RFQ or test result.

## Financial control before build

The existing INR ranges are planning assumptions, not approval-ready forecasts. Before EVT release, finance must produce a monthly cash plan and a bottom-up build-cost model with quantity, supplier, lead time, currency/FX exposure, MOQ, yield, freight, GST treatment, fixture amortisation, installer labour, cloud/device/month, support/device/month, warranty reserve, channel margin and working capital. Each cost line receives a confidence level: quote/contract, market reference, or hypothesis.

Funding gates are: discovery survey and architecture evidence; EVT metrology/safety proof; DVT compliance and production-fixture plan; controlled paid pilot; PVT yield and service economics. Do not fund broad inventory or public accuracy claims before the preceding gate has passed.
