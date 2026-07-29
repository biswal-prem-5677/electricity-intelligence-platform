# 10 — BOM, Cost and Unit Economics

**Document ID:** EIP-10 · **Version:** 1.0 · **Status:** Planning estimate — every figure is an assumption until quoted · **Owner:** Product / Hardware
**Supersedes:** Cost & Manufacturing Analysis, cost sections of Commercialization

---

## 1. Reading rule

**Every number here is an assumption until a supplier quote or an invoice replaces it.** Figures are in INR, exclusive of GST unless stated. Cost work is done at three volumes because the answer changes shape between them: **100 (pilot) · 1,000 (V1 commercial) · 10,000 (scale)**.

## 1a. Fund by tier, not by total (R23)

> **₹41–94 lakh is the Commercial MVP figure. It is not the entry price, and treating it as "the budget" is the single most common way a hardware project fails to start.**

The first tier costs less than a used car and can kill the project. Each tier has its own go/no-go and its own plausible funding source. **You do not need tier 5 money to begin — you need tier 1 money and the discipline to stop if tier 1 fails.**

| Tier | What it buys | Phases | Cost | Cumulative | Ends at | Plausible source |
|---|---|---|---|---|---|---|
| **1 — Bench Prototype** | Eval board + off-the-shelf CTs + borrowed/hired reference meter. Answers *can we measure at all, and how far off are we?* Plus the DB survey and the classification opinion | 0 | **₹50k – 2 L** | ₹0.5–2 L | 🚦 **G0** | Self-funded |
| **2 — Engineering Prototype** | AFE benchmark, CT characterisation across ≥20 samples, full component selection, isolation review, backend/mobile skeleton | 1 | **₹3 – 8 L** | ₹3.5 – 10 L | 🚦 **G1** | Self / friends & family / small angel |
| **3 — EVT** | First custom PCB, 20 units, enclosure prototype, EMC pre-scan, first calibration procedure. Firmware and cloud build out alongside | 2, 3, 4 | **₹9 – 19 L** | ₹12.5 – 29 L | 🚦 **G2–G4** | Angel / pre-seed |
| **4 — DVT + Pilot** | PCB respin, 50 units, **traceable metrology campaign**, safety and EMC pre-compliance, 30-day soak, production fixture, 20–30 site pilot | 5, 6 | **₹13 – 28 L** | ₹25.5 – 57 L | 🚦 **G5–G6** | Seed, raised against Gate 1 evidence |
| **5 — Commercial MVP** | Full certification, enclosure tooling, PVT lot of 200–500, installer network, support and warranty infrastructure | 7 | **₹15 – 35 L** | **₹41 – 94 L** | 🚦 **G7** | Seed / revenue |

**Three consequences of reading it this way:**

1. **Tier 1 is self-fundable.** The most valuable task in the project — surveying 30 distribution boards — costs about ₹40,000 and needs a camera, a tape measure and persistence. No capital required.
2. **The cheap tiers carry the killing questions.** Do CTs fit? Can we measure? What does certification cost? All answered under ₹2 lakh. Tiers 4 and 5 spend the real money only after the fatal unknowns are retired.
3. **Raise against evidence, not against a deck.** Gate 1 (measured accuracy from selected parts) and Gate 5 (traceable metrology report + safety evidence) are the two moments when this project becomes materially more fundable. Time raises to land just after them.

**What you can build yourself versus what needs external capital:** tiers 1 and 2 are a competent hardware engineer with a bench. Tier 3 onward needs an EMS, a lab, and money that does not belong to you.

Salaries are excluded from every figure above and modelled separately in §7.

## 2. Prototype BOM — Circuit-8, quantity ~20 (EVT)

| Item | Qty | Est. unit ₹ | Est. line ₹ | Note |
|---|---|---|---|---|
| Metering AFE (6-ch class, ×2) | 2 | 350–600 | 700–1,200 | Pending §2 benchmark in [09](09_component_selection.md) |
| Wireless MCU module (pre-certified) | 1 | 250–400 | 250–400 | ETA-valid part |
| Secure element | 1 | 40–90 | 40–90 | SEC-001 |
| External flash 16 MB | 1 | 60–120 | 60–120 | FWR-009/010 |
| Split-core CTs (1 service + 8 branch) | 9 | 150–300 | 1,350–2,700 | **Dominant line item** |
| Isolated voltage reference + protection | 1 set | 150–350 | 150–350 | |
| Certified isolated AC/DC module | 1 | 150–300 | 150–300 | |
| Fuse, MOV, passives, connectors | 1 set | 200–350 | 200–350 | Keyed CT harness included |
| PCB (4-layer, prototype qty) | 1 | 300–700 | 300–700 | Falls sharply with volume |
| Enclosure (prototype / 3D print) | 1 | 200–600 | 200–600 | Tooled cost is far lower |
| Assembly (prototype) | 1 | 300–600 | 300–600 | |
| **Prototype unit total** | | | **≈ ₹3,700–7,400** | Excludes NRE |

**The CT stack is the cost story.** Nine CTs at ₹150–300 each is ₹1,350–2,700 — comparable to the entire rest of the board. Any serious cost reduction starts there.

