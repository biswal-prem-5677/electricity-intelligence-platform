# Phase 4 — Firmware Engineering Plan

## Architecture and execution model

Use a small RTOS only if measured task separation/latency needs exceed a cooperative design; the decision is made at EVT with timing evidence. Modules: secure boot, board support, metrology driver, acquisition/aggregation, local journal, provisioning, network/MQTT, configuration, OTA, diagnostics and LED/status. Metrology timing is isolated from cloud retries and UI/diagnostic work.

## State machines

    RESET → secure-verify → self-test → uncommissioned / operational / recovery
                                            │                 │
                                     provisioning ────────────┘
    Connectivity: disconnected → connecting → authenticated → online → backoff
    OTA: available → downloaded → verified → staged boot → healthy / rollback

Each state transition records reason and monotonic timestamp. Failed self-test does not imply electrical protection; it results in a visible fault state and support route.

## Drivers, resilience and power

Drivers have timeouts, bounded retries, versioned configuration and simulation seams. Brownout/flash-write handling uses atomic journal records. Watchdog servicing is owned by a health supervisor, not sprinkled across tasks. Wi-Fi reconnect uses exponential backoff; local storage applies quota and data-downsampling policy before exhaustion.

## OTA, encryption and provisioning

Create reproducible signed builds, an SBOM, immutable release manifest and hardware compatibility matrix. OTA validates signature/hash/size, writes an inactive slot, boots with trial flag and reports health before commitment. Provisioning must be proximity-bound, short-lived, user-visible and recoverable only through an authorised process. TLS trust anchors, device credentials and secrets are never logged.

## Diagnostics and verification

Export bounded non-sensitive diagnostics: firmware/build, reset reason, radio state, metrology health, buffer depth and error counters. Unit-test calculation and parser boundaries; hardware-in-loop test SPI faults, CT disconnection, clock loss, power interruption, MQTT replay, corrupt configuration and interrupted update. Production firmware is tagged to source, compiler, SBOM, signing key ID and test report.

