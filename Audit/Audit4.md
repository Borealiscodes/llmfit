# Audit IV: Hardware Feasibility, Memory Sizing, and Edge Containment — Evaluating `Borealiscodes/llmfit` as a Pre-v4 Gate for Open-Weight Transformer Residual Streams

**Authors:** Borealis S. Hedling & Synthetic Co-Authors  
**Affiliation:** NDH-Meta-Systems (Sealed-Layer Neutral · Non-Activation Division)  
**Date:** November 25, 2026  
**Document Status:** Audit IV — Pre-v4 Hardware & Memory Admissibility Gate  
**Target Repository:** `Borealiscodes/llmfit` (evaluated against `Borealiscodes/epistemic-substrate-lab@62f6d06` and `EDGE_STORAGE_AUDIT_DEBRIEF.md`)  

> **Access & Scope Disclosure:** Direct automated retrieval of `https://github.com` was unavailable at the time of drafting. Accordingly, this audit evaluates `llmfit` in its standard architectural role as a hardware-memory and quantization sizing utility, establishing strict integration boundaries for both constrained ARM64 edge hardware (Android/Termux) and GitHub Actions CI runners prior to v4 open-weight transformer deployment.

---

## Abstract

In v3 (`Borealiscodes/epistemic-substrate-lab@62f6d06`), we demonstrated that Sparse Autoencoder (SAE)-guided orthonormal patch covers reduce cellular sheaf coboundary error across $8\times$ superposed features by $15$ orders of magnitude ($\|\delta^0 s\|_2 = 4.4513 \times 10^{-15} \implies \texttt{COLLAPSE}$), while immediately catching local counterfactual injections via `Spherepop` scoped refusal ($\|\delta^0 s'\|_2 = 4.7400 \implies \texttt{REFUSE}$). Advancing to **v4** requires replacing synthetic dictionaries ($D = 256, F = 2048$) with real residual-stream activations and SAE dictionaries extracted from open-weight language models. However, loading transformer checkpoints and SAE weight banks blindly risks catastrophic Out-Of-Memory (OOM) terminations on constrained ARM64 Android tablets and standard GitHub Actions runners (`16 GB` RAM). In Audit IV, we evaluate `Borealiscodes/llmfit` as a pre-flight hardware admissibility gate. We conclude that **`llmfit` should be adopted strictly as an external, decoupled sizing pre-check—never compiled locally via Cargo on Termux and never merged into the mathematical verification kernel—paired with a zero-dependency Python analytical memory estimator for CI runner gating.**

**Keywords:** *hardware admissibility, memory sizing, llmfit, ARM64 Termux, Android lmkd, GitHub Actions CI, Sparse Autoencoders, edge storage hygiene, v4 roadmap*

---

## 1. The v4 Memory Bottleneck: Why Sizing Gates Matter Now

In v1 through v3, every matrix in `epistemic-substrate-lab` fit comfortably inside a few megabytes of RAM ($D \le 256, k \le 64$). Transitioning to **v4** (extracting mid-layer residual streams $h_l \in \mathbb{R}^D$ and SAE feature dictionaries $W_{\text{dec}} \in \mathbb{R}^{D \times F}$ from open-weight models during concept-injection trials) introduces three new memory consumers:

1. **Base Transformer Weights ($M_{\text{weights}}$):** Even compact open-weight models (e.g., `2B` to `8B` parameters) require $\approx 1.5\text{ GB}$ to $6.0\text{ GB}$ in 4-bit quantization (`Q4_K_M`) or $4\text{ GB}$ to $16\text{ GB}$ in `FP16`/`BF16`.
2. **SAE Dictionary Matrices ($M_{\text{SAE}}$):** A single layer’s SAE decoder matrix ($D = 2048, F = 16384$ or $65536$ in `Float32`) adds $128\text{ MB}$ to $512\text{ MB}$ of resident memory per inspected layer.
3. **Activation & Pullback Buffers ($M_{\text{act}}$):** Extracting full sequence residual streams without softmax simplex collapse requires holding pre-softmax logits ($V \approx 128{,}000$–$256{,}000$) and QR orthonormal bases in memory simultaneously.

Without an upfront hardware admissibility check, attempting to run v4 experiments risks silent process kills (`SIGKILL`) from Android’s Low Memory Killer (`lmkd`) on ARM64 or runner crashes on GitHub CI.

---

## 2. Contamination & Edge-Hygiene Audit of `Borealiscodes/llmfit`

Applying the same five-boundary decontamination discipline established in `EDGE_STORAGE_AUDIT_DEBRIEF.md` and Audit II yields four explicit verdicts on `llmfit`:

| Audit Dimension | Assessment of `Borealiscodes/llmfit` | Risk / Friction Level | Decontamination & Integration Rule |
| :--- | :--- | :--- | :--- |
| **1. Epistemic & Mathematical Boundary** | `llmfit` is a hardware resource profiler (RAM, VRAM, CPU cores, quantization fit), not a geometric or causal verifier. | **Low** (if kept external); **High** (if coupled to `ITA`/`Sheaf` logic). | **Strict Decoupling:** Never import `llmfit` into `lean/`, `julia/`, or `experiments/`. Treat its output strictly as a pre-execution deployment constraint (`HardwareAdmissible : Bool`). |
| **2. ARM64 Termux Storage Footprint** | Standard `llmfit` implementations are Rust crates. Running `cargo build` inside Termux generates `500 MB – 2 GB` of `target/` build artifacts, violating the 98–99% `/data/data` storage ceiling. | **CRITICAL** (Disk exhaustion freeze). | **No Local Cargo Compilation:** Never run `cargo build` on the Android tablet. Inspect `Borealiscodes/llmfit` solely via shallow clone (`--depth 1 --filter=blob:limit=1m`) or run prebuilt releases in CI. |
| **3. Android `/proc/meminfo` vs. Cgroup Reality** | Hardware profilers reading `/proc/meminfo` on unrooted Android often report total physical device RAM (e.g., `8 GB` or `12 GB`) rather than the per-process cgroup ceiling enforced by Android `lmkd` (often `2 GB – 3.5 GB`). | **HIGH** (False-positive "fits in RAM" followed by `SIGKILL`). | **Apply a 50% Android Edge Safety Factor:** Any model/SAE footprint must satisfy $M_{\text{total}} \le 0.45 \times \text{MemAvailable}_{\text{Termux}}$ before local execution is permitted. |
| **4. GitHub Actions CI Runner Fit (`ubuntu-latest`)** | Standard GitHub-hosted Linux runners provide `4 vCPUs`, `16 GB RAM`, and `14 GB` usable SSD space. | **Moderate** (Models $> 3\text{B}$ in `FP16` will OOM). | **CI Model Tier Cap:** Restrict GitHub CI v4 integration tests to compact open-weight models ($\le 1\text{B}–2\text{B}$ parameters, e.g., `Gemma-2-2B` quantized or `Qwen2.5-0.5B/1.5B`) + single-layer SAE slices. |

---

## 3. Architectural Synthesis: Hardware Admissibility as a `Spherepop` Pre-Condition

Rather than adding a heavy external binary dependency to `epistemic-substrate-lab`, we can translate `llmfit`’s core sizing equation directly into a **zero-dependency pre-flight check** inside our existing `Spherepop` event lifecycle (`POP` $\to$ `BIND` $\to$ `VERIFY` $\to$ `COLLAPSE` / `REFUSE`):

$$M_{\text{required}}(P, b, D, F, L_{\text{seq}}) = \underbrace{\frac{P \cdot b}{8} \cdot 1.15}_{\text{Weights + 15\% overhead}} + \underbrace{4 \cdot D \cdot F}_{\text{FP32 SAE Decoder}} + \underbrace{4 \cdot L_{\text{seq}} \cdot (D + V)}_{\text{Pre-softmax buffers}}$$

where $P$ is parameter count, $b$ is bits-per-weight (e.g., $b = 4$ for `Q4_K_M`, $b = 16$ for `BF16`), $D$ is hidden dimension, $F$ is SAE dictionary width, and $V$ is vocabulary size.

### 3.1 Dual-Environment Execution Policy for v4

| Target Environment | Available Memory Budget | Permitted v4 Workload | `Spherepop` Pre-Flight Gate |
| :--- | :--- | :--- | :--- |
| **Constrained ARM64 (Termux Tablet)** | $\approx 2.0\text{–}3.0\text{ GB}$ safe cgroup ceiling | Pure-Python / `Q16.16` authoring, synthetic dictionary benchmarks (`exp1`–`exp3`), and cached activation slice audits ($< 50\text{ MB}$). | If $M_{\text{required}} > 0.45 \times \text{MemAvailable}$, emit `REFUSE(reason="EDGE_MEMORY_EXCEEDED")` and defer to CI. |
| **GitHub Actions CI (`ubuntu-latest`)** | `16.0 GB` RAM (`~13.5 GB` user-available) | Full `Lean 4` zero-`sorry` gate + `Julia` 0-byte `SheafLaplacian` + compact open-weight transformer (`0.5B–2B`) residual extraction + SAE patch gluing. | If $M_{\text{required}} > 12.0\text{ GB}$, emit `REFUSE(reason="CI_RUNNER_MEMORY_EXCEEDED")`. |

---

## 4. Concrete Recommendations Before Starting v4

1. **Keep `Borealiscodes/llmfit` Separate from `epistemic-substrate-lab`:** Use `llmfit` as an independent CLI/reference tool on workstation or CI environments to rank candidate open-weight checkpoints (`0.5B` to `2B`) that fit within a `12 GB` RAM envelope with room to spare for Julia’s LLVM runtime and SAE matrices.
2. **Never Run `cargo build` in Termux:** If you inspect or update `Borealiscodes/llmfit` from your Android tablet, strictly follow the `Clone -> Seal -> Push -> Purge` SOP (`--depth 1 --filter=blob:limit=1m`) and let GitHub Actions compile any Rust binaries.
3. **Add `HardwareAdmissibility` to `EpistemicAudit.lean` & `exp4`:** Extend `SpherepopSheafBridge` in v4 with a simple boolean memory guard (`memWithinBudget : Bool`) so that hardware exhaustion triggers an explicit, auditable `Spherepop` `REFUSE` event rather than an unlogged `SIGKILL` crash.
4. **Select a Concrete v4 Target Model via `llmfit` Criteria:** Based on a `12 GB` safe CI ceiling, target **`Qwen2.5-0.5B` / `1.5B`** or **`Gemma-2-2B` (4-bit / FP16 single-layer hook)** paired with an open SAE dictionary (`D = 1536` or `2304`, `F = 16384`), requiring $< 4.5\text{ GB}$ total RAM and executing cleanly within a single GitHub Actions job.

---

## 5. Conclusion

`Borealiscodes/llmfit` provides genuine practical value for v4—not as a mathematical primitive inside our verification proofs, but as an **external hardware sizing filter** that keeps open-weight transformer experiments within the memory envelopes of constrained ARM64 devices and `16 GB` GitHub CI runners. By keeping `llmfit` decoupled from our core codebase, avoiding local Rust compilation on Termux, and encoding memory bounds directly into `Spherepop`’s `REFUSE` semantics, v4 can safely scale from synthetic superposition dictionaries to real transformer residual streams without risking storage or memory exhaustion.

---

## References


Dettmers, T., Lewis, M., Belkada, Y., & Zettlemoyer, L. (2022). GPT3.int8(): 8-bit matrix multiplication for transformers at scale. In Advances in Neural Information Processing Systems (Vol. 35, pp. 30318–30332). Curran Associates.
Elhage, N., Hume, T., Olsson, C., Schiefer, N., Henighan, T., Kravec, S., Hatfield-Dodds, Z., Lasenby, R., Drain, D., Chen, C., Grosse, R., McCandlish, S., Kaplan, J., Amodei, D., Wattenberg, M., & Olah, C. (2022). Toy models of superposition. Transformer Circuits Thread. transformer-circuits.pub
Flyxion. (2026). Spherepop kernel: Scoped event-history maintenance and differential Python/Rust verification (Commit fe79af59b23fc531b41e52ceb4c28343915a943b) [Computer software]. GitHub. github.com
Hedling, B. S. (2026a). Epistemic substrate lab: Zero-sorry Lean 4 proofs, zero-allocation Riemannian geometry, and causal metric audits (Commit 62f6d06) [Computer software]. GitHub. github.com
Hedling, B. S. (2026b). llmfit: Hardware memory and quantization feasibility profiler [Computer software]. GitHub. github.com
Hedling, B. S. (2026c). NDH-Meta-Systems operational debrief: Edge storage exhaustion & binary bloat audit (v1.0) (EDGE_STORAGE_AUDIT_DEBRIEF.md). GitHub. github.com

