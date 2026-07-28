# Assumptions, Traceability and References

## A. Controlled assumptions

| ID | Assumption | Why it is used | Validation owner / exit criterion |
|---|---|---|---|
| A-01 | India, 230 V nominal, 50 Hz, single-phase homes are the first configuration | Largest practical initial SKU; avoids three-phase complexity in V1 | Product: confirm launch states and addressable panels |
| A-02 | V1 is one 63 A nominal service-live CT plus 8 branch-live CT channels; any 100 A maximum claim is deferred pending CT, thermal and installation evidence | Covers many homes/small shops while allowing compact CTs | Hardware: thermal, CT linearity and installation survey |
| A-03 | Customer has 2.4 GHz Wi-Fi at the distribution board or can use a qualified extender | Lowest recurring connectivity cost | Pilot: measure successful commissioning rate ≥95% |
| A-04 | Installation is performed by a qualified electrician | Mains-panel work is hazardous | Operations: signed installation SOP and installer training |
| A-05 | Target is insight, not utility billing | Avoids unearned revenue-grade claim and utility integration | Legal/compliance: claim review |
| A-06 | INR costs are planning estimates excluding GST unless stated | Distributor pricing, FX and volumes move | Supply chain: request RFQs before design freeze |
| A-07 | Data can leave India only after privacy/legal approval; default production region will be India when available | Conservatively supports customer trust and enterprise procurement | Legal/DPO: data map and contracts |

Unvalidated assumptions are not requirements. Each is carried into pilot exit review.

## B. Requirement identifiers

Requirements use the prefix below. Every implementation, test and release claim must reference an identifier.

| Prefix | Meaning |
|---|---|
| PRD | Product requirement |
| HWR | Hardware requirement |
| FWR | Firmware requirement |
| CLR | Cloud/backend requirement |
| AIR | AI/analytics requirement |
| MOB | Mobile requirement |
| ADM | Admin requirement |
| TST | Verification requirement |
| SEC | Security requirement |
| MFG | Manufacturing requirement |
| OPS | Operations requirement |

## C. Evidence hierarchy

1. Applicable law, regulation, certification scheme, or signed customer contract.
2. Current standard and accredited-laboratory test report.
3. Manufacturer datasheet and controlled component qualification.
4. Reproducible internal test result with calibrated equipment.
5. Market research, vendor claim, or interview.

Marketing claims require evidence levels 1–4; level 5 is hypothesis only.

## D. External facts used in this suite

- India has a large and active smart-meter deployment programme: a December 2025 Ministry of Power release reported 20.33 crore smart meters sanctioned under RDSS and 4.93 crore installed nationally. This validates market awareness, but utility smart meters are not a shortcut to an approved consumer monitoring product. [Ministry of Power / PIB](https://www.pib.gov.in/PressReleasePage.aspx?PRID=2202459&lang=1&reg=3)
- CEA maintains the governing meter installation-and-operation regulations. The legal status and applicability of any 2026 amendment must be confirmed from the final official/Gazette publication before it is relied on. Any product marketed as a meter or connected to a utility-metering workflow requires a dedicated compliance assessment. [CEA meter regulations](https://cea.nic.in/regulations-category/metering-regulations/?lang=en)
- India lists smart meters under IS 16444 Part 1 and Part 2 in the compulsory certification scheme. This suite therefore avoids claiming a consumer monitor is compliant merely because it uses a metering IC. [BIS Scheme I listing](https://www.bis.gov.in/product-certification/products-under-compulsory-certification/scheme-i-mark-scheme/?lang=tm)
- Emporia demonstrates the market value of circuit-level measurement, offering 8/16 branch-monitor variants and stating ±2% monitoring accuracy; this is a benchmark, not a target copied into our claim. [Emporia Vue](https://www.emporiaenergy.com/energy-monitors/)
- Sense's former panel monitor used high-rate whole-home sampling but is moving toward utility smart-meter partnerships, reinforcing that device inference and circuit measurement are different product strategies. [Sense support update](https://help.sense.com/hc/en-us/articles/46109574810131-Support-For-Sense-Home-Energy-Monitors)
- ADE9153A is a viable single-phase metering IC candidate, with stated Class 1/2 support via autocalibration when used within its documented design constraints; it does not remove system-level validation. [Analog Devices product page](https://www.analog.com/en/products/ade9153a.html)
- TI publishes current-transformer and standalone-ADC e-meter reference designs reaching high accuracy classes. These are technical evidence for the sensor trade-off, not a certification result for this product. [TI smart-meter resources](https://www.ti.com/applications/industrial/energy-infrastructure/smart-meter/overview.html)

## E. Cost notation and confidence

| Confidence | Definition |
|---|---|
| Low | Conceptual estimate, ±50%; do not commit or quote |
| Medium | Two independent market/distributor references or a preliminary supplier quote, ±25% |
| High | Current written RFQ/contract at stated MOQ, ±10% |

All financial values in this suite are medium or low confidence until a controlled RFQ is attached.
