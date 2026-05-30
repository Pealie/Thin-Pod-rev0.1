# Thin-Pod rev 0.1

<p align="left">
  <a href="https://certification.oshwa.org/uk000091.html">
    <img src="images/certification/oshwa-uk000091.png" width="260" alt="Open Source Hardware Association certification mark: UK000091">
  </a>
</p>
<p align="left">
  <strong>OSHWA Certified Open Source Hardware · UK000091</strong><br>
  Certified 28 May 2026 · <a href="https://certification.oshwa.org/uk000091.html">View certification record</a>
</p>

## Overview

Thin-Pod rev 0.1 is a compact analogue vibration-sensing carrier board intended for experimental condition-monitoring work on motors, pumps, fans, gearboxes and similar rotating machinery. The board supports an ADXL1005-based analogue accelerometer signal path and provides protected power entry, a regulated and switched sensor supply, signal conditioning, accessible test points and a mating interface for a commercially supplied Qorvo DWM3001-CDK development board.

The design is published as an open-hardware PCB release: editable KiCad source files, project-local library files, fabrication outputs, bill of materials and verification documentation are provided in this repository.

## Certification scope

The OSHWA certification for **Thin-Pod rev 0.1** covers the creator-designed sensor-node carrier PCB and its release documentation.

### Included in scope

* Two-layer Thin-Pod carrier PCB design.
* Protected power-entry path, fuse provision and reverse-polarity protection.
* Regulated 3.3 V power path and PFET-switched accelerometer supply.
* ADXL1005 analogue sensor interface and conditioned signal path.
* Electrical test points for bring-up and measurement.
* Thin-Pod-authored mating interface for a Qorvo DWM3001-CDK.
* Corrected carrier-board ground return for the DWM3001-CDK interface.
* Editable KiCad source files, fabrication outputs, BOM and documentation.

### Outside scope

* The Qorvo DWM3001-CDK itself as open hardware.
* The Analog Devices ADXL1005 device or any vendor evaluation board as open hardware.
* The Pololu S7V8F3 regulator module as open hardware.
* The separately developed Thin-Pod Gateway.
* STM32 NUCLEO-N657X0-Q, ESP32-C6 or Gateway-side hardware.
* Implemented UWB communication, radio firmware or networking.
* Gateway DSP, TinyML, anomaly detection or predictive-maintenance system claims.

The commercial devices used by the board are identified as third-party components; they are not relicensed or represented as creator-designed open hardware.

## Infrastructure context

Thin-Pod rev 0.1 is the sensing-node hardware foundation for a broader experimental condition-monitoring architecture. Its role is to provide a reproducible, inspectable and electrically verified vibration-measurement endpoint for rotating machinery.

The infrastructure concerns addressed by this release are measurement integrity, power-path robustness, interface definition, third-party component boundaries, traceable validation evidence and manufacturable open-hardware documentation. Gateway transport, DSP, TinyML and predictive-maintenance workflows are intentionally outside the certified rev 0.1 scope, but this board is designed to provide the physical measurement layer on which those later infrastructure functions can be built.

## Functional architecture

```text
Mechanical vibration
        ↓
ADXL1005 analogue accelerometer interface
        ↓
Analogue conditioning / filtered signal node
        ↓
Accessible test point and CDK ADC interface
        ↓
Commercial Qorvo DWM3001-CDK connection
```

Power is arranged as:

```text
RAW_IN → fuse provision → 1N5817 reverse-polarity protection
       → Pololu S7V8F3 regulated 3.3 V rail
       → PFET-switched accelerometer supply
```

## System role

Thin-Pod rev 0.1 is the sensing-node hardware layer within a broader experimental condition-monitoring architecture. Its certified scope is limited to the creator-designed vibration-sensor carrier PCB, its documented power path, analogue accelerometer interface, test points, mating interface and release documentation.

