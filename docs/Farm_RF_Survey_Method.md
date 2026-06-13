# Farm RF Survey Method for Pig Location Tracking

## Purpose

This document defines a repeatable RF survey method for testing LoRaWAN coverage across a farm before deploying pig-mounted location tags at herd scale.

The survey should answer four practical questions:

1. Can a tag near pig height reach the gateway from all relevant fields?
2. Which areas create packet loss because of trees, dips, buildings, ridges or wet ground?
3. Is one gateway enough, or is a second gateway justified?
4. Which LoRaWAN data rates and spreading factors are realistic for normal 15-minute pig-tracking uplinks?

## Survey principle

The survey should use measured packet delivery, RSSI, SNR, data rate and gateway metadata. It should not rely on generic LoRa range claims. Rural range can look excellent in open ground and disappointing in local terrain shadows.

A survey result is only useful if it records the actual conditions: gateway location, antenna height, test-node height, weather, vegetation, terrain, and test route.

## Equipment

Recommended equipment:

- outdoor 8-channel LoRaWAN gateway, EU868;
- known-good 868 MHz outdoor antenna;
- low-loss coax, correctly weatherproofed;
- PoE injector or protected power supply;
- test node or development tracker capable of sending a fixed payload;
- GPS/GNSS phone or handheld receiver for survey-point coordinates;
- notebook, CSV sheet or field form;
- optional: handheld spectrum analyser or SDR for interference checks;
- optional: second test node at pig-tag mounting height.

The test node should transmit a compact packet containing at least:

- survey point number;
- sequence number;
- battery voltage;
- optional local GNSS coordinate;
- optional test-mode flag.

The network server should log:

- timestamp;
- device ID;
- frame counter;
- gateway ID;
- RSSI;
- SNR;
- data rate or spreading factor;
- channel/frequency;
- decoded payload;
- duplicate gateway receptions where present.

## Gateway candidate assessment

Before driving or walking the field, inspect candidate gateway locations.

Record:

| Item | Notes |
|---|---|
| Candidate name | Farmhouse roof, shed mast, hill shed, pole, etc. |
| Grid reference / lat-lon | Use WGS84 decimal degrees for consistency |
| Antenna height | Estimate above ground and above roofline |
| Cable length | Keep coax short where possible |
| Power | PoE, mains, solar, UPS |
| Backhaul | Ethernet, Wi-Fi bridge, 4G/5G, local-only store-and-forward |
| Obstructions | trees, buildings, ridge lines, tanks, silos |
| Weather exposure | wind, rain, lightning, livestock access |
| Maintenance access | ladder, roof work, livestock movement, safe isolation |

Prefer a location that is high, clear, powered and maintainable rather than a theoretically perfect mast that is awkward or unsafe to service.

## Survey point selection

Define survey points before testing. The set should include:

- near-gateway reference point;
- each field centre;
- far corners of the holding;
- gates and likely escape routes;
- tree-lined areas;
- dips and hollows;
- behind stone walls or metal sheds;
- water troughs and wallowing areas;
- locations where pigs are expected to congregate;
- likely dead spots identified by farm staff.

Use at least 30 survey points for a multi-square-kilometre farm. For a farm with complex folds, trees and buildings, 50 to 100 points is more realistic.

## Test heights

Pig-mounted tags are close to the ground compared with mast antennas. Testing only with a handheld node held at chest height can give an over-optimistic result.

Recommended tests at important points:

| Height | Purpose |
|---:|---|
| 0.4 m | approximate pig/tag working height |
| 1.2 m | carried-node diagnostic height |
| 2.0 m | optimistic comparison height |

If the 0.4 m test fails but 1.2 m succeeds, the problem is likely ground clutter, terrain masking, vegetation or body/antenna orientation rather than total distance.

## Transmission test pattern

At each survey point:

1. Record point ID, coordinate, terrain type and visual notes.
2. Place or hold the test node at the target height.
3. Send ten unconfirmed uplinks spaced at least 15 to 30 seconds apart.
4. Repeat with the antenna in at least two orientations if the final tag orientation is uncertain.
5. Record received count, missing count, RSSI, SNR and data rate.
6. Photograph the point if it is a suspected problem area.

