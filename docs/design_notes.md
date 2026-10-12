# Design Notes — Radiation-Hardened SRAM

## 1. Overview

This document describes the circuit architecture and design methodology used in the implementation and evaluation of conventional 6T SRAM and radiation-hardened DICE SRAM cells. The designs were evaluated through transistor-level simulations using Cadence Virtuoso with a 180 nm CMOS technology library.

## 2. Conventional 6T SRAM

The conventional 6T SRAM cell consists of six MOS transistors:

- **Four transistors** form two cross-coupled inverters that store one bit of data.
- **Two access transistors** connect the internal storage nodes to the complementary bitlines, BL and BLB, under control of the wordline (WL).

The cross-coupled inverters maintain the stored state while the cell is powered. The access transistors enable communication with the bitlines during read and write operations.

### Functional Operations

- **Hold:** The wordline remains inactive, isolating the storage nodes from the bitlines.
- **Read:** The bitlines are precharged before the wordline is activated. The stored state produces a differential response on BL and BLB.
- **Write:** Complementary data values are applied to the bitlines, and the wordline is activated to update the internal storage nodes.

## 3. DICE SRAM

The Dual Interlocked Storage Cell (DICE) architecture uses redundant, interlocked storage nodes to improve resilience against transient disturbances.

The implemented DICE cell uses a 12-transistor storage architecture with four internal storage nodes, designated A, B, C, and D, and two access transistors.

The interlocked feedback structure allows the unaffected nodes to help restore a disturbed storage node to its correct logic state. This provides greater tolerance to certain single-event-upset conditions than a conventional 6T SRAM cell.

## 4. Simulation Environment

| Parameter | Configuration |
|---|---|
| Design tool | Cadence Virtuoso |
| Technology | 180 nm CMOS |
| Supply voltage | 1.8 V |
| Analysis | Transistor-level circuit simulation |

## 5. Functional Verification

Both SRAM architectures were evaluated under normal operating conditions.

The verification process included:

- Hold-state stability
- Write operation and data storage
- Read operation and storage-node behavior
- Transient response during circuit operation

## 6. SEU Transient Injection

Single-event-upset behavior was investigated by applying transient current pulses to internal storage nodes and observing whether the stored state changed.

For the conventional 6T SRAM cell, the injected current amplitude was increased to identify the threshold at which an upset occurred. The corresponding critical-charge values were calculated using the method documented in the project report.

For the DICE cell, the storage-node response was examined under transient current injection to evaluate its recovery behavior.

## 7. Comparative Evaluation

The two architectures were compared in terms of their storage-node response, sensitivity to transient disturbances, and ability to preserve or recover the stored logic state.

The 6T SRAM results demonstrate that upset sensitivity can vary with the affected storage node and the stored logic value. The DICE architecture uses interlocked redundancy to improve tolerance to single-node disturbances.

Refer to the project report for the measured critical-charge values, waveforms, and detailed simulation results.

## References

See the references section of the project report for the sources used in the design and analysis of the SRAM architectures.
