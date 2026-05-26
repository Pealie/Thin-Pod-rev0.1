# Thin-Pod rev 0.1: Future Revisions and Deferred Scope

**Project:** Thin-Pod  
**Current release:** rev 0.1  
**Prepared by:** Neil Thomson / Pealie  
**Document status:** Scope-control and future-work record  
**Document date:** 26 May 2026  

## 1. Purpose

Thin-Pod rev 0.1 is intentionally a bounded first open-hardware release: a vibration-sensor node carrier PCB with a documented analogue measurement path and a corrected commercial-CDK interface.

This document records worthwhile work that is deliberately **not** folded into the rev 0.1 OSHWA certification scope. It prevents future ambitions from obscuring the released hardware claim, while preserving a technically coherent path from an initial verified sensor-node board towards a more complete open condition-monitoring platform.

## 2. Rev 0.1 release boundary

Rev 0.1 is concerned with:

- reproducible open-hardware source for the sensor-node carrier PCB;
- protected and regulated power;
- switched accelerometer supply;
- ADXL1005-based analogue vibration-sensing interface;
- test points and observable raw/filtered signal behaviour;
- commercial Qorvo DWM3001-CDK mating interface;
- corrected CDK ground return in released PCB copper;
- public BOM, fabrication outputs, provenance and verification documentation.

Rev 0.1 is not extended to include a Gateway, radio firmware, analytic model or industrial monitoring claim merely because those elements motivate the longer project.

## 3. Post-release verification without a design-version change

The first outstanding activity after freezing the corrected rev 0.1 release is not necessarily a new design revision. It is physical confirmation of the already released correction.

| Activity | Why it matters | Version treatment |
|---|---|---|
| Fabricate a PCB from the corrected rev 0.1 Gerbers | Confirms the released fabrication package is buildable | Remains rev 0.1 if no design change is introduced |
| Test CDK power/ground behaviour without temporary jumper | Confirms the corrected copper ground return | Verification update to rev 0.1 documentation |
| Repeat sensor rail and analogue tap-response checks | Confirms prototype results on released copper implementation | Verification update to rev 0.1 documentation |
| Add photographs and measurement results | Strengthens OSHWA repository evidence | Documentation update tied to rev 0.1 release |

A new hardware revision is warranted only if the post-release test produces a design change rather than new evidence for the same design.

## 4. Candidate sensor-node rev 0.2 changes

The following improvements belong in a subsequent node revision because they alter or extend the hardware design beyond the rev 0.1 release boundary.

### 4.1 ADXL1005 control-line defaults

The ADXL1005 exposes control functions that should receive explicit design treatment in a future board.

| Signal | Current rev 0.1 treatment | Candidate rev 0.2 treatment | Rationale |
|---|---|---|---|
| `STANDBY` / `STB` | Not fully formalised as a controlled released feature | Add default pulldown to `GND`, likely 100 kΩ, or route to MCU where duty-cycling is required | Establishes a defined default state and supports power-management experiments |
| `ST` / self-test | Not fully formalised as a controlled released feature | Add pulldown to `GND` plus optional test pad or MCU control line | Enables repeatable sensor self-test and removes floating-control ambiguity |

These are particularly suitable for rev 0.2 because they convert prototype learning into explicit measurement-integrity features without altering the central sensor-platform concept.

### 4.2 Layout and manufacturability refinement

| Candidate change | Motivation |
|---|---|
| Move regulator and local passives closer to the sensor supply section where practical | Reduce supply-loop length and improve physical organisation |
| Evaluate more compact component placement | Reduce board area while retaining accessible testability |
| Move selected components to SMT where appropriate | Improve manufacturing readiness and reduce assembly bulk |
| Retain clear test points during miniaturisation | Avoid trading verifiability for compactness |
| Improve silkscreen and fabrication-layer annotation | Make assembly and review cleaner without changing function |
| Formalise enclosure/mounting interface | Prepare the sensing node for repeatable machine attachment |

### 4.3 Sensor implementation decision

Rev 0.1 is based on an ADXL1005-related module/interface arrangement suitable for first hardware proof. A later release should explicitly decide between:

| Direction | Advantages | Considerations |
|---|---|---|
| Continue with a commercial evaluation/breakout implementation | Fast assembly and lower assembly risk | Larger, less product-like and dependent on a third-party module |
| Integrate bare ADXL1005 device onto the custom PCB | Cleaner OSHWA hardware artefact and manufacturing path | LFCSP assembly requirements, layout care and test/assembly risk |

This decision should be made in conjunction with assembly capability and the intended OSHWA/manufacturing-readiness objective.

## 5. Communications-module evolution

Rev 0.1 uses a carrier interface to a **Qorvo DWM3001-CDK** development board. Future designs may move towards the smaller **DWM3001C** castellated module.

| Revision direction | Hardware treatment | Rationale |
|---|---|---|
| Rev 0.1 | CDK-based interface retained | Suitable for prototype/release evidence and accessible development |
| Later node or Gateway revision | Consider DWM3001C module on a custom PCB | Smaller, cleaner module-based integration while retaining Qorvo’s local UWB subsystem |
| Premature chip-down DW3110 integration | Not presently preferred | Increases radio/integration burden and weakens the modular architecture |

The preferred architectural principle is to treat the Qorvo module as a communications subsystem with a stable documented interface, rather than prematurely merging radio-level complexity into the creator-designed sensing or Gateway board.

