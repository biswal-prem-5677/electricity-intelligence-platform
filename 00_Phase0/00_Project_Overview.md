# Project Overview

## Vision and mission

Electricity Intelligence Platform turns safe electrical measurements into clear, actionable decisions for homes and small businesses. Its mission is to reduce avoidable electricity cost, reveal abnormal energy use early, and establish a trustworthy measurement foundation for future energy services.

## Problem statement

Most customers receive a monthly aggregate bill after the opportunity to act has passed. A utility meter reports energy for billing; it does not usually attribute usage to circuits, explain unusual load, correlate patterns to tariff windows, or provide a service workflow for a suspected fault. Existing consumer products often rely on opaque appliance detection, assume a foreign panel style, or are impractical to install/support in India.

## Target users and pains

| User | Primary pain | V1 value |
|---|---|---|
| Home owner | Cannot identify high-use circuits or verify behavioural changes | Live circuit dashboard, historical cost estimate, abnormal-use alert |
| Tenant/landlord | Shared or unexplained load disputes | Read-only reporting and time-stamped circuit evidence |
| Small shop/restaurant owner | Refrigeration, HVAC or signage load runs after hours | Schedule-baseline exception alert |
| Installer | Has no standard way to commission/support energy monitors | QR provisioning, installation checklist and diagnostics |
| Facility manager (later) | Needs multi-site comparison, permissions and export | Enterprise dashboard in a later release |

## Product philosophy and design principles

1. Measurement before inference. A circuit-labelled CT is the source of truth; AI may enrich but never overwrite it.
2. Safety before feature count. No V1 switching, no DIY mains installation, and fail-safe monitoring behaviour.
3. Explainable advice. Each recommendation links to a measurable pattern, confidence, expected range and limitation.
4. Offline tolerance. The device continues measuring and buffers data during internet loss.
5. Affordable lifecycle. Avoid subscriptions needed merely to view the customer’s own data.
6. Privacy by design. Collect only telemetry required for the stated function; treat occupancy-like inference as sensitive.

## Success metrics

| Metric | 90-day pilot target | Guardrail |
|---|---:|---|
| Successful electrician commissioning | ≥95% | No bypass of safety checklist |
| Devices reporting ≥95% of expected 5-minute intervals | ≥98% | Exclude planned maintenance transparently |
| Metered energy error vs traceable reference | ≤±1.0% under specified test envelope | Do not extrapolate to revenue billing |
| Users viewing a circuit insight weekly | ≥40% of active installs | No engagement dark pattern |
| Actionable alert precision after tuning | ≥80% | Alert volume ≤2/site/week median |
| 90-day hardware return rate | <2% | Root-cause every safety-related return |

**North Star metric:** verified customer energy decisions per active site per month—an action is verified only when the customer confirms it or a measurable post-action reduction persists after normalisation for schedule/weather where relevant.

## Business and engineering goals

Business: prove paid installation demand, achieve gross margin capable of installer support, and establish a channel of trained electricians. Engineering: build a calibrated, secure, serviceable monitoring platform with traceability from requirement to field evidence.

## Long-term vision

After a successful monitoring platform, add three-phase and enterprise SKUs, solar/EV-aware analytics, opt-in demand-response integrations, and carefully validated appliance-level inference. Utility integration, load control and financial products are separate programmes with their own certification, liability and regulatory gates.

