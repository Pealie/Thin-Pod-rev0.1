# Livestock Pig Location Tracking

## Purpose

This document defines a practical livestock-location extension for the Scottish Farms LoRaWAN project. The initial use case is a herd of approximately 130 outdoor pigs distributed across several square kilometres of Ayrshire farmland, including open fields, shelterbelts, trees, hollows, dips, wet ground and farm buildings.

The aim is not to build a consumer-style live tracker. The aim is to build a field-resilient farm telemetry layer that helps locate animals, detect escape or welfare events, and preserve an auditable record of last known position without excessive battery drain or radio airtime.

The preferred operating model is:

- routine location uplinks every 15 minutes;
- randomised transmission jitter so tags do not all transmit at once;
- alert-mode uplinks at a shorter interval only when needed;
- farm-local data ownership where practical;
- rugged, welfare-aware tag design;
- clear separation between statutory livestock identification and optional farm telemetry.

## Scope

This extension covers the conceptual architecture, field survey method, welfare and attachment notes, a versioned payload format, a JavaScript LoRaWAN payload decoder, and a minimal Python backend for ingesting decoded location uplinks and generating geofence alerts.

This extension does not claim to replace official pig identification, movement reporting, keeper registration, veterinary inspection, staff judgement or legal compliance. In Scotland, pig identification and movements remain governed by the relevant official requirements, including holding registration, herd marks and ScotEID movement reporting.

## Design summary

The recommended baseline architecture is:

```text
Pig-mounted location tag
  GNSS / assisted GNSS / LoRa Edge-class location engine
  accelerometer
  battery monitor
  LoRaWAN EU868 radio
        |
        | 15-minute location uplink, FPort 10, payload v1
        v
Outdoor LoRaWAN gateway
  8-channel concentrator
  868 MHz antenna mounted high and clear
  wired, PoE or protected mains backhaul
        |
        v
LoRaWAN network server
  ChirpStack, The Things Stack, or equivalent
        |
        v
Farm backend
  HTTP/MQTT ingest
  SQLite/PostGIS/storage
  latest-position map
  geofence and missed-ping alerts
```

The tag should normally be asleep. It wakes on a scheduled interval, powers the GNSS receiver if a fresh fix is needed, encodes a compact binary payload and transmits a short LoRaWAN uplink. If the animal is outside its permitted area, the tag is tampered with, the battery is low, or the backend detects missed pings, the system raises an alert.

## Why interval pings rather than continuous tracking

For a herd of 130 pigs, true real-time location is rarely the correct first requirement. Continuous tracking creates four problems:

1. **Battery drain**: GNSS acquisition and radio transmission dominate the power budget.
2. **Radio airtime**: LoRaWAN is a low-duty-cycle technology, not a continuous telemetry stream.
3. **Operational noise**: a map that updates constantly can be visually impressive but less useful than clear exception alerts.
4. **Ruggedisation pressure**: larger batteries and antennas make animal attachment harder.

A 15-minute normal interval gives 96 uplinks per pig per day. For 130 animals, that is 12,480 routine uplinks per day before alert traffic. This is a reasonable design target for a private 8-channel LoRaWAN farm deployment if payloads are compact, spreading factors are controlled, and transmissions are jittered.

Suggested modes:

| Mode | Trigger | Interval | Notes |
|---|---:|---:|---|
| Normal | healthy tag, in field, no alert | 15 minutes | baseline herd monitoring |
| Movement-rich | high movement or fresh release into field | 5 minutes | temporary, not all day |
| Alert | outside geofence, tamper, repeated poor fix | 1 to 2 minutes | bounded duration, then fall back |
| Recovery | previously missing tag returns | immediate uplink plus next normal interval | include alert flag |
| Low battery | below configured threshold | 30 to 60 minutes | preserve remaining battery |

## Location technology options

### 1. GNSS in every tag

This is the clearest baseline. A GNSS receiver in each tag provides a direct position fix. The backend only needs to display and analyse coordinates. Accuracy is normally sufficient for field-level retrieval, although trees, dips, buildings and poor antenna orientation can degrade fix time and accuracy.

