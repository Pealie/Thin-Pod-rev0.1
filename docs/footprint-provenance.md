# Thin-Pod rev 0.1: Footprint Provenance and Publication Record

**Project:** Thin-Pod  
**Hardware release:** rev 0.1  
**Repository:** `Pealie/Thin-Pod-rev0.1`  
**Prepared by:** Neil Thomson / Pealie  
**Document status:** Hardware-source provenance record  
**Document date:** 26 May 2026  
**EDA tool:** KiCad 10.0.1  

## 1. Purpose

A public open-hardware repository must allow the released PCB to be opened, modified and fabricated without introducing ambiguity about the ownership or licence status of its source dependencies. This document records the provenance and publication treatment of non-standard footprints used in Thin-Pod rev 0.1.

The key distinction is between:

- a Thin-Pod-authored interface footprint created for the released board;
- third-party CAD footprint files downloaded under their own published licensing terms; and
- vendor-supplied files excluded from release because they are not required or do not have a documented redistribution basis.

## 2. Published footprint inventory

| Footprint file | Board function | Provenance category | Publication status | Licence treatment |
|---|---|---|---|---|
| `ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod` | Qorvo DWM3001-CDK mating interface at `U5` | Thin-Pod-authored interface footprint | Publish | CERN-OHL-W-2.0 |
| `AnalogDevices_EVAL-ADXL100xZ_20p35mm.kicad_mod` | ADXL1005-based evaluation-board / module interface at `U2` | Downloaded from SnapEDA / SnapMagic design-file library | Publish as required third-party dependency, subject to the recorded terms | CC BY-SA 4.0 + SnapMagic Design Exception 1.0 |
| `Pololu_S7V8F3_S7V8x_Module.kicad_mod` | Pololu regulator-module interface at `U4` | Downloaded from SnapEDA / SnapMagic design-file library | Publish as required third-party dependency, subject to the recorded terms | CC BY-SA 4.0 + SnapMagic Design Exception 1.0 |
| `PFET_ZVP2106A_GSD_THT.kicad_mod` | ZVP2106A THT pin/lead arrangement at `U3` | To be confirmed before public submission | Publish only after provenance confirmation | Pending confirmation |

Standard KiCad library footprints used for ordinary passives, protection parts, connectors, mounting holes and test points need not be duplicated in the project-local library unless portability requires it. If copied into the repository, their originating licence and attribution must be preserved.

## 3. Thin-Pod-authored DWM3001-CDK mating-interface footprint

### 3.1 Released file

```text
hardware/source/footprints/ThinPod.pretty/
└── ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod
```

### 3.2 Function

The footprint represents the **carrier-board mating interface** required to mount and connect a commercially supplied Qorvo DWM3001-CDK to Thin-Pod. It is not a representation of the CDK’s internal PCB design, radio module layout or vendor-owned design source.

It defines the physical connection points needed by the Thin-Pod carrier board, including the J1 and J10 interface pads and the relevant mechanical support geometry.

### 3.3 Authorship and reconstruction method

The released DWM3001-CDK mating-interface footprint was created for Thin-Pod rather than redistributed from a Qorvo CAD library file. Its construction basis is:

- standard connector geometry where applicable;
- publicly available Qorvo product/interface information;
- physical verification against a commercially purchased DWM3001-CDK; and
- Thin-Pod-specific net and ground-return requirements established during prototype bring-up.

The original Qorvo-supplied CDK footprint used during earlier development is not included in the rev 0.1 OSHWA-facing source package.

### 3.4 Corrected ground-return pads

The released footprint makes the required physical pads available for the corrected carrier-board ground arrangement:

| CDK interface pad | Function in released Thin-Pod design |
|---|---|
| `J1_2` | Connected to Thin-Pod `GND` |
| `J10_6` | Connected to Thin-Pod `GND` |
| `J10_9` | Connected to Thin-Pod `GND` |
| `J10_14` | Connected to Thin-Pod `GND` |
| `J10_20` | Connected to Thin-Pod `GND` |
| `J10_25` | Connected to Thin-Pod `GND` |
| `J10_15` | Analogue ADC/sensor signal interface, as defined by the schematic |
| `J10_24` | PFET/control interface, as defined by the schematic |