For a fast first pass, three packets per point may be enough to identify gross coverage holes. For final acceptance, use at least ten packets per point.

## Suggested CSV format

```csv
survey_date,gateway_id,gateway_lat,gateway_lon,antenna_height_m,point_id,point_lat,point_lon,node_height_m,terrain,vegetation,weather,tx_count,rx_count,min_rssi_dbm,median_rssi_dbm,max_rssi_dbm,min_snr_db,median_snr_db,max_snr_db,data_rate,gateway_count,notes
2026-06-13,GW01,55.000000,-4.000000,8.0,P001,55.001000,-4.001000,0.4,open pasture,short grass,dry,10,10,-112,-106,-101,2.0,5.5,8.0,SF9BW125,1,near trough
```

The repository backend can later ingest this into a map or notebook. The first priority is clean evidence, not visual polish.

## Acceptance categories

Classify each point after testing:

| Category | Packet delivery | Link margin | Meaning |
|---|---:|---|---|
| A | 9 to 10 of 10 | strong RSSI/SNR | normal operation expected |
| B | 7 to 8 of 10 | usable | monitor, may need lower data rate |
| C | 4 to 6 of 10 | marginal | investigate gateway/antenna/data rate |
| D | 0 to 3 of 10 | poor | probable dead zone or second gateway candidate |

A farm-scale deployment should not rely on Category C or D coverage for routine welfare or escape detection.

## Data-rate testing

Run the survey at the intended adaptive or fixed data rate. If ADR is enabled during testing, record the selected data rate. If ADR is disabled, test at the planned fixed spreading factor.

Suggested field sequence:

- start at SF9BW125 for a practical balance;
- test SF7BW125 in open and near-gateway areas;
- test SF10 or SF11 only where required;
- avoid designing the whole system around SF12 unless the message volume is very low or extra gateways are impossible.

Long airtime at high spreading factor reduces network capacity and battery life. It is often better to add a better antenna or second gateway than to run every tag at a pessimistic data rate.

## Terrain-specific checks

### Trees and shelterbelts

Test both sides of shelterbelts. Foliage changes seasonally, so repeat problem tests in leaf-on and wet conditions where possible.

### Dips and hollows

Walk the bottom of the dip and both sides. If reception returns quickly on the slope, a second gateway on a different bearing may solve the problem.

### Buildings and metal structures

Test behind sheds, trailers, tanks and gates. Metal can create sharp shadows and reflections. Do not assume a point is good because another point only 30 metres away is good.

### Wet ground and wallows

Pig tags may be partly blocked by body mass, mud, water and low antenna height. Test near troughs and wallows, not only on dry tracks.

## Gateway decision rule

Add a second gateway if any of the following are true:

- important fields contain repeated Category C or D points;
- far-side dips fail at pig height but recover at human height;
- geofence boundary areas have weak coverage;
- likely escape routes are poorly covered;
- one gateway produces good coverage only at high spreading factor;
- gateway redundancy is required for welfare or operational confidence.

A second gateway should be positioned to add a different angle into dead terrain, not merely to repeat the first gateway's view.

## Post-survey outputs

After the survey, create:

1. **RF survey CSV** with all measured points.
2. **Coverage map** showing A/B/C/D categories.
3. **Gateway recommendation** with one-gateway or two-gateway decision.
4. **Dead-zone list** with mitigation notes.
5. **Data-rate recommendation** for tag firmware.
6. **Deployment risk register** for areas that require farm procedure rather than electronics.

## Minimum field report template

```markdown
# Farm RF Survey Report

Date:
Farm area:
Gateway candidate:
Antenna height:
Weather:
Vegetation state:
Test node:
Firmware version:
Network server:

## Result summary

- Total survey points:
- Category A:
- Category B:
- Category C:
- Category D:
- Recommended gateway count:
- Recommended normal data rate:

## Main coverage risks

1.
2.
3.

## Recommended mitigations

1.
2.
3.

## Deployment decision

Proceed / repeat survey / add second gateway / change gateway location.
```

## Field discipline

Do not tune the deployment by anecdote. If a pig is hard to find, the system must be able to answer whether the problem was location fix quality, LoRaWAN coverage, tag damage, battery state, gateway outage or backend alerting.

That distinction only exists if the RF survey and operational logs are preserved.

