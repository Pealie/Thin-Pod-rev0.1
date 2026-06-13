# Pig Tag Welfare and Attachment Notes

## Purpose

This document records welfare, attachment and ruggedisation considerations for pig-mounted location tags. It is intended to keep the project honest: a tag that works electrically but fails on the animal is not a successful farm system.

Outdoor pigs are mechanically difficult hosts for electronics. They root, rub, chew, wallow, fight, grow quickly, squeeze through gaps and scrape against gates, trees and shelters. A livestock tracker must therefore be designed as an animal-welfare object before it is treated as an electronics enclosure.

## Design principles

The tag design should follow these principles:

- no sharp edges;
- no protruding screws, brittle corners or snag loops;
- no exposed wires;
- no toxic materials or coatings;
- no easy chew points;
- no enclosure seam that traps dirt against skin;
- no attachment that tightens as the animal grows;
- no attachment that can strangle or entangle;
- no normal operating temperature that can irritate tissue;
- no battery chemistry exposed to biting, crushing or water ingress;
- no design that hides injury or rubbing under the enclosure.

The system must be easy to remove manually and should fail safely if trapped. If a breakaway feature is used, the backend should detect probable tag loss.

## Attachment options

### Ear-tag form factor

An ear-mounted tag is the most plausible first direction for pigs because pig identification already uses ear-based concepts in many livestock contexts. A telemetry tag may be heavier and thicker than a simple identification tag, so the design must not assume ordinary ear-tag welfare characteristics without testing.

Advantages:

- familiar inspection location;
- no neck fit problem;
- less entanglement risk than a collar;
- visible during routine checks;
- loss or damage may be noticed quickly.

Risks:

- tearing or irritation if too heavy;
- chewing by other pigs;
- impact against troughs, gates and shelters;
- antenna detuning by head/ear position;
- mud and water ingress;
- poor GNSS sky view when head is down.

Design implication: begin with a dummy mechanical tag before fitting live electronics. Record wear, dirt ingress, rubbing and tag survival.

### Collar

A collar is mechanically tempting because it gives more volume for electronics and battery. For pigs, collars can be problematic because of body shape, growth, rooting behaviour, snagging and entanglement. A collar should not be the default design without strong veterinary and farm justification.

Advantages:

- more volume for battery and antenna;
- easier to prototype;
- less constrained by ear weight.

Risks:

- tightening as the animal grows;
- loosening and catching;
- rubbing under the neck;
- snagging on fencing or vegetation;
- damage during social behaviour;
- difficult fit across animals of different size.

Design implication: collars require a breakaway mechanism, frequent inspection and a conservative trial protocol.

### Harness or leg-mounted tag

Harnesses and leg-mounted tags are not recommended for the first prototype. They create higher snagging, rubbing, dirt-ingress and welfare complexity.

## Mechanical requirements

A prototype tag should be evaluated against the following checklist:

| Requirement | Target |
|---|---|
| Edge radius | smooth, no sharp corners |
| Fasteners | recessed or internal |
| Enclosure | sealed, impact-resistant, non-toxic |
| Ingress protection | target IP67 or better for mature design |
| Cleaning | no deep dirt traps against animal tissue |
| Battery access | secure against chewing and water ingress |
| Antenna | internal or protected, not a snagging whip |
| Failure mode | breakaway or detectable loss |
| Marking | visible tag ID, human-readable fallback |
| Heat | no perceptible warming at skin-contact surfaces |

## Electrical and battery safety

Battery and charging design must be conservative. A pig-mounted tag may be bitten, crushed, submerged, contaminated with mud, or exposed to pressure and impact.

Minimum design expectations:

- protected battery cell or pack;
- short-circuit protection;
- reverse-polarity protection where relevant;
- no exposed charging contacts unless corrosion-safe and isolated;
- sealed enclosure around lithium cells;
- conservative charge current if rechargeable;
- no field charging while mounted on the animal unless explicitly designed and risk assessed;
- brownout-safe firmware that cannot transmit corrupted identity or location data.

Primary lithium cells may simplify long deployment but create replacement logistics. Rechargeable cells reduce waste but complicate sealing and charging. The correct choice depends on trial duration, replacement interval and farm maintenance capability.

## Firmware behaviour related to welfare

The firmware should support welfare-aware telemetry:

- motion state: moving, resting, unusually still;
- possible tag detachment: no motion for long period or impossible movement pattern;
- excessive temperature: tag internal temperature above expected environmental range;
- low battery: early warning before tag becomes silent;
- missed pings: backend alert if tag disappears;
- alert mode: shorter interval only when needed.

The backend should avoid false welfare certainty. A stationary tag may indicate a resting pig, a sleeping pig, a lost tag, a dead battery, poor coverage or a welfare problem. The alert should preserve that uncertainty and prompt inspection rather than overstate the diagnosis.

## Trial protocol

### Dummy mechanical trial

Before any live electronics trial:

1. Fit a non-powered dummy tag of similar size, weight and shape.
2. Observe behaviour after fitting.
3. Inspect attachment point daily during the initial trial.
4. Record rubbing, swelling, tearing, dirt accumulation, chewing and loss.
5. Modify enclosure and attachment before electrical deployment.

### Small live trial

Initial live deployment should use five to ten pigs, not the full herd.

Record:

- tag ID and animal ID;
- attachment date and time;
- fitting method;
- tag weight and enclosure revision;
- inspection schedule;
- observed behaviour;
- skin/ear condition;
- battery state;
- packet success;
- location usefulness;
- removal date and reason.

### Escalation criteria

Stop or redesign the trial if any of the following occur:

- visible rubbing injury;
- swelling or tearing;
- repeated tag chewing;
- repeated loss from the same attachment method;
- snagging incident;
- heat or battery concern;
- farm staff find the tag difficult or unsafe to inspect;
- location results are too poor to justify attachment burden.

## Inspection schedule

Suggested early-stage inspection:

| Trial stage | Inspection frequency |
|---|---|
| First 24 hours | multiple visual checks |
| Days 2 to 7 | daily close inspection |
| Weeks 2 to 4 | several times per week |
| Mature deployment | routine welfare check plus alert-driven inspection |

Any new enclosure, attachment geometry, battery or animal age group should reset the inspection assumption.

## Data and privacy

Animal location telemetry is operational farm data. It may reveal field use, farm routines, livestock numbers, gateway locations and staff patterns. Store it deliberately.

Recommended controls:

- avoid public dashboards for live animal positions;
- restrict backend access;
- store only useful location history;
- document retention periods;
- avoid publishing exact farm maps in public repositories;
- use synthetic or redacted coordinates in examples.

## Statutory identification boundary

This project should treat pig tracking as a supplementary farm-management and welfare tool. It should not imply that an electronic telemetry tag replaces statutory pig identification or movement reporting.

For a real Scottish deployment, check the latest official guidance on:

- keeper registration;
- holding number and herd mark;
- approved identification methods;
- movement reporting;
- record keeping;
- veterinary and welfare requirements.

## Practical judgement

The tag is successful only if the pig can live normally with it. A slightly less elegant electronics design that is safe, inspectable and robust is better than a technically superior design that irritates, snags or disappears in mud.

