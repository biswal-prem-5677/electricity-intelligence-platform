# Phase 8 — Testing, Validation and Certification Plan

## Test pyramid and environments

| Level | Evidence |
|---|---|
| Unit | Calculation, parser, crypto, API and UI behaviour on each change |
| Integration | Device-broker, API-data store, app-auth and OTA interactions |
| Hardware-in-loop | Metering accuracy, CT mapping, brownout, radio and production fixture |
| System / field | Qualified panel installation, WAN loss, real tariff/alerts, support flow |
| Compliance | Pre-compliance then accredited-lab plan for final product classification |

## Metrology and hardware validation

Test voltage, current, active/reactive/apparent energy, PF, frequency, phase sign, CT variants and temperature. Use traceable reference equipment and controlled load points including low-current and poor-PF cases. Record instrument ID/calibration due date, product revision, CT serial, ambient, wiring and raw result. A mismatch with utility bill is investigation evidence, not proof of device error without aligned interval/tariff/load conditions.

## Safety, environmental and reliability

Before any pilot: design-review isolation, fault/overload behaviour, enclosure/terminal mechanics, thermal rise, ESD/surge/EMC pre-scan, power cycling, storage corruption, update interruption and radio coexistence. Formal standard/certification scope is determined by a qualified Indian compliance professional and laboratory after the final product classification is locked. BIS lists smart meters under IS 16444; a behind-the-meter monitor must not self-classify as exempt or compliant without that review. [BIS](https://www.bis.gov.in/product-certification/products-under-compulsory-certification/scheme-i-mark-scheme/?lang=tm)

## Pilot and manufacturing QA

Run 50–100 authorised sites with signed installer/customer consent. Measure commissioning time, data completeness, reference accuracy sample, alert precision, return reasons, support burden and any safety event. Production QA includes incoming inspection, AOI/process controls, automated programming/calibration/function test, serial genealogy, sample audit and CAPA.

## Release gates

No release when a safety/security blocker, unbounded data loss, tenant-access defect, failed metrology target, missing traceability, or unresolved certification requirement exists. Waivers name an executive owner, compensating control, expiry and affected population.