Advantages:

- simple mental model;
- direct latitude and longitude in the payload;
- works with one gateway if radio coverage is adequate;
- geofencing is straightforward.

Disadvantages:

- higher power consumption than radio-only telemetry;
- larger tag and antenna design challenge;
- GNSS can struggle near buildings, trees, banks and wallows.

### 2. LoRaWAN geolocation without GNSS

LoRaWAN network-based location can use RSSI, SNR or time-difference approaches. It should not be assumed to give reliable pig-finding precision on a farm. RSSI-based position can be too coarse for animal retrieval. TDoA-style approaches require multiple gateways with suitable geometry and time synchronisation.

This may be useful as a fallback or coarse farm-zone indicator, but it is not the recommended primary method for this use case.

### 3. Assisted GNSS / LoRa Edge-class design

A LoRa Edge-class tracker can scan GNSS and sometimes Wi-Fi signals, then rely on a solver outside the tag to produce a position. This can reduce energy consumption compared with a full GNSS fix on the animal. It may be a strong future option where cloud or local solver architecture is acceptable.

Trade-offs include vendor dependence, location-solver cost, cloud reliance, and reduced transparency compared with a simple GNSS coordinate payload.

## Farm topology assumptions

The Ayrshire use case contains several RF and GNSS obstacles:

- trees and shelterbelts;
- dips, gullies and folds in terrain;
- wet ground and wallows;
- metal-sided buildings, gates and trailers;
- animals close to the ground;
- antennas at poor orientations;
- seasonal vegetation changes;
- weather-driven changes in soil and foliage attenuation.

The architecture should therefore assume that one gateway might be enough, but should not rely on that assumption until a survey has been completed. The system should be designed so that adding a second gateway is an ordinary expansion, not an architectural redesign.

## Gateway placement

The first gateway should be placed as high and clear as practical, preferably on a farmhouse, shed, mast, hill-facing building or other protected structure with power and backhaul. A proper outdoor 868 MHz antenna and low-loss cable matter more than theoretical radio range figures.

Recommended first deployment:

- one outdoor 8-channel LoRaWAN gateway;
- EU868 regional plan;
- antenna mounted above local roofline where possible;
- PoE with surge protection;
- IP-rated enclosure and cable glands;
- local Ethernet or reliable broadband backhaul;
- packet logging enabled from day one.

A second gateway should be considered if the RF survey reveals recurring dead zones, especially in dips, wooded areas, behind stone or metal structures, or at the far side of a ridge.

## Tag hardware requirements

Minimum practical tag capability:

- LoRaWAN EU868 radio;
- GNSS or assisted GNSS location source;
- accelerometer for motion state, wake logic and tamper detection;
- battery measurement;
- optional temperature measurement for tag/environment telemetry;
- rugged animal-safe enclosure;
- field-replaceable or long-life battery;
- unique tag identity mapped to farm records;
- waterproofing suitable for mud, rain, wallowing and pressure washing proximity.

The tag should expose at least the following operational states:

- normal;
- moving;
- stationary;
- no recent GNSS fix;
- geofence alert;
- tamper or possible detachment;
- low battery;
- alert mode.

## Backend requirements

The first backend should be deliberately simple. It should prove the data chain before growing into a larger farm-management system.

Minimum backend functions:

- accept decoded uplinks from The Things Stack, ChirpStack or an equivalent LoRaWAN network server;
- store every location packet with timestamp and tag identity;
- maintain latest known position per animal;
- emit a GeoJSON latest-position file for map display;
- detect stale tags;
- detect low batteries;
- detect geofence breaches;
- provide logs suitable for field debugging.

Later backend functions:

- map dashboard;
- group/herd heatmap;
- route replay;
- tag battery-health history;
- seasonal coverage comparison;
- animal welfare event review;
- integration with farm records;
- SMS, email or messaging alerts.

## Data model

The minimal backend record should include:

