# Prometheus Benchmark: Three-QPU Spatial Calibration & Crossover Sweep

## Executive Summary
This experiment evaluates how hardware-aware circuit compilation interacts with spatial calibration heterogeneity across three 156-qubit IBM Heron processors (`ibm_fez`, `ibm_kingston`, and `ibm_marrakesh`) compared against Qiskit SABRE O3 and Quantinuum TKET.

Across **540 physical executions** (`180 PUBs x 3 QPUs x 2,048 shots = 1,105,920 physical shots`), the results demonstrate that global processor summary statistics (such as chip-wide median two-qubit error) do not by themselves determine workload execution quality: detouring onto quieter contiguous regions improved 6-to-7-qubit QAOA outcomes on a spatially heterogeneous calibration profile (`ibm_marrakesh`) while losing to minimal-swap routing on more spatially uniform profiles (`ibm_fez`, `ibm_kingston`) and at wider (`N = 8`) routing-dominated scales.

---

## Experimental Premise & Quality Observables

Conventional heuristic routers primarily minimize physical two-qubit (`2Q`) gate count and depth. On processors with spatially non-uniform error distributions, accepting additional routing swaps (`Δ C_2Q > 0`) to execute within a lower-noise region can either help or hurt depending on whether regional calibration quality outweighs added gate overhead.

Quality is evaluated across `N in {6, 7, 8}` qubits using task-specific observables:
* **QAOA MaxCut:** Expected cut value (`<C>`).
* **Quantum Fourier Transform (`QFT`):** Target distribution top-1 bitstring probability (`P_top1`).

---

## Spatial Heterogeneity vs. Global Median Error

In the recorded calibration window, `ibm_fez` had a lower chip-wide median 2Q error (`0.0028`) than `ibm_marrakesh` (`0.0034`), yet outcomes diverged sharply by spatial error distribution:

* **Spatially Uniform Profiles (`ibm_fez`, `ibm_kingston`):** Relatively homogeneous coupler error rates left insufficient regional headroom to offset extra routing swaps, so lower-overhead SABRE O3 trajectories led.
* **Spatially Heterogeneous Profile (`ibm_marrakesh`):** Contiguous low-error regions enabled calibration-guided detours (`~48%` of paired trials) to outperform SABRE O3 on `67%` of conditional `N in {6, 7}` QAOA comparisons (`38%` on deeper `QFT` circuits).
* **Width Crossover at `N = 8`:** Across all three processors, `N = 8` entered a routing-dominated regime where additional two-qubit gate overhead outweighed regional calibration differences.

---

## Data Provenance & Local Verification

The benchmark comprises 540 physical circuit executions (`1,105,920` shots) across `ibm_fez` (`job-da2hqsc3jnrc73aduof0`), `ibm_kingston` (`job-da2i2hbotlns7398iqrg`), and `ibm_marrakesh` (`job-da2ibseaa69c739hhqc0`).

| Component | Description |
| :--- | :--- |
| `verify_benchmark.py` | Standalone verifier unpacking raw `SamplerV2` `BitArray` payloads against decoded count ledgers. |
| `mechanistic_sweep_*/` | Returned IBM `job-*-result.json` payloads and decoded count ledgers (`*_counts_decoded.json`). |

```bash
cd 05-3qpu-mechanistic-sweep
python verify_benchmark.py
```

---

## Scope & Public Evidence Boundary

* **What this benchmark establishes:** Global median gate error obscures spatial calibration structure; detour routing helps on spatially heterogeneous sectors at moderate widths (`6Q-7Q`) and loses on uniform profiles or wider circuits (`8Q`), motivating multi-compiler candidate selection alongside Qiskit SABRE and Quantinuum TKET.
* **Public evidence boundary:** Returned IBM `job-*-result.json` payloads and decoded count ledgers are preserved in full; routed physical OpenQASM circuits and proprietary hardware-aware optimization internals are withheld per [`../PUBLIC_DISCLOSURE.md`](../PUBLIC_DISCLOSURE.md).
