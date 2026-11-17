# Exploratory Studies of Physical Placement and Routing on IBM Hardware

*Explore the portfolio: [Prometheus Quantum Overview](https://github.com/PrometheusDynamicsCanada/prometheus-quantum-overview)*

**The circuit with the fewest gates is not always the circuit that gives the best measured result. These experiments examine how physical placement and routing affect execution.**

Positioned as **exploratory supporting research** rather than a standalone product benchmark, these five studies examine when routing around noisy couplers improves hardware output and when added SWAP gates outweigh cleaner links:

1. **Most `6-9` qubit randomized circuits improved, while `25%` lost to added gates (`Exp 02`):** Detour routing beat Qiskit SABRE in **`30/40` (`75%`)** randomized instances on `ibm_marrakesh` and lost in **`10/40` (`25%`)** where extra gates hurt more than noisy links.
2. **Gains reversed across chip sectors and at `10` qubits (`Exp 03` & `Exp 05`):** Detour routing won `67%` of `6-7` qubit QAOA runs on an uneven `ibm_marrakesh` sector, trailed SABRE on more uniform chip sectors, and crossed from gains at `6-9` qubits to losses at `10` qubits.
3. **Output concentration and output accuracy can move in opposite directions (`Exp 01` & `Exp 04`):** Deeper placements shifted output entropy independently of ideal-state agreement and reduced measured all-zero (`0...0`) counts by **`62.9%`** at `13` qubits (`206` vs. `555` shots). Computational-basis histograms establish these distribution changes directly; physical mechanisms remain open interpretations.

**[Detailed results](MASTER_RESULTS.csv) · [Verify the evidence](#verify-the-evidence) · [Discuss a pilot](#discuss-a-pilot)**

![Randomized Scaling Trends and Gate-Overhead Trade-Offs](02-randomized-instance-robustness/assets/scaling_trend.png)
*Opening summary (`Experiment 02`, Job `job-da1httsdedkc73er2k70` on `ibm_marrakesh`, `40` matched randomized `6Q-9Q` instances / `120` PUBs): Squared Hellinger Fidelity comparison showing both the `30/40` hardware-aware wins and the `10/40` gate-overhead crossover boundary.*

---

## 1. Summary of the Five Exploratory Studies (`01` - `05`)

*Campaign Scope:* `14` archived IBM Heron jobs, `774` physical PUBs, and `4,125,440` physical shots across 156-qubit `ibm_fez`, `ibm_marrakesh`, and `ibm_kingston`.

> [!NOTE]
> **Metric Conventions:** In [`MASTER_RESULTS.csv`](MASTER_RESULTS.csv) and `02`/`03` `summary.csv`, `hellinger_fidelity` denotes Squared Hellinger Fidelity $F_H(P_{\text{ideal}}, P_{\text{meas}}) = (\sum_x \sqrt{P_{\text{ideal}}(x) P_{\text{meas}}(x)})^2 = F_B^2$ `(↑)` (where $F_B = \sqrt{F_H}$ is Classical Bhattacharyya Overlap, labeled `"State Fidelity"` in Studio UIs). Total Variation Distance is $\text{TVD} = \frac{1}{2}\sum_x |P_{\text{ideal}}(x) - P_{\text{meas}}(x)|$ `(↓)`, and Shannon Entropy is $H = -\sum_x P_{\text{meas}}(x)\log_2 P_{\text{meas}}(x)$ `(bits)`. **Bold** marks the winning value in each comparison.

| Study Directory | Target QPU(s) & IBM Job ID(s) | Sample Size & Shots | Primary Observables | Plain-English Takeaway & Measured Values |
| :--- | :--- | :--- | :--- | :--- |
| **[`01 - Distributional Concentration`](01-distributional-concentration/)** | `ibm_kingston`, `ibm_fez` (`8` jobs in [`IBM_JOB_EVIDENCE_INDEX.csv`](IBM_JOB_EVIDENCE_INDEX.csv)) | `5Q-8Q`, up to `100,000` shots/circuit (`216` rows) | Shannon Entropy $H$ `(bits)`, KL Divergence $D_{\text{KL}}$ `(↓)` | **Concentration is not accuracy:** On `8Q` QFT (`100k` shots), a `7.0x` deeper path lowered output entropy (**`6.6045`** vs. `7.9554` bits) while worsening $D_{\text{KL}}$ (`1.6370` vs. **`0.0453`**), showing output spread and ideal-state agreement are separable. |
| **[`02 - Randomized Instance Robustness`](02-randomized-instance-robustness/)** | `ibm_marrakesh` (`job-da1httsdedkc73er2k70`) | `40` matched `6Q-9Q` QAOA & CrossEnt pairs (`120` PUBs) | Squared Hellinger Fidelity $F_H$ `(↑)`, $\text{TVD}$ `(↓)` | **75% wins + 25% SWAP-penalty boundary:** Higher $F_H$ than SABRE O3 in **`30/40` (`75.0%`)** instances (**`4/5`** on `9Q` CrossEnt, e.g., **`0.7450`** vs. `0.6438`), while `10/40` lost to added routing gates. |
| **[`03 - Micro-Gradient Scaling`](03-micro-gradient-scaling/)** | `ibm_marrakesh` (`job-da1do46g52gs73clh7c0`) | `3Q-10Q` QFT, QAOA, CrossEnt, GHZ (`96` PUBs, `960,000` shots) | Squared Hellinger Fidelity $F_H$ `(↑)`, $\text{TVD}$ `(↓)` | **Width crossover at `10Q`:** Positive $\Delta F_H$ on tested `6Q-9Q` instances (`QFT-9` **`+0.1197`**, `QAOA-6` **`+0.1627`**, `QAOA-9` **`+0.1461`**) reversed to losses at `10Q` (`QAOA-10` `-0.0591`, `QFT-10` `-0.0306`). |
| **[`04 - All-Zero Output & Entropy Shift`](04-topological-mapping-ground-state-avoidance/)** | `ibm_kingston` (`job-d971l1gtcv6s73dkc89g`) | `13Q` Cross-Entanglement (`80,000` shots/PUB) | All-Zero (`0...0`) Count `(↓)`, Shannon Entropy $H$ `(bits)` | **Measured distribution shift despite `8.2x` depth:** Deeper placement (`346` vs. `42` depth) reduced measured `0...0` counts by **`-62.9%`** (**`206`** vs. `555` shots) and raised output entropy (**`12.2850`** vs. `11.0683` bits). *(Physical mechanism remains an open interpretation.)* |
| **[`05 - Three-QPU Sector Sweep`](05-3qpu-mechanistic-sweep/)** | `ibm_fez`, `ibm_kingston`, `ibm_marrakesh` (`da2hqsc...`, `da2i2hb...`, `da2ibse...`) | `540` physical circuits (`180` PUBs x `3` QPUs, `1,105,920` shots) | QAOA MaxCut $\langle C \rangle$ `(↑)`, QFT Top-1 $P_{\text{top1}}$ `(↑)` | **Chip-sector dependence:** Detour routing won **`67%`** of conditional `6Q-7Q` QAOA runs on heterogeneous `ibm_marrakesh`, while trailing SABRE O3 on tested `ibm_fez` and `ibm_kingston` sectors and at `8Q`. |

<details>
<summary><strong>Expand Supporting Scatter Figure (Physical Circuit Depth vs. Measured Output Entropy, Experiment 01)</strong></summary>

![Physical Gate Depth vs. Measured Shannon Entropy](01-distributional-concentration/assets/depth_vs_entropy_scatter.png)
*Physical Circuit Depth `(↓)` vs. Measured Output Shannon Entropy $H$ `(bits)` across `100` paired executions (`200` rows: `n = 100` SABRE O3 with `104.5` `*(mean)*` depth and `15.9744` `*(mean)*` bits vs. `n = 100` Prometheus with `1,054.9` `*(mean)*` depth and `14.7082` `*(mean)*` bits) from [`01-distributional-concentration/data/prometheus_telemetry.csv`](01-distributional-concentration/data/prometheus_telemetry.csv).*

</details>

---

## Verify the Evidence

**Verification Kind:** Direct **raw Qiskit `BitArray` tensor payload reconstruction** across all `14` archived unmodified `job-*-result.json` payloads (`4,125,440` physical shots; see [`REPRODUCE.md`](REPRODUCE.md) and [`PUBLIC_DISCLOSURE.md`](PUBLIC_DISCLOSURE.md)):

```bash
python verify_sha256_evidence.py
python 01-distributional-concentration/scripts/verify_distribution.py
python 02-randomized-instance-robustness/verify_benchmark.py
python 03-micro-gradient-scaling/verify_benchmark.py
python 04-topological-mapping-ground-state-avoidance/verify_benchmark.py
python 05-3qpu-mechanistic-sweep/verify_benchmark.py
```

---

## Discuss a Pilot

**Bring a representative workload and an acceptance criterion. We can compare Prometheus with your existing execution approach and measure quality, acceptance rate and charged hardware resources.**

- **Contact:** [william@prometheusdynamics.ca](mailto:william@prometheusdynamics.ca) | [prometheusdynamics.ca](https://prometheusdynamics.ca)
