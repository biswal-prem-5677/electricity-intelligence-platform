# 01 — Project Definition

**Document ID:** EIP-01 · **Version:** 1.0 · **Status:** Baseline for V1 execution · **Owner:** Product
**Supersedes:** Project Overview, Product Definition Document, Market Research, Competitor Analysis, Product Family Strategy

---

## 1. The product in one paragraph

**Circuit-8** is an electrician-installed electricity monitor for single-phase Indian sites. It clamps one CT on the service conductor and up to eight CTs on labelled branch circuits, measures them continuously, and shows the owner where their electricity actually goes — circuit by circuit, in near real time, with a cost estimate and evidence-backed alerts. It measures. It does not estimate, guess, switch loads, or replace the utility meter.

## 2. The problem

A monthly bill arrives after the opportunity to act has passed. The utility meter reports a total for billing; it does not attribute usage to circuits, explain an unusual load, correlate a pattern to a tariff window, or give anyone a workflow when something looks wrong. Existing consumer products either rely on opaque appliance inference, assume a foreign panel style, or cannot be installed and supported in India.

## 3. Who we sell to — and why this changed

> **Decision D-002: the beachhead is single-phase small commercial, not premium homes.**

The independent feasibility review established that the original consumer-home target does not clear a payback test. The hardware does not change. The customer does.

| | Small commercial (shop, restaurant, clinic, small office, PG/hostel) | Premium urban home |
|---|---|---|
| Typical monthly bill *(assumption — validate in Phase 0)* | ₹15,000–45,000 | ₹2,500–6,000 |
| 8% saving | ₹1,200–3,600/month | ₹200–480/month |
| Payback on ~₹14,000 installed | **4–12 months** | **29–70 months** |
| Buying logic | Operational — "my cold room ran all night" | Curiosity — decays after 90 days |
| Referral density | High: one market street, ten similar buyers, clustered installer routes | Low and scattered |
| Verdict | **V1 target** | Secondary; sell to it only if the pilot proves willingness to pay |

Savings percentages are drawn from published real-time-feedback literature (typically 5–12% with decay) and are an **assumption until the V1 pilot measures our own number.** All bill figures are assumptions to be replaced by 20 real invoices per segment in Phase 0.

### Segment map

| Segment | V1 fit | Constraint |
|---|---|---|
| Small retail, restaurants, clinics, offices — **single-phase** | **Primary** | Must confirm single-phase service and CT clearance at survey |
| PGs, hostels, small institutions | Primary–secondary | Procurement cycle; multi-tenant reporting is V2 |
| Premium urban homes | Secondary | Weak payback; sell only where the customer values visibility for its own sake |
| Three-phase commercial | **Excluded from V1** | New hardware platform — [future/Three_Phase.md](future/Three_Phase.md) |
| Hospitals, industry, DISCOM | Excluded | Certified 3-phase, SLA and procurement capability we do not have |

### The person who buys and the person who installs

| Role | Pain | What V1 gives them |
|---|---|---|
| Shop/restaurant owner | Refrigeration, HVAC or signage runs after closing; bill jumped and nobody can say why | Circuit-level evidence, after-hours alerts, cost estimate |
| Site/facility owner | Cannot verify whether a behavioural change worked | Before/after circuit history |
| Electrician/installer | No standard way to commission or support an energy monitor | QR provisioning, guided checklist, diagnostics, remote support |

## 4. Positioning and USP

**India-first, electrician-installed circuit measurement with transparent logic** — not a promise that AI can identify every appliance from one waveform.

Three claims we make, and can prove:
1. **Measured, not inferred.** A labelled circuit is the source of truth.
2. **Transparent cost logic.** Every rupee shown traces to kWh × a tariff version the customer configured.
3. **It keeps working when the internet doesn't.** Seven days of local buffering, monotonic energy counters, honest freshness indicators.

## 5. Competitive position

| Category | Attribution | Install | V1 relationship |
|---|---|---|---|
| **Emporia Vue** (US$99.99 / $149.99 / $199.99 for mains-only/8/16, ±2% stated) | Measured circuit | Panel electrician | Closest architectural analogue. Landed in India with duty and GST it is ~₹19–20k, so we are cheaper. Benchmark its package clarity, not its panel assumptions |
| **Sense** | Inferred appliance | Panel electrician | **Has moved away from selling panel monitors toward utility partnerships.** Read as a demand signal about the consumer segment, not just an architecture lesson |
| **Shelly Pro 3EM** | Measured, DIN | Electrician | Valuable form-factor reference for Site-3P |
| **Schneider (Wiser 6-channel), Siemens SENTRON, Legrand DIN** | Measured | Professional | Validates the circuit-monitor category in India. We cannot beat them on channel or brand; we beat them on small-site insight, price and support |
| **Sonoff, Tuya, TP-Link, Xiaomi, Wipro, Syska, Havells** | Per-plug only | DIY | Fragmented plugs cannot produce a whole-site circuit view |
| **DISCOM smart meters (RDSS: 20.33 crore sanctioned, 4.93 crore installed as of Dec 2025)** | Total only | Utility-led | **A genuine substitution threat to total-consumption visibility.** Our defensible line is circuit attribution. Coexist; never assume access to their data |

