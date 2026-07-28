# Manufacturing SRS

## Purpose and scope

Defines controlled build, traceability, test, quality and change-control requirements from pilot units through commercial manufacturing.

## Architecture and interfaces

Approved vendor list → incoming inspection → PCB assembly → programming/calibration → functional/safety test → final inspection → serialisation → packing → shipment/RMA feedback. MES or controlled production records tie serial, BOM revision, firmware, calibration, operator and test result.

## Requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| MFG-001 | Build only released BOM/PCB/firmware revisions. | Traveller blocks undocumented substitution. |
| MFG-002 | Assign unique serial and retain unit genealogy. | Serial query returns lot, supplier/date code, calibration and final-test record. |
| MFG-003 | Test every unit for identity, metrology sanity, radio, safety checks and firmware signature. | Test fixture result is stored and failed unit is quarantined. |
| MFG-004 | Calibrate using controlled fixture/reference and protect coefficients. | Sample audit meets HWR accuracy requirement. |
| MFG-005 | Apply incoming inspection, ESD, workmanship and nonconformance process. | Supplier/line CAPA closes within defined SLA. |

## Non-functional requirements

Process capability, yield, rework, burn-in policy and sampling plan are established from pilot data. No safety-critical or metrology-critical substitute is accepted without engineering and compliance approval.

## Future extensions

OEM/white-label projects have separate serial ranges, configuration signing and support obligations; they never silently share customer telemetry.

