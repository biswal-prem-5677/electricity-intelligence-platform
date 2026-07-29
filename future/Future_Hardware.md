# Future — Hardware Beyond Circuit-8

**Status:** Parked. The V1 architecture is frozen (see [../14_decision_log.md](../14_decision_log.md), D-001). Nothing in this document may be added to V1.

## The portfolio, sequenced

The original plan called for four products built as a family. Building them in parallel is how small hardware teams die. They share identity, data model, cloud, mobile experience and installation standards — but their mains design, switching risk and certification evidence are entirely independent.

| SKU | Job | Status | Why not V1 |
|---|---|---|---|
| **Circuit-8** | Single-phase circuit monitoring, 1 service + 8 branch CTs | **V1 — shipping** | — |
| Circuit-16 | Same, 16 branch channels | V1.1 | Trivial extension once Circuit-8 metrology is proven; gated on channel-cost evidence |
| Plug-16 | Appliance-level visibility for movable 6/10/16 A loads | V2 discovery | Thermal design, Indian socket quality variance, and switching risk are all underrated. High return rate risk |
| Switch-16 | Behind-switch module for light/fan/socket circuits | Research | Neutral is frequently unavailable behind Indian wall switches. Installation depth, local-control behaviour and relay safety vary by site. No universal claim is possible |
| Site-3P | Three-phase commercial | V2 — see [Three_Phase.md](Three_Phase.md) | Separate hardware platform |

## Load switching / relay

**Deliberately absent from V1, and this is the single most valuable deletion in the project.** A relay converts a monitoring appliance into a device that can fail closed, fail open, weld, or interrupt a customer's refrigeration. It changes the liability model, the certification scope, the warranty exposure and the support cost.

Before any relay ships, a separate safety case is required covering: contact rating with margin, weld detection, manual override, abnormal-state analysis, fail-safe default on firmware fault, behaviour on brownout and on OTA interruption, and an explicit customer agreement about what the device is permitted to switch. None of this is a firmware feature.

**Rule:** monitoring evidence comes first. If customers do not value knowing, they will not value switching.

## Neutral / earth-imbalance channel

**Priority candidate for V1.1.** One additional CT on the neutral conductor allows computation of the live-neutral imbalance, which approximates earth leakage.

Why it matters in India: variable wiring quality and endemic RCCB nuisance-tripping mean "your geyser circuit's leakage has grown from 4 mA to 18 mA over six weeks — have an electrician check it" is a genuinely differentiated insight that no competitor in our comparison set markets.

Constraints: it must be framed strictly as **monitoring insight, never protection**. The device is not an RCCB and must never imply it replaces one. It requires its own hazard review, an accuracy statement at low current, and careful wording review before any customer-facing claim.

## Solar, battery and EV

Requires bidirectional measurement (the V1 channel record already carries a `direction` field for this reason), plus generation-side CT placement, export-tariff modelling, and — for any optimisation feature — a control safety case. V2+ at the earliest.

## Hardware ideas explicitly rejected, with reasons

| Idea | Why rejected |
|---|---|
| Hall-effect current sensing | Offset and thermal drift make the accuracy target harder, not easier, for no installation benefit over CTs |
| Shunt current sensing | Requires interrupting the customer's conductor, adds thermal loss, and puts the measurement front end in the hazardous domain. Utility/controlled contexts only |
| Direct high-impedance resistive divider into a hazardous-domain AFE | Would place hazardous potential adjacent to the MCU, radio and service interfaces. Rejected in favour of an isolated voltage-reference path. Any revisit requires a written isolation architecture, component ratings, fault analysis and safety-review approval |
| CT clamped around live + neutral together | The opposing fields cancel; the reading is invalid. This is an installer training point, not a design option |
| DIY / customer self-installation | Mains distribution-board work is not a consumer activity in this market |