**Learn:** concise live UX, expandable channel strategy, installer documentation, non-alarmist alerts.
**Do not copy:** foreign-panel assumptions, cloud lock-in without export, unexplained AI labels, DIY encouragement for mains work, or revenue-grade wording without product-specific evidence.

Competitor pricing is recorded as a **validation status**, not a scraped number. Before any pricing decision, capture SKU, country, tax, channel, installation and subscription on the same date.

## 6. Design principles

1. **Measurement before inference.** A labelled CT is truth; analytics may enrich, never overwrite.
2. **Safety before feature count.** No V1 switching. No DIY mains work. Fail-safe monitoring behaviour.
3. **Explainable advice.** Every recommendation links to a measurable pattern with its window, baseline and limitation.
4. **Offline tolerance.** Keep measuring and buffering when the internet drops.
5. **No subscription to see your own data.**
6. **Privacy by design.** Collect only what the function requires; treat occupancy-revealing data as sensitive.
7. **Execution rule.** *If a document, feature or task does not directly help us build, test, validate or launch V1, it must not block V1 — it moves to [future/README.md](future/README.md).*

## 7. Product boundaries — what we never claim

The device shows an **estimated** tariff cost. The utility invoice and utility meter remain authoritative. Circuit-8 is not a revenue-grade meter, not a protection device, not an MCB/RCCB replacement, not a fault-diagnosis tool, and not a substitute for statutory energy accounting. No recommendation may promise a guaranteed saving. No accuracy figure may be published outside a defined, tested envelope.

## 8. Pricing hypothesis

| Offer | Test price (excl. GST) | Decision gate |
|---|---:|---|
| Hardware only, Circuit-8 | ₹8,999–₹11,999 | 30 paid pilot installs |
| Installed package | ₹11,999–₹16,999 | Installer duration and NPS |
| Power-quality report add-on | ₹99–₹199/month, V1.1 | Only after core monitoring works without any subscription |

GST at 18% and a payment fee of ~2–2.5% sit on top of these figures and are part of what the customer feels. Target mature blended gross margin ≥45% **after** warranty, cloud, support, payment fees and channel margin — see [10_bom_and_cost.md](10_bom_and_cost.md).

## 9. Success metrics for V1

| Metric | Pilot target | Guardrail |
|---|---:|---|
| Successful electrician commissioning | ≥95% | No safety-checklist bypass |
| Devices reporting ≥95% of expected 5-min intervals | ≥98% | Planned maintenance excluded transparently |
| System active-energy error vs. traceable reference | Within the published spec ([03](03_hardware_architecture.md) §4) | Never extrapolated to revenue billing |
| Alert precision after tuning | ≥80% | Median ≤2 alerts/site/week |
| **Verified customer payback period** | **<18 months in ≥1 segment** | Measured, not modelled |
| 90-day hardware return rate | <2% | Root-cause every safety-related return |

**North Star:** verified energy decisions per active site per month. A decision counts only when the customer confirms it, or a measurable post-action reduction persists after normalisation.

## 10. Market sizing approach

Do not publish a top-down market number. Build bottom-up: TAM = eligible single-phase small-commercial premises × addressable share × annual hardware-and-service value; SAM = TAM × launch cities × qualified-installer availability; SOM = SAM × channel conversion × support capacity. **The first commercial target is 1,000 paid monitored sites**, not a percentage of a speculative forecast.

## 11. Roadmap

| Release | Contents |
|---|---|
| **V1 — Circuit-8** | Single-phase circuit monitoring, cost estimate, rule-based alerts, power-quality/outage log, installer workflow |
| V1.1 | Circuit-16, bill forecast, budget tracking, carbon estimate, neutral/earth-imbalance channel, power-quality report as a product |
| V2 | Site-3P, solar import/export, multi-site, Pro tier, billing verification |
| V3+ | Fault candidates, circuit health, NILM research, generative assistant |

Everything past V1 lives in [future/](future/) and is governed by [future/README.md](future/README.md).
