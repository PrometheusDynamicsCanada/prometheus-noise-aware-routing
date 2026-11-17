# Experiment 04: 13-Qubit Placement Depth vs. Measured All-Zero Output and Entropy

## Summary
This directory archives the hardware result payload (`job-d971l1gtcv6s73dkc89g`) for a 13-qubit symmetrical cross-entanglement circuit executed on the 156-qubit `ibm_kingston` Heron processor (`80,000` shots per circuit in the archived ledger payload; `80,064` shots in the companion series).

Both compilations were batched into a single `SamplerV2` submission so they executed inside the same hardware window:
1. **SABRE O3 (`PUB 0`):** Compiled to a physical depth of **42** (`290` total gates), returning **`555 / 80,000` (`0.69%`)** shots in the all-zero bitstring (`0...0`) and an output Shannon entropy of **`11.0683` bits** (`870 / 80,064` [`1.09%`] and `9.1408` bits in the companion series).
2. **Prometheus Placement (`PUB 1`):** Compiled to an `8.2x` deeper physical depth of **346** (`1,309` total gates) on an alternate physical qubit subgraph, returning **`206 / 80,000` (`0.26%`, `-62.9%`)** shots in the all-zero bitstring (`0...0`) and a higher output Shannon entropy of **`12.2850` bits** (`672 / 80,064` [`0.84%`] and `9.5570` bits in the companion series).

| Series / Pipeline | Physical Depth `(↓)` | Physical Gates `(↓)` | Measured All-Zero (`0...0`) Shots `(↓)` | Output Shannon Entropy `(bits)` |
| :--- | :---: | :---: | :---: | :---: |
| **Archived Ledger (`job-d971l1gtcv6s73dkc89g`, PUB 0: SABRE O3)** | **42** | **290** | 555 / 80,000 (0.69%) | 11.0683 bits |
| **Archived Ledger (`job-d971l1gtcv6s73dkc89g`, PUB 1: Prometheus)** | 346 | 1,309 | **206 / 80,000 (0.26%)** | **12.2850 bits** |
| **Companion Series (80,064 shots, PUB 0: SABRE O3)** | **42** | **290** | 870 / 80,064 (1.09%) | 9.1408 bits |
| **Companion Series (80,064 shots, PUB 1: Prometheus)** | 346 | 1,309 | **672 / 80,064 (0.84%)** | **9.5570 bits** |

## Direct Observation vs. Physical Interpretation
- **What the computational-basis measurements directly establish:** Physical circuit depth alone did not predict the measured output distribution in this 13-qubit test; the deeper alternate placement produced `62.9%` fewer all-zero bitstrings and `1.22` bits higher Shannon entropy in the archived `80,000`-shot run.
- **What requires further evidence (interpretation boundary):** Computational-basis bitstring histograms alone do not establish *why* all-zero counts decreased (for example, distinguishing qubit-specific $T_1$ relaxation differences from readout-assignment bias or coherent gate errors), nor do they certify multipartite entanglement survival without state/process tomography or targeted calibration controls. Similarly, higher entropy is a diagnostic of output spread on distributed target states rather than a general measure of computational accuracy.

## Local Verification (`Raw BitArray Tensor Unpacking`)

```bash
cd 04-topological-mapping-ground-state-avoidance
python verify_benchmark.py
```

- `Base_QASM/topological_stress_vector_13q.qasm`: Unrouted 13-qubit OpenQASM 2.0 logical circuit.
- `IBM_Execution_Ledger/job-d971l1gtcv6s73dkc89g-result.json`: Unmodified returned IBM Quantum `SamplerV2` result payload.
