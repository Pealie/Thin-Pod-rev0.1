# Thin-Pod rev 0.1: Prototype Bring-Up and Release Correction

**Project:** Thin-Pod  
**Release documented:** rev 0.1  
**Prepared by:** Neil Thomson / Pealie  
**Document status:** Prototype evidence and pre-release design-correction record  
**Document date:** 26 May 2026  

## 1. Purpose

This document records the relationship between:

1. the first physically manufactured Thin-Pod prototype used for electrical bring-up;
2. the ground-return issue found at the Qorvo DWM3001-CDK interface during that work; and
3. the corrected Thin-Pod rev 0.1 design prepared for open-hardware release and OSHWA certification.

The document prevents two opposite errors: concealing a real bring-up issue, or presenting a known and already corrected prototype fault as a permanent requirement of the release design.

## 2. Prototype objective

The first Thin-Pod PCB prototype was created to establish the electrical viability of a compact vibration-sensing carrier board. Its immediate aims were:

- to validate the protected power-entry and regulated supply path;
- to verify the switched accelerometer supply;
- to examine the analogue output and filtered measurement node from an ADXL1005-based sensing section;
- to confirm that an attached Qorvo DWM3001-CDK could form part of the intended pod-side interface; and
- to gather evidence for a release-quality open-hardware design.

The broader Gateway, wireless communications implementation, DSP and TinyML path were not required for this first PCB bring-up.

## 3. Prototype hardware under test

The prototype included the following relevant design blocks:

| Design block | Function |
|---|---|
| RAW_IN power input | External power entry to the carrier board |
| Fuse provision and 1N5817 diode | Basic input protection and reverse-polarity protection |
| Pololu S7V8F3 module | Local regulated 3.3 V supply |
| ZVP2106A PFET circuit | Switched supply control for the sensor section |
| ADXL1005-based sensing interface | Analogue vibration signal production |
| Analogue filtering / ADC node | Conditioned sensor signal for observation and downstream sampling |
| TP1–TP7 | Bring-up and measurement access points |
| Qorvo DWM3001-CDK interface | Commercial CDK connection for intended pod-side processing/radio development |

## 4. Bring-up discovery: CDK ground return

### 4.1 Observed behaviour

During initial testing of the assembled prototype, the intended CDK power/interface arrangement did not provide the expected usable ground-return connection through the assumed path. The attached CDK did not behave as expected until its negative/reference connection was bonded to a confirmed CDK ground point.

### 4.2 Temporary prototype correction

A short temporary ground connection was added between the relevant CDK return point and a known functional CDK ground reference. This was a bring-up correction applied to the physical prototype in order to continue testing the sensor-node electrical path.

The temporary connection served a diagnostic purpose:

- it established a common electrical reference for the attached CDK/interface arrangement;
- it allowed the board to proceed through powered testing; and
- it revealed the required correction for the public release design.

### 4.3 Engineering classification

The finding is treated as a **design-interface error in the pre-release carrier-board implementation**: the interface design relied on a ground-return assumption that proved inadequate in physical bring-up. The outcome is neither concealed nor carried forward as an avoidable assembly workaround.

The appropriate engineering response was to incorporate the necessary ground relationship into the carrier-board release source.

## 5. Prototype measurement evidence after temporary correction

With the temporary ground-return connection in place, prototype testing established proof of life for the essential analogue sensor-node chain.

| Measurement / observation | Prototype result | Interpretation |
|---|---|---|
| Power-up after CDK ground-reference correction | Successful | Prototype could proceed with powered validation once the interface return was established |
| Switched accelerometer supply rail, `/ACC_3V3_SW` | Approximately 3.35 V | Sensor supply path was active and close to the intended 3.3 V rail |
| Filtered sensor output / `SENSOR_SIGNAL_FILTERED` / ADC-node measurement | Approximately 1.76 V at rest | Mid-rail biased analogue output consistent with a 3.3 V-powered analogue accelerometer signal path |
| Gentle tap / mechanical excitation response | Clearly observable on the oscilloscope | The sensor/analogue observation path responded to physical vibration |

