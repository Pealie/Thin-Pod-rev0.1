# Thin-Pod rev 0.1 Bill of Materials

**Project:** Thin-Pod  
**Release:** rev 0.1  
**Scope:** OSHWA-facing vibration-sensor node carrier PCB release  
**BOM status:** Release draft derived from the rev 0.1 KiCad source; procurement identifiers marked for confirmation must be completed before public certification submission.

## Scope and release boundary

This BOM covers the corrected **Thin-Pod rev 0.1 sensor-node carrier PCB** only. It includes the commercial sensor, power and Qorvo CDK modules that mate with or are fitted to the carrier board, because those parts are required to reproduce the released hardware assembly.

The following are excluded from this BOM and from the rev 0.1 certification scope:

* Thin-Pod Gateway hardware;
* STM32 NUCLEO-N657X0-Q hardware;
* Gateway-side DWM3001-CDK or DWM3001C hardware;
* ESP32-C6 networking hardware;
* DSP, TinyML, firmware and wireless-system functionality.

The release design incorporates the corrected CDK ground return in PCB copper: `J1\\\_2`, `J10\\\_6`, `J10\\\_9`, `J10\\\_14`, `J10\\\_20` and `J10\\\_25` connect to the carrier-board `GND` net. A temporary ground jumper used during pre-release prototype bring-up is therefore not included as a release BOM item.



## Open-hardware and third-party component boundary



This BOM lists all parts required to reproduce the Thin-Pod rev 0.1 hardware assembly, including commercially supplied proprietary components. Inclusion in the BOM does not imply that a purchased component is open hardware or covered by the Thin-Pod hardware licence. The CERN-OHL-W-2.0 licence applies to the Thin-Pod-created carrier-board design files and project-authored hardware source; third-party components remain subject to their manufacturers' terms.



## Assembly BOM

