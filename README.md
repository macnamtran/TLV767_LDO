# TLV767 Adjustable LDO Board

## 1. Project Overview

### Schematic

![TLV767 schematic](images/schematic.png)

### PCB Layout

![TLV767 PCB layout](images/layout.png)

### 3D View

![TLV767 3D view](images/3D.png)

This project is a KiCad PCB design for an adjustable low-dropout linear regulator board using the **TLV76701DRVx**.

The board takes a DC input voltage and produces an adjustable regulated output voltage. The main purpose of this project is to practise reading a regulator datasheet, choosing external components, calculating feedback resistor values, and designing a PCB layout that is ready for fabrication review.

## 2. Design Target

| Parameter | Value |
|---|---:|
| Regulator IC | TLV76701DRVx |
| Package | DRV, 6-pin WSON, 2 mm × 2 mm |
| PCB tool | KiCad |
| Input voltage | VIN |
| Output voltage | Adjustable |
| Target output current | Up to 1 A, depending on thermal conditions |
| PCB type | 2-layer PCB |
| Main ground strategy | Bottom-layer GND plane |
| Manufacturer check | KiCad DRC + JLCPCB/JLCDFM Gerber check |

## 3. Datasheet Information Used

The TLV767 datasheet gives the following key information used in this design:

| Item | Datasheet information |
|---|---|
| Input voltage range | 2.5 V to 16 V |
| Adjustable output range | 0.8 V to 14.6 V |
| Maximum output current | 1 A when VIN ≥ 3 V |
| Feedback voltage | 0.8 V |
| Recommended input capacitor | 1 µF minimum |
| Recommended output capacitor | 1 µF to 220 µF |
| Output capacitor ESR | 2 mΩ to 500 mΩ |
| Enable pin | Can be connected to IN or left floating |
| Thermal pad | Connect to GND or leave floating; GND plane improves thermal performance |

## 4. Pin Connection Summary

The selected package is the **DRV adjustable 6-pin WSON** package.

| Pin | Name | Connection |
|---:|---|---|
| 1 | OUT | Connected to VOUT |
| 2 | FB | Connected to feedback divider |
| 3 | GND | Connected to GND |
| 4 | EN | Connected to VIN for always-on operation |
| 5 | GND | Connected to GND |
| 6 | IN | Connected to VIN |
| 7 / Thermal pad | PAD | Connected to GND plane |

Important note: for this chip, the exposed thermal pad is **GND**, not VOUT. This is different from the previous LT3080 design, where the exposed pad was VOUT.

## 5. Component choices

From the data sheet:
**1. Stable with 1 µF ceramic capacitors:**  
The datasheet states that the TLV767 is stable with 1 µF ceramic capacitors. Therefore, C3 = 1 µF is used as the required output capacitor for stability, and C4 = 100 nF is added in parallel as a high-frequency bypass capacitor for fast voltage spikes. The same structure is used on the input side, with C1 = 1 µF and C2 = 100 nF.

**2. VOUT = VFB x (1 + R1/R2); VFB = 0.8V:** 
Use a potentiometer (RV1=500kOHM) for R1, R2 (=100kOHM) => VOUT = [0.8V, 4.8V]
