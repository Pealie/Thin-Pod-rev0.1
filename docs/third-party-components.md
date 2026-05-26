# Thin-Pod rev 0.1: Third-Party Components and Licence Boundary

**Project:** Thin-Pod  
**Hardware release:** rev 0.1  
**Repository:** `Pealie/Thin-Pod-rev0.1`  
**Prepared by:** Neil Thomson / Pealie  
**Document status:** Release documentation  
**Document date:** 26 May 2026  

## 1. Purpose

Thin-Pod rev 0.1 is an open-hardware carrier PCB assembled using commercially available electronic parts and development modules. This document distinguishes:

1. Thin-Pod-created design material released as open hardware;
2. proprietary commercial components required to reproduce or use the assembly;
3. third-party CAD/design-file dependencies included under their own licence terms; and
4. vendor materials that are referenced but not redistributed.

A complete BOM must include required proprietary components. Their presence in the BOM is a statement of reproducibility, not a claim that those products have become open source.

## 2. OSHWA component principle

OSHWA requires creator-controlled contributions to be open and calls for best efforts to distinguish proprietary third-party components. OSHWA recognises that open-hardware projects routinely use externally supplied closed components, such as integrated circuits, where those parts are outside the creator’s control and supporting datasheets are accessible and shareable.

Accordingly, Thin-Pod rev 0.1 openly releases its carrier-board design and documentation while identifying commercial components accurately and without attempting to relicense them.

## 3. Thin-Pod-created open-hardware contribution

The following material is intended to be released under **CERN-OHL-W-2.0**:

| Thin-Pod-created material | Description |
|---|---|
| Carrier-board schematic | Electrical design of the rev 0.1 sensor-node PCB |
| Carrier-board PCB layout | Copper, mask, legend, board outline and drilling design |
| Fabrication outputs | Gerbers and drill outputs generated from the released PCB source |
| Thin-Pod-authored CDK mating-interface footprint | Independently produced carrier-board interface representation for the DWM3001-CDK |
| Other project-authored hardware source, where present | Custom material whose provenance is recorded as Thin-Pod-created |

Creator-authored explanatory documents, diagrams and images are licensed separately under **CC-BY-4.0**.

## 4. Commercial components included in the assembly

The principal commercial third-party parts are listed below. They remain products of their respective manufacturers and are not claimed as open-hardware contributions of Thin-Pod.

| Ref. | Component | Manufacturer | Function in Thin-Pod rev 0.1 | Open-hardware status in this release | Public supporting information |
|---|---|---|---|---|---|
| `U2` | ADXL1005-based accelerometer implementation / evaluation-board interface | Analog Devices | Wide-bandwidth analogue vibration sensing | Proprietary third-party sensor or module; not relicensed | ADXL1005 product page and datasheet |
| `U4` | S7V8F3 fixed 3.3 V step-up/step-down regulator module | Pololu | Regulated local power supply | Proprietary third-party module; not relicensed | Pololu product page |
| `U5` | DWM3001-CDK development board | Qorvo | Commercial pod-side MCU/UWB board connected through the carrier interface | Proprietary third-party development board; not relicensed | Qorvo product documentation |
| `U3` | ZVP2106A P-channel MOSFET | Diodes Incorporated | High-side switching of sensor supply rail | Standard proprietary commercial semiconductor; identified in BOM | Manufacturer product information |
| `D1` | 1N5817 Schottky diode | Commercially sourced | Series reverse-polarity protection | Standard proprietary commercial component; identified in BOM | Manufacturer datasheet |
| `J1`, `F1`, `R1–R4`, `C1–C3`, test hardware | Standard interconnect, protection and passive parts | Commercially sourced | Supporting assembly functions | Standard commercial components; identified in BOM | Orderable part information / manufacturer datasheets where applicable |

The precise orderable part numbers and fitted implementation for parts marked for confirmation in the BOM must be completed before the OSHWA submission is frozen.

## 5. U5 / Qorvo DWM3001-CDK treatment

`U5` is correctly included in the BOM because the carrier PCB is designed to interface with it and a functional assembled prototype depends upon that commercial module. The following boundary applies:

| Material | Treatment |
|---|---|
| Qorvo DWM3001-CDK physical development board | Listed in BOM as commercial third-party hardware; not claimed as open |
| Public Qorvo product and interface documentation | Referenced for component identification and interface information |
| Earlier Qorvo-supplied CAD footprint, if held during development | Not distributed in this release repository |
| Thin-Pod CDK mating-interface footprint | Independently authored project file released under CERN-OHL-W-2.0 |
| Thin-Pod PCB copper/net connection to the CDK pads | Creator-designed hardware source released under CERN-OHL-W-2.0 |

The released rev 0.1 design connects the relevant CDK ground pins directly to carrier `GND`, incorporating the ground-return correction discovered in prototype bring-up.

## 6. U2 and U4 footprint files sourced through SnapEDA / SnapMagic

The KiCad footprint files associated with the ADXL1005 evaluation-board interface and the Pololu regulator-module interface were sourced from the SnapEDA library on the web, now presented under the SnapMagic service name.

SnapMagic defines downloaded CAD models, schematics, layouts and related CAD files as **Design Files**. Its published Terms of Use state that individual Design Files downloaded from SnapMagic are licensed under **Creative Commons Attribution-ShareAlike 4.0 International (`CC BY-SA 4.0`)**, together with **Design Exception 1.0**. The Design Exception permits the design, manufacture, use and distribution of circuit-board designs and circuit boards formed by combining SnapMagic Design Files with other chosen circuit elements, and permits those combinations to be conveyed under terms chosen by the designer.