|Ref.|Qty|Function|Part / value|Manufacturer / MPN|Footprint|Release note|
|-|-:|-|-|-|-|-|
|`PCB1`|1|Thin-Pod carrier printed circuit board|Thin-Pod rev 0.1, 2-layer FR-4 PCB|Fabricator selected by builder|`N/A — PCB fabrication item`|Fabricate from hardware/fabrication outputs; editable source in hardware/source.|
|`J1`|1|RAW\_IN power entry|JST PH, 2-pin, 2.00 mm pitch, horizontal/side-entry|JST / S2B-PH-K series — confirm exact purchased suffix before public release|`Connector\\\_JST:JST\\\_PH\\\_S2B-PH-K\\\_1x02\\\_P2.00mm\\\_Horizontal`|Matches rev 0.1 KiCad footprint; exact orderable MPN remains to be recorded.|
|`F1 (holder)`|1|Input overcurrent protection holder|5 × 20 mm PCB fuse holder / clip arrangement|Littelfuse-compatible / 445/030 series footprint basis — confirm fitted holder|`Fuse:Fuseholder\\\_Littelfuse\\\_445\\\_030\\\_series\\\_5x20mm`|KiCad design specifies a 5 × 20 mm holder footprint.|
|`F1 (fuse element)`|1|Input overcurrent protection|1 A, 5 × 20 mm cartridge fuse|To be selected / Confirm fuse characteristic and rating before public release|`Installed in F1 holder`|Schematic value is 'Fuse 1A'; slow/fast-blow characteristic is not presently specified.|
|`D1`|1|Reverse-polarity series protection|1N5817 Schottky diode, 20 V, 1 A|Vishay or equivalent / 1N5817|`Diode\\\_THT:D\\\_DO-41\\\_SOD81\\\_P10.16mm\\\_Horizontal`|Polarity must match RAW\_FUSED to RAW\_PROT series path.|
|`U4`|1|Fixed 3.3 V regulated supply|S7V8F3 step-up/step-down voltage regulator module, 3.3 V fixed|Pololu / 2122 / S7V8F3|`ThinPod\\\_PowerModules:Pololu\\\_S7V8F3\\\_S7V8x\\\_Module`|Project-local mating/interface footprint must be independently authored or appropriately licensed before publication.|
|`U3`|1|PFET high-side switch for accelerometer supply rail|ZVP2106A P-channel enhancement-mode MOSFET|Diodes Incorporated / ZVP2106A|`ThinPod:PFET\\\_ZVP2106A\\\_GSD\\\_THT`|Verify G-S-D mapping against the published footprint before repository release.|
|`U2`|1|Analogue vibration sensing|ADXL1005 evaluation-board / breakout implementation|Analog Devices / EVAL-ADXL1005Z — confirm exact physically used assembly before public release|`ThinPod:AnalogDevices\\\_EVAL-ADXL100xZ\\\_20p35mm`|The footprint is a module interface, not a bare ADXL1005BCPZ chip footprint; publish only if independently authored or appropriately licensed.|
|`U5`|1|Commercial pod-side MCU/UWB board interface and ADC/control connection|DWM3001CDK Ultra-Wideband Module Development Kit|Qorvo / DWM3001CDK|`ThinPod:ThinPod\\\_DWM3001CDK\\\_Mating\\\_Interface\\\_revA`|Release footprint is Thin-Pod-authored; corrected rev 0.1 interface ties J1\_2 and J10\_6/9/14/20/25 to carrier GND.|
|`R1`|1|Analogue signal series resistor / RC filter element|8 kΩ|Generic / Any suitable THT axial resistor; 1% recommended|`Resistor\\\_THT:R\\\_Axial\\\_DIN0207\\\_L6.3mm\\\_D2.5mm\\\_P7.62mm\\\_Horizontal`|With C1 = 680 pF, nominal single-pole corner frequency is approximately 29.3 kHz.|
|`R2`|1|PFET gate bias resistor|100 kΩ|Generic / Any suitable THT axial resistor; 1% recommended|`Resistor\\\_THT:R\\\_Axial\\\_DIN0207\\\_L6.3mm\\\_D2.5mm\\\_P7.62mm\\\_Horizontal`|Function and connection should be checked against final corrected schematic.|
|`R3`|1|PFET control/gate series resistor|1 kΩ|Generic / Any suitable THT axial resistor; 1% recommended|`Resistor\\\_THT:R\\\_Axial\\\_DIN0207\\\_L6.3mm\\\_D2.5mm\\\_P7.62mm\\\_Horizontal`|Connected between PFET\_CTRL and PFET\_GATE in the rev 0.1 design.|
|`R4`|1|ADC\_NODE pull-down / input reference path|1 MΩ|Generic / Any suitable THT axial resistor; 1% recommended|`Resistor\\\_THT:R\\\_Axial\\\_DIN0207\\\_L6.3mm\\\_D2.5mm\\\_P7.62mm\\\_Horizontal`|—|
|`C1`|1|ADC\_NODE RC filter capacitor|680 pF|Generic / C0G/NP0 ceramic recommended|`Capacitor\\\_THT:C\\\_Disc\\\_D3.0mm\\\_W1.6mm\\\_P2.50mm`|With R1 = 8 kΩ, nominal single-pole corner frequency is approximately 29.3 kHz.|
|`C2`|1|Local accelerometer supply decoupling|100 nF|Generic / Ceramic capacitor; suitable voltage rating|`Capacitor\\\_THT:C\\\_Disc\\\_D3.0mm\\\_W1.6mm\\\_P2.50mm`|—|
|`C3`|1|Local accelerometer supply bulk decoupling|10 µF, non-polar|Generic / Non-polar ceramic or suitable non-polar component; confirm fitted part|`Capacitor\\\_THT:C\\\_Disc\\\_D5.0mm\\\_W2.5mm\\\_P5.00mm`|Rev 0.1 source uses a non-polar THT footprint.|
|`TP1`|1|RAW\_IN measurement point|RAW\_IN|Generic / plated PCB test point|`TestPoint:TestPoint\\\_THTPad\\\_D1.5mm\\\_Drill0.7mm`|—|
|`TP2`|1|RAW\_FUSED measurement point|RAW\_FUSED|Generic / plated PCB test point|`TestPoint:TestPoint\\\_THTPad\\\_D1.5mm\\\_Drill0.7mm`|—|
|`TP3`|1|RAW\_PROT measurement point|RAW\_PROT|Generic / plated PCB test point|`TestPoint:TestPoint\\\_THTPad\\\_D1.5mm\\\_Drill0.7mm`|—|
|`TP4`|1|Regulated supply measurement point|3V3+|Generic / plated PCB test point|`TestPoint:TestPoint\\\_THTPad\\\_D1.5mm\\\_Drill0.7mm`|—|
|`TP5`|1|Ground reference measurement point|GND|Generic / plated PCB test point|`TestPoint:TestPoint\\\_THTPad\\\_D1.5mm\\\_Drill0.7mm`|—|
|`TP6`|1|Raw sensor analogue measurement point|SENSOR\_SIGNAL\_RAW / ACC\_VOUT|Generic / plated PCB test point|`TestPoint:TestPoint\\\_THTPad\\\_D1.5mm\\\_Drill0.7mm`|—|
|`TP7`|1|Filtered sensor / ADC input measurement point|SENSOR\_SIGNAL\_FILTERED / ADC\_NODE|Generic / plated PCB test point|`TestPoint:TestPoint\\\_THTPad\\\_D1.5mm\\\_Drill0.7mm`|—|

