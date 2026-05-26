# Thin-Pod rev 0.1: OSHWA Certification Scope

**Project:** Thin-Pod  
**Hardware release:** rev 0.1  
**Repository:** `Pealie/Thin-Pod-rev0.1`  
**Prepared by:** Neil Thomson / Pealie  
**Document status:** Submission documentation  
**Document date:** 26 May 2026  

## 1. Purpose

This document defines the precise hardware, documentation and licensing boundary for the Open Source Hardware Association (OSHWA) self-certification application for **Thin-Pod rev 0.1**.

Thin-Pod rev 0.1 is deliberately presented as a bounded first open-hardware release. It is not a claim that the complete future Thin-Pod monitoring system, Gateway, radio communications path, DSP processing chain or TinyML functionality has been completed or certified.

## 2. Certified item definition

The item submitted for certification is:

> **Thin-Pod rev 0.1: an open-hardware vibration-sensor node carrier PCB for rotating-machinery experiments.**

The board provides a physical and electrical platform for an analogue vibration-sensing path built around an ADXL1005-based accelerometer implementation and an interface to a commercially supplied Qorvo DWM3001-CDK development board. It includes protected power input, regulated and switched sensor power, analogue signal conditioning, test points and the corrected ground-return interface required by the CDK connection.

## 3. Included in the certification scope

The following creator-controlled design contributions form the certified hardware release.

| Scope element | Description | Release material |
|---|---|---|
| Carrier PCB | Two-layer Thin-Pod sensor-node printed circuit board design | Editable KiCad PCB source and fabrication outputs |
| Power-entry path | RAW_IN connector provision, fuse provision and series reverse-polarity protection | Schematic, PCB, BOM and fabrication outputs |
| Regulated supply | 3.3 V supply interface using the specified commercial regulator module | Schematic, PCB and BOM |
| Switched sensor rail | PFET-controlled supply path for the accelerometer section | Schematic, PCB and BOM |
| Sensor interface | Electrical and mechanical interface for an ADXL1005-based analogue accelerometer implementation | Schematic, PCB, BOM and applicable footprint source |
| Analogue signal path | Raw sensor output, filtering / ADC node and accessible measurement points | Schematic, PCB and verification record |
| Test-point provision | TP1–TP7 for power and analogue bring-up measurements | Schematic, PCB and BOM |
| CDK mating interface | Thin-Pod-authored interface footprint and carrier connections for a Qorvo DWM3001-CDK | Project-local footprint source, PCB and BOM |
| Corrected CDK ground return | Carrier-board connection of the relevant J1 and J10 CDK ground pins to the Thin-Pod GND net | Schematic, PCB and released Gerbers |
| Documentation | Scope, component boundary, footprint provenance, bring-up history, verification status and future-revision record | Files under `docs/` |
| Fabrication files | Gerber and drill outputs generated from the rev 0.1 release design | Files under `hardware/fabrication/` |

## 4. Explicitly excluded from the certification scope

The following items are outside the Thin-Pod rev 0.1 certification boundary.

| Excluded item | Reason for exclusion |
|---|---|
| Qorvo DWM3001-CDK as a product | Commercial third-party development board; used by the assembly but not claimed as creator-designed open hardware |
| Analog Devices ADXL1005 device or evaluation board as a product | Commercial third-party sensor/device implementation; used by the assembly but not claimed as creator-designed open hardware |
| Pololu S7V8F3 regulator module as a product | Commercial third-party module; specified in the BOM but not claimed as open hardware |
| Thin-Pod Gateway hardware | Separate evolving board design and future certification candidate |
| STM32 NUCLEO-N657X0-Q | Gateway-related commercial development hardware, not part of this node release |
| ESP32-C6 or other onward-networking hardware | Gateway/networking functionality outside this node release |
| Implemented UWB transmission | No certification claim is made for a completed wireless communications path |
| Gateway-side DSP or TinyML | Analytic functionality outside this hardware release |
| Predictive-maintenance or fault-detection performance | No performance claim forms part of this certified carrier-PCB release |
| Regulatory approvals | OSHWA certification is not EMC, radio, safety or product-compliance approval |

## 5. Open-hardware and third-party component boundary

Thin-Pod rev 0.1 includes commercial components because an open PCB design must identify the actual parts required to reproduce its assembly. Inclusion of a component in the BOM does not imply that the component itself is open hardware or relicensed by the project.

The creator-controlled contribution consists of the Thin-Pod carrier-board circuit, PCB layout, Thin-Pod-authored interface footprint material, fabrication outputs and creator-authored documentation. Commercial modules and components remain subject to their suppliers’ terms.

This treatment follows OSHWA’s Creator Contribution principle: creator-controlled contributions are made open, while third-party proprietary components outside the creator’s control are identified distinctly and supported by publicly accessible vendor information.

## 6. Release design and pre-release prototype distinction

A physically assembled pre-release prototype was used for bring-up of the Thin-Pod concept. During that work, the CDK interface was found to require an explicit ground-return connection. The prototype was successfully tested after adding a temporary ground jumper.

