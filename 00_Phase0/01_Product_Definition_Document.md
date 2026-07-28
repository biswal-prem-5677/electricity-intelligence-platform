# Product Definition Document

## Positioning and USP

**Positioning:** an India-first, electrician-installed energy intelligence monitor that makes circuit-level electricity use understandable and actionable.

**USP:** reliable circuit attribution, transparent savings logic, and serviceable installation—not a promise that AI can perfectly identify every appliance from one mains waveform.

## Product family

| SKU | Customer | Measurement | Status |
|---|---|---|---|
| Home-8 | Single-phase homes | 1 service-live + 8 branch-live CTs | MVP |
| Home-16 | Larger homes/small shops | 1 service-live + 16 branch-live CTs | Post-pilot |
| Site-3P | Small commercial / three-phase | 3 phase + branch expansion | Future |
| Cloud Pro | Multi-site reports, SSO, export | Same hardware; paid software | Future |

## Core MVP features

- RMS voltage/current, real/reactive/apparent power, energy, power factor and line frequency at mains; active power/energy at labelled branch circuits.
- Live view, 5-minute historical summaries, daily/monthly cost estimate, tariff-plan configuration and circuit labelling.
- Rule-based alerts: unusual after-hours load, high continuous load, voltage excursion and device-offline state.
- Installation workflow, calibration record, device health, signed firmware update, data export and privacy controls.

## Functional boundary

| In scope | Explicitly out of scope in V1 |
|---|---|
| Monitoring circuits and estimating cost | Utility billing, prepaid metering or meter replacement |
| Advice based on measured patterns | Automatic breaker/contact control |
| Customer-configured tariff estimate | Legal/tariff advice or guaranteed savings |
| Anomaly candidate alert | Electrical fault diagnosis or safety certification |
| Cloud/mobile access | Monitoring a site with no authorised installation |

## Personas and journey

1. Customer selects a package after confirming panel type and electrician availability.
2. Installer performs site safety/fit assessment, installs CTs and enclosure, then scans device QR code.
3. Customer labels circuits, checks live readings against known loads, and selects tariff plan.
4. System establishes seven days of baseline; then presents evidence-backed alerts and weekly review.
5. Customer acts, dismisses, or asks an electrician. Product records feedback for rule/model quality—not as ground truth without review.

## MVP definition and roadmap

MVP is successful when Home-8 can be installed safely, maintains stated monitoring accuracy, detects a bounded set of understandable patterns, and can be supported remotely. V1.1 adds Home-16 and solar import/export measurement. V2 adds three-phase and small-business features. Appliance recognition, control and utility collaboration are research/partnership tracks, not committed feature dates.
