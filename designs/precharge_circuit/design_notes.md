# SRAM Precharge Circuit — Design Notes

## 1. Overview

This directory documents the precharge circuitry associated with the conventional SRAM design.

The purpose of the circuit is to establish the required bitline conditions before a read operation. Proper bitline preparation is important for obtaining meaningful read behavior from the SRAM cell.

## 2. Circuit Architecture

The schematic contains circuitry associated with the complementary bitlines:

- **BL:** Bitline.
- **BLB:** Complementary bitline.
- **PC:** Precharge control signal.
- **Supply and reference connections:** Provide the operating voltages required by the circuit.

The precharge network interfaces with the SRAM bitlines and prepares them for subsequent memory access.

## 3. Operating Principle

The precharge circuit prepares the bitlines before the SRAM read phase.

A typical read sequence consists of the following stages:

1. **Precharge:** Establish the initial bitline voltages using the precharge circuitry.
2. **Read activation:** Activate WL to connect the internal storage nodes to the bitlines.
3. **Bitline response:** The stored state influences the voltages on BL and BLB.
4. **Read completion:** Deactivate the access path and prepare the circuit for the next operation as required by the control sequence.

The precise timing and active polarity of PC depend on the implemented circuit and its applied control waveform.

## 4. Integration with the SRAM Cell

The precharge circuit supports the read operation by preparing BL and BLB before the wordline activates the SRAM access transistors.

The precharge network and SRAM cell must operate with compatible control timing. Incorrect sequencing can interfere with the expected bitline response and affect read behavior.

## 5. Design and Simulation Environment

| Parameter | Configuration |
|---|---|
| Design platform | Cadence Virtuoso |
| Technology | 180 nm CMOS |
| Supply voltage | 1.8 V |

## 6. Verification Focus

The precharge circuit should be evaluated for:

- Correct initialization of BL and BLB.
- Appropriate response to the PC control signal.
- Compatibility with the SRAM wordline sequence.
- Consistent bitline behavior during read preparation and access.

Refer to the project report and associated simulation waveforms for the circuit's observed behavior.

## Related Designs

- [`../6t_sram/`](../6t_sram/) — Conventional SRAM storage cell.
- [`../dice_sram/`](../dice_sram/) — Radiation-hardened SRAM architecture.

## References

See [`../../references/references.md`](../../references/references.md) for the research references associated with this project.
