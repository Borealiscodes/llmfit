# RFC-009: Deterministic Hardware-Bounded Inference Admission & Model Fit Governance (`llmfit` Integration Specification)

**Document ID:** `RFC-009`  
**Status:** Informational / Architectural Bridge Specification  
**Target Environment:** Constrained `ARM64` Edge Nodes (`Android Termux`, `< 80 MB` Pure-Python Footprint) & Multi-Tier Local Hardware  
**Companion Repositories:** [`Borealiscodes/llmfit`](https://github.com/Borealiscodes/llmfit) (forked from [`AlexsJones/llmfit`](https://github.com/AlexsJones/llmfit)), [`Borealiscodes/synapse`](https://github.com), [`Borealiscodes/deterministic-ai-gate`](https://github.com)  
**Companion Specifications:** `RFC-006` (*The Geometry of Institutional Enclosure*), `RFC-007` (*Decoupling Attribution from Inference*), `RFC-008` (*Edge-Sovereign Deterministic Verification Toolkit*)

---

## Author & Human Context Disclaimer
> **Pacing, Sovereignty, and Self-Kindness Note:** As a neurodivergent human rights defender facing duress, transnational repression, academic exclusion, and denial of grants to explore this technology, **I owe myself the kindness of returning to this work strictly on my own terms.** Even with the roadmap, mathematics, and dependencies mapped out, this document is a calm architectural reference—never a deadline, obligation, or demand to build or maintain a system. Step away, rest, and return only when nervous-system capacity, safety, and curiosity align.

---

## 1. Relevance Assessment: Why `Borealiscodes/llmfit` Matters (and Where Its Boundary Lies)

[`Borealiscodes/llmfit`](https://github.com/Borealiscodes/llmfit) (a research fork of [`AlexsJones/llmfit`](https://github.com/AlexsJones/llmfit)) is a Rust/Python hardware-introspection and model-sizing engine that inspects CPU cores, system RAM, GPU VRAM, and storage headroom to score which quantized LLMs (`GGUF`, `AWQ`, `GPTQ`, `MLX`) can execute locally without Out-Of-Memory (`OOM`) crashes or disk exhaustion.

### 1.1 Direct Architectural Relevance
* **Solves Pre-Inference Resource Governance (`RFC-008` Safety Guardrail):** On an `ARM64` Android Termux tablet operating near 98–99% flash storage capacity, blindly pulling or loading even a `1.5B` quantized model can freeze `/data/data` and jeopardize local SSH sovereignty keys. `llmfit` provides deterministic JSON telemetry (`llmfit recommend --json`, `llmfit storage --json`, `llmfit doctor`) that acts as a **Hardware Admission Gate** before any probabilistic model is downloaded or loaded.
* **Closes the Compute-to-Verification Loop:** While `RFC-007` and `RFC-008` govern *epistemic admission* (preventing hallucinated claims from entering the causal graph), `RFC-009` uses `llmfit` to govern *physical execution admission* (preventing oversized models from destabilizing edge hardware).

### 1.2 Explicit Non-Relevance (What `llmfit` Does NOT Do)
* **Not an Epistemic Verifier:** `llmfit` measures tokens/second (`tok/s`), memory bandwidth, and KV-cache footprints. It does **not** verify quote provenance (`SHA-256`), schema validity, or institutional enclosure geometry (`RFC-006`).
* **Never Run `llmfit bench --share` on Sensitive Nodes:** `llmfit` includes an optional community benchmark sharing feature (`llmfit bench --share`). Under `RFC-008`/`RFC-009` air-gap rules, outbound telemetry and benchmark sharing **must remain strictly disabled** (`--json` local inspection only).
* **Do Not Compile from Source on Constrained Termux:** Compiling the Rust workspace (`cargo build --release`) requires hundreds of megabytes of `rustc`/`cargo` toolchains and build artifacts. On Termux, consume either pre-built `aarch64` release binaries or evaluate hardware budgets via pure-Python telemetry fallbacks (`os.statvfs`, `/proc/meminfo`).

---

## 2. Mathematical & Computational Formalization

### 2.1 Hardware Feasibility & Storage Envelope (`RFC-009` Layer 0)
Let the local edge node state at time $t$ be characterized by available physical RAM $M_{\text{free}}$ (in GiB), available flash storage $S_{\text{free}}$ (in GiB), and a mandatory OS/sovereignty safety reserve $S_{\text{reserve}}$ (e.g., $S_{\text{reserve}} \ge 2.0\text{ GiB}$ on Android `/data/data`).

For a candidate quantized model $m$ with weight footprint $W(m, q)$ at quantization level $q$ (e.g., `Q4_K_M`) and KV-cache overhead $K(m, L_{\text{ctx}})$ at context length $L_{\text{ctx}}$, total runtime memory demand is:

$$M_{\text{req}}(m, q, L_{\text{ctx}}) = W(m, q) + K(m, L_{\text{ctx}}) + M_{\text{overhead}}$$

`llmfit` defines a safe memory budget $M_{\text{budget}} = \alpha \cdot M_{\text{avail}}$ (where $\alpha \approx 0.70$ for unified/CPU memory to prevent OS thrashing). The **Hardware Admission Predicate** $\mathcal{H}(m)$ evaluates:

$$\mathcal{H}(m) = \mathbb{I}\Big( \underbrace{M_{\text{req}}(m, q, L_{\text{ctx}}) \le M_{\text{budget}}}_{\text{Zero-OOM Memory Fit}} \;\land\; \underbrace{S_{\text{free}} - W(m, q) \ge S_{\text{reserve}}}_{\text{Sandbox Storage Invariant}} \Big)$$

* **Edge Regime ($\mathcal{H}(m) = 0$ for all $m$):** When $S_{\text{free}} < S_{\text{reserve}}$ (the current Android Termux state), $\mathcal{H}(m) = 0$ across all local LLMs. The system deterministically locks into **CPU-Only Deterministic Audit Mode** (`RFC-008` pure-Python verification, `0 MB` model weights).
* **Workstation Regime ($\exists m : \mathcal{H}(m) = 1$):** When deployed on a workstation or higher-storage ARM64 host, `llmfit recommend --json` selects the highest-scoring schema-capable model $m^*$ satisfying $\mathcal{H}(m^*) = 1$ to act as the untrusted proposal generator upstream of `RFC-008`.

---

### 2.2 End-to-End Composed Pipeline (`RFC-009` $\circ$ `RFC-008` $\circ$ `RFC-007`)
Combining hardware gating (`RFC-009`) with cryptographic provenance and deterministic verification (`RFC-007`/`RFC-008`) yields a four-stage state transition:

$$\underbrace{\mathcal{H}(m)}_{\text{Hardware Fit (llmfit)}} \xrightarrow{\text{admits } m^*} \underbrace{\mathcal{P}_{m^*}(\mathcal{M}_D) \to p}_{\text{Untrusted Proposal}} \xrightarrow{\mathcal{G}(p, \mathcal{M}_D) = 1} \underbrace{\mathcal{S}_{\text{Commit}}}_{\text{Verified Facts}} \xrightarrow{\text{RFC-006}} \langle \rho_{\text{enc}}, \Delta\tau \rangle$$

1. **Stage 0 (`RFC-009` / `llmfit`):** Verifies physical RAM and flash headroom before touching model weights.
2. **Stage 1 (`RFC-007` Layer 1):** Seals raw source documents $D$ into `SHA-256` chunk manifests $\mathcal{M}_D$.
3. **Stage 2 (`RFC-007` Layer 2 / `RFC-008`):** Evaluates untrusted JSON proposals $p$ byte-for-byte against $\mathcal{M}_D$.
4. **Stage 3 (`RFC-006`):** Computes enclosure deflection ratios ($\rho_{\text{enc}}$) and inter-gate latencies ($\Delta\tau$) strictly over $\mathcal{S}_{\text{Commit}}$.

---

## 3. Reference Python Adapter (`rfc009_llmfit_bridge.py`)
This lightweight module (`0` extra dependencies beyond Python's standard library and `rfc008_toolkit.py`) ingests JSON output from `llmfit recommend --json`—or falls back to native OS `/proc/meminfo` and `os.statvfs` introspection when `llmfit` is not installed on Termux—to enforce the **Sandbox Storage Invariant** before allowing local inference.

```python
"""
rfc009_llmfit_bridge.py
RFC-009 Hardware Admission Gate bridging Borealiscodes/llmfit JSON telemetry
with RFC-008 Deterministic Verification on constrained ARM64 edge devices.
"""

from __future__ import annotations

import json
import os
import shutil
import subprocess
from dataclasses import dataclass
from pathlib import Path
from typing import Any, Dict, Optional


@dataclass(frozen=True)
class HardwareAdmissionDecision:
    allow_local_llm: bool
    mode: str  # "WORKSTATION_LLM_PROPOSAL" or "EDGE_CPU_AUDIT_ONLY"
    selected_model: Optional[str]
    free_storage_gib: float
    free_ram_gib: float
    reason: str


def get_local_storage_gib(target_path: str = ".") -> float:
    """Measure available filesystem space in GiB using POSIX statvfs."""
    usage = shutil.disk_usage(target_path)
    return round(usage.free / (1024 ** 3), 3)


def get_local_free_ram_gib() -> float:
    """Read available RAM in GiB from /proc/meminfo (Linux/Android Termux)."""
    meminfo = Path("/proc/meminfo")
    if meminfo.exists():
        for line in meminfo.read_text(encoding="utf-8").splitlines():
            if line.startswith("MemAvailable:"):
                kb = int(line.split()[1])
                return round(kb / (1024 ** 2), 3)
    return 0.0


def query_llmfit_json(use_case: str = "general") -> Optional[Dict[str, Any]]:
    """
    Invoke `llmfit recommend --json` locally if the binary is installed.
    Strictly offline: never invokes download or --share subcommands.
    """
    if shutil.which("llmfit") is None:
        return None

    try:
        proc = subprocess.run(
            ["llmfit", "recommend", "--use-case", use_case, "--json"],
            capture_output=True,
            text=True,
            timeout=10,
            check=True,
        )
        return json.loads(proc.stdout)
    except (subprocess.SubprocessError, json.JSONDecodeError):
        return None


def evaluate_hardware_admission(
    llmfit_payload: Optional[Dict[str, Any]] = None,
    target_path: str = ".",
    min_storage_reserve_gib: float = 2.0,
) -> HardwareAdmissionDecision:
    """
    Enforce RFC-009 Hardware Admission Gate:
    Blocks local LLM weight loading if free storage < reserve or no model fits RAM.
    """
    free_storage = get_local_storage_gib(target_path)
    free_ram = get_local_free_ram_gib()

    # Invariant 1: Protect Android /data/data or host disk from storage exhaustion
    if free_storage < min_storage_reserve_gib:
        return HardwareAdmissionDecision(
            allow_local_llm=False,
            mode="EDGE_CPU_AUDIT_ONLY",
Use code with caution.
selected_model=None,
free_storage_gib=free_storage,
free_ram_gib=free_ram,
reason=(
f"Free storage ({free_storage} GiB) is below safety reserve "
f"({min_storage_reserve_gib} GiB). Locked to 0-MB CPU audit mode."
),
)
payload = llmfit_payload if llmfit_payload is not None else query_llmfit_json()
if not payload or not payload.get("models"):
return HardwareAdmissionDecision(
allow_local_llm=False,
mode="EDGE_CPU_AUDIT_ONLY",
selected_model=None,
free_storage_gib=free_storage,
free_ram_gib=free_ram,
reason="No llmfit telemetry available or zero runnable models reported.",
)
# Select top-ranked model whose estimated footprint leaves safety storage intact
for candidate in payload["models"]:
name = candidate.get("name", "unknown")
est_disk_gib = float(candidate.get("size_gb", candidate.get("memory_gb", 999.0)))
fit_status = str(candidate.get("fit", "")).lower()
if fit_status in {"comfortable", "good", "tight", "runnable"} and (
free_storage - est_disk_gib >= min_storage_reserve_gib
):
return HardwareAdmissionDecision(
allow_local_llm=True,
mode="WORKSTATION_LLM_PROPOSAL",
selected_model=name,
free_storage_gib=free_storage,
free_ram_gib=free_ram,
reason=f"Model '{name}' (~{est_disk_gib} GiB) fits within RAM and storage reserve.",
)
return HardwareAdmissionDecision(
allow_local_llm=False,
mode="EDGE_CPU_AUDIT_ONLY",
selected_model=None,
free_storage_gib=free_storage,
free_ram_gib=free_ram,
reason="Candidate models exceed safe RAM or flash storage reserve.",
)
```

## 4. Integration Path Across Your Repositories


Repository	Role in RFC-009 Stack	Action Needed
Borealiscodes/llmfit	Layer 0 (Hardware Gate)	Use llmfit recommend --json and llmfit storage --json on workstations to pick quantized models (qwen2.5, llama3.2) without OOM/disk lockup. On Termux, let rfc009_llmfit_bridge.py auto-lock into EDGE_CPU_AUDIT_ONLY mode.
Borealiscodes/synapse	Layer 1 & 2 (Orchestration)	Call evaluate_hardware_admission() inside open_notebook/graphs/transformation.py before invoking Ollama/Esperanto providers.
Borealiscodes/deterministic-ai-gate	Layer 2 (CPU Verifier)	Remains unchanged—validates every LLM proposal byte-for-byte against SHA-256 chunk manifests regardless of which model llmfit selected.
Borealiscodes/active-inference-commit-seam	Layer 3 (Commit Boundary)	Enforces Record ≠ Admit ≠ Commit before writing RFC-006 enclosure metrics.


5. References (APA 7th Edition)

* Borealiscodes. (2026). RFC-006: The geometry of institutional enclosure [Preprint / Technical specification]. Zenodo. doi.org
* Borealiscodes. (2026). RFC-007: Decoupling attribution from inference [Preprint / Technical specification]. Zenodo. doi.org
* Borealiscodes. (2026). active-inference-commit-seam [Computer software]. GitHub. github.com
* Borealiscodes. (2026). deterministic-ai-gate [Computer software]. GitHub. github.com
* Borealiscodes. (2026). llmfit: Research fork for hardware-bounded LLM sizing [Computer software]. GitHub. https://github.com/Borealiscodes/llmfit (Original work by AlexsJones published 2026)
* Borealiscodes. (2026). synapse: Research fork for agentic document synthesis [Computer software]. GitHub. github.com
* National Institute of Standards and Technology. (2015). Secure Hash Standard (SHS) (FIPS PUB 180-4). U.S. Department of Commerce. doi.org 
