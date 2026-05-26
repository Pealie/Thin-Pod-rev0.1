# Thin-Pod rev 0.1 Release Manifest

**Release status:** Submitted for OSHWA self-certification  
**Submission date:** 26 May 2026  
**OSHWA UID:** Pending receipt or public directory confirmation

## Release Identification

|Item|Release record|
|-|-|
|Release|Thin-Pod rev 0.1|
|Submission tag|`v0.1-oshwa-submission`|
|KiCad version|`10.0.1`|
|PCB source file|`Thin-Pod\_rev0.1.kicad\_pcb`|
|Schematic file|`Thin-Pod\_rev0.1.kicad\_sch`|
|Gerber archive|`Thin-Pod\_rev0.1\_fabrication\_outputs.zip`|
|BOM files|`Thin-Pod\_rev0.1\_BOM.md` / `Thin-Pod\_rev0.1\_BOM.csv`|
|CDK footprint|`ThinPod\_DWM3001CDK\_Mating\_Interface\_revA`|
|Ground correction|Included in released copper design|
|Physical evidence|Prototype analogue bring-up validated; corrected release PCB not yet physically re-tested|

## Release Scope

This manifest identifies the fabrication and source-file set associated with the **Thin-Pod rev 0.1** OSHWA certification submission. The released item is the open-hardware vibration-sensor node carrier PCB, including protected power entry, regulated and switched sensor supply, the ADXL1005-based analogue sensing interface, test points and the corrected carrier-board interface for a commercially supplied Qorvo DWM3001-CDK.

The Thin-Pod Gateway, implemented UWB communications, DSP, TinyML and any predictive-maintenance system claim are outside the rev 0.1 certification scope.

## CDK Ground-Return Correction

The released rev 0.1 copper design incorporates the ground-return correction identified during prototype bring-up. The relevant Qorvo DWM3001-CDK interface ground connections are assigned to the Thin-Pod carrier-board `GND` net:

```text
J1\_2    GND    ─┐
J10\_6   GND\_1  ─┤
J10\_9   GND\_2  ─┤
J10\_14  GND\_3  ─┼── GND
J10\_20  GND\_4  ─┤
J10\_25  GND\_5  ─┘
```

A temporary ground jumper was used during bring-up of an earlier pre-release prototype. It is retained as part of the development record, but is not an assembly requirement of the released rev 0.1 PCB.

## Verification Boundary

The pre-release prototype established successful analogue sensor-node proof of life after the temporary ground-return correction, including:

* successful powered bring-up;
* approximately 3.35 V on the switched accelerometer supply rail;
* approximately 1.76 V at the filtered analogue output node at rest; and
* visible response to mechanical excitation on the oscilloscope.

The corrected ground-return arrangement is implemented in the released editable source and Gerber outputs. Physical testing of a PCB manufactured from these corrected release Gerbers, operating without the temporary ground jumper, has not yet been claimed and will be recorded separately when completed.

## Release Files

The following files form the principal source and fabrication record for the submitted release:

```text
hardware/
├── source/
│   ├── Thin-Pod\_rev0.1.kicad\_pro
│   ├── Thin-Pod\_rev0.1.kicad\_sch
│   ├── Thin-Pod\_rev0.1.kicad\_pcb
│   └── footprints/
│       └── ThinPod.pretty/
│           └── ThinPod\_DWM3001CDK\_Mating\_Interface\_revA.kicad\_mod
│
├── fabrication/
│   ├── Thin-Pod\_rev0.1\_fabrication\_outputs.zip
│   └── RELEASE-MANIFEST.md
│
└── bom/
    ├── Thin-Pod\_rev0.1\_BOM.md
    └── Thin-Pod\_rev0.1\_BOM.csv
```

The editable KiCad source files are the normative design source. Gerber and drill files are derived fabrication outputs supplied for manufacture and release reproducibility.

## Associated Documentation

|Document|Purpose|
|-|-|
|`README.md`|Public release landing page and certification summary|
|`docs/certification-scope.md`|Formal certification inclusion and exclusion boundary|
|`docs/third-party-components.md`|Commercial component and licensing boundary|
|`docs/footprint-provenance.md`|Source and licence treatment of non-standard footprints|
|`docs/prototype-bring-up-and-release-correction.md`|Prototype result and incorporated ground-return correction|
|`docs/design-verification.md`|Design and fabrication verification record|
|`docs/future-revisions.md`|Deferred hardware and system scope|
|`oshwa/application-record.md`|Public record of the submitted OSHWA application|

## Future Verification Update

Following manufacture of a board from the corrected rev 0.1 Gerber archive, this manifest may be supplemented with a dated verification update confirming:

1. continuity of the corrected CDK ground connections;
2. correct CDK power-up without the prototype ground jumper;
3. stable regulated and switched supply rails; and
4. repeated analogue vibration response from the released copper implementation.

Any future electrical or mechanical change to the released PCB design should be assigned a subsequent hardware revision rather than silently modifying the submitted rev 0.1 record.