A footprint supplies pads and mechanical geometry; the electrical bonding of the ground pads is implemented in the schematic/PCB net assignment and copper design.

### 3.5 KiCad file identity and UUIDs

The released footprint is saved in a KiCad 10-compatible `.kicad_mod` format. Its internal KiCad UUIDs are distinct from the earlier development footprint. UUIDs assist KiCad in tracking pads, graphical elements and text objects; they do not by themselves establish authorship or dimensional accuracy.

Independent authorship rests on the reconstruction method and exclusion of the earlier vendor-supplied CAD file. Dimensional correctness rests on mechanical verification, PCB inspection and eventual assembly fit testing.

## 4. SnapEDA / SnapMagic-derived footprint dependencies

### 4.1 Applicable published terms

The footprint files for the ADXL1005 evaluation-board/module interface and Pololu regulator-module interface were sourced from the SnapEDA web library, now operated under the SnapMagic name.

SnapMagic’s published Terms of Use define these downloaded CAD files as **Design Files**. The terms state that individual Design Files downloaded from SnapMagic are licensed under **Creative Commons Attribution-ShareAlike 4.0 International (`CC BY-SA 4.0`)** and under **Design Exception 1.0**.

The Design Exception permits SnapMagic Design Files to be combined with other selected circuit elements in a circuit-board design, and permits the resulting combination and manufactured board to be conveyed under terms chosen by the designer. The terms also impose a public-distribution restriction of no more than ten Design Files through any one location without prior written permission.

### 4.2 Third-party folder structure

To make the licence boundary unambiguous, the SnapEDA / SnapMagic files should be stored separately from the Thin-Pod-authored footprint library:

```text
hardware/source/footprints/
├── ThinPod.pretty/
│   └── ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod
│
└── third-party-snapeda.pretty/
    ├── AnalogDevices_EVAL-ADXL100xZ_20p35mm.kicad_mod
    ├── Pololu_S7V8F3_S7V8x_Module.kicad_mod
    └── NOTICE.md
```

### 4.3 Attribution and file treatment

The standalone SnapEDA / SnapMagic footprint files are not represented as Thin-Pod-created source. They remain third-party Design Files under their stated licensing treatment. Their inclusion is limited to the source material needed to open, edit and reproduce the released Thin-Pod PCB.

The combined Thin-Pod PCB design may be released under the Thin-Pod hardware licence in reliance on the SnapMagic Design Exception, while this document and the neighbouring notice preserve the origin and treatment of the standalone source dependencies.

### 4.4 Source evidence to retain

Prior to release freeze, retain sufficient provenance evidence for each downloaded footprint:

| Evidence item | Purpose |
|---|---|
| SnapEDA / SnapMagic model-page URL or screenshot | Records public source and associated part identity |
| Original downloaded archive, where retained | Records the file as obtained |
| File hash of published `.kicad_mod` file | Identifies the precise distributed dependency |
| Copy or dated reference to applicable SnapMagic Terms of Use | Records the terms relied upon at release time |

## 5. Excluded vendor or unrelated CAD files

The following files are excluded from Thin-Pod rev 0.1 publication:

| File or material | Exclusion reason |
|---|---|
| `MODULE_DWM3001CDK.kicad_mod` | Earlier Qorvo-supplied/development CDK footprint; replaced by the independently authored Thin-Pod mating-interface footprint |
| `Qorvo_DWM3001C_kicad9_flip.kicad_mod` | DWM3001C module footprint not required for this CDK-based rev 0.1 board and not part of the release scope |
| `XCVR_DWM3001C.kicad_mod` | DWM3001C-related source not required for the released rev 0.1 carrier PCB |
| Qorvo CDK or DWM3001C vendor 3D models | Not needed to fabricate the board and not distributed without an explicit compatible licence |
| Gateway-module footprints | Outside the Thin-Pod rev 0.1 sensor-node certification scope |

## 6. Footprint verification record

