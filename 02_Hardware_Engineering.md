# Phase 2 — Hardware Engineering Plan

## Component selection and metrology decision

| Function | Recommended V1 | Alternatives | Why / trade-off |
|---|---|---|---|
| Current sensing | Split-core CTs, selected per mains/branch range | Hall sensor, shunt, Rogowski | Non-invasive retrofit and isolation; CT phase/low-current performance must be validated |
| Metering AFE | Dedicated multi-channel metering AFE or multiplexed precision ADC architecture | Per-channel low-cost ADC | Dedicated metrology lowers calculation/linearity risk; channel cost is the key trade |
| MCU/radio | Wi-Fi/BLE industrial MCU with secure boot capability | Separate MCU + radio | Lower BOM and mature SDK; qualify RF coexistence and lifecycle |
| Power | Certified isolated AC/DC module plus input fuse, MOV and thermal protection | Discrete flyback | Faster safe path; module availability/height trade |
| Enclosure | Flame-rated, electrician-accessible polymer enclosure with strain relief | Generic ABS / DIN-only | Safety/serviceability; tooling cost and panel-fit validation |

For an isolated single-phase shunt reference, Analog Devices’ ADE9153A supports rich metrology and autocalibration, but its application notes emphasise hazardous-domain separation. It is a feasible evaluation candidate, not an automatic selection for a multi-CT SKU. [ADI layout note](https://www.analog.com/en/resources/app-notes/2020/09/21/17/59/an-1562.html) TI’s CT/ADC reference resources provide an alternative accuracy path. [TI](https://www.ti.com/applications/industrial/energy-infrastructure/smart-meter/overview.html)

## Energy-metering IC comparison

| Candidate | Strength | Constraint | Recommendation |
|---|---|---|---|
| ADE9153A | Class 1/2-oriented feature set, autocalibration, voltage/current/PQ | Single-phase channel architecture and dangerous-domain layout | EVT benchmark for mains channel |
| STPM33 | Up to four 24-bit ADCs, supports CT/shunt/Rogowski | Firmware/layout/calibration effort | Evaluate for multi-channel architecture |
| ADC + metrology MCU | Flexible, multi-channel and supply options | Highest firmware/verification burden | Use if channel count/cost win survives DVT |

STPM33 is an active metering ASSP designed for multiple precision ADC channels and CT/shunt/Rogowski inputs. [STMicroelectronics](https://www.st.com/content/st_com/en/products/data-converters/metering-ics/stpm33.html)

## Electrical design

- Voltage sensing: an isolated potential-transformer or equivalently qualified isolated voltage-reference path with primary fuse/protection, secondary protection/anti-aliasing, phase-characterisation and validated fault/thermal derating. A direct high-impedance divider into a hazardous-domain AFE is not the V1 baseline and requires a separately approved isolation architecture.
- CT inputs: keyed connectors, burden/protection network, common-mode/ESD design, input-open detection if technically viable, and CT-specific phase/gain calibration.
- Power: fused line entry, surge protection sized by final test plan, isolated supply, SELV rails, brownout detection and no reliance on protective earth for basic function.
- Isolation: document functional/basic/reinforced isolation boundaries, creepage/clearance after pollution degree/material group/altitude are frozen, and prohibit copper/slots/fasteners from violating them.
- Relay: none in V1. Future switching requires contact rating, weld detection, manual override, abnormal-state analysis and a separate safety case.

## PCB, thermal, EMI/EMC and mechanical

Use a 4-layer FR-4 PCB as the DVT default: controlled ground reference, separated hazardous/metrology/SELV areas, keep-out barrier, stitched RF ground in SELV zone, and test-point access. Place surge/fuse/AC input away from CT and RF circuits; keep metrology input loops short, guard sensitive inputs where justified, and route high-current paths only after thermal calculation. Use slots only when mechanically/production validated.

Model worst-case enclosure temperature at high line, radio transmit and ambient target. Validate with thermocouples and derate components. Pre-scan conducted/radiated emissions and immunity before tooling; cure root causes at PCB/enclosure level, not with unsupported component substitutions.

## Safety, reliability and failure modes

| Failure mode | Detection / mitigation |
|---|---|
| CT unplugged, reversed or wrong circuit | Commissioning verification, plausibility flag, installer photo/checklist |
| Overvoltage/surge | Fuse/protection network, qualified PSU, test plan |
| Supply brownout/reset | Supervisor, local journal, watchdog, diagnostic counter |
| Calibration corruption | CRC/versioned coefficients, manufacturing record, re-calibration workflow |
| Wi-Fi loss | Local buffer, reconnect backoff, clear status |
| Enclosure heat/terminal stress | Thermal and mechanical validation, installation exclusions |

## Engineering gates, BOM and cost

EVT proves measurement architecture and hazardous-domain layout; DVT proves enclosure, pre-compliance, reliability and production fixture; PVT proves yield and operator process. The final BOM must include manufacturer part, lifecycle status, two-source strategy, approved alternates, safety class, unit cost at MOQ, lead time and change owner. Hardware cost targets are in the Phase 0 cost analysis; no cost reduction may remove a protection, test or traceability control.
