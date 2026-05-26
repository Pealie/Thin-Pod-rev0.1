# Thin-Pod rev 0.1: Design Verification Record

**Project:** Thin-Pod  
**Hardware release:** rev 0.1  
**Repository:** `Pealie/Thin-Pod-rev0.1`  
**Prepared by:** Neil Thomson / Pealie  
**Document status:** Release verification record, to be updated as final evidence is committed  
**Document date:** 26 May 2026  
**EDA environment:** KiCad 10.0.1  

## 1. Verification purpose

This document records verification evidence for the Thin-Pod rev 0.1 OSHWA-facing release. It distinguishes evidence established on the initial manufactured prototype from evidence established by inspection of the corrected release design files and evidence still requiring confirmation on a PCB fabricated from the corrected Gerbers.

The certified release claim is appropriately limited: Thin-Pod rev 0.1 is an open-hardware analogue vibration-sensor carrier PCB. It is not a validated end-to-end radio/Gateway/DSP/TinyML product.

## 2. Release hardware summary

Thin-Pod rev 0.1 provides:

- protected RAW_IN power entry;
- fuse provision and 1N5817 reverse-polarity protection;
- a Pololu S7V8F3-based regulated 3.3 V path;
- a PFET-switched accelerometer supply rail;
- an ADXL1005-based analogue vibration-sensor interface;
- raw and filtered analogue signal observation points;
- test points for bring-up;
- a Qorvo DWM3001-CDK mating interface; and
- a corrected CDK ground-return arrangement implemented in the release PCB design.

## 3. Normative release source

The normative source of the rev 0.1 hardware release is the editable KiCad design under `hardware/source/`. Fabrication outputs under `hardware/fabrication/` are derived outputs.

| Source class | Expected file or folder | Verification purpose |
|---|---|---|
| KiCad project | `hardware/source/Thin-Pod_rev0.1.kicad_pro` | Opens the editable design project |
| Schematic | `hardware/source/Thin-Pod_rev0.1.kicad_sch` | Defines circuit/net intent |
| PCB layout | `hardware/source/Thin-Pod_rev0.1.kicad_pcb` | Defines manufacturable board layout |
| Footprint tables | `hardware/source/fp-lib-table` | Resolves project-local footprint libraries |
| Symbol tables | `hardware/source/sym-lib-table` | Resolves project-local symbol libraries |
| Thin-Pod-authored footprint | `hardware/source/footprints/ThinPod.pretty/ThinPod_DWM3001CDK_Mating_Interface_revA.kicad_mod` | Defines published CDK carrier mating interface |
| Third-party footprint dependencies | `hardware/source/footprints/third-party-snapeda.pretty/` | Supplies declared U2/U4 design-file dependencies under their own terms |
| Fabrication outputs | `hardware/fabrication/` | Allows PCB manufacture from the release design |
| BOM | `hardware/bom/` | Identifies assembly components and sourcing boundary |

## 4. Electrical net and test-point intent

### 4.1 Principal power and signal paths

```text
Power entry:
J1 / RAW_IN → F1 → D1 / reverse-polarity protection → U4 regulator → 3V3+

Switched sensor rail:
3V3+ → U3 PFET switch → ACC_3V3_SW → accelerometer supply / local decoupling

Analogue signal path:
ADXL1005-based interface output → SENSOR_SIGNAL_RAW / ACC_VOUT
                               → R1/C1 signal conditioning
                               → SENSOR_SIGNAL_FILTERED / ADC_NODE
                               → CDK interface ADC connection
```

### 4.2 Test-point map

| Test point | Signal / net purpose | Verification use |
|---|---|---|
| `TP1` | `RAW_IN` | Input-supply measurement before protection path |
| `TP2` | `RAW_FUSED` | Post-fuse power-path measurement |
| `TP3` | `RAW_PROT` | Protected input after series diode |
| `TP4` | `3V3+` | Regulated supply measurement |
| `TP5` | `GND` | Measurement reference |
| `TP6` | `SENSOR_SIGNAL_RAW` / sensor raw output as labelled in release source | Analogue sensor observation |
| `TP7` | `SENSOR_SIGNAL_FILTERED` / `ADC_NODE` | Filtered signal / downstream ADC-node observation |

The final labels and net names in the public KiCad files and BOM must agree; any naming refinement should be applied consistently before tagging the OSHWA submission release.

