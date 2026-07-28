# Cloud Interface Contract

## MQTT topic contract

Topics are logical examples; production identifiers are opaque and access-controlled.

| Direction | Topic | QoS / retention | Purpose |
|---|---|---|---|
| Device → cloud | v1/devices/{deviceId}/telemetry | QoS 1, not retained | Quality-flagged measurement records |
| Device → cloud | v1/devices/{deviceId}/health | QoS 1, not retained | Health/diagnostic heartbeat |
| Cloud → device | v1/devices/{deviceId}/desired | QoS 1, retained where appropriate | Versioned configuration |
| Device → cloud | v1/devices/{deviceId}/reported | QoS 1, retained where appropriate | Applied configuration/version |
| Cloud → device | v1/devices/{deviceId}/ota | QoS 1, not retained | Signed OTA offer metadata |

Each device is authorised only for its own paths. Topic wildcards are not granted to devices. Payloads require schemaVersion, deviceId, sequence, timestampUtc, clockQuality, firmwareVersion and calibrationVersion.

## Telemetry example

    {
      "schemaVersion": "1.0",
      "sequence": 918273,
      "timestampUtc": "2026-07-29T12:00:01Z",
      "clockQuality": "synced",
      "channelId": "branch-03",
      "activePowerW": 812.4,
      "apparentPowerVA": 890.1,
      "reactivePowerVAR": 364.8,
      "voltageV": 231.8,
      "currentA": 3.84,
      "powerFactor": 0.913,
      "frequencyHz": 50.02,
      "activeEnergyWh": 12591.4,
      "quality": ["calibrated"],
      "calibrationVersion": "cal-2026-07-01"
    }

## API and data model baseline

| Resource | Required fields | Key rules |
|---|---|---|
| Tenant | ID, region, retention policy | Tenant boundary is server-enforced |
| User role | user ID, tenant/site role, MFA state | Privilege is least-privilege and audited |
| Site | timezone, tariff version, topology | No utility billing authority implied |
| Device | serial, hardware revision, credential state, owner | Ownership transfer is audited |
| Channel | mapping, label, sensor type, orientation, calibration | Mapping changes are effective-dated |
| Measurement aggregate | interval, values, quality, source version | Raw and derived data never overwrite invisibly |
| Insight | evidence window, rule/model version, confidence, action | Must be explainable and dismissible |
| OTA release | compatibility, signature, cohort, rollback state | Dual approval for production promotion |

Initial REST endpoints: POST /v1/sites, GET /v1/sites/{id}/measurements, PATCH /v1/devices/{id}/configuration, GET /v1/insights, POST /v1/exports and POST /v1/support-consents. Endpoint schemas, error codes, pagination and rate limits are defined in an OpenAPI artifact before implementation.