These measurements demonstrate the functional promise of the power and analogue signal portions of the prototype. They do not by themselves demonstrate completed wireless sampling, packet transport, Gateway processing or analytic fault detection.

## 6. Release correction incorporated into Thin-Pod rev 0.1

Before the first OSHWA-facing release was frozen or submitted, the CDK carrier interface was corrected in the editable KiCad source and regenerated fabrication outputs.

The released rev 0.1 design connects all relevant exposed CDK ground pins to the Thin-Pod carrier-board `GND` net:

```text
J1_2    GND    ─┐
J10_6   GND_1  ─┤
J10_9   GND_2  ─┤
J10_14  GND_3  ─┼── GND
J10_20  GND_4  ─┤
J10_25  GND_5  ─┘
```

This correction is combined with the project-local, Thin-Pod-authored CDK mating-interface footprint:

```text
ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod
```

The new release Gerbers were generated from the corrected design source and are intended to be the fabrication outputs cited by the OSHWA application.

## 7. Versioning position

The corrected ground return is properly part of **Thin-Pod rev 0.1** because:

- the earlier manufactured PCB was a development/prototype bring-up build;
- OSHWA certification had not yet been submitted;
- no frozen public OSHWA release documentation had been issued before correction; and
- the corrected source and Gerbers constitute the first intended open-hardware release definition.

This document preserves the prototype history without requiring builders of the released rev 0.1 design to implement a known unnecessary wire correction.

## 8. What is and is not verified

The evidence must be read precisely.

| Claim | Status |
|---|---|
| A pre-release prototype PCB was fabricated and assembled | Verified |
| The prototype required a temporary CDK ground-reference correction during bring-up | Verified and recorded |
| The power and analogue signal path displayed successful proof-of-life behaviour after the prototype correction | Verified and recorded |
| The released rev 0.1 KiCad source includes the corrected copper/net ground-return design | Implemented in release source; confirm at final tag |
| The released rev 0.1 Gerbers include the corrected ground-return design | Generated and visually inspected in working release set; include in repository |
| A newly manufactured PCB from the corrected release Gerbers has been tested without the temporary wire | Not yet claimed unless subsequently completed and recorded |

## 9. Evidence to include in the repository

The following files or images should be placed in the public release repository where available:

| Evidence item | Suggested location |
|---|---|
| Photographs of the first manufactured prototype PCB | `images/prototype-bring-up/` |
| Photograph of the temporary ground-return correction | `images/prototype-bring-up/` |
| Oscilloscope image showing tap response | `images/prototype-bring-up/` or `images/test-evidence/` |
| Recorded voltage measurement images or log extract | `images/test-evidence/` |
| Screenshot of corrected schematic CDK ground mapping | `images/released-design/` |
| Screenshot of corrected PCB / Gerber interface area | `images/gerber-inspection/` |
| Clean Gerber archive from corrected release design | `hardware/fabrication/` |

## 10. Remaining physical confirmation test for the released correction

A board fabricated from the corrected release Gerbers should be tested using the following focused procedure when available:

1. Visually inspect the new board and confirm correct CDK connector orientation.
2. Use continuity mode, with power removed, to confirm carrier `GND` continuity to `J1_2`, `J10_6`, `J10_9`, `J10_14`, `J10_20` and `J10_25`.
3. Confirm no short exists between the intended supply rail and `GND`.
4. Install the CDK without the prototype ground jumper.
5. Apply current-limited power under the correct supply conditions.
6. Confirm expected CDK power behaviour and stable sensor supply.
7. Repeat the analogue rest-bias and mechanical-excitation observation.
8. Update `docs/design-verification.md` with measured results and dated evidence.

## 11. Conclusion

The first Thin-Pod prototype achieved its central purpose: it revealed both a viable analogue vibration-sensing path and a correctable interface defect. The OSHWA-facing rev 0.1 release incorporates the CDK ground-return lesson into the PCB source and fabrication outputs, leaving the temporary jumper where it belongs: in the documented history of disciplined prototype bring-up rather than in the instructions for the released design.
