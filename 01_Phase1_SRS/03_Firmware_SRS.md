# Firmware SRS

## Purpose and scope

Defines embedded operation from boot through measurement, local persistence, communication, diagnostics and recovery.

## Architecture and interfaces

Boot ROM/secure bootloader → signed application → metrology acquisition, aggregation, storage, connectivity, device-management and diagnostics modules. Interfaces: metering IC SPI, CT configuration, NVS, Wi-Fi/BLE, MQTT/TLS and signed OTA package.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| FWR-001 | Verify firmware signature before execution/update. | Invalid image cannot boot; event is recorded. |
| FWR-002 | Keep monotonic energy counters and timestamp-quality flag. | Counter never decreases except controlled factory erase; unsynchronised data is marked. |
| FWR-003 | Buffer telemetry during WAN loss and drain without duplicate accounting. | Seven-day configured outage test preserves one-minute aggregates and event journal, then uploads ordered records without double counting. |
| FWR-004 | Use watchdog, brownout handling and crash diagnostics. | Fault injection recovers to measurement state or a safe update-recovery state. |
| FWR-005 | Apply configuration atomically and report desired/reported versions. | Interrupted configuration cannot leave a mixed metrology mapping. |
| FWR-006 | Provide authenticated commissioning through a short-lived, user-visible flow. | Unauthorized BLE/Wi-Fi commissioning attempt is rejected and logged. |

## Non-functional requirements

Measurement acquisition has priority over telemetry. Memory limits, worst-case buffer capacity, boot time, update bandwidth and task timing are controlled design inputs. Firmware must produce SBOM, reproducible build metadata and release signature records.

## Future extensions

New analytics fields are backward compatible or versioned. Remote control is excluded and would require a separate safety case.