### 6.1 DWM3001-CDK mating interface

| Verification action | Status | Record |
|---|---|---|
| New project-local footprint name assigned | Completed | `ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod` |
| KiCad 10-compatible file format repaired/saved | Completed in working file; final project-local saved copy to be retained | KiCad 10.0.1 |
| New UUID set distinct from earlier footprint | Observed | Supporting identity evidence only |
| Required CDK ground pads represented | Completed | J1/J10 ground pads included |
| Ground pads assigned to common `GND` in released design | Completed in corrected source/Gerbers | Confirm against final KiCad source before tag |
| Gerbers regenerated from corrected release design | Completed in working release set | Final files to be committed under `hardware/fabrication/` |
| Top silkscreen viewed independently of fabrication layer | Completed | No visible unacceptable legend overlap in inspected view |
| Mechanical fit on newly fabricated corrected release PCB | Pending if no corrected board has yet been manufactured | Must not be claimed as completed until tested |

### 6.2 SnapEDA / SnapMagic dependencies

| Verification action | Status required before submission |
|---|---|
| Confirm the two named local files are the downloaded SnapEDA / SnapMagic files used in the PCB | Required |
| Place them in `third-party-snapeda.pretty/` rather than `ThinPod.pretty/` | Required |
| Add local `NOTICE.md` recording source/licence treatment | Required |
| Confirm total distributed SnapMagic Design Files does not exceed ten | Required |
| Confirm no excluded vendor CAD material remains embedded or published unintentionally | Required |

## 7. Recommended `NOTICE.md` for the third-party footprint folder

The following notice may be placed at:

```text
hardware/source/footprints/third-party-snapeda.pretty/NOTICE.md
```

```markdown
# Third-Party SnapEDA / SnapMagic Footprints

The following CAD footprint files were downloaded from the SnapEDA / SnapMagic
design-file library and are included because they are required to modify and
reproduce the Thin-Pod rev 0.1 PCB source:

- `AnalogDevices_EVAL-ADXL100xZ_20p35mm.kicad_mod`
- `Pololu_S7V8F3_S7V8x_Module.kicad_mod`

These standalone files are third-party Design Files and are not claimed as
independently authored Thin-Pod source. SnapMagic's published Terms of Use
license individual downloaded Design Files under Creative Commons
Attribution-ShareAlike 4.0 International together with the SnapMagic Design
Exception 1.0.

The complete Thin-Pod rev 0.1 circuit-board design combines these dependency
files with Thin-Pod-created circuit and PCB design material. The combined
Thin-Pod board design is released under CERN-OHL-W-2.0 in reliance on the
SnapMagic Design Exception 1.0.

This repository distributes two SnapMagic Design Files, below the published
restriction against distributing more than ten Design Files through one
location without prior written permission.

Applicable terms:
- https://www.snapeda.com/about/terms/
- https://creativecommons.org/licenses/by-sa/4.0/
```

## 8. Release sign-off checklist

- [ ] `ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod` is the footprint actually used by the released PCB.
- [ ] The Qorvo-supplied/development CDK footprint is absent from the public repository.
- [ ] The two SnapEDA / SnapMagic footprint files are isolated in the third-party folder with their notice.
- [ ] The PFET footprint provenance is confirmed and documented.
- [ ] Any copied standard KiCad footprint retains its originating licence/attribution, or remains an external library dependency.
- [ ] The final Gerbers were generated from the same clean footprint set as the published editable PCB source.
- [ ] DRC, layer inspection and final source-file opening checks are recorded before submission freeze.

## References

- SnapMagic / SnapEDA, *Website Terms of Use*: <https://www.snapeda.com/about/terms/>
- Creative Commons, *CC BY-SA 4.0*: <https://creativecommons.org/licenses/by-sa/4.0/>
- OSHWA, *Certification Requirements*: <https://certification.oshwa.org/requirements.html>
- OSHWA, *Documentation*: <https://certification.oshwa.org/process/documentation.html>
- Qorvo, *DWM3001-CDK product page*: <https://www.qorvo.com/products/p/DWM3001CDK>