```text
Machine / motor / pump
        ↓
Thin-Pod rev 0.1 sensing node
        ↓
ADC / UWB-capable commercial module interface
        ↓
Gateway transport and analysis layer
        ↓
Maintenance telemetry / diagnostic workflow
```

Only the Thin-Pod rev 0.1 sensing-node carrier PCB and its release documentation are included in the certified open-hardware scope. The ADC-capable commercial module interface is documented as part of the board design, but the commercial module itself is not claimed as open hardware.

Gateway transport, implemented UWB communications, networking, DSP, TinyML, anomaly detection and maintenance-diagnostic workflows are part of the wider Thin-Pod development direction. They are shown here to clarify the intended system context, but they are outside the certified rev 0.1 hardware release.

## DWM3001-CDK ground-return correction

During bring-up of an earlier pre-release manufactured prototype, the CDK did not establish the assumed functional ground return through the original power-entry arrangement alone. A temporary ground jumper allowed successful electrical bring-up and measurement of the sensor path.

Before freezing the rev 0.1 open-hardware release, the CDK mating interface and carrier-board connection were corrected so that all relevant CDK ground connections resolve directly to the Thin-Pod `GND` net:

```text
J1_2    GND    ─┐
J1_6    GND_1  ─┤
J10_9   GND_2  ─┤
J10_14  GND_3  ─┼── GND
J10_20  GND_4  ─┤
J10_25  GND_5  ─┘
```

The editable source and fabrication outputs in this repository include that correction. The temporary jumper is part of the prototype history and is **not** an assembly requirement for the released rev 0.1 design.

## Validation status

The first manufactured pre-release prototype established that the core sensor-node approach is electrically viable. Measurements made during prototype bring-up included:

|Validation item|Result|Evidence status|
|-|-|-|
|Manufactured PCB inspection and initial assembly|Completed|Documented in bring-up record and images|
|Power-up after temporary CDK ground-return correction|Successful|Documented prototype result|
|Switched accelerometer supply rail|Approximately 3.35 V measured|Documented prototype result|
|Filtered analogue sensor output|Approximately 1.76 V measured at rest|Documented prototype result|
|Mechanical excitation / tap response|Visible on oscilloscope|Documented prototype result|
|Ground-return correction implemented in released KiCad source and Gerbers|Completed|Included in this release|
|Physical re-test of the corrected copper implementation without jumper|To be recorded after manufacture of the corrected release board|Not claimed as completed|

The prototype measurement evidence supports the power and analogue signal-chain design. The corrected ground-return implementation is present in the released design files; it is not represented as physically re-tested until a board manufactured from these release Gerbers has been assembled and measured.

## Repository contents

```text
Thin-Pod-rev0.1/
├── README.md
├── LICENSE-HARDWARE.md
├── LICENSE-DOCUMENTATION.md
│
├── hardware/
│   ├── source/
│   │   ├── Thin-Pod_rev0.1.kicad_pro
│   │   ├── Thin-Pod_rev0.1.kicad_sch
│   │   ├── Thin-Pod_rev0.1.kicad_pcb
│   │   ├── fp-lib-table
│   │   ├── sym-lib-table
│   │   ├── footprints/
│   │   │   └── ThinPod.pretty/
│   │   │       └── ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod
│   │   └── symbols/
│   │       └── ThinPod.kicad_sym
│   │
│   ├── fabrication/
│   │   ├── gerbers/
│   │   ├── drills/
│   │   └── Thin-Pod_rev0.1_fabrication_outputs.zip
│   │
│   └── bom/
│       ├── Thin-Pod_rev0.1_BOM.md
│       └── Thin-Pod_rev0.1_BOM.csv
│
├── docs/
│   ├── certification-scope.md
│   ├── third-party-components.md
│   ├── footprint-provenance.md
│   ├── prototype-bring-up-and-release-correction.md
│   ├── design-verification.md
│   └── future-revisions.md
│
├── images/
│   ├── certification/
│   │   └── oshwa-uk000091.png
│   ├── prototype-bring-up/
│   ├── gerber-inspection/
│   └── released-design/
│
└── oshwa/
    └── application-record.md
```

