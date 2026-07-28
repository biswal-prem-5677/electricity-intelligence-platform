# Hardware Architecture Decision

## Options compared

| Option | Accuracy / attribution | Installation | Safety & complexity | Cost | Verdict |
|---|---|---|---|---|---|
| Smart plug | Excellent per plug; misses fixed loads | Easy | Low mains design but limited coverage | Low per load | Companion only |
| Main-panel CT only | Accurate total; no circuit attribution | Electrician | Moderate | Low | Useful base channel, insufficient alone |
| Branch CT circuit monitor | Deterministic circuit attribution | Electrician | Moderate, scalable | Medium | **V1 primary** |
| DIN-rail per-circuit monitor | Good, professional modularity | Electrician / panel space | Higher wiring and SKU complexity | Medium-high | V2 commercial option |
| Hall sensor | DC and AC possible | Requires conductor integration | Offset/drift and design complexity | Medium | Not V1 |
| Shunt | High accuracy | Conductor interruption / thermal loss | High hazardous-domain engineering | Low part cost | Utility/controlled future only |
| NILM | Broad theoretical coverage | Electrician | Algorithm/data risk | Low hardware, high R&D | Supplement only |
| Hybrid CT + NILM | Strong measured foundation plus inference | Electrician | Higher software cost | Medium-high | Future roadmap |

## Recommended baseline

Home-8 uses one split-core CT on the service-live conductor and up to eight split-core CTs on labelled branch-live conductors. A shared, isolated voltage-reference path supplies the metrology front end. The MCU/radio and all accessible service interfaces remain in the SELV domain; V1 has no switching actuator.

The previous phrase “two mains CTs” is retired. It was ambiguous for a normal single-phase installation and could lead to invalid measurement if a CT were fitted around both live and neutral conductors. A second independent service feeder or solar channel is a future SKU decision, not a V1 requirement.

### Why CTs

CTs enable retrofit measurement without opening a conductor, provide galvanic separation at the sensor, avoid shunt heat loss, and map naturally to a circuit-labelling user experience. They still demand correct orientation, conductor placement, burden/protection design, phase alignment and calibration. A CT is not automatically accurate or safe merely because it clamps around a wire.

## Architecture block

    Service / branch conductors
              │
         Qualified CTs ──> protected analogue inputs ──> metering AFE ──> MCU
                                                                            │
    Isolated AC/DC supply ────────────────────────────────────────────────┼── Wi-Fi MQTT
                                                                            └── BLE commissioning

High-voltage, metrology, safety-isolation and SELV/radio zones must remain explicitly separated through schematic, PCB and enclosure reviews.

## Decision constraints

V1 is excluded if the panel lacks safe CT clearance, has inaccessible/unsafe conductors, has ambiguous neutral/phase topology, or cannot accommodate the approved enclosure. Site rejection is a safety outcome, not a failed sales outcome.