### 4.3 Corrected CDK ground mapping

The released design must show the following CDK physical interface pins connected to the same Thin-Pod `GND` net:

```text
J1_2    GND    ─┐
J10_6   GND_1  ─┤
J10_9   GND_2  ─┤
J10_14  GND_3  ─┼── GND
J10_20  GND_4  ─┤
J10_25  GND_5  ─┘
```

## 5. Prototype physical validation evidence

A pre-release physically manufactured PCB was assembled and tested. During CDK interface bring-up, a missing/insufficient assumed ground-return path was identified and corrected temporarily with a ground jumper. After that correction, the core analogue sensor-node path demonstrated proof of life.

| Verification item | Evidence state | Result |
|---|---|---|
| Physical PCB manufactured and assembled | Completed on pre-release prototype | Board available for bring-up |
| CDK interface ground-return issue identified | Completed on pre-release prototype | Temporary common-ground correction required |
| Powered bring-up after temporary correction | Completed on pre-release prototype | Successful |
| Switched sensor supply rail measurement | Completed on pre-release prototype | Approximately 3.35 V |
| Filtered sensor/ADC-node resting output measurement | Completed on pre-release prototype | Approximately 1.76 V |
| Mechanical excitation observation | Completed on pre-release prototype | Visible tap response on oscilloscope |

### Evidence limitations

This prototype evidence validates the basic power and analogue sensing path after the discovered interface issue was physically corrected. It does not establish:

- completion of a UWB communications path;
- CDK firmware sampling or packet transmission;
- Gateway receipt or memory transfer;
- DSP, TinyML or anomaly-detection results; or
- physical validation of a newly fabricated PCB containing the corrected ground connection in copper.

## 6. Release-design correction verification

The OSHWA-facing rev 0.1 design integrates the ground-return correction before release. The correction must be verified in the published source and outputs.

| Release-design verification check | Status | Evidence / action |
|---|---|---|
| Thin-Pod-authored CDK mating-interface footprint replaces the earlier development/vendor footprint | Completed in working design | Confirm final committed KiCad project references the clean footprint |
| `J1_2` and J10 GND pads are assigned to carrier `GND` | Completed in working design | Confirm schematic and PCB net highlighting before tag |
| Corrected Gerbers generated | Completed in working design | Commit final fabrication-output archive |
| Top silkscreen inspected without confusing `F.Fab` overlay | Completed | Inspected view shows no unacceptable top-silkscreen overlap |
| Gerbers correspond exactly to committed source | Required before tag | Regenerate or verify checksums after final source commit |
| Corrected board fabricated and tested without jumper | Pending unless later completed | Do not claim until measured |

## 7. CAD integrity and provenance verification

| Check | Acceptance criterion | Status before OSHWA submission |
|---|---|---|
| KiCad project opens without missing libraries | All required local tables and files resolve from repository-relative paths | To confirm after final repository population |
| DWM3001-CDK interface footprint provenance | Published footprint is Thin-Pod-authored and earlier Qorvo-supplied footprint is absent | To confirm in final public tree |
| U2/U4 SnapEDA-derived footprint boundary | Files are stored in third-party folder with notice and terms recorded | To confirm in final public tree |
| PFET footprint provenance | Independent authorship or compatible origin recorded | Pending confirmation |
| Schematic ERC | No unresolved errors relevant to released hardware | Run and record final report |
| PCB DRC | No unresolved fabrication/connectivity violations relevant to released hardware | Run and record final report |
| BOM/source consistency | References, values and footprint identities agree across BOM and KiCad design | Confirm before tag |
| Gerber/source consistency | Fabrication archive generated from final committed PCB source | Confirm before tag |

## 8. Gerber and fabrication-output inspection

The release fabrication outputs should be inspected layer-by-layer in KiCad Gerber Viewer before submission and before any fabrication order.

### 8.1 Layer inspection checklist

| Layer / output | Required inspection |
|---|---|
| `F.Cu` | Tracks, pads, pours and corrected CDK ground connectivity appear as intended |
| `B.Cu` | Tracks, pours and via connections appear as intended |
| `F.Mask` | Pad openings and exposed test-point clearances are correct |
| `B.Mask` | Bottom pad/via mask behaviour is correct |
| `F.Silkscreen` | References, polarity marks and board identification are legible and avoid exposed pad openings |
| `B.Silkscreen` | Any bottom legend is intentional and unobstructed |
| `Edge.Cuts` | Closed, correct board outline with intended mounting geometry |
| `PTH drill` | Plated through-holes align with connector and component pads |
| `NPTH drill` | Mechanical and support holes align with intended fit |

