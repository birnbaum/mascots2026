# MASCOTS 2026 Resubmission: GPU Power Modeling

This repository contains the LaTeX source and data for the paper **"On the Predictive Power of Compute Utilization Metrics for GPU Power Modeling"**.
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

## Prioritized TODO List for MASCOTS '26

### 1. Methodology & Transparency (High Priority)
- [x] **Define Convergence:** SEM < 1% of mean over a 20-sample sliding window (Section 3.1).
- [ ] **Fix/Replace Table 2:** Address the "low $R^2$ / high variance" issue. Consider using MAPE or Coefficient of Variation (CV) for steady-state analysis.
- [ ] **Hardware Table:** Add a table with host system specifications (CPU, RAM, OS version, driver versions) for all 5 testbeds.
- [x] **Equation 1 Cleanup:** $FLOPS_{max}$ and $FLOPS_{req}$ are defined in Section 2.1.2.
- [x] **AMD MI210 Explanation:** ROCm's `GRBM_COUNT` is a binary activity counter — explained in Section 4.1.
- [ ] **Repository Cleanup:** Add the Python/Hydra benchmarking pipeline to the public repository.

### 2. Narrative & Content (Medium Priority)
- [ ] **Strengthen the "Story":** Better link the telemetry validation (Section 4.1) to the utilization experiments. Frame validation as a prerequisite.
- [ ] **Compute vs. Memory Bound:** Discuss the impact of arithmetic intensity. Add a section or paragraph exploring cases where memory bandwidth is the bottleneck (and MFU fails).
- [x] **Terminology Pass:** "compute-bound" and "memory-bound" are defined in Section 2.1.2.
- [ ] **Figure Polish:** Add legends to all plots. Ensure consistent color mapping across figures and describe it in the text.

### 3. Technical Fixes (Low Priority)
- [x] **Add Missing Citations:** `calflops` (Section 2.1.2 and 3.2), `Hydra` (Section 3.1), `AccelWattch` (Related Work).
- [x] **RTX 4070 Ti Noise:** Workstation-level noise characteristics discussed in Section 4.1 and Section 5.

---

## Implementation Strategy
For the MASCOTS resubmission, the focus should shift from a "measurement report" to a "modeling study." We should emphasize the utility of MFU for **simulation frameworks** (like Vidur) where hardware counters are physically impossible to obtain. This turns the "weakness" of MFU into a "strength" (portability).