SnapMagic’s terms also state that public files containing Design Files remain subject to the restriction that no more than ten Design Files may be distributed through one location without prior written permission.

### SnapEDA / SnapMagic design-file inventory for this release

| Standalone design file | Used for | Intended repository treatment | Standalone file licence treatment |
|---|---|---|---|
| `AnalogDevices_EVAL-ADXL100xZ_20p35mm.kicad_mod` | `U2` ADXL1005-based evaluation-board / module interface | Include only as required source dependency in a clearly identified third-party library folder | Third-party SnapMagic Design File: CC BY-SA 4.0 + Design Exception 1.0 |
| `Pololu_S7V8F3_S7V8x_Module.kicad_mod` | `U4` regulator-module interface | Include only as required source dependency in a clearly identified third-party library folder | Third-party SnapMagic Design File: CC BY-SA 4.0 + Design Exception 1.0 |

No claim is made that these two standalone files were independently authored by Thin-Pod. They should not be placed inside a folder or licence notice that suggests that they are covered solely as Thin-Pod-created CERN-OHL-W material.

### Recommended directory separation

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

The standalone SnapEDA / SnapMagic design files should be accompanied by a local `NOTICE.md` recording the file names, source, applicable terms and the count of SnapMagic Design Files distributed in the repository.

## 7. SnapEDA / SnapMagic distribution count

At the time of preparing this document, the intended published SnapMagic-derived footprint set contains:

| Count | File |
|---:|---|
| 1 | `AnalogDevices_EVAL-ADXL100xZ_20p35mm.kicad_mod` |
| 2 | `Pololu_S7V8F3_S7V8x_Module.kicad_mod` |

**Total SnapMagic Design Files intended for distribution in this repository: 2.**

This remains below the ten-file public-distribution threshold specified in the SnapMagic Terms of Use. If further SnapEDA / SnapMagic files are later included, this table must be revised before public release.

## 8. Files deliberately not distributed

The following material is excluded from the Thin-Pod rev 0.1 repository unless a separate redistribution-compatible licence is subsequently documented:

| Excluded material | Reason |
|---|---|
| `MODULE_DWM3001CDK.kicad_mod` | Earlier Qorvo-supplied/development CDK footprint; replaced by the independently authored Thin-Pod mating-interface footprint |
| `Qorvo_DWM3001C_kicad9_flip.kicad_mod` | DWM3001C module footprint not required for this CDK-based rev 0.1 board and not part of the release scope |
| `XCVR_DWM3001C.kicad_mod` | DWM3001C-related source not required for the released rev 0.1 carrier PCB |
| Qorvo CDK or DWM3001C vendor 3D models | Not needed to fabricate the board and not distributed without an explicit compatible licence |
| Gateway-related hardware source and CAD files | Outside the Thin-Pod rev 0.1 sensor-node certification scope |
| NUCLEO or ESP32-C6 carrier files | Outside the Thin-Pod rev 0.1 sensor-node certification scope |

## 9. Repository licence matrix

| Repository material | Responsible source | Licence / rights statement |
|---|---|---|
| `hardware/source/Thin-Pod_rev0.1.*` and Thin-Pod-authored hardware source | Thin-Pod / Neil Thomson | CERN-OHL-W-2.0 |
| `hardware/fabrication/` outputs generated from the released design | Thin-Pod / Neil Thomson, incorporating declared dependencies | Released with the Thin-Pod hardware design subject to the recorded third-party dependency boundary |
| `docs/`, creator-authored diagrams and creator-owned photographs | Thin-Pod / Neil Thomson | CC-BY-4.0 |
| `ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod` | Thin-Pod / Neil Thomson | CERN-OHL-W-2.0 |
| Standalone SnapEDA / SnapMagic footprint files | SnapMagic Design Files / contributing source as applicable | CC BY-SA 4.0 + SnapMagic Design Exception 1.0; retained as third-party source |
| Commercial purchased components and vendor documentation | Respective manufacturers | Referenced only; not relicensed by Thin-Pod |

## 10. Publication checks

Before public OSHWA submission, the release must confirm:

- [ ] The BOM lists all commercial components required to reproduce the assembly.
- [ ] Each commercial module is marked as a third-party proprietary component.
- [ ] No Qorvo-supplied footprint or vendor 3D model is distributed.
- [ ] The Thin-Pod CDK mating-interface footprint is the footprint used by the released PCB.
- [ ] Each SnapEDA / SnapMagic footprint included in the repository has source evidence retained.
- [ ] The SnapMagic Design File count remains at or below ten, unless written permission is obtained.
- [ ] A third-party footprint `NOTICE.md` is present alongside any distributed SnapMagic files.
- [ ] The project licences do not purport to relicense commercial products or standalone third-party files contrary to their own terms.

## References

- OSHWA, *Certification Requirements*: <https://certification.oshwa.org/requirements.html>
- OSHWA, *Documentation*: <https://certification.oshwa.org/process/documentation.html>
- SnapMagic / SnapEDA, *Website Terms of Use*, sections 4 and 5: <https://www.snapeda.com/about/terms/>
- Creative Commons, *CC BY-SA 4.0*: <https://creativecommons.org/licenses/by-sa/4.0/>
- Qorvo, *DWM3001CDK product page*: <https://www.qorvo.com/products/p/DWM3001CDK>
- Analog Devices, *ADXL1005 product page*: <https://www.analog.com/en/products/adxl1005.html>
- Pololu, *3.3 V Step-Up/Step-Down Voltage Regulator S7V8F3*: <https://www.pololu.com/product/2122>
