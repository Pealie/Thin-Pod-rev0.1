# LoRaWAN Pig Tracker Payload v1

## Purpose

This document defines version 1 of the compact binary uplink payload for pig-location tags. It is designed for routine 15-minute LoRaWAN uplinks from a herd-scale outdoor deployment.

The payload is intentionally small, fixed-length and easy to decode in The Things Stack, ChirpStack or a local backend.

## Summary

| Item | Value |
|---|---|
| Payload version | 1 |
| Message type | 1 = location uplink |
| FPort | 10 |
| Length | 26 bytes |
| Endianness | little-endian for multi-byte integers |
| Normal interval | 15 minutes, with jitter |
| Uplink type | unconfirmed for routine messages |
| Coordinate format | signed integer, decimal degrees x 1e7 |

## Byte layout

| Offset | Size | Name | Type | Scale / unit | Notes |
|---:|---:|---|---|---|---|
| 0 | 1 | `protocol_version` | uint8 | none | must be `1` |
| 1 | 1 | `message_type` | uint8 | none | `1` = location uplink |
| 2 | 1 | `flags` | uint8 | bitfield | see flag table |
| 3 | 1 | `fix_quality` | uint8 | bitfield | fix type and satellite count |
| 4 | 2 | `animal_id` | uint16 | none | farm-visible animal/tag mapping |
| 6 | 4 | `latitude_e7` | int32 | degrees x 1e7 | WGS84 latitude |
| 10 | 4 | `longitude_e7` | int32 | degrees x 1e7 | WGS84 longitude |
| 14 | 2 | `horizontal_accuracy_dm` | uint16 | decimetres | 65535 = unknown |
| 16 | 2 | `battery_mv` | uint16 | millivolts | battery voltage |
| 18 | 2 | `temperature_dc` | int16 | deci-degrees C | 215 = 21.5 C |
| 20 | 2 | `speed_cm_s` | uint16 | cm/s | 65535 = unknown |
| 22 | 2 | `heading_cdeg` | uint16 | centi-degrees | 0 to 35999, 65535 = unknown |
| 24 | 2 | `fix_age_s` | uint16 | seconds | 65535 = unknown |

Total: 26 bytes.

## Flags byte

| Bit | Mask | Name | Meaning |
|---:|---:|---|---|
| 0 | `0x01` | `geofence_alert` | tag believes or backend has configured this as an alert state |
| 1 | `0x02` | `moving` | recent accelerometer/GNSS movement |
| 2 | `0x04` | `stationary` | low movement over configured window |
| 3 | `0x08` | `no_gnss_fix` | no valid current/recent location fix |
| 4 | `0x10` | `alert_mode` | tag is transmitting at a shorter interval |
| 5 | `0x20` | `tamper_or_detached` | possible detachment, damage or abnormal motion pattern |
| 6 | `0x40` | `low_battery` | battery below configured threshold |
| 7 | `0x80` | `reserved` | reserved for future use |

`moving` and `stationary` are advisory. Both may be false if the tag cannot classify the recent motion window. Both should not normally be true at the same time.

## Fix-quality byte

The fix-quality byte combines fix type and satellite count:

```text
bits 0..2: fix_type
bits 3..7: satellites_used, capped at 31
```

Fix types:

| Value | Meaning |
|---:|---|
| 0 | no fix |
| 1 | GNSS 2D fix |
| 2 | GNSS 3D fix |
| 3 | assisted GNSS / solved snapshot |
| 4 | network or coarse fallback |
| 5 to 7 | reserved |

Satellite count is stored as `min(satellites, 31) << 3`.

## Invalid or unknown values

Use these sentinel values:

| Field | Sentinel | Meaning |
|---|---:|---|
| `latitude_e7` | `-2147483648` | unknown or invalid |
| `longitude_e7` | `-2147483648` | unknown or invalid |
| `horizontal_accuracy_dm` | `65535` | unknown |
| `speed_cm_s` | `65535` | unknown |
| `heading_cdeg` | `65535` | unknown |
| `fix_age_s` | `65535` | unknown |

If `no_gnss_fix` is set, the backend should not trust latitude and longitude even if numeric fields are present.

## Coordinate representation

Latitude and longitude use signed 32-bit integers:

```text
latitude_e7  = round(latitude_degrees  x 10,000,000)
longitude_e7 = round(longitude_degrees x 10,000,000)
```

Examples:

| Coordinate | Encoded |
|---|---:|
| 55.4500000 | 554500000 |
| -4.6300000 | -46300000 |

This gives a theoretical resolution of approximately 1 cm at the equator, far beyond the expected accuracy of a low-power livestock tracker. The format is used because it is standard, compact and unambiguous.

## Example decoded object

```json
{
  "protocol_version": 1,
  "message_type": 1,
  "animal_id": 101,
  "latitude": 55.45,
  "longitude": -4.63,
  "horizontal_accuracy_m": 8.4,
  "battery_mv": 3630,
  "temperature_c": 12.7,
  "speed_m_s": 0.18,
  "heading_deg": 241.35,
  "fix_age_s": 12,
  "fix_type": "GNSS_3D",
  "satellites_used": 11,
  "flags": {
    "geofence_alert": false,
    "moving": true,
    "stationary": false,
    "no_gnss_fix": false,
    "alert_mode": false,
    "tamper_or_detached": false,
    "low_battery": false,
    "reserved": false
  }
}
```

## JavaScript decoder

The matching decoder is:

```text
software/decoders/pig_tracker_decoder.js
```

It exports:

- `decodeUplink(input)` for The Things Stack-style JavaScript payload formatters;
- `Decode(fPort, bytes)` for older ChirpStack-style examples;
- `decodePigTrackerPayload(bytes, fPort)` for local Node.js testing.

## Backend ingest

The matching minimal backend is:

```text
software/backend/pig_location_ingest.py
```

It accepts decoded webhook JSON, stores packet history in SQLite and writes a latest-position GeoJSON file.

## Operational notes

Normal pig-location traffic should use unconfirmed uplinks. Confirmed uplinks should be avoided for routine herd tracking because downlinks consume gateway resources and can reduce network scalability.

A sensible implementation should add random jitter to each tag's nominal interval. For example, a tag scheduled for a 15-minute uplink may transmit at 15 minutes ±120 seconds. This reduces collision risk and avoids bursts from the whole herd.

The backend should preserve the original network-server metadata, especially RSSI, SNR, data rate, gateway ID and frame counter. These fields are essential when diagnosing whether a missing pig is really missing, whether a tag has failed, or whether a coverage hole exists.

## Versioning policy

Payload changes should be versioned in byte 0. Do not silently reinterpret v1 fields. A v2 payload may add optional TLV fields, compact accelerometer statistics or gateway-assist metadata, but v1 should remain fixed and stable for field trials.