### 8.2 Silkscreen inspection record

During Gerber review, apparent text overlaps were initially seen while the front fabrication/assembly drawing layer was displayed. Once `F.Fab` was unselected and the actual front silkscreen was inspected with mask, board outline and drills, the top silkscreen appeared acceptable:

| Inspection point | Finding |
|---|---|
| Component references | Legible in inspected front-silkscreen view |
| Board title / revision marking | Legible |
| Text crossing exposed pad openings | No obvious unacceptable overlap observed |
| D1 polarity/orientation indicator | Visible and useful |
| U5/CDK footprint outline | Clear and uncluttered |
| Earlier apparent text overlap | Located on `F.Fab`, not the manufactured top silkscreen |

Insert retained screenshots in `images/gerber-inspection/` and link them here when committed.

## 9. Corrected release-board physical test plan

The following test sequence is required when a board fabricated from the corrected rev 0.1 Gerbers becomes available.

### 9.1 Unpowered checks

| Check | Expected result | Measured result | Status |
|---|---|---|---|
| Visual inspection of connector orientation and polarity marks | Matches design |  | Pending |
| `GND` continuity to `J1_2` | Continuity |  | Pending |
| `GND` continuity to `J10_6` | Continuity |  | Pending |
| `GND` continuity to `J10_9` | Continuity |  | Pending |
| `GND` continuity to `J10_14` | Continuity |  | Pending |
| `GND` continuity to `J10_20` | Continuity |  | Pending |
| `GND` continuity to `J10_25` | Continuity |  | Pending |
| RAW input / regulated rails to GND | No unintended short |  | Pending |
| Sensor signal node to GND | No unintended short beyond designed paths |  | Pending |

### 9.2 Powered checks

Use a current-limited bench supply and confirm the correct operating supply conditions for each fitted commercial module before power is applied.

| Check | Expected result | Measured result | Status |
|---|---|---|---|
| Correct CDK power-up without temporary ground wire | Normal power behaviour |  | Pending |
| Protected input path | Expected voltage after diode drop |  | Pending |
| Regulated rail | Approximately 3.3 V |  | Pending |
| Switched accelerometer rail | Approximately 3.3 V when enabled |  | Pending |
| Resting filtered sensor signal | Mid-rail analogue value consistent with sensor operation |  | Pending |
| Mechanical excitation response | Observable analogue response at raw/filtered measurement point |  | Pending |
| Abnormal heating | None observed during controlled bring-up |  | Pending |

## 10. Release acceptance statement

At the time this document was prepared, the following statement is defensible:

> The initial Thin-Pod prototype physically demonstrated successful power and analogue vibration-signal bring-up after a temporary ground-return correction at the CDK interface. The OSHWA-facing rev 0.1 release source and Gerbers incorporate that correction in the carrier-board design and publish a clean interface-footprint provenance boundary. Physical testing of a board fabricated from the corrected release Gerbers will be recorded separately when completed and is not claimed prematurely.

## 11. Final sign-off record

Complete this table before tagging the OSHWA submission release.

| Release check | Completed by | Date | Evidence / commit reference |
|---|---|---|---|
| Final KiCad source opens from repository clone |  |  |  |
| Footprint licence/provenance audit complete |  |  |  |
| ERC completed and reviewed |  |  |  |
| DRC completed and reviewed |  |  |  |
| Gerbers regenerated from final committed source |  |  |  |
| Gerber layer inspection complete |  |  |  |
| BOM reconciled to final source |  |  |  |
| Licence files present and correct |  |  |  |
| OSHWA application record prepared |  |  |  |
| Submission tag / release created |  |  |  |

## Related documents

- [`certification-scope.md`](certification-scope.md)
- [`third-party-components.md`](third-party-components.md)
- [`footprint-provenance.md`](footprint-provenance.md)
- [`prototype-bring-up-and-release-correction.md`](prototype-bring-up-and-release-correction.md)
- [`future-revisions.md`](future-revisions.md)