## Design and fabrication files

|Resource|Location|Purpose|
|-|-|-|
|Editable KiCad project, schematic and PCB|[`hardware/source/`](hardware/source/)|Preferred source for modification|
|Project-local footprints and symbols|[`hardware/source/`](hardware/source/)|Portable project dependencies|
|Gerber and drill files|[`hardware/fabrication/`](hardware/fabrication/)|PCB fabrication outputs|
|Bill of materials|[`hardware/bom/`](hardware/bom/)|Assembly and sourcing record|
|Certification and design documentation|[`docs/`](docs/)|Scope, provenance and validation record|
|OSHWA application and certification record|[`oshwa/application-record.md`](oshwa/application-record.md)|Public submission details and certification UID|

The editable KiCad source files are the normative design source. Gerbers and drill files are derived fabrication outputs supplied for convenience and reproducibility.

## Bill of materials

The release BOM covers the Thin-Pod rev 0.1 sensor-node carrier board only. It identifies the fitted PCB components, commercial subassemblies, component values, footprints and sourcing information required to reproduce this release.

The Gateway, Gateway-side radio components, ESP32 networking hardware and analytic processing hardware are not part of this BOM because they are outside the rev 0.1 certification scope.

See:

* [`hardware/bom/Thin-Pod_rev0.1_BOM.md`](hardware/bom/Thin-Pod_rev0.1_BOM.md)
* [`hardware/bom/Thin-Pod_rev0.1_BOM.csv`](hardware/bom/Thin-Pod_rev0.1_BOM.csv)

## Third-party components

Thin-Pod rev 0.1 is an open carrier-board design built around commercially available components. The following items are used or interfaced by the design but are not claimed as open-hardware contributions of this project:

|Component|Function in Thin-Pod rev 0.1|Treatment in this release|
|-|-|-|
|Analog Devices ADXL1005-based accelerometer implementation|Analogue vibration sensing|Commercial third-party component; interface documented|
|Qorvo DWM3001-CDK|Commercial module connected through the pod-side mating interface|Commercial third-party development board; not relicensed|
|Pololu S7V8F3|Regulated 3.3 V supply module|Commercial third-party module|
|ZVP2106A and standard passive/protection components|Power switching and analogue support circuitry|Commercial bought-in components identified in BOM|

Vendor documentation is referenced for sourcing and interface understanding; vendor CAD files, proprietary board layouts and vendor 3D models are not distributed under the Thin-Pod hardware licence.

## CDK mating-interface footprint provenance

The published footprint:

```text
hardware/source/footprints/ThinPod.pretty/
└── ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod
```

is a Thin-Pod project-local mating-interface footprint for the commercially supplied DWM3001-CDK. It is intended to describe only the mechanical and electrical connection required by the Thin-Pod carrier PCB.

The release footprint was independently constructed in KiCad using standard connector geometry, publicly available interface information and mechanical verification against a purchased CDK unit. It does not distribute a Qorvo-supplied CAD footprint, CDK PCB design or vendor 3D model.

## Opening and modifying the design

The source design is maintained in KiCad 10.0.1. The project is intended to be portable through project-local symbol and footprint tables.

A reproducible modification workflow is:

1. Open `hardware/source/Thin-Pod_rev0.1.kicad_pro` in KiCad 10.0.1 or a compatible later version.
2. Confirm that project-local symbol and footprint libraries resolve through `sym-lib-table` and `fp-lib-table`.
3. Run schematic ERC and PCB DRC before fabrication.
4. Regenerate Gerber and drill outputs after any accepted design change.
5. Inspect copper, solder mask, silkscreen, board outline and drill alignment in KiCad Gerber Viewer before ordering.

## Documentation and verification record

