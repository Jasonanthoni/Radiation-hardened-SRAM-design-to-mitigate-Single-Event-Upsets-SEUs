# DICE SRAM — Design Notes

## 1. Overview

This directory documents the Dual Interlocked Storage Cell (DICE) SRAM architecture implemented to improve resistance to radiation-induced Single Event Upsets (SEUs).

Unlike conventional 6T SRAM, which stores a bit using two cross-coupled inverters, DICE uses redundant, interlocked storage nodes to help preserve the correct state during a transient disturbance.

## 2. Circuit Architecture

The implemented DICE cell uses a 12-transistor architecture comprising:

- **Four interlocked storage nodes:** A, B, C, and D.
- **Interconnected feedback paths:** Maintain the relationships among the internal storage nodes.
- **Access transistors:** Provide the interface between the storage structure and the complementary bitlines during memory operations.

The interlocked storage structure allows the unaffected nodes to reinforce the correct logic state when a single storage node is disturbed.

## 3. Storage and Recovery Mechanism

The four internal nodes participate in maintaining the encoded logic state through their interdependent feedback connections.

When a transient current pulse disturbs one of the storage nodes, the other nodes can preserve the information needed to restore the original state. Once the disturbance subsides, the feedback network can drive the affected node back toward its correct logic value.

This recovery mechanism is the central feature that distinguishes DICE from conventional 6T SRAM.

## 4. Functional Operation

The DICE SRAM was evaluated under normal memory operating conditions.

### Hold Operation

The cell retains its stored state through the interlocked feedback network while the access path is inactive.

### Write Operation

The access path allows data to be written into the storage structure. Correct operation depends on the applied bitline values and control sequence.

### Read Operation

The stored information is accessed through the bitline interface using the appropriate wordline control.

## 5. SEU Transient Injection

The DICE architecture was evaluated by applying transient current injection to an internal storage node and observing the resulting node-voltage response.

The recorded simulations showed temporary disturbance followed by recovery of the affected node, with no permanent bit flip observed under the tested conditions.

This behavior illustrates the benefit of interlocked storage redundancy for mitigating single-node transient disturbances.

## 6. Design and Simulation Environment

| Parameter | Configuration |
|---|---|
| Design platform | Cadence Virtuoso |
| Technology | 180 nm CMOS |
| Supply voltage | 1.8 V |

## 7. Comparison with Conventional 6T SRAM

The conventional 6T cell relies on two cross-coupled inverters to retain the stored bit. A sufficiently strong disturbance can cause the cell to settle into the opposite stable state.

DICE introduces additional storage nodes and interlocked feedback paths. These provide a mechanism for recovering from certain transient disturbances without external correction.

The project results demonstrate improved resistance to the investigated SEU conditions compared with the conventional 6T SRAM cell.

## Related Designs

- [`../6t_sram/`](../6t_sram/) — Conventional SRAM reference architecture.
- [`../precharge_circuit/`](../precharge_circuit/) — Bitline preparation circuitry.

## References

See [`../../references/references.md`](../../references/references.md) for the research references associated with this project.
