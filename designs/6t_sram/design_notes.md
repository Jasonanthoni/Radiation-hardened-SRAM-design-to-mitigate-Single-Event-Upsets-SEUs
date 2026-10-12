# 6T SRAM — Design Notes

## 1. Overview

This directory documents the conventional 6-transistor (6T) Static Random Access Memory (SRAM) cell implemented using Cadence Virtuoso and a 180 nm CMOS technology library.

The cell serves as the reference architecture for evaluating the susceptibility of conventional SRAM to Single Event Upsets (SEUs) and comparing its behavior with the radiation-hardened DICE SRAM architecture.

## 2. Circuit Architecture

The 6T SRAM cell consists of six MOS transistors organized into two functional sections:

- **Cross-coupled inverter pair:** Four transistors form two interconnected CMOS inverters that maintain the stored data.
- **Access transistor pair:** Two transistors connect the internal storage nodes to the complementary bitlines, BL and BLB, controlled by the wordline (WL).

The cross-coupled inverters provide regenerative feedback, allowing the cell to retain its stored state while power is supplied.

The internal nodes, Q and Q̅, represent complementary logic values.

## 3. Functional Operation

### Hold Operation

When WL is inactive, the access transistors isolate the internal storage nodes from BL and BLB. The cross-coupled inverters maintain the previously stored logic state.

### Write Operation

Complementary data values are applied to BL and BLB. Activating WL connects the bitlines to the internal storage nodes, allowing the cell to transition to the desired state.

### Read Operation

The bitlines are prepared before WL is activated. The stored state influences the bitline voltages through the access transistors and internal pull-down paths, enabling the stored information to be sensed.

## 4. Design and Simulation Environment

| Parameter | Configuration |
|---|---|
| Design platform | Cadence Virtuoso |
| Technology | 180 nm CMOS |
| Supply voltage | 1.8 V |

## 5. Single Event Upset Analysis

The conventional SRAM cell was evaluated using transient current injection to emulate the electrical disturbance caused by a radiation-induced particle strike.

The injected current amplitude was increased to identify the threshold at which the stored state changed. Critical-charge values were then calculated for the investigated storage nodes and logic states.

The recorded results demonstrate that the cell's sensitivity to an upset depends on the affected node and the stored logic value.

Refer to the project report for the threshold currents, critical-charge measurements, and detailed simulation results.

## 6. Design Considerations

The 6T SRAM architecture offers compact storage using six transistors per bit. However, its cross-coupled inverter structure can transition to the opposite stable state when a sufficiently strong transient disturbance overcomes the stored state.

The cell therefore provides a useful baseline for assessing the benefits of radiation-hardened storage architectures.

## Related Designs

- [`../precharge_circuit/`](../precharge_circuit/) — Bitline preparation circuitry.
- [`../dice_sram/`](../dice_sram/) — Radiation-hardened DICE SRAM architecture.

## References

See [`../../references/references.md`](../../references/references.md) for the research references associated with this project.
