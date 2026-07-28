# Cost and Manufacturing Analysis

## Cost model

These are **low/medium-confidence planning estimates** for a Home-8 monitor in INR, excluding GST, installer labour and warranty reserve. They are not supplier quotes.

| Cost group | EVT prototype (1–10) | Pilot (100–500) | 5k annual run | Basis / risk |
|---|---:|---:|---:|---|
| Electronics + PCB assembly | 4,500–7,500 | 2,600–3,800 | 1,750–2,500 | CT, metering IC, radio, PSU; FX and yield risk |
| Enclosure, harnesses, labels | 900–1,500 | 550–900 | 350–600 | Tooling excluded |
| Test, calibration and packaging | 700–1,200 | 350–650 | 220–400 | Fixture and takt-time dependent |
| Landed COGS | 6,100–10,200 | 3,500–5,350 | 2,320–3,500 | Includes 8–12% scrap/handling |
| Installer labour (customer-facing) | 1,500–3,500 | 1,200–2,500 | 1,000–2,000 | City/panel dependent |

## Commercial price hypothesis

| Offer | Suggested test price | Why | Decision gate |
|---|---:|---|---|
| Hardware only, Home-8 | ₹8,999–₹11,999 | Leaves room for channel margin and support | 30 paid pilot installs |
| Installed package | ₹11,999–₹16,999 | Customer buys an outcome, not a risky component kit | Installer duration and NPS |
| Optional Pro analytics | ₹99–₹199/month later | Only after core monitoring works without subscription | ≥25% voluntary conversion |

Target mature blended gross margin before R&D is ≥45%; it must fund warranty, cloud, payment fees, channel margin and support. Never use prototype cost as a retail-price basis.

## Non-recurring costs

| Item | Planning range | Stage | Notes |
|---|---:|---|---|
| Electronics/mechanical engineering | ₹25–60 lakh | EVT–DVT | Team/time dependent |
| Tooling and fixtures | ₹8–25 lakh | DVT–PVT | Enclosure and production test |
| Pre-compliance and formal tests | ₹10–35 lakh | DVT–PVT | Scope depends on radio, safety and claims |
| Certification / compliance programme | ₹15–75 lakh+ | Commercial | Get laboratory/consultant quote after classification |
| Pilot operations and support | ₹8–20 lakh | Pilot | Installer training, replacement pool, travel |

BIS laboratory listings show that type-test components can themselves be substantial; certification scope must be confirmed by a qualified Indian compliance advisor before a schedule or public claim is made. [BIS LIMS example scope](https://lims.bis.gov.in/home_lab_scope/328/)

## Strategy decision

Prototype on an evaluation-quality/custom PCB; pilot on a contract-manufactured custom PCB; commercialise through an Indian EMS partner with controlled test fixtures. An OEM/white-label product can accelerate channel learning but weakens metrology, security and service differentiation. Use it only for a separately branded market test, never as evidence that the custom product is qualified.

## Cost reduction roadmap

1. Do not reduce protection, isolation, CT qualification or production test.
2. Consolidate SKU components after field evidence.
3. Move harness/CT sourcing to dual qualified vendors.
4. Automate calibration and final functional test.
5. Negotiate at annual forecast/blanket-PO level only after yield is stable.

## Founder expense plan: separate the stages

The following ranges are planning assumptions for India, exclude founder salary, GST and working capital, and must be replaced by current RFQs before approval. They prevent the common error of comparing a breadboard cost with a commercial-product cost.

| Expense category | Learning / student prototype | Investor-ready EVT/DVT prototype | Commercial launch preparation |
|---|---:|---:|---:|
| Research, teardown and site survey | ₹20,000–₹60,000 | ₹1–4 lakh | ₹3–8 lakh |
| Test equipment and calibration | ₹25,000–₹80,000 | ₹1.5–6 lakh | ₹4–12 lakh |
| PCB iterations and assembly | ₹10,000–₹40,000 | ₹4–15 lakh | ₹12–35 lakh |
| Enclosure / mechanical | ₹5,000–₹20,000 | ₹2–8 lakh | ₹8–25 lakh plus tooling |
| Firmware, cloud and mobile | self-developed time; cloud typically <₹2,000/month | ₹8–30 lakh plus ₹10k–₹75k/month pilot operations | ₹25 lakh+ depending on team and SLA |
| Safety, EMC and reliability | ₹20,000–₹1 lakh+ | ₹5–20 lakh | ₹15–75 lakh+ including classification/certification scope |
| Pilot build, install and support | not applicable | ₹8–25 lakh for 50–100 controlled sites | working capital based on forecast |

These ranges are deliberately broader than the unit-cost table because scope, test fixtures, certification classification and staffing dominate actual spend. “BIS certified” is a commercial claim only after the exact product classification, standard, laboratory route and evidence are approved.

## Illustrative BOM evolution: not a quote

| Stage | Approximate electronics BOM / unit | Interpretation |
|---|---:|---|
| Breadboard / development modules | ₹2,000–₹3,000 | Learning platform, not saleable or safety-qualified |
| Low-volume prototype PCB | ₹1,200–₹2,500 | Depends strongly on CT count, isolated PSU and test hooks |
| 100-unit pilot | ₹1,000–₹1,900 | Requires supplier quotes; exclude assembly, scrap and warranty |
| 1,000+ units | ₹750–₹1,500 | Only after qualification, yield and forecast support sourcing |

The prior Home-8 landed-COGS model remains the decision basis because it includes assembly, enclosure, test and scrap. This BOM table is included solely to distinguish engineering learning cost from product economics.
