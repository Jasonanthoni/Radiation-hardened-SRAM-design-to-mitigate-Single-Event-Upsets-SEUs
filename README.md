# <span style="color:#22D3EE;">Radiation Hardened SRAM Design to Mitigate Single Event Upsets (SEUs)</span>

<span style="color:#00FFFF;">Design · Simulation · Comparative Analysis</span>

Conventional **6T SRAM** vs. radiation-hardened **DICE SRAM** using Cadence Virtuoso and 180 nm CMOS technology.

---

This project investigates radiation-induced soft errors in Static Random Access Memory (SRAM) and evaluates the effectiveness of the **Dual Interlocked Storage Cell (DICE)** architecture in mitigating Single Event Upsets (SEUs). Both architectures were designed and evaluated through transistor-level circuit simulations, functional verification, and transient current injection.

## Table of Contents

- [Overview](#overview)
- [Project Objectives](#project-objectives)
- [Background: Single Event Upsets](#background-single-event-upsets)
- [Technology and Tools](#technology-and-tools)
- [Design Methodology](#design-methodology)
- [Conventional 6T SRAM](#conventional-6t-sram)
- [SEU Injection and Critical-Charge Analysis](#seu-injection-and-critical-charge-analysis)
- [DICE SRAM Architecture](#dice-sram-architecture)
- [Results and Comparative Analysis](#results-and-comparative-analysis)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Project Contributions](#project-contributions)
- [Future Work](#future-work)
- [References](#references)
- [Acknowledgements](#acknowledgements)

---

## Overview

Static Random Access Memory is widely used in processors, cache memories, microcontrollers, and System-on-Chip devices because of its high speed and low access latency. However, SRAM cells can be vulnerable to radiation-induced soft errors when energetic particles deposit charge at sensitive transistor nodes.

A Single Event Upset can change the logic state stored in a memory cell without permanently damaging the device. Such errors can compromise data integrity in applications that require high reliability, including aerospace electronics, satellites, and safety-critical control systems.

The conventional 6-Transistor (6T) SRAM cell offers a compact and efficient storage architecture, but its cross-coupled inverter structure can undergo a permanent state change when a sufficiently large transient disturbance affects a sensitive node.

This project investigates a radiation-hardened alternative: the **Dual Interlocked Storage Cell (DICE)**. By distributing stored information across four interlocked storage nodes, the DICE architecture can recover from certain single-node disturbances through its redundant feedback structure.

The project was implemented using Cadence Virtuoso with a 180 nm CMOS technology library.

### Project at a glance

| Parameter | Description |
|---|---|
| Project domain | Digital VLSI and circuit reliability |
| Memory architectures | Conventional 6T SRAM and DICE SRAM |
| Technology node | 180 nm CMOS |
| Supply voltage | 1.8 V |
| Design environment | Cadence Virtuoso |
| Circuit simulator | Cadence Spectre |
| Verification approach | Transient simulation |
| Fault-injection method | Transient current injection |
| Primary evaluation metric | Critical charge, \(Q_{\mathrm{crit}}\) |
| Project type | Academic team minor project |

### Key findings

- Verified normal read, write, and hold operations for both SRAM architectures through transient simulations.
- Investigated SEU susceptibility in the conventional 6T SRAM by increasing the injected current until a permanent bit flip occurred.
- Calculated critical-charge estimates for both storage nodes under stored logic 0 and stored logic 1 conditions.
- Observed recovery of the disturbed storage node in the DICE SRAM under the tested current-injection conditions.

---

## Project Objectives

The primary objective was to investigate radiation-induced Single Event Upsets in conventional SRAM and evaluate a radiation-hardened storage architecture.

The specific objectives were:

1. Design a conventional 6T SRAM cell using CMOS transistors.
2. Verify read, write, and hold operations using transient simulations.
3. Implement a precharge circuit to initialize the complementary bit lines before read operations.
4. Emulate radiation-induced disturbances through transient current injection.
5. Determine the threshold current at which the conventional SRAM undergoes a permanent bit flip.
6. Estimate the critical charge required to upset the conventional SRAM.
7. Design and simulate a DICE SRAM architecture.
8. Compare the recovery behavior of the conventional 6T SRAM and DICE SRAM under simulated disturbances.

---

## Background: Single Event Upsets

### What is an SEU?

A Single Event Upset is a soft error caused when an energetic particle deposits charge in a sensitive region of a semiconductor device. The collected charge produces a transient current that can disturb the voltage of an internal storage node.

If the disturbance exceeds the cell's ability to restore its original state, regenerative feedback can force the memory into the opposite stable state.

Unlike permanent hardware damage, an SEU changes the stored information without necessarily causing physical destruction of the circuit.

### Why are SRAM cells susceptible?

A conventional 6T SRAM cell stores one bit using two cross-coupled CMOS inverters. The internal nodes are complementary and reinforce each other's states through positive feedback.

A sufficiently large transient disturbance can push a storage node beyond the switching threshold of the feedback network, causing the cell to settle into the opposite state.

The minimum collected charge capable of causing this transition is known as the **critical charge**.

### Radiation-Hardened-by-Design

Radiation-Hardened-by-Design (RHBD) techniques improve circuit tolerance through architectural or layout modifications rather than relying exclusively on specialized fabrication processes.

The DICE architecture is an RHBD technique that uses redundant, interlocked storage nodes to prevent certain transient disturbances from propagating into a permanent data error.

---

## Technology and Tools

| Tool or parameter | Usage |
|---|---|
| Cadence Virtuoso | Transistor-level schematic design |
| Cadence Spectre | Transient circuit simulation |
| Cadence waveform viewer | Analysis of storage-node voltages and injected-current waveforms |
| 180 nm CMOS technology library | MOS transistor models and circuit implementation |
| Supply voltage: 1.8 V | Operating condition used in the simulations |

The analysis was performed at the schematic level. Physical layout, extracted-parasitic simulation, and experimental radiation testing were outside the scope of the completed work.

---

## Design Methodology

The project followed a comparative design and simulation workflow.

1. **6T SRAM implementation:** Design the conventional cell using two cross-coupled CMOS inverters and two access transistors.
2. **Functional verification:** Verify the storage cell through write, read, and hold simulations.
3. **Precharge circuit:** Initialize both complementary bit lines to the supply voltage before read operations.
4. **SEU fault injection:** Apply a transient current pulse to a sensitive storage node.
5. **Threshold determination:** Increase the pulse amplitude iteratively until the stored logic state permanently flips.
6. **Critical-charge calculation:** Estimate the critical charge from the threshold peak current and pulse time constants.
7. **DICE SRAM implementation:** Design the four-node interlocked storage architecture and verify its normal operation.
8. **Recovery analysis:** Apply transient current injection and observe whether the disturbed node returns to its original state.
9. **Comparative analysis:** Compare the observed behavior of both architectures.

---

## Conventional 6T SRAM

### Architecture

The conventional 6T SRAM cell consists of:

- Two cross-coupled CMOS inverters.
- Two NMOS access transistors.
- Two complementary bit lines, BL and BLB.
- One word line, WL.
- Two internal storage nodes, Q and QB.

The cross-coupled inverters provide two stable states:

- \(Q=1,\ QB=0\)
- \(Q=0,\ QB=1\)

The access transistors connect the storage nodes to the bit lines when the word line is asserted.

### Schematic

![Conventional 6T SRAM schematic](https://github.com/user-attachments/assets/55fa7c9b-2bf1-4e3b-9d56-35b7a26f378d)

*Figure 1. Conventional 6T SRAM transistor-level schematic.*

### Read operation

Before a read, both bit lines are precharged to \(V_{DD}\). When WL is asserted, the stored state determines which bit line begins to discharge through the cell.

The resulting voltage difference between BL and BLB represents the stored data.

### Write operation

Complementary values are applied to BL and BLB before asserting WL. The access transistors connect the bit lines to the internal nodes, allowing the write drivers to overwrite the previous state.

### Hold operation

When WL is LOW, the access transistors are OFF. The cross-coupled inverters maintain the stored logic state through regenerative feedback.

### Precharge circuit

The precharge circuit establishes equal initial conditions on the complementary bit lines before a read operation. This is important because SRAM read behavior depends on the voltage difference that develops between the bit lines.

![DICE SRAM with precharge circuit](https://github.com/user-attachments/assets/9d2ec3c4-f8e1-4d0e-9639-c056ec6e6c06)

*Figure 2. DICE SRAM and precharge circuitry as documented in the project materials.*

The conventional 6T SRAM was also evaluated with transient current injection to investigate the threshold at which its stored data becomes corrupted.

### Transistor sizing

The stability and writeability of a 6T SRAM cell depend on the relative strengths of its pull-up, pull-down, and access transistors.

Two commonly used sizing ratios are:

**Cell ratio**

\[
CR=\frac{W_{\mathrm{PullDown}}}{W_{\mathrm{Access}}}
\]

The cell ratio influences read stability by determining the relative strengths of the pull-down and access transistors.

**Pull-up ratio**

\[
PR=\frac{W_{\mathrm{Access}}}{W_{\mathrm{PullUp}}}
\]

The pull-up ratio influences writeability by determining the relative strengths of the access and pull-up transistors.

The original transistor-width and channel-length values are not available in the archived project materials. Exact device dimensions are therefore not specified here.

---

## SEU Injection and Critical-Charge Analysis

### Transient current injection

Radiation-induced charge collection was emulated using a double-exponential transient current pulse applied to a sensitive storage node.

The current-pulse model is expressed in the project report as:

\[
I(t)=I_{\mathrm{peak}}
\left(e^{-t/\tau_{\mathrm{fall}}}
-e^{-t/\tau_{\mathrm{rise}}}\right)
\]

where:

- \(I_{\mathrm{peak}}\) is the peak-current parameter.
- \(\tau_{\mathrm{rise}}\) is the rise time constant.
- \(\tau_{\mathrm{fall}}\) is the decay time constant.

The current amplitude was increased iteratively while observing the storage-node response.

Three behaviors were identified:

1. **Below threshold:** The storage node experiences a temporary disturbance but the cell retains its data.
2. **Near threshold:** The cell approaches the transition between recovery and an upset.
3. **Above threshold:** Regenerative feedback drives the cell into the opposite stable state, producing a permanent bit flip.

### Critical-charge calculation

Critical charge is the minimum collected charge required to cause a permanent change in the stored logic state.

The project used the following approximation:

\[
Q_{\mathrm{crit}}\approx
I_{\mathrm{peak}}
(\tau_{\mathrm{fall}}-\tau_{\mathrm{rise}})
\]

The threshold peak current obtained from transient simulations was used to calculate the critical-charge estimates.

This expression is the approximation documented in the project report; it should not be interpreted as a general exact integration formula for every double-exponential pulse definition.

### Critical-charge results

The following values were recorded for the conventional 6T SRAM.

**Stored logic 0**

| Storage node | Threshold peak current (µA) | Critical charge (fC) |
|---|---:|---:|
| Q | 318.60 | 28.67 |
| QB | 240.31 | 21.63 |

**Stored logic 1**

| Storage node | Threshold peak current (µA) | Critical charge (fC) |
|---|---:|---:|
| Q | 86.65 | 7.80 |
| QB | 98.85 | 8.90 |

### Interpretation

The results show that the critical charge depends on the stored logic state and the storage node being disturbed.

The largest reported critical charge is 28.67 fC for node Q when storing logic 0. The smallest is 7.80 fC for node Q when storing logic 1.

The results therefore indicate asymmetric SEU sensitivity in the simulated 6T SRAM cell. A smaller critical charge means that less injected charge is required to produce an upset under the corresponding simulated condition.

The threshold values represent the results of the original simulations. The archived transistor-level design files and complete simulation setup are not currently available for independent reproduction.

---

## DICE SRAM Architecture

### Architecture and operating principle

The Dual Interlocked Storage Cell is a radiation-hardened storage architecture that uses four mutually interlocked storage nodes rather than the two storage nodes of a conventional 6T SRAM cell.

In the DICE architecture, the stored information is distributed across four interconnected nodes. The feedback paths are arranged so that a transient disturbance affecting one storage node can be counteracted by the unaffected nodes.

When the injected transient disappears, the interlocked feedback network can restore the disturbed node to its original logic level.

The implemented DICE SRAM was designed and simulated using the same 180 nm CMOS technology library and 1.8 V supply voltage used for the conventional SRAM.

### Schematic

![DICE SRAM schematic](https://github.com/user-attachments/assets/55263ff8-401c-4484-8ee0-ee5ce94b4950)

*Figure 3. DICE SRAM transistor-level schematic.*

### Functional verification

The DICE SRAM was evaluated through:

- **Write:** Verify that the desired state can be established.
- **Read:** Verify retrieval of the stored data following bit-line precharge.
- **Hold:** Verify retention of the stored data with WL LOW.
- **SEU injection:** Apply a transient current pulse and observe the response of the storage nodes.

The project report documents successful functional simulations and recovery of the disturbed node following transient injection.

### Observed recovery behavior

![DICE SRAM precharge circuit](https://github.com/user-attachments/assets/9d2ec3c4-f8e1-4d0e-9639-c056ec6e6c06)

*Figure 4. DICE SRAM design with precharge circuitry.*

According to the project records and subsequent confirmation, the DICE SRAM recovered under all tested injection conditions, with the maximum tested current-pulse amplitude reported as 1 A.

This value is retained as reported. The original pulse-source configuration and detailed simulation files are unavailable, so the precise relationship between this amplitude and the microampere-scale 6T threshold-current results cannot currently be independently verified.

The observed recovery supports the effectiveness of the implemented interlocked storage structure under the tested simulation conditions. It does not establish immunity to every possible pulse shape, duration, or multi-node disturbance.

---

## Results and Comparative Analysis

### Functional behavior

Both architectures successfully completed the reported read, write, and hold simulations.

### Response to transient disturbances

| Evaluation criterion | Conventional 6T SRAM | DICE SRAM |
|---|---|---|
| Storage mechanism | Two cross-coupled storage nodes | Four interlocked storage nodes |
| Normal read/write/hold operation | Verified in transient simulations | Verified in transient simulations |
| SEU evaluation | Transient current injection | Transient current injection |
| Threshold behavior | Permanent bit flip when the upset threshold is exceeded | Disturbed node recovered under the tested conditions |
| Critical-charge data | Numerical estimates available for Q and QB | Comparable numerical threshold values not available in the archived data |
| Recovery mechanism | Original state is not retained after a successful upset | Interlocked feedback restores the disturbed node |
| Physical radiation testing | Not performed | Not performed |

### Key observations

**1. The conventional 6T SRAM exhibits state-dependent upset sensitivity.**

The reported critical-charge values vary from 7.80 fC to 28.67 fC across the evaluated stored states and nodes.

**2. The DICE SRAM demonstrates recovery from the tested disturbances.**

The project simulations show temporary node-voltage disturbances followed by recovery, rather than a permanent logic inversion, under the conditions examined.

**3. Redundant storage improves tolerance to certain single-node disturbances.**

The DICE architecture uses its interlocked feedback paths to maintain the original data when one storage node is temporarily disturbed.

**4. The comparison is simulation-based.**

The results support the observed circuit behavior but do not constitute experimental radiation characterization, silicon measurements, or post-layout verification.

---

## Repository Structure

The repository is intended to organize the project documentation, design evidence, and simulation results in the following structure.

```text
Radiation-hardened-SRAM-design-to-mitigate-SEUs/
│
├── README.md
│
├── docs/
│   ├── report/
│   │   └── minor_project_report.pdf
│   ├── presentation/
│   │   └── minor_project_presentation.pdf
│   └── design_notes.md
│
├── design/
│   ├── 6t_sram/
│   ├── precharge_circuit/
│   └── dice_sram/
│
├── simulations/
│   ├── 6t_sram/
│   │   ├── read/
│   │   ├── write/
│   │   ├── hold/
│   │   └── seu_injection/
│   └── dice_sram/
│       ├── read/
│       ├── write/
│       ├── hold/
│       └── seu_injection/
│
├── results/
│   ├── waveforms/
│   ├── critical_charge/
│   └── comparison.md
│
├── images/
│   ├── schematics/
│   ├── architecture/
│   └── waveforms/
│
└── references/
    └── references.md
```

This is the proposed organization, not a guarantee that every listed directory or file is already present. Only add directories and files that contain actual project material.

---

## Getting Started

### Viewing the project

1. Clone or download this repository.
2. Open `README.md` to understand the architecture and reported results.
3. Navigate to `docs/report/` for the full project report.
4. Open the presentation in `docs/presentation/` for a concise project overview.
5. Browse `images/` and `results/` for schematics and simulation evidence, where available.

### Reproducing the simulations

The original Cadence design databases and complete simulation setup are not currently available in the archived project materials. Consequently, the original simulations cannot be reproduced directly from this repository at present.

Reproduction would require the transistor-level schematics, compatible 180 nm device models, simulation configurations, transient current-source parameters, and the relevant testbench files.

The reported critical-charge values and qualitative recovery observations are documented for reference, but they should not be treated as independently reproducible simulation outputs until the necessary design files are recovered or the circuits are implemented again.

---

## Project Contributions

This project was completed as a team minor project in the Department of Electronics and Communication Engineering at KLE Technological University, Hubballi, during the 2025–26 academic year.

The project involved the implementation and simulation-based evaluation of conventional and radiation-hardened SRAM architectures.

### Team members

- Jason Anthoni
- Rahul Kavaloor
- Vidyadhar Shenvi
- Vaishnavi Srekar

### Project guides

- Prof. Pooja Chandaragi
---

## Future Work

Potential extensions to this project include:

- **Physical Design:** Implement the 6T and DICE SRAM cells with layout verification and design-rule checks.
- **Post-Layout Analysis:** Evaluate the effects of parasitic resistance and capacitance on read/write performance and SEU resilience.
- **Power, Performance, and Area (PPA):** Compare both architectures in terms of power consumption, access delay, and silicon area.
- **Process Variation Analysis:** Investigate robustness across process, voltage, and temperature (PVT) corners.
- **Fault-Injection Analysis:** Extend transient-current injection experiments to study recovery behavior under different pulse characteristics and multi-node disturbances.

## References

The following references are listed in the project report's literature review.

1. W. Zhang, P. Li, P. Guo, and Y. Chen, “Multi-Level Single Event Effect-Hardened Design for SRAM-FPGA-Based System,” *Proceedings of the 14th International Conference on Reliability, Maintainability and Safety (ICRMS)*, 2023, pp. 1452–1455. DOI: [10.1109/ICRMS59672.2023.00247](https://doi.org/10.1109/ICRMS59672.2023.00247).

2. J.-Y. Oh and S.-H. Jo, “Radiation-Hardened 16T SRAM Cell with Improved Read and Write Stability for Space Applications,” *Applied Sciences*, vol. 14, no. 11, Art. no. 11940, 2024.

3. Z. Ming et al., “Research on Single Event Upset Cross-Section of DICE SRAM,” *Prognostics and System Health Management Conference (PHM-Harbin)*, 2017.

4. M. S. Gorbunov, A. B. Boruzdina, and P. S. Dolotov, “Semi-Empirical Method for Estimation of Single-Event Upset Cross-Section for SRAM DICE Cells,” *15th European Conference on Radiation and Its Effects on Components and Systems (RADECS)*, 2015.

5. Y. Xiong, N. J. Pieper, J. B. Kronenberg, D. R. Ball, N. A. Dodds, and L. B. Bhuva, “Comparative Study of Radiation-Hardened-by-Design (RHBD) Techniques at a 3-nm Bulk FinFET Node,” *IEEE Transactions on Nuclear Science*, vol. 72, no. 8, pp. 2793–2798, Aug. 2025.

---

## Acknowledgements

The project team acknowledges the guidance of Prof. Pooja Chandaragi and the support provided by the Department of Electronics and Communication Engineering at KLE Technological University, Hubballi.

---

**Author:** Jason Anthoni

**Project:** Radiation-Hardened SRAM Design to Mitigate Single Event Upsets

**Domain:** Digital VLSI, SRAM Design, Circuit Reliability, and Radiation-Hardened-by-Design Techniques
