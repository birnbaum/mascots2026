# MASCOTS 2026 Resubmission: GPU Power Modeling

This repository contains the LaTeX source and data for the paper **"Evaluating MFU as a Portable Power Proxy for Energy-Aware LLM Training Simulation"**.
The paper was originally submitted to Euro-Par 2026 and is being revised for [MASCOTS 2026](https://mascots26.iitis.pl/call-for-papers/).

## Project Overview

This project investigates the relationship between **Model FLOPS Utilization (MFU)**, **GPU Utilization**, and **GPU Power Consumption** in the context of LLM training. 
The goal is to determine if MFU (a hardware-agnostic metric) can serve as a reliable proxy for power consumption in simulators and analytical models where hardware counters (like GPU Utilization) are unavailable.

---

## Euro-Par 2026 Review Summary (Submission #276)

The paper received mixed reviews (1, -1, -1, 1). The following points summarize the feedback from four reviewers:

### Reviewer 1 (Weak Accept)
- **Strengths:** Important, timely topic; well-written; reproducible; thorough evaluation.
- **Weaknesses:** Small target audience; "gaming GPUs" (RTX 4070 Ti) may not be the primary target for these studies; many small deficiencies in methodology/interpretation (e.g., related to measurement variance).
- **Recommendation:** Improve methodology, use "proper" data-center GPUs if possible.

### Reviewer 2 (Weak Reject)
- **Structural Issues:** Poorly written narrative, incoherent sections, mixed terminology.
- **Methodology:**
    - Telemetry vs. external power validation is "relatively irrelevant" to the story and lacks detail on tools/explainability of the offset.
    - **Table 2:** Results are untrustworthy due to low base values and high variance.
    - Missing details on independent variables: "model architecture" and "warmup/cooldown" specifics are lacking.
    - "Statistically significant convergence criterion" is not defined.
    - AMD MI210 utilization reporting is binary (100% or 0%), yet it somehow achieved high $R^2$ in some analysis? (Needs clarification).
- **Terminology:** "Compute-bound regions" and other terms are not explained.
- **Figures:** Figure 3 only studies dtype, but batch size is also mentioned as significant. Missing legends/descriptions for colors.
- **Missing Citations:** `CalFlops`, `Hydra`, `AccelWattch`.

### Reviewer 3 (Weak Reject)
- **Transparency:** The GitHub repository was found to be incomplete (missing benchmarking scripts, only plots and uncategorized CSVs).
- **Methodology:** 
    - Lack of description for the 504 workloads (range/scope).
    - Missing host system/hardware details (CPU, motherboard, cooling).
    - Limitation to NVIDIA; AMD analysis was inconclusive.
- **Clarity:** Equation 1 parameters ($FLOPS_{max}$ and $FLOPS_{req}$) are not explained.

### Reviewer 4 (Weak Accept)
- **Depth:** Lacks deep exploration of *when* MFU and GPU Utilization behave differently.
- **Bottlenecks:** Expected more discussion on memory-bound scenarios (memory hierarchy bottlenecks) vs. pure FLOPS metrics like MFU.
- **Impact:** Little information on how arithmetic intensity or memory requirements impact predictive power.

---

## Status

The narrative pivot to *"MFU as a portable, software-defined power proxy for LLM training simulation"* is complete. Title, abstract, intro contributions, §3.2 (search space + software), §4.1 (linear MFU-power per GPU + new headline table with bootstrap CIs), §4.2 (regimes of reliability — merged old §4.2 + §4.3), §4.3 (memory-bound failure mode from existing batch-1 data), §5 (limitations trimmed), conclusion (composition argument), and takeaways have all been rewritten.

Current page count: 7. ~1 page of headroom for further additions.

## Open work

### Still to do
- [ ] **Hardware Table:** host system specs (CPU, RAM, OS, driver versions) for all testbeds — addresses Reviewer 3.
- [ ] **Repository Cleanup:** add the Python/Hydra benchmarking pipeline to the public `gpu_power_benchmark` repo.
- [ ] **Figure Polish:** consistent color mapping + legends across all plots.
- [ ] **L4 results:** L4 is listed in the §3.2 design grid but no data yet; add row to Table 2, fill in `fig:Prediction Error Figure` panel, update abstract config count once measured.

### Candidate page-fill additions (we have ~1 page)
- [ ] **Memory-bound plot for §4.3:** power and MFU vs batch size, one line per GPU. Turns the strongest table in the paper into the strongest figure. Existing data, no new measurements. *Highest impact.*
- [ ] **Per-GPU regression-line plot for §4.1:** MFU on x-axis, power on y-axis, five colored regression lines on one panel. Visualizes the "different slopes, same form" claim that currently lives only in prose.
- [ ] **GPU Util MAPE column in Table 2** (`tab:per_gpu_mfu_power`) for parity with MFU MAPE. Cheap, low risk.
- [ ] **Drop the Hardware slice block** from the per-config table in §4.2 (already covered by §4.1's Table 2). Tightens the table.
- [ ] **Worked composition example** (small inset or paragraph): how a simulator user pulls MFU from Vidur, looks up the per-GPU slope, integrates over a trace to get energy. Concrete, addresses "earn the title."
- [ ] **Slope-vs-peak-FLOPS scatter** (small inset): 5 points, directly evidences the "slopes scale with peak FLOPS" claim.

### Items closed in this revision (history)
- [x] Define Convergence (Section 3.1).
- [x] Replace aggregated Table 2 with bootstrap-CI headline table (`tab:per_gpu_mfu_power`, Section 4.1).
- [x] Equation 1 parameters defined (Section 2.1.2).
- [x] AMD MI210 GRBM_COUNT explanation (Section 4.1).
- [x] Terminology pass: "compute-bound" and "memory-bound" defined (Section 2.1.2).
- [x] Missing citations added (`calflops`, `Hydra`, `AccelWattch`).
- [x] RTX 4070 Ti noise: caveat in Section 5; not framed as a result.
- [x] Narrative reframe: §4 restructured around (a) linear MFU-power per GPU, (b) regimes of reliability, (c) memory-bound scope boundary.
- [x] Memory-bound finding from existing data: §4.3 batch-1 vs batch-128 table extracted from the 504-config sweep — no new experiments needed.
- [x] Limitations section: dropped the hardware-metering paragraph; not load-bearing for the new story.
- [x] Title shifted to "...LLM Training Simulation" (honest about scope).