## 6. Thin-Pod Gateway as a separate product

The Gateway is not part of the Thin-Pod rev 0.1 certification. It should continue as a separate design and, when mature, as a separate OSHWA certification candidate.

### 6.1 Preferred Gateway architecture

The current preferred architecture is modular:

```text
Thin-Pod sensor node
    ↓  UWB vibration-window transport
DWM3001C / DWM3001-CDK subsystem on Gateway side
    ↓  documented host interface
STM32 NUCLEO-N657X0-Q or later custom supervisor platform
    ↓
DSP / feature extraction / TinyML / onward network reporting
```

The Qorvo subsystem should handle UWB communications locally through its nRF52833/DW3110 arrangement, while the NUCLEO-class host acts as analytic supervisor rather than attempting direct low-level control of the radio IC.

### 6.2 Gateway work explicitly deferred

| Deferred work | Reason for exclusion from rev 0.1 |
|---|---|
| Gateway PCB fabrication and verification | Separate hardware artefact |
| DWM3001C module integration on Gateway PCB | Separate interface/layout work |
| NUCLEO host interface proof | Separate communications and firmware milestone |
| ESP32-C6 onward-networking path | Separate networking subsystem |
| PoE supply integration | Gateway power-domain issue, not sensor-node rev 0.1 |
| Gateway OSHWA application | Requires its own frozen source, BOM and verification package |

## 7. Firmware and data-transport path

Firmware is outside the rev 0.1 certification scope, but the future design direction is sufficiently clear to record without claiming completion.

### 7.1 Intended data model

The intended transport model is based on short vibration windows rather than always-on streaming:

```text
Sensor samples a bounded vibration window
        ↓
Window is framed with asset/calibration/processing metadata
        ↓
UWB subsystem transfers the window locally
        ↓
Gateway receives window into analytic memory
        ↓
DSP features and optional TinyML decisions are generated
```

### 7.2 First firmware proof milestones

| Milestone | Description |
|---|---|
| Synthetic buffer path | Demonstrate a known buffer travelling from the Gateway-side Qorvo subsystem into NUCLEO-accessible memory |
| Received vibration-window path | Replace the synthetic buffer with an actual UWB-received sensor window |
| DSP feature proof | Calculate RMS, kurtosis, crest factor, band energy and envelope from a received window |
| Versioned processing record | Record firmware/calibration/processing version metadata alongside results |
| TinyML evaluation | Begin only after the data path and DSP baseline are reproducible |

None of these milestones is claimed by the rev 0.1 hardware certification package.

## 8. Measurement integrity and security posture

Security is not merely an application-layer addition for a sensing system: it affects confidence in the measurement record. From rev 0.2 onward, documentation should begin to formalise:

- sensor identity and calibration state;
- firmware identity and build version;
- packet path and transport integrity;
- Gateway processing version;
- model identity, if TinyML is introduced;
- alert/decision traceability; and
- reproducible comparison between raw windows and derived metrics.

This work belongs in a dedicated security-and-trust document for later system releases, not in a claim that rev 0.1 already implements end-to-end trust.

## 9. Research and evaluation direction

A mature Thin-Pod platform may support publishable engineering investigation, especially where it measures the relationship between signal fidelity, transmission cost and energy use.

A promising future research question is:

> How much fault-relevant vibration information is retained per joule consumed and per byte transmitted as window length, sample rate, feature selection and UWB transport settings vary?

This question is deliberately deferred until the sensor-node hardware, communications path and Gateway data ingestion path have each been made reproducible.

## 10. Candidate revision roadmap

| Stage | Deliverable | Definition of completion |
|---|---|---|
| rev 0.1 | OSHWA-facing sensor-node carrier PCB release | Public editable source, BOM, fabrication files, provenance boundary and certification submission |
| rev 0.1 verification update | Corrected-board physical test evidence | Board fabricated from released Gerbers powers and responds without temporary jumper |
| rev 0.2 | Refined sensor-node hardware | ST/STANDBY treatment, layout/manufacturing refinements and any chosen sensor/module transition |
| Gateway revision | Independently released Gateway hardware | Frozen source/BOM/fabrication documentation and verified host/UWB interface |
| System demonstration | End-to-end vibration-window transfer | Received window arrives at Gateway memory reproducibly |
| Analytic release | DSP baseline and optional TinyML | Versioned processing pipeline with measurable validation evidence |

## 11. Change-control rule

No future feature should be silently folded into the rev 0.1 certification record after submission. Later changes should be handled as follows:

| Change type | Treatment |
|---|---|
| Additional photographs, measurement results or clarification for unchanged rev 0.1 design | Update documentation with dated record |
| Correction of minor typographical/documentation error | Update documentation transparently |
| Electrical, mechanical or PCB-layout design change after rev 0.1 is frozen | Assign a subsequent hardware revision and retain rev 0.1 documentation |
| Addition of Gateway, radio implementation, DSP or TinyML claim | Separate product/system documentation and, where appropriate, separate OSHWA submission |

## 12. Conclusion

Thin-Pod rev 0.1 should remain small, exact and defensible: an open vibration-sensor node PCB with a corrected commercial-CDK interface and a truthful prototype-to-release record. Its value is strengthened, not diminished, by keeping the Gateway, communications system and analytic ambition visible as future work rather than prematurely absorbing them into the first certification boundary.
