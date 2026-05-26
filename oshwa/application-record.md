# Thin-Pod rev 0.1: OSHWA Application Record

**Project:** Thin-Pod  
**Hardware release:** rev 0.1  
**Responsible individual:** Neil Thomson / Pealie  
**Repository:** [Pealie/Thin-Pod-rev0.1](https://github.com/Pealie/Thin-Pod-rev0.1)  
**Application status:** Submitted to the Open Source Hardware Association (OSHWA) certification programme  
**Submission date:** 26 May 2026  
**OSHWA UID:** Pending receipt / public directory confirmation  
**Document status:** Public application record

## 1\. Purpose

This document records the public, non-sensitive substance of the OSHWA certification application submitted for **Thin-Pod rev 0.1**.

The OSHWA application includes private correspondence and address information supplied directly to OSHWA. That information is intentionally not reproduced in this public repository record. This document instead records the certified item, the declared scope, the licensing position, the public documentation location and the supporting design-release evidence.

Once an OSHWA certification UID has been supplied or confirmed in the public certification directory, this document and the repository `README.md` should be updated to include it.

## 2\. Submitted certified item

The item submitted for OSHWA certification is:

> \*\*Thin-Pod rev 0.1: an open-hardware vibration-sensor node carrier PCB for rotating-machinery experiments.\*\*

Thin-Pod rev 0.1 is a creator-designed sensor-node carrier PCB providing protected power entry, regulated and switched sensor power, an ADXL1005-based analogue vibration-sensing interface, signal conditioning, accessible electrical test points and a corrected mating interface for a commercially supplied Qorvo DWM3001-CDK development board.

## 3\. Certification boundary

### Included in the submitted hardware scope

|Included element|Description|
|-|-|
|Thin-Pod rev 0.1 PCB|Two-layer creator-designed sensor-node carrier board|
|Power entry and protection|RAW\_IN connection, fuse provision and 1N5817 reverse-polarity protection|
|Regulated power path|Interface for the specified commercial 3.3 V regulator module|
|Switched sensor supply|PFET-controlled accelerometer supply rail|
|Analogue sensing interface|ADXL1005-based vibration-sensor electrical/mechanical interface|
|Signal conditioning and measurement|Raw/filtered signal path and TP1–TP7 bring-up points|
|CDK carrier interface|Thin-Pod-authored mating-interface footprint and PCB connectivity for the DWM3001-CDK|
|Corrected CDK ground return|Carrier-board ground connection incorporated into the released KiCad source and Gerbers|
|Release source and documentation|Editable KiCad files, fabrication outputs, BOM, provenance record and verification documentation|

### Explicitly outside the submitted scope

|Excluded element|Reason for exclusion|
|-|-|
|Qorvo DWM3001-CDK as open hardware|Commercial third-party development board used by the assembly, not a Thin-Pod-authored product|
|Analog Devices ADXL1005 device or evaluation board as open hardware|Commercial third-party sensing component/module|
|Pololu S7V8F3 module as open hardware|Commercial third-party regulator module|
|Thin-Pod Gateway|Separate evolving hardware artefact|
|STM32 NUCLEO-N657X0-Q and ESP32-C6 Gateway hardware|Outside the sensor-node carrier-board release|
|Implemented UWB communications|No completed wireless-implementation claim was submitted for rev 0.1|
|DSP, TinyML or anomaly detection|Future/system-level processing functionality outside this hardware application|
|Predictive-maintenance performance|No product-performance or diagnostic-accuracy claim forms part of this certification|
|EMC, radio, safety or other regulatory approval|OSHWA certification does not imply regulatory compliance certification|

## 4\. Project information recorded in the application

|Application field|Submitted public project information|
|-|-|
|Project name|`Thin-Pod`|
|Project version|`0.1`|
|Certifying party|Individual|
|Responsible individual|Neil Thomson|
|Country|United Kingdom|
|Project website|[Thin-Pod-rev0.1 GitHub repository](https://github.com/Pealie/Thin-Pod-rev0.1)|
|Documentation location|[Thin-Pod-rev0.1 GitHub repository](https://github.com/Pealie/Thin-Pod-rev0.1)|
|Primary project type|Electronics|
|Additional project type|Science|
|Incorporates hardware already registered with OSHWA|No previous certified Thin-Pod hardware identified for incorporation|
|Previous Thin-Pod OSHWA-certified versions|None|

Private postal-address and OSHWA-correspondence-email information supplied through the application form is omitted from this public record.

## 5\. Project description

The submitted application describes the release boundary in the following terms:

> Thin-Pod rev 0.1 is an open-hardware vibration-sensor node carrier PCB for rotating-machinery experiments. It provides protected power entry, regulated and switched sensor supply, an ADXL1005 analogue signal path, test points, and a corrected carrier-board interface for a commercially supplied Qorvo DWM3001-CDK. The Gateway, wireless implementation, DSP and TinyML are outside this certification scope.

## 6\. Project keywords

The project is described using the following technical keywords:

```text
vibration sensing, condition monitoring, accelerometer, ADXL1005,
rotating machinery, sensor node, KiCad, PCB, open hardware
```

## 7\. Licensing declarations

### 7.1 Submitted licence selections

|Material category in OSHWA application|Declared licence / position|
|-|-|
|Hardware|CERN Open Hardware Licence Version 2 – Weakly Reciprocal (`CERN-OHL-W-2.0`)|
|Software|No software required or supplied for the certified rev 0.1 hardware scope|
|Documentation|Creative Commons Attribution 4.0 International (`CC-BY-4.0`)|

### 7.2 Licence boundary within the repository

|Repository material|Licence treatment|
|-|-|
|Thin-Pod-created schematic, PCB layout and creator-authored hardware source|CERN-OHL-W-2.0|
|Thin-Pod-authored DWM3001-CDK mating-interface footprint|CERN-OHL-W-2.0|
|Thin-Pod-created documentation, diagrams and creator-owned images|CC-BY-4.0|
|Standalone SnapEDA / SnapMagic-derived footprint dependencies, where published|Retained as third-party Design Files under CC BY-SA 4.0 together with the SnapMagic Design Exception 1.0|
|Official KiCad-library-derived footprint dependencies, where locally distributed|Retained under the KiCad Libraries Licence: CC BY-SA 4.0 with the KiCad library exception|
|Purchased components and vendor product documentation|Identified or referenced for reproduction; not relicensed by Thin-Pod|

## 8\. Commercial component declaration

The submitted design necessarily uses commercially supplied hardware components. Their inclusion in the BOM is required for reproduction of the assembly and does not represent a claim that those commercial items are open hardware.

|Ref. / role|Commercial component|Treatment in the release|
|-|-|-|
|`U2` / sensing interface|Analog Devices ADXL1005-based sensor implementation|Proprietary third-party component/module; interface documented|
|`U4` / regulated supply|Pololu S7V8F3 regulator module|Proprietary third-party module; specified in BOM|
|`U5` / CDK connection|Qorvo DWM3001-CDK development board|Proprietary third-party development board; not relicensed|
|`U3`, `D1`, passives, connector and fuse items|Standard commercially supplied electronic components|Specified in BOM for reproducibility|

The openly licensed contribution is the Thin-Pod-created carrier-board design, associated released source and creator-authored documentation, subject to the separately identified third-party CAD dependency boundary.

## 9\. CAD and footprint provenance position

The application documentation distinguishes creator-authored and third-party CAD material as follows.

|Footprint or dependency|Function|Provenance / release position|
|-|-|-|
|`ThinPod\_DWM3001CDK\_Mating\_Interface\_revA.kicad\_mod`|Qorvo CDK carrier-board mating interface|Independently authored Thin-Pod source, released under CERN-OHL-W-2.0|
|`AnalogDevices\_EVAL-ADXL100xZ\_20p35mm.kicad\_mod`|ADXL1005-based module/interface footprint|SnapEDA / SnapMagic third-party Design File, published only under its applicable recorded terms|
|`Pololu\_S7V8F3\_S7V8x\_Module.kicad\_mod`|Regulator-module footprint|SnapEDA / SnapMagic third-party Design File, published only under its applicable recorded terms|
|`PFET\_ZVP2106A\_GSD\_THT.kicad\_mod` or equivalent U3 footprint dependency|PFET THT footprint|Official KiCad-library dependency or KiCad-library-derived local copy, identified under the KiCad Libraries Licence|

No earlier Qorvo-supplied CDK CAD footprint, proprietary vendor board-layout file or vendor 3D model is presented as Thin-Pod-created hardware source in the OSHWA release.

## 10\. Ground-return prototype finding and release correction

A pre-release manufactured prototype identified that the initially assumed Qorvo DWM3001-CDK ground-return arrangement was inadequate for powered bring-up. A temporary jumper to a known CDK ground reference enabled continued prototype testing and successful observation of the analogue sensing path.

Before the rev 0.1 OSHWA-facing release was frozen or submitted, the design was corrected so that the relevant exposed CDK ground connections are bonded directly to the Thin-Pod carrier-board `GND` net:

```text
J1\_2    GND    ─┐
J10\_6   GND\_1  ─┤
J10\_9   GND\_2  ─┤
J10\_14  GND\_3  ─┼── GND
J10\_20  GND\_4  ─┤
J10\_25  GND\_5  ─┘
```

The released rev 0.1 editable source and fabrication outputs incorporate this correction. The temporary jumper is retained in the documentation as a prototype bring-up finding; it is not an assembly requirement for the released design.

## 11\. Verification evidence submitted through the repository

### 11.1 Prototype evidence

|Evidence item|Recorded result|
|-|-|
|Initial PCB manufacture and assembly|Completed for the pre-release prototype|
|Powered bring-up after temporary CDK ground-return correction|Successful|
|Switched accelerometer supply rail|Approximately 3.35 V measured|
|Filtered analogue sensor/ADC-node output at rest|Approximately 1.76 V measured|
|Mechanical excitation / tap test|Visible response observed on the oscilloscope|

### 11.2 Released design evidence

|Evidence item|Recorded status|
|-|-|
|Corrected CDK ground-return design in editable KiCad source|Incorporated before submission|
|Thin-Pod-authored CDK mating-interface footprint|Included as creator-authored hardware source|
|Gerber and drill outputs generated from corrected release design|Included in the release documentation package|
|Gerber top-silkscreen inspection|Reviewed separately from assembly/fabrication drawing layers; no evident unacceptable top-legend overlap|
|Physical test of a newly fabricated PCB from corrected release Gerbers without jumper|Not claimed as completed unless added by a subsequent dated documentation update|

This distinction is deliberate. The repository records successful prototype signal-path bring-up and documents the corrected released source without claiming a physical retest that has not yet occurred.

## 12\. Public documentation package

The application documentation is supplied through the release repository. Its intended principal files are:

|Documentation item|Repository path|
|-|-|
|Main release description|[`README.md`](../README.md)|
|Hardware licence|[`LICENSE-HARDWARE.md`](../LICENSE-HARDWARE.md)|
|Documentation licence|[`LICENSE-DOCUMENTATION.md`](../LICENSE-DOCUMENTATION.md)|
|Editable KiCad hardware source|[`hardware/source/`](../hardware/source/)|
|Gerber and drill fabrication outputs|[`hardware/fabrication/`](../hardware/fabrication/)|
|Bill of materials|[`hardware/bom/`](../hardware/bom/)|
|Certification scope|[`docs/certification-scope.md`](../docs/certification-scope.md)|
|Third-party component boundary|[`docs/third-party-components.md`](../docs/third-party-components.md)|
|Footprint provenance record|[`docs/footprint-provenance.md`](../docs/footprint-provenance.md)|
|Prototype bring-up and release correction|[`docs/prototype-bring-up-and-release-correction.md`](../docs/prototype-bring-up-and-release-correction.md)|
|Design verification record|[`docs/design-verification.md`](../docs/design-verification.md)|
|Deferred/future revisions|[`docs/future-revisions.md`](../docs/future-revisions.md)|

## 13\. Certification status record

|Milestone|Status|Date / identifier|
|-|-|-|
|Dedicated OSHWA-facing repository established|Completed|May 2026|
|Scope narrowed to Thin-Pod sensor-node carrier PCB|Completed|May 2026|
|CDK footprint provenance boundary established|Completed|May 2026|
|Ground-return correction incorporated into release design|Completed before application submission|May 2026|
|OSHWA certification application submitted|**Completed**|**26 May 2026**|
|OSHWA UID received or confirmed in directory|Pending|Add UID when available|
|Certification identifier added to README and this record|Pending|Complete after UID confirmation|
|Optional corrected-PCB physical re-verification|Pending / future evidence update|Record only when completed|

## 14\. Post-submission update procedure

When an OSHWA UID is received or the project listing becomes visible in the OSHWA certification directory:

1. Add the UID to the table above.
2. Replace `Pending receipt / public directory confirmation` in the document header with the UID.
3. Update `README.md` with the UID and certification-status wording.
4. Add the OSHWA project-directory link, once available.
5. Apply the OSHWA certification mark or identifier only in accordance with the Certification Mark Licence Agreement and OSHWA branding requirements.
6. Preserve the submitted release source and fabrication outputs as a tagged version of the repository, recommended tag: `v0.1-oshwa-submission`.

Any later PCB electrical or mechanical design change should be assigned a subsequent hardware revision rather than silently replacing the submitted rev 0.1 design record.

## 15\. Public-record privacy note

This document deliberately excludes:

* private postal-address details supplied to OSHWA;
* the OSHWA private correspondence-email address;
* any submission confirmation data not intended for public display.

The OSHWA directory may display the public contact information supplied through the application, in accordance with the application form.

## 16\. References

* OSHWA Certification Application: [https://application.oshwa.org/apply](https://application.oshwa.org/apply)
* OSHWA Certification Requirements: [https://certification.oshwa.org/requirements.html](https://certification.oshwa.org/requirements.html)
* OSHWA Documentation Guidance: [https://certification.oshwa.org/process/documentation.html](https://certification.oshwa.org/process/documentation.html)
* OSHWA Certification Mark Licence Agreement: [https://certification.oshwa.org/license-agreement.html](https://certification.oshwa.org/license-agreement.html)
* Thin-Pod rev 0.1 release repository: [https://github.com/Pealie/Thin-Pod-rev0.1](https://github.com/Pealie/Thin-Pod-rev0.1)
* SnapMagic / SnapEDA Terms of Use: [https://www.snapeda.com/about/terms/](https://www.snapeda.com/about/terms/)
* KiCad Libraries Licence: [https://www.kicad.org/libraries/license/](https://www.kicad.org/libraries/license/)

\---

**Submission record:** Thin-Pod rev 0.1 was submitted to the OSHWA certification programme on 26 May 2026 as a bounded open-hardware vibration-sensor node carrier PCB release. This public record will be updated with the OSHWA UID when it is supplied or publicly confirmed.

