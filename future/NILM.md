# Future — Non-Intrusive Load Monitoring (NILM)

**Status:** Research track. V3+ at the earliest. Explicitly not a dependency of any shipped release.

## The idea

Infer individual appliance identity and consumption from a single high-rate measurement point (typically the service conductor), without a sensor per appliance. If it worked reliably it would remove most of the installation cost and complexity from the product.

## Why it is parked, stated plainly

1. **Attribution certainty is the product.** V1 sells a measured, electrician-labelled circuit. NILM sells a probabilistic guess. Mixing them without a visible confidence boundary destroys trust in the measured data, which is the only asset we have.
2. **The dataset transfer problem is real.** Models trained on published datasets (largely US/EU appliance populations and 60 Hz or different appliance mixes) generalise poorly to Indian homes and small commercial sites — different appliance classes, heavy inductive loads, poor power quality, wide voltage excursions.
3. **The market has already voted.** Sense built the best consumer appliance-detection UX in the world, at a high sampling rate, with serious data science, and has moved away from selling panel monitors toward utility smart-meter partnerships. That is the strongest available evidence that NILM-as-a-consumer-hardware-business is harder than it looks.
4. **It needs ground truth we do not have.** Training and validating NILM requires directly measured per-appliance data — which is exactly what our circuit CTs and (future) smart plugs produce. **We must ship the measured product first in order to earn the dataset that makes NILM possible.**

## The strategic point

NILM is not a shortcut around the hardware. It is a downstream product of having shipped the hardware. Our circuit-level installed base is the moat that makes a credible Indian NILM model possible later; nobody else will have labelled Indian circuit-level data.

## Prerequisites before any NILM work is funded

| Prerequisite | Detail |
|---|---|
| Installed base | ≥1,000 sites with ≥6 months of continuous circuit data |
| Ground truth | ≥100 sites with per-appliance direct measurement (smart plugs — see [Future_Hardware.md](Future_Hardware.md)) |
| Sampling capability | A hardware revision proven to capture the required waveform features, with the storage and bandwidth cost modelled |
| Evaluation protocol | Per-appliance-class precision/recall, calibration curves, and a published false-identification limit |
| Correction UX | The customer can correct a wrong identification, and the correction demonstrably improves the model |
| Claim review | Written boundary on what may be said in marketing. "Detected" and "estimated" are different words with different legal weight |

## Interim capability that is honest and cheap

Before any inference model, the product can already answer most of what customers want NILM for, using measured circuits plus simple rules:

- "This circuit ran for 6 hours after closing time."
- "This circuit's overnight floor rose 0.4 kW versus its own baseline."
- "Your unmonitored residual load is 220 W and never goes to zero."

Ship that. It requires no research, carries no false-identification risk, and is defensible.