|Document|Content|
|-|-|
|[`docs/certification-scope.md`](docs/certification-scope.md)|Formal inclusion and exclusion boundary for certification|
|[`docs/third-party-components.md`](docs/third-party-components.md)|Commercial component and vendor-material notice|
|[`docs/footprint-provenance.md`](docs/footprint-provenance.md)|Origin and verification of the CDK mating-interface footprint|
|[`docs/prototype-bring-up-and-release-correction.md`](docs/prototype-bring-up-and-release-correction.md)|Prototype test result and ground-return design correction|
|[`docs/design-verification.md`](docs/design-verification.md)|Electrical and fabrication verification record|
|[`docs/future-revisions.md`](docs/future-revisions.md)|Items explicitly outside this release|

## Licensing

|Material|Licence|File|
|-|-|-|
|Creator-designed hardware source, including schematics, PCB layout and Thin-Pod-authored footprint files|CERN Open Hardware Licence Version 2 — Weakly Reciprocal (`CERN-OHL-W-2.0`)|[`LICENCE-HARDWARE.md`](LICENCE-HARDWARE.md)|
|Creator-authored documentation, diagrams and photographs|Creative Commons Attribution 4.0 International (`CC-BY-4.0`)|[`LICENCE-DOCS.md`](LICENCE-DOCS.md)|
|Software / firmware|None required or supplied for the certified rev 0.1 hardware scope|Not applicable|

Commercial components, vendor documentation and any third-party intellectual property remain subject to their respective owners' terms and are not covered by the Thin-Pod licences. The OSHWA certification mark is used to identify this certified release and is not relicensed as Thin-Pod documentation.

## OSHWA certification record

**Thin-Pod rev 0.1 is certified open source hardware under OSHWA UID UK000091.** The certification mark displayed near the top of this README identifies this specific certified release and links to the public OSHWA record.

|Field|Value|
|-|-|
|Project name|Thin-Pod|
|Project version|0.1|
|Primary project type|Electronics|
|Certification scope|Vibration-sensor node carrier PCB|
|Certification date|28 May 2026|
|Hardware licence|CERN-OHL-W-2.0|
|Documentation licence|CC-BY-4.0|
|Software licence|No software required or supplied for the certified hardware scope|
|OSHWA UID|UK000091|
|Official record|[certification.oshwa.org/uk000091](https://certification.oshwa.org/uk000091.html)|

The certification UID and matching mark are retained in this README and in [`oshwa/application-record.md`](oshwa/application-record.md), keeping the boundary of the certified rev 0.1 release explicit.

## Safety and regulatory note

Thin-Pod rev 0.1 is experimental sensing hardware. It has not been certified for installation on safety-critical machinery, hazardous environments or regulated industrial monitoring applications. Appropriate isolation, current-limited bench testing, secure mechanical mounting and safe working practices are required during evaluation.

This open-hardware release does not imply EMC, radio, electrical-safety or product-regulatory approval. Any commercially supplied radio or development module remains subject to its own vendor documentation and applicable regulatory conditions.

## References

* [Thin-Pod OSHWA Certification Record — UK000091](https://certification.oshwa.org/uk000091.html)
* [OSHWA Certification Mark Usage Guide](https://certification.oshwa.org/mark-usage.html)
* [OSHWA Certification Requirements](https://certification.oshwa.org/requirements.html)
* [OSHWA Certification Documentation Process](https://certification.oshwa.org/process/documentation.html)
* [OSHWA Certification Application](https://application.oshwa.org/apply)
* [Qorvo DWM3001-CDK product page](https://www.qorvo.com/products/p/DWM3001CDK)
* [Analog Devices ADXL1005 product page](https://www.analog.com/en/products/adxl1005.html)

\---

**Thin-Pod rev 0.1** is a deliberately bounded first open-hardware release: a documented vibration-sensor node PCB with a clear commercial-module boundary, an independently authored interface footprint and a release design that incorporates the principal lesson discovered during prototype bring-up.