The open-hardware **rev 0.1 release design** incorporates that finding in the source files and fabrication outputs. The released PCB connects the relevant CDK ground pins directly to the Thin-Pod `GND` net:

```text
J1_2    GND    ─┐
J10_6   GND_1  ─┤
J10_9   GND_2  ─┤
J10_14  GND_3  ─┼── GND
J10_20  GND_4  ─┤
J10_25  GND_5  ─┘
```

The temporary wire is therefore part of the development and prototype record; it is not an assembly instruction or BOM item for the released rev 0.1 design.

Because no OSHWA submission had been made and no certification release had been frozen before this correction, inclusion of the corrected ground return in rev 0.1 is a pre-release engineering correction rather than an undocumented change to an already-certified product.

## 7. Software scope

No firmware or software is required or supplied as part of the certified Thin-Pod rev 0.1 carrier-PCB scope.

The board provides an analogue sensor path and a physical/electrical connection for a commercial CDK module. Future firmware enabling sampling, UWB packet transport, Gateway processing or analytics is outside this application and will be documented separately if released.

**OSHWA application software licence selection:** `No software`.

## 8. Release licensing

| Material | Licence | Application to this release |
|---|---|---|
| Thin-Pod-created hardware source | CERN Open Hardware Licence Version 2 – Weakly Reciprocal (`CERN-OHL-W-2.0`) | Creator-designed schematic, PCB and Thin-Pod-authored hardware source |
| Thin-Pod-created documentation, diagrams and images | Creative Commons Attribution 4.0 International (`CC-BY-4.0`) | Repository prose, records and creator-owned imagery |
| Standalone SnapEDA / SnapMagic-derived footprints, where included | CC BY-SA 4.0 with SnapMagic Design Exception 1.0, subject to the applicable SnapMagic Terms of Use | Third-party design files kept distinct from Thin-Pod-authored source |
| Commercial hardware components | Manufacturer terms | Physical components are listed for reproduction, not relicensed |

## 9. Application-ready OSHWA field summary

| OSHWA application field | Proposed entry |
|---|---|
| Project name | `Thin-Pod` |
| Project version | `0.1` |
| Certification on behalf of | Individual |
| Responsible individual | Neil Thomson |
| Primary project type | Electronics |
| Additional project type | Science |
| Hardware licence | CERN-OHL-W-2.0 |
| Software licence | No software |
| Documentation licence | CC-BY-4.0 |
| Documentation URL | Public release URL for this repository after completion and release freeze |
| Incorporates already OSHWA-certified hardware | No |
| Previous certified versions | None |

### Project description for application form

> Thin-Pod rev 0.1 is an open-hardware vibration-sensor node carrier PCB for rotating-machinery experiments. It provides protected power entry, regulated and switched sensor supply, an ADXL1005 analogue signal path, test points, and a corrected carrier-board interface for a commercially supplied Qorvo DWM3001-CDK. The Gateway, wireless implementation, DSP and TinyML are outside this certification scope.

## 10. Public documentation required before submission

The repository is ready to serve as the OSHWA documentation location once the following material is present and checked.

| Required or supporting material | Repository location | Status required before submission |
|---|---|---|
| Editable KiCad project/source | `hardware/source/` | Present, openable and checked |
| Project-local footprint and symbol sources | `hardware/source/` | Present with provenance recorded |
| Gerber and drill outputs | `hardware/fabrication/` | Generated from final corrected rev 0.1 source |
| Revision-specific BOM | `hardware/bom/` | Complete and publicly readable |
| Hardware licence | `LICENSE-HARDWARE.md` | Present |
| Documentation licence | `LICENSE-DOCUMENTATION.md` | Present |
| Scope declaration | `docs/certification-scope.md` | Present |
| Third-party component declaration | `docs/third-party-components.md` | Present |
| Footprint provenance record | `docs/footprint-provenance.md` | Present |
| Prototype and correction record | `docs/prototype-bring-up-and-release-correction.md` | Present |
| Verification status | `docs/design-verification.md` | Present and factually current |
| Application record | `oshwa/application-record.md` | Present before or immediately after submission |

## 11. Submission freeze rule

Immediately before submission:

1. Confirm that all published design files correspond to the corrected rev 0.1 release.
2. Confirm that no unlicensed Qorvo-supplied CAD file or vendor 3D model is included.
3. Confirm that all SnapEDA / SnapMagic-derived files are identified under their applicable terms.
4. Confirm that the BOM matches the source and fabrication output set.
5. Confirm that verification claims do not imply physical retesting of the corrected copper design unless that test has actually occurred.
6. Create a repository tag or GitHub release, recommended tag: `v0.1-oshwa-submission`.
7. Use the frozen public release URL in the OSHWA application.

## References

- OSHWA, *Certification Requirements*: <https://certification.oshwa.org/requirements.html>
- OSHWA, *Documentation*: <https://certification.oshwa.org/process/documentation.html>
- OSHWA, *Certification Application*: <https://application.oshwa.org/apply>
- CERN, *CERN Open Hardware Licence Version 2*: <https://ohwr.org/cern_ohl_w_v2.txt>
- Creative Commons, *Attribution 4.0 International*: <https://creativecommons.org/licenses/by/4.0/>
