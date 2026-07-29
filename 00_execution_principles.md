# 00 — Execution Principles

**Document ID:** EIP-00 · **Version:** 1.0 · **Status:** 🔒 **Constitutional — outranks every other document** · **Owner:** Founder

---

## How this document works

These are the project's constitution. Where any other document conflicts with a rule here, **this document wins** and the other document is wrong and must be corrected.

A rule is amended only by a logged decision in [14_decision_log.md](14_decision_log.md) with a named approver. **Amending a rule is a serious act.** Rules exist precisely to bind us on the day we would rather not be bound — the day the demo is close, the investor is asking, and the shortcut looks harmless.

---

## I. Sequence — the order work happens in

### R1 — Never build before validating.
Every build spends money to answer a question. Write the question down first. If you cannot state what the build will prove, you are not ready to build it.

### R2 — Run the cheapest killing experiment first.
Order work by *cost to disprove*, not by what is interesting. A ₹40,000 survey that can kill the project outranks a ₹4,00,000 board that assumes the project is alive.

### R3 — Measure before you model.
Rules before ML. Ground truth before inference. **No AI unless deterministic rules have been tried and demonstrably failed** — and "failed" means a recorded, specific inadequacy, not a feeling that AI would be more impressive.

### R4 — Prototype with parts you can buy today before designing parts you must make.
Eval boards, off-the-shelf CTs and a borrowed reference meter answer most physics questions for under ₹2 lakh. A custom PCB answers the same questions for twenty times the cost and three months later.

---

## II. Scope — what gets built

### R5 — Every feature answers three questions before it enters scope.
**Who benefits? · How much? · How is it measured?** A feature that cannot answer all three is not a feature; it is an opinion with a ticket number.

### R6 — Every feature has a success metric defined before implementation starts.
Written down, numeric, and falsifiable. Defining success after shipping is how everything ships successfully.

### R7 — No feature without measurable customer value.
Technical elegance is not customer value. Neither is competitive parity, nor "it's nearly free since we're in there anyway."

### R8 — If it does not help build, test, validate or launch V1, it moves to `future/`.
The governing rule. It applies to features, documents, meetings, integrations, refactors and research.

### R9 — Any addition to V1 scope must name what it removes.
Recorded in [12_progress_tracker.md](12_progress_tracker.md) §9. An addition with no subtraction is a schedule slip that has not been admitted yet.

### R10 — Parking is not rejecting.
A parked capability keeps its rationale, its prerequisites and its unparking gate, so nobody has to reconstruct the thinking later. See [future/README.md](future/README.md).

---

## III. Evidence — what counts as true

### R11 — Every assumption must eventually become evidence.
Assumptions are permitted, but each one is a labelled debt with a due date and an owner. An assumption that has been repeated often enough to sound like a fact is the most dangerous object in the project.

### R12 — Every requirement must be testable.
If you cannot describe the test, you have not written a requirement — you have written an aspiration. Every requirement maps to at least one test case in [08_testing_validation.md](08_testing_validation.md).

### R13 — A requirement without current evidence is open.
An informal bench demonstration does not close it. Neither does a demo, a screenshot, or a confident engineer.

### R14 — No published claim without the HWR-007 checklist.
Topology, part number, calibration method, envelope, reference standard. **"99% accurate" is prohibited** — it is a marketing phrase wearing a metrology costume.

### R15 — Suppress rather than guess.
When calibration, clock quality or measurement quality is invalid, output nothing. A wrong number displayed confidently costs more trust than a gap does.

---

## IV. Change — how decisions hold

### R16 — No hardware change without an Architecture Decision Record.
The architecture is frozen ([03_hardware_architecture.md](03_hardware_architecture.md)). Reopening it requires a logged decision naming the approver, the schedule impact and the cost impact. **"It would be better if…" is not grounds. A critical engineering or safety problem is.**

### R17 — Decisions are written down with what was rejected and what reversal costs.
A decision recorded without its rejected alternatives will be relitigated by the next person who joins, including you in six months.

### R18 — No document without implementation purpose.
Every file must let someone do something: select a component, write code, start a schematic, stand up a service, or run a test. **A document that only informs belongs in `future/`.**

---

## V. Safety and honesty — the rules with no waiver path

### R19 — Safety gates have no waiver path.
No engineering build enters a customer panel before the hazard review approves the panel type, installation method, enclosure, line protection, installer PPE, work instruction and site rejection criteria. Schedule pressure is not an input to this decision.

### R20 — Reject the site rather than improvise.
A rejected installation is a safety outcome, not a failed sale. An installer who improvises at a panel is the project's largest uninsured liability.

### R21 — The device measures. It does not protect, and it does not diagnose.
No relay, no control path, no safety conclusion, in hardware or in software. This is architectural (F6), not a product preference.

### R22 — Report what happened, not what was hoped.
A pilot that produces no verified saving is a product answer, not a marketing problem. Progress is reported as evidence stage, never as a percentage.

---

## VI. Money

### R23 — Fund by tier, not by total.
The project has five funding tiers with separate go/no-go points. **₹41–94 lakh is the Commercial MVP figure, not the entry price.** The first tier costs under ₹2 lakh. See [10_bom_and_cost.md](10_bom_and_cost.md) §1.

### R24 — Cost-reduce only what has been validated.
Optimising an unproven design means paying for two sets of mistakes.

---

## VII. The current phase

> **Planning is frozen. The project is in Implementation Engineering.**

The bottleneck is no longer *what should we build* — it is *how do we build the first working unit*. Under **R18**, from this point forward every new file must be an implementation artifact:

| Order | Artifact | Who must be able to act on it |
|---|---|---|
| 1 | Component selection with exact part numbers, alternates, lifecycle status, suppliers | A buyer can raise a purchase order |
| 2 | Hardware block diagrams | A hardware engineer can partition the design |
| 3 | Electrical schematics | A PCB designer can start layout |
| 4 | PCB design | A fab can quote and build |
| 5 | Firmware implementation | A firmware engineer can write code against a defined interface |
| 6 | Cloud backend | A backend engineer can create services against a defined contract |
| 7 | Mobile application | A mobile engineer can build screens against a defined API |
| 8 | Bench testing | A tester knows exactly what to verify and against what |
| 9 | Field pilot | An electrician can install and a customer can use it |

**No further planning documents will be written.** The 15 core documents are the complete planning set; they are now maintained, not extended.

---

## The one-line version

**Measure before you claim. Validate before you build. Park anything that does not ship V1.**