### Volume trajectory (planning assumption only)

| Volume | Target BOM | Target landed cost incl. assembly, test, packaging |
|---|---|---|
| 100 | ₹3,000–5,500 | ₹4,500–7,500 |
| 1,000 | ₹2,000–3,200 | ₹2,800–4,300 |
| 10,000 | ₹1,400–2,300 | ₹2,000–3,000 |

## 3. NRE and one-time costs — the ones that get forgotten

| Item | Est. ₹ | When | Note |
|---|---|---|---|
| **Metrology bench / reference standard** | **8,00,000 – 25,00,000** | Phase 1 | Buy, rent or use a lab. **Without a traceable reference there is no accuracy claim.** Renting lab time is the cash-sensible option |
| Programmable AC source + load bank | 2,00,000 – 8,00,000 | Phase 1 | Can be partly substituted with lab hire |
| EMC pre-scan, **per hardware revision** | 75,000 – 2,50,000 | Phase 2–3 | Budget for **at least three** revisions |
| Full safety + EMC certification | 3,00,000 – 10,00,000 | Phase 6 | Scope set by the classification opinion |
| Classification opinion / regulatory consultant | 50,000 – 2,00,000 | **Phase 0** | Blocking input to everything downstream |
| PCB tooling, stencil, first articles | 50,000 – 1,50,000 | Phase 2 | |
| Enclosure tooling | 3,00,000 – 12,00,000 | Phase 6 | Defer until the design is field-proven |
| Production test fixture + calibration jig | 1,50,000 – 5,00,000 | Phase 5 | Required before any volume build |
| Product liability insurance | 50,000 – 2,00,000 /yr | Before first customer install | Non-optional |
| **Total pre-revenue NRE (realistic band)** | **₹25,00,000 – 70,00,000** | | Excludes salaries |

## 4. Recurring costs per unit and per month

| Item | Assumption |
|---|---|
| Cloud per device | ₹8–25/month at pilot scale, falling with volume. **Depends entirely on D-004 holding** — an always-on 1-second stream multiplies this by ~100× |
| Warranty reserve | **3–6% of revenue.** Set aside from unit one |
| RMA truck roll | ₹800–2,500 per incident, plus the replacement unit |
| Support | ₹100–400 per active site per month at pilot scale |
| Customer acquisition | ₹2,000–6,000 per customer for a first-time hardware brand |
| Installation (electrician) | ₹800–2,500 per site — **charge it, do not absorb it** |
| Firmware support commitment | 5-year obligation; ~0.5 FTE ongoing from launch |
| GST | 18% on sale, handled separately from margin |

## 5. Gross margin — computed honestly

> **A hardware gross margin computed without warranty, support and cloud is not a gross margin.**

Illustrative at 1,000 units, hardware sold at ₹12,000:

| Line | ₹ |
|---|---|
| Price (ex-GST) | 12,000 |
| Landed cost | (3,500) |
| Warranty reserve @4.5% | (540) |
| First-year cloud (₹15 × 12) | (180) |
| First-year support | (2,400) |
| Allocated CAC | (4,000) |
| **Contribution, year 1** | **≈ 1,380** |

Thin — and that is the point of writing it down. The levers are CAC, support load, and the CT stack. Installation is charged separately and is not a margin source. **This model must be replaced with pilot actuals before any pricing is committed publicly.**

## 6. Customer payback — the test that chose the segment

| Segment | Monthly bill ₹ | 8% saving ₹/mo | System cost ₹ (incl. install) | Payback |
|---|---|---|---|---|
| **Small commercial (beachhead)** | 15,000–45,000 | 1,200–3,600 | ~14,000 | **4–12 months** ✅ |
| Premium home | 2,500–6,000 | 200–480 | ~14,000 | 29–70 months ❌ |

This table is the reason for **Decision D-002** in [14_decision_log.md](14_decision_log.md). The 8% figure is an **assumption** carried from published behavioural-feedback literature; it must be replaced with the pilot's measured result (test §5 of [08](08_testing_validation.md)).

## 7. Team cost (3–5 people, 12–18 months)

| Role | Count | Note |
|---|---|---|
| Hardware / metrology | 1 | The critical hire. Nothing ships without this person |
| Embedded firmware | 1 | |
| Backend + cloud | 1 | Also owns deployment |
| Mobile | 0.5–1 | Can be contract for V1 |
| Product / founder (also does test, ops, sales) | 1 | |

Salary cost is deliberately not estimated here — it varies too much to be useful as a planning constant. Model it separately in the funding plan, then add the §3 NRE and a runway buffer of ≥6 months beyond the planned pilot date.

## 8. Cost reduction roadmap — in the order it becomes safe

1. **CT sourcing volume and dual-source negotiation** (largest single lever)
2. Reduce the AFE part count once the benchmark is settled
3. Panelisation and assembly optimisation with the EMS
4. Enclosure tooling once the mechanical design is field-proven
5. Cloud aggregation tier tuning after real ingest data exists

**Do not attempt any of these before DVT.** Cost-reducing an unvalidated design is how you pay for two sets of mistakes.
