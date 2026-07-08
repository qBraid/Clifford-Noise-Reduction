# Clifford Noise Reduction (CliNR) Tutorial Series

Clifford Noise Reduction (CliNR) is a method for running a Clifford circuit more accurately on a noisy quantum computer, without the overhead of full quantum error correction. The operation is prepared on a separate set of qubits and verified with a small number of stabilizer measurements; only a preparation that passes verification is teleported onto the data. Because faults are detected before they reach the data, they cannot propagate into the computation. CliNR occupies the middle ground between error correction, which requires large qubit overheads, and error mitigation, which incurs an exponential sampling cost.

This series introduces CliNR from first principles and carries it through to a run on real trapped-ion hardware via qBraid. The benefit of CliNR grows with the size of the circuit it protects: it does not pay off on a small circuit, reaches breakeven at ten qubits, and — as shown in the source papers — gives a clear reduction in logical error rate at twenty qubits and beyond. The four notebooks follow this progression, and are meant to be worked through in order.

The series is based on Delfosse and Tham (arXiv:2407.06583), which introduces the method, and Tham and Delfosse (arXiv:2504.13356), which optimizes it and demonstrates it on hardware. Familiarity with quantum circuits and the stabilizer formalism is assumed. The notebooks implement and verify these protocols in code; read them alongside the papers for the full derivations.

## Launch on qBraid

Use the **Launch on qBraid** button on this tutorial's page in the qBraid Explore hub. It clones the repository into qBraid Lab and installs the required environment. The notebooks run with the pre-configured `qBraid-SDK (v0.12.0)` environment. Once it's ready, select it as the notebook kernel and run the notebooks in order, starting with `1_State_and_Gate_Teleportation.ipynb`. For selecting environments and kernels in qBraid Lab, see the [environments guide](https://docs.qbraid.com/v2/lab/user-guide/environments).

## The notebooks

### 1. State and Gate Teleportation — `1_State_and_Gate_Teleportation.ipynb`

Builds the two primitives CliNR rests on: state teleportation, then gate teleportation, which applies a Clifford operation by building it into a resource state and correcting a Pauli byproduct at the end. Each protocol is verified against its definition in simulation. No prior knowledge of CliNR is assumed.

### 2. Clifford Noise Reduction (CliNR) — `2_Clifford_Noise_Reduction.ipynb`

Adds the one missing ingredient to gate teleportation — verification of the resource state — to arrive at CliNR. The protocol is demonstrated on a three-qubit Clifford circuit, where it does not yet outperform a direct implementation. This establishes the baseline — the verification overhead only pays off on larger circuits — before the later notebooks show it succeeding.

### 3. Reaching Breakeven with CZNR — `3_Reaching_Breakeven_with_CZNR.ipynb`

Introduces CZNR, the specialization of CliNR to circuits of controlled-Z gates, which uses a graph state as its resource. The all-pairs CZ circuit on ten qubits is run in simulation and compared against a direct implementation, reaching breakeven. Two families of verification stabilizers and several verification lengths are compared, followed by a sweep over the noise rate.

### 4. CZNR on IonQ Hardware — `4_CZNR_on_IonQ_Hardware.ipynb`

Runs the CZNR circuit on IonQ Forte through qBraid. Because Forte 1 trapped-ion hardware does not support mid-circuit measurement, the corrections and restarts are applied in classical post-processing rather than by in-circuit feedforward. The notebook validates the full pipeline on a free simulator, submits to hardware, and analyzes the results with a progressive stabilizer-ignoring study that shows the verification removing genuine errors.

## Requirements

All packages are pre-installed in the Launch-on-qBraid environment. Each notebook also begins with a commented optional `%pip install` cell listing what it needs for other setups.

- **Stim** — exact, efficient simulation of the Clifford circuits used in Notebooks 1-3.
- **Qiskit** — circuit construction for the hardware submission in Notebook 4.
- **qBraid** — unified quantum device access and job submission, including to **IonQ Forte** hardware. The hardware section of Notebook 4 additionally requires a qBraid account with IonQ access and credits.

## Running the tutorials

- Run each notebook from top to bottom, in order. If the kernel state becomes inconsistent, use **Kernel > Restart Kernel and Run All Cells**.
- In Notebook 4, hardware submission is off by default, so **Run All Cells** is a safe dry run. Set `RUN_ON_HARDWARE = True` only if you have IonQ access and intend to spend credits; leave it `False` otherwise.
- Hardware jobs are queued and may take from minutes to hours. You do not need to keep the notebook open — the job continues on the backend, and its results can be retrieved later from the qBraid **Jobs** panel or by job ID. When re-running a notebook to retrieve and analyze a previous job, make sure to set `RUN_ON_HARDWARE = False` so a new job isn't accidentally submitted.

## Notes and troubleshooting

- Import or kernel errors immediately after launch usually mean the environment is still installing. Wait for it to finish, then select the tutorial's kernel from the kernel menu.
- Hardware availability and queuing. Check the hardware device's status in the Devices panel on the right of qBraid Lab before submitting. The job will be added to the Queue until the device's next available time window. If the device status shows `unavailable`, the job submission may throw an error — try again when the device is available. 



