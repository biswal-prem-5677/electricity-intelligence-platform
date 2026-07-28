# AI System SRS

## Purpose and scope

Defines analytics that transform measured, consented telemetry into ranked, explainable insights. AI never becomes a measurement source, electrical diagnosis, or automatic actuator in V1.

## Architecture and interfaces

Validated aggregate telemetry → feature pipeline → versioned rule/model service → insight store → API/notification. Model registry, data lineage, evaluation report and rollback record are mandatory interfaces.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| AIR-001 | Establish a site/circuit baseline only after sufficient valid data. | Insight is suppressed and reason shown when coverage is inadequate. |
| AIR-002 | Produce tariff-aware cost estimate using versioned customer inputs. | Recalculation shows tariff version and does not alter measured kWh. |
| AIR-003 | Attach evidence, confidence and safe action language to every insight. | Review sample contains all fields and no diagnostic/guaranteed-savings claim. |
| AIR-004 | Track feedback, false positives and dismissals. | Feedback is tenant-scoped and enters a monitored evaluation dataset. |
| AIR-005 | Version, monitor and roll back rules/models. | Canary regression causes automatic halt or operator rollback. |

## Non-functional requirements

Offline evaluation must report precision, recall, calibration and cohort coverage; online monitoring must detect input drift, missing data and alert-volume regression. Sensitive data use follows the approved privacy inventory. A human owner approves production promotion.

## Future extensions

Forecasting, health trends, NILM and a generative assistant require separate safety/evaluation cards and may not inherit V1 approval.