| Field | Meaning |
|---|---|
| `received_at` | UTC time the backend received the uplink |
| `device_id` | LoRaWAN device identifier, DevEUI or tag ID |
| `animal_id` | farm-visible animal identifier |
| `latitude` | decimal degrees, if a valid fix is present |
| `longitude` | decimal degrees, if a valid fix is present |
| `horizontal_accuracy_m` | estimated horizontal accuracy |
| `fix_age_s` | age of the GNSS fix when transmitted |
| `battery_mv` | battery voltage in millivolts |
| `temperature_c` | optional temperature measurement |
| `speed_m_s` | optional movement estimate |
| `heading_deg` | optional heading estimate |
| `flags` | decoded status flags |
| `raw_json` | original network-server webhook payload |

## Scaling target for 130 pigs

For initial planning:

```text
130 pigs x 96 normal uplinks/day = 12,480 uplinks/day
```

This is approximately 520 uplinks per hour across the herd, before retries or alert traffic. The design should not schedule all tags exactly on the quarter-hour. Each tag should add random jitter to its uplink schedule, for example ±120 seconds. Where LoRaWAN MAC layer controls are available, ADR should be used carefully after field testing.

Suggested constraints:

- fixed binary payload, 26 bytes for v1;
- FPort 10 for location uplinks;
- unconfirmed uplinks for normal traffic;
- confirmed uplinks only for exceptional cases, if at all;
- avoid unnecessary downlinks;
- keep spreading factors as low as coverage permits;
- log RSSI, SNR, data rate and gateway ID for every received packet.

## Field trial phases

### Phase 0: bench simulation

- Use the JavaScript decoder with known byte payloads.
- Post sample decoded packets into the Python ingest endpoint.
- Confirm SQLite storage and GeoJSON output.
- Confirm geofence alert script catches outside and stale positions.

### Phase 1: static field survey

- Place the gateway in the proposed first location.
- Carry a test node or tracker to fixed survey points.
- Record packet delivery, RSSI, SNR and spreading factor.
- Repeat in open fields, dips, tree cover and near buildings.

### Phase 2: small animal trial

- Fit five to ten tags for a short supervised period.
- Inspect attachment points frequently.
- Compare map positions with staff observations.
- Record losses, rubbing, chewing, enclosure damage and battery decline.

### Phase 3: partial herd

- Expand to 25 to 30 pigs.
- Confirm backend behaviour under realistic message volume.
- Tune normal and alert intervals.
- Review practical retrieval usefulness with farm staff.

### Phase 4: full herd

- Deploy to approximately 130 pigs.
- Add second gateway if required by measured coverage, not guesswork.
- Formalise maintenance, battery replacement and lost-tag procedures.

## Practical success criteria

A prototype should be considered successful only if it proves all of the following:

- at least 95 percent routine packet delivery in normal field conditions;
- clear identification of any persistent RF dead zones;
- location accuracy sufficient to find an animal in the field, not just identify the farm;
- tag attachment survives ordinary pig behaviour for the trial period;
- no visible rubbing injury, snagging risk or behaviour change caused by the tag;
- backend provides useful alerts rather than noisy false alarms;
- battery life projection meets the replacement interval acceptable to the farm;
- the system remains understandable and maintainable by a technically competent farm operator or local engineer.

## Repository layout

This livestock extension adds:

```text
docs/Livestock_Pig_Location_Tracking.md
docs/Farm_RF_Survey_Method.md
docs/Pig_Tag_Welfare_and_Attachment_Notes.md
docs/LoRaWAN_Pig_Tracker_Payload_v1.md
software/decoders/pig_tracker_decoder.js
software/backend/pig_location_ingest.py
software/backend/geofence_alerts.py
```

## Reference anchors

These references define the external constraints that should be checked before deployment:

- LoRa Alliance, LoRaWAN Regional Parameters, RP002.
- Ofcom, UK Interface Requirement IR 2030, licence-exempt short-range devices.
- Scottish Government livestock identification and traceability guidance for pigs.
- ScotEID movement reporting requirements for pig keepers.
- The Things Stack payload formatter documentation.
- ChirpStack integration and codec documentation for the selected network-server version.

Record the exact document versions and dates used for any real deployment.

