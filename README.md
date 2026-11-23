# Exploratory Studies of Physical Placement & Routing Trade-Offs

*Portfolio Hub: [Prometheus Quantum Overview](https://github.com/PrometheusDynamicsCanada/prometheus-quantum-overview) · [Master CSV](MASTER_RESULTS.csv) · [Reproduction Guide](REPRODUCE.md)*

## Introduction

- **The question:** When does routing around noisy hardware couplers improve output accuracy, and when do the extra SWAP gates hurt more than the noisy links they avoid?
- **The test:** Across five exploratory mechanistic studies (`14` archived IBM Heron jobs, `774` PUBs, `4,125,440` physical shots on `ibm_fez`, `ibm_marrakesh`, and `ibm_kingston`), we compared detour-routed placements against Qiskit SABRE O3 across `3Q-13Q` circuits.
- **The finding:** On `40` matched `6Q-9Q` randomized circuits (`Exp 02`), detour routing improved Squared Hellinger Fidelity in **`30/40` (`75.0%`)** instances and lost in **`10/40` (`25.0%`)**. Separately (`Exp 01` & `04`), deeper paths lowered Shannon entropy while worsening ideal-state divergence, and cut `13Q` all-zero (`0...0`) counts by **`-62.9%`** (`206` vs. `555` shots).
- **The significance:** Establishes empirically why production compilation must weigh coupler error against added gate count rather than routing around bad links unconditionally.
- **The boundary:** Gains at `6Q-9Q` reversed to losses at `10Q` (`Exp 03`) and on uniform chip sectors (`Exp 05`); physical mechanisms behind `Exp 04` entropy shifts remain open interpretations.
- **The evidence:** Opening `Exp 02` trade-off chart and 5-study table below, with raw Qiskit `BitArray` verification underneath.

![Randomized Scaling Trends and Gate-Overhead Trade-Offs](02-randomized-instance-robustness/assets/scaling_trend.png)
*Opening summary (`Experiment 02`, `40` matched `6Q-9Q` pairs on `ibm_marrakesh`): `30/40` hardware-aware wins alongside `10/40` SWAP-overhead losses.*

---

## Summary of the Five Exploratory Studies (`14` IBM Heron Jobs, `774` PUBs, `4,125,440` Shots)

| Study | Processor(s) & Job ID(s) | Scope & Shots | Primary Metrics | Measured Finding |
| :--- | :--- | :--- | :--- | :--- |
| **[`01 - Concentration`](01-distributional-concentration/)** | `kingston`, `fez` (`8` jobs) | `5Q-8Q`, `100k` shots/circ | Entropy $H$, $D_{\text{KL}}$ `(↓)` | `7.0x` deeper `8Q` QFT lowered $H$ (**`6.60`** vs. `7.96` bits) while raising $D_{\text{KL}}$ (`1.64` vs. **`0.045`**) |
| **[`02 - Randomized 40`](02-randomized-instance-robustness/)** | `marrakesh` (`da1httsd..`) | `40` `6Q-9Q` pairs (`120` PUBs) | $F_H$ `(↑)`, `TVD` `(↓)` | **`30/40` (`75.0%`)** higher $F_H$ vs. SABRE; `10/40` SWAP-overhead losses |
| **[`03 - Width Crossover`](03-micro-gradient-scaling/)** | `marrakesh` (`da1do46g..`) | `3Q-10Q` (`96` PUBs, `960k` shots) | $F_H$ `(↑)`, `TVD` `(↓)` | Gains at `6Q-9Q` (`+0.12` to `+0.16` $F_H$) reversed at `10Q` (`-0.03` to `-0.06`) |
| **[`04 - 13Q All-Zero`](04-topological-mapping-ground-state-avoidance/)** | `kingston` (`d971l1gt..`) | `13Q` (`80k` shots/PUB) | `0...0` Count `(↓)`, $H$ | **`-62.9%`** fewer `0...0` shots (**`206`** vs. `555`) at `346` vs. `42` depth *(mechanism open)* |
| **[`05 - 3-QPU Sweep`](05-3qpu-mechanistic-sweep/)** | `fez`, `kingston`, `marrakesh` | `540` circs (`1,105,920` shots) | MaxCut $\langle C \rangle$, Top-1 | **`67%`** wins on uneven `marrakesh` `6Q-7Q`; trailed SABRE on uniform sectors & `8Q` |

*Metric Key:* `hellinger_fidelity` in [`MASTER_RESULTS.csv`](MASTER_RESULTS.csv) is Squared Hellinger Fidelity $F_H = F_B^2$ `(↑)`.

---

## Inspect the Evidence & Reproduce Supported Checks Offline

- **Command:** `python verify_sha256_evidence.py` (plus per-study `verify_benchmark.py` scripts in [`REPRODUCE.md`](REPRODUCE.md))
- **Dependencies & measured runtime:** Python 3 standard library (`0` cloud credentials, **`~0.04 s`** for `verify_sha256_evidence.py`; `numpy` for per-study tensor scripts).
- **Start a pilot:** [william@prometheusdynamics.ca](mailto:william@prometheusdynamics.ca) | [prometheusdynamics.ca](https://prometheusdynamics.ca)

---

## Supporting Depth-vs-Entropy Scatter Plot

<details>
<summary><strong>Expand Physical Gate Depth vs. Measured Output Entropy Figure (Experiment 01) - Click to expand</strong></summary>

![Physical Gate Depth vs. Measured Shannon Entropy](01-distributional-concentration/assets/depth_vs_entropy_scatter.png)

</details>
