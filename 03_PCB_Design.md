# Phase 3 — PCB Schematic, Layout and Assembly Plan

## Schematic planning

Partition the schematic into: AC entry/protection, isolated power, voltage sensing, CT channels, metering AFE/ADC, MCU/radio, programming/debug, indicators and manufacturing test. Every connector pin, net class and domain boundary must be labelled. A schematic review cannot close until it verifies fault path, isolation path, component ratings, CT open-circuit behaviour, test access and power-up/down sequence.

## Placement and routing rules

| Area | Rule |
|---|---|
| Hazardous input | Place fuse/protection/AC connector together; minimise unprotected trace length |
| Isolation barrier | Maintain calculated clearance/creepage on every layer; no silkscreen or copper shortcut; document slots |
| Metering analogue | Keep CT/voltage paths short and symmetric where needed; isolate from switching supply/RF return currents |
| RF | Follow module reference keep-out, antenna clearance and matching guidance; no metal/enclosure obstruction without test |
| Power | Size traces/vias for fault and continuous current; do not use thermal relief where safety current requires solid connection |
| Ground | Use intentional return paths; do not join hazardous and SELV grounds except through the approved architecture |

## Stack-up, signal integrity and test

Default to a 4-layer stack-up with manufacturing-controlled dielectric data. Layer assignment is a design output, not a rule of thumb: protect metrology return paths, give RF a solid reference, and route noisy switching currents compactly. SPI/clock lines require source termination or edge-rate control only when simulation/measurement warrants it. Provide labelled test points for rails, reset, programming, metrology reference, CT/voltage inputs, production UART and fixture ground; unsafe-domain points require guarded fixture instructions.

## DFM/DFA and assembly

Use panelisation, fiducials, tooling holes, paste apertures, component spacing and polarity markings agreed with the EMS. Include AOI/X-ray requirements for packages where visual inspection is inadequate. Generate controlled Gerber/ODB++, drill, fab drawing, assembly drawing, centroid, BOM, schematic PDF, pick-and-place, programming image and revision manifest. First articles receive 100% visual inspection and electrical fixture test.

## Release checklist

Run ERC/DRC, 3D interference check, impedance/stack-up signoff, creepage/clearance audit, thermal review, assembly review, test-fixture review, and independent schematic/layout safety review. No routing exception is accepted verbally; every exception has a drawing location, calculated rationale, approver and revalidation trigger.

