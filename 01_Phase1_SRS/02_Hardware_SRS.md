# Hardware SRS

## Purpose and scope

Specifies Home-8 electrical, mechanical and production requirements. It is not a certified utility meter design.

## Architecture and interfaces

One service-live CT and eight branch-live CT analogue interfaces, plus a shared isolated voltage reference, feed an energy-metrology front end. An isolated supply powers the MCU/radio domain. CT connectors, voltage-reference interface, AC input, programming/test pads, status LED and enclosure mounting are controlled interfaces.

## Functional and performance requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| HWR-001 | Measure 230 V, 50 Hz single-phase mains using one service-live CT and a shared voltage reference; 63 A nominal service current; define overload and derating. | Bench report covers 180–260 V, 45–65 Hz and the approved current/PF/temperature envelope. |
| HWR-002 | Use qualified split-core CTs with keyed/orientation-controlled harnessing. | No normal installation permits wrong branch connector mapping without app confirmation. |
| HWR-003 | Achieve system energy error ≤±1.0% in approved envelope after calibration. | Traceable reference comparison at low/nominal/high current and PF points. |
| HWR-004 | Contain no V1 load switching path. | Schematic/BOM review shows no relay/contactor/triac output. |
| HWR-005 | Provide isolation, protection, creepage/clearance and enclosure design appropriate to final classification. | Independent design review plus applicable pre-compliance evidence. |
| HWR-006 | Retain calibration and hardware-revision identity. | Unit test reads signed/CRC-protected record after power cycle. |

## Reliability, safety and lifecycle

Design for service temperature, humidity, surge/ESD/EMC and ingress targets only after enclosure class and laboratory plan are selected. No component substitution crosses a controlled qualification boundary. Safety-critical components require approved manufacturer, second-source strategy or a documented exception.

## Future extensions

Three-phase must be a new hardware platform/revision review; it is not a firmware configuration of the single-phase SKU.