## Proprietary third-party component boundary

The following commercially supplied items are necessary physical components of the released assembly, but are **not** claimed as open-hardware contributions of Thin-Pod:

|Reference|Third-party item|Purpose|
|-|-|-|
|`U2`|Analog Devices ADXL1005 evaluation-board / breakout implementation|Analogue vibration sensing|
|`U4`|Pololu S7V8F3 fixed 3.3 V regulator module|Local regulated supply|
|`U5`|Qorvo DWM3001CDK development board|Pod-side MCU/UWB and ADC/control interface|

The Thin-Pod hardware licence applies to the creator-designed carrier PCB, editable design files and Thin-Pod-authored interface footprints. Commercial components and vendor documentation remain subject to their original manufacturers' terms.

## Footprint and publication notes

|Item|Release treatment|
|-|-|
|`U5` CDK interface|Publish `ThinPod\\\_DWM3001CDK\\\_Mating\\\_Interface\\\_revA.kicad\\\_mod`, the independently authored Thin-Pod mating-interface footprint. |
|`U2` ADXL1005 module interface|Footprint: third-party-snapeda:AnalogDevices\_EVAL-ADXL100xZ\_20p35mm<br />Release note: SnapEDA/SnapMagic third-party Design File included under CC BY-SA 4.0 + Design Exception 1.0.|
|`U4` Pololu module interface|Footprint: third-party-snapeda:Pololu\_S7V8F3\_S7V8x\_Module<br />Release note: SnapEDA/SnapMagic third-party Design File included under CC BY-SA 4.0 + Design Exception 1.0.|
|`U3` PFET footprint|Provenance recorded for the project-local ZVP2106A footprint before public release.|
|Standard KiCad footprints|Retained as standard-library dependencies or preserved as their originating licence and attribution if copied into the repository.|

## Board features not separately populated in this BOM

|Reference / feature|Description|Status|
|-|-|-|
|`H1`–`H3`|M2.5 mounting holes, `MountingHole:MountingHole\\\_2.7mm\\\_M2.5`|PCB features; mounting hardware selected according to enclosure/test setup|
|CDK support holes within the U5 interface|Mechanical support provisions included in the mating footprint|PCB features unless spacers/fasteners are specified|
|Copper ground-return correction|CDK J1/J10 ground pins bonded to carrier `GND` in release source and Gerbers|Implemented in PCB copper; no wire-link BOM item|

## Items to confirm before OSHWA submission

The following entries should be closed before the repository is made public and used in the OSHWA application:

1. Confirm the exact orderable JST connector fitted at `J1`.
2. Confirm the fuse element type, breaking characteristic and orderable part number fitted at `F1`.
3. Confirm the precise ADXL1005 evaluation-board or breakout assembly used at `U2`.
4. Confirm independent authorship or redistribution-compatible provenance for the `U2`, `U3` and `U4` project-local footprints.
5. Confirm that the final corrected KiCad schematic and PCB match this BOM and the published fabrication outputs.

## Source basis

The BOM is based on the Thin-Pod rev 0.1 KiCad schematic and PCB source, with the OSHWA release update that replaces the earlier CDK footprint with the independently authored Thin-Pod mating-interface footprint and incorporates the corrected CDK ground return.

Vendor component references:

* Qorvo DWM3001CDK: https://www.qorvo.com/products/p/DWM3001CDK
* Analog Devices ADXL1005: https://www.analog.com/en/products/adxl1005.html
* Pololu S7V8F3: https://www.pololu.com/product/2122
* Diodes Incorporated ZVP2106A: https://www.diodes.com/part/view/ZVP2106A
* Vishay 1N5817 datasheet: https://www.vishay.com/docs/88525/1n5817.pdf

## Licensing

This BOM is documentation for the Thin-Pod rev 0.1 release and is intended to be published under **CC-BY-4.0** together with the project documentation. The creator-designed hardware source files are licensed separately under **CERN-OHL-W-2.0**.

