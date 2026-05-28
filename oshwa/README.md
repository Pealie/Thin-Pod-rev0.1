# OSHWA Certification Record

This directory contains the public certification record for **Thin-Pod rev 0.1**, an open-hardware vibration-sensor node carrier PCB for rotating-machinery experiments.

## Certification status

**Thin-Pod rev 0.1 is OSHWA Certified Open Source Hardware.**

|Field|Value|
|-|-|
|Project|Thin-Pod|
|Hardware release|rev 0.1|
|OSHWA UID|UK000091|
|Certification date|28 May 2026|
|Certified item|Vibration-sensor node carrier PCB|
|Hardware licence|CERN-OHL-W-2.0|
|Documentation licence|CC-BY-4.0|
|Official certification record|[certification.oshwa.org/uk000091](https://certification.oshwa.org/uk000091.html)|

## Contents

|File|Purpose|
|-|-|
|[`application-record.md`](application-record.md)|Public record of the submitted project description, certification boundary, licence declarations, commercial-component treatment, provenance position and verification evidence supporting the certified release.|

## Certified scope

The certification applies to the **Thin-Pod rev 0.1 creator-designed carrier PCB** and its released documentation. The certified hardware scope includes the protected power-entry path, regulated and switched accelerometer supply, ADXL1005-based analogue sensing interface, conditioned signal path, bring-up test points, Thin-Pod-authored DWM3001-CDK mating interface, and the corrected CDK ground-return implementation incorporated in the released design files.

The following remain outside this certification scope:

* commercially supplied components or development boards, including the Qorvo DWM3001-CDK, Analog Devices ADXL1005 device or evaluation hardware, and Pololu S7V8F3 module;
* the separately developed Thin-Pod Gateway;
* implemented UWB communications, radio firmware and networking;
* Gateway DSP, TinyML, anomaly-detection and predictive-maintenance system claims;
* EMC, radio, safety or other regulatory approval.

## Documentation boundary

The open-hardware contribution is the Thin-Pod-created carrier-board design, editable source, fabrication outputs and creator-authored documentation. Bought-in components and third-party CAD dependencies are identified for reproduction and provenance purposes; they are not presented as Thin-Pod-created open hardware or relicensed by this project.

The repository-level description of the certified release is held in [`../README.md`](../README.md). Hardware and documentation licences are provided at:

* [`../LICENCE-HARDWARE.md`](../LICENCE-HARDWARE.md)
* [`../LICENCE-DOCS.md`](../LICENCE-DOCS.md)

## Public-record note

[`application-record.md`](application-record.md) records the public, non-sensitive substance of the OSHWA certification application and subsequent certification milestone. Private postal-address information, private correspondence details and any non-public application data are intentionally excluded.

Any later electrical or mechanical design change should be recorded as a subsequent hardware revision rather than silently replacing the documented **rev 0.1** certified release.

\---

**Thin-Pod rev 0.1 · OSHWA UID UK000091 · Certified 28 May 2026**

