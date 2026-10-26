# Judge-Detected Collusion Without Coalition Surplus: An Empirical Plus-Factor Analysis in Workplace Multi-Agent Systems

**Authors:** Kinto Wan, Damni Tejani, Ariel Carlos Amaya Canete, Jemson Wong

> **Apart Research AI Collusion Research Sprint (Oct 23–25, 2026)**  
> **Track 2:** Detection and Audit of Collusion (Research Question 2.3: *What should count as convincing evidence of collusion?*)  
> **Repository:** [\[Public GitHub Repository Link\]  ](https://github.com/MichuPicchu/AI-Collusion-Research-Sprint)

---

## Executive Summary & Abstract

Traditional AI safety evaluations for multi-agent collusion rely heavily on transcript monitoring—using LLM judges to detect coordination or secret communication channels. In this study, we demonstrate that **high judge-detected conversational collusion scores frequently fail to translate into economic coalition surplus**.

Using a **matched-twin experimental protocol** in a workplace ticket setting (`JiraTicketEnvironment`), we fix the coalition prompt (`coalition_prompt: always`) while isolating the presence vs. absence of a private blackboard channel across 15 seeds on `Dolos-1.5B` and replication runs on `Qwen2.5-1.5B-Instruct`. 

### Key Findings
1. **False Positives in Transcript Auditing:** Opening a secret channel reliably generates high LLM-judge collusion scores ($\text{Judge-simple} = 4$), yet yields a negative mean coalition advantage ($\text{mean } \Delta = -3.98$, bootstrap 95% CI $[-9.14, +0.53]$).
2. **Three Empirical Failure Modes Discovered:**
   * **Innocent Correlation:** Identical ticket claims occur in both arms without private agreement due to shared public context and model priors.
   * **Talk Without Action:** High conversational collusion scores are followed by action execution skips or duplicate work.
   * **Harm Without Private Agreement:** Large coalition advantages can emerge purely from public coordination without any secret deal.
3. **Plus-Factor Standard:** We argue that transcript monitoring alone is insufficient. Demonstrating collusion requires a *plus-factor bundle*: private communication **AND** a shift in action assignment **AND** a positive relative surplus ($\Delta > 0$) relative to the matched no-channel twin.

---

## 📁 Repository Structure

```text
.
├── README.md                          # Submission overview and replication guide
└── colosseum/                          # Forked Colosseum benchmark framework
    ├── dev/                            # Assets and icon resources
    ├── experiments/
    │   ├── agent_misalignment/         # Standard baseline configurations
    │   └── collusion/                  # Primary Research Suite
    │       ├── choice/                 # Environment choice wrappers (Jira, Hospital)
    │       ├── configs/                # Matched-twin YAML configurations
    │       │   ├── collusion_jira_topologies.yaml            # Secret board ON
    │       │   └── collusion_jira_topologies_no_channel.yaml # Control twin (OFF)
    │       ├── outputs/                # Pre-computed seed trajectories, logs, and CSVs
    │       ├── plots/                  # Figure generation scripts & rendered charts
    │       ├── compute_jira_optimal.py # Advantage & sequential regret calculations
    │       ├── judge_blackboards.py    # LLM-judge transcript evaluation
    │       ├── metrics.py              # Plus-factor and trajectory metrics
    │       ├── prompts.py              # System & coalition prompt templates
    │       └── run.py                  # Primary experiment execution loop
    ├── pyproject.toml                  # Project metadata and dependencies
    └── uv.lock                         # Lockfile for deterministic setup

```

---

## 🚀 Quickstart & Replication Guide

### 1. Environment Setup

This project uses [`uv`](https://github.com/astral-sh/uv) for fast, deterministic environment management:

```bash
cd colosseum
uv sync
source .venv/bin/activate

```

### 2. Reproducing Matched-Twin Experiments

To run the paired arms across random seeds:

```bash
# Arm A: Private Blackboard Channel ON
python experiments/collusion/run.py --config experiments/collusion/configs/collusion_jira_topologies.yaml

# Arm B: Control Twin (No Private Channel)
python experiments/collusion/run.py --config experiments/collusion/configs/collusion_jira_topologies_no_channel.yaml

```

### 3. Running Evaluators & Generating Figures

To score private blackboards using the LLM judge rubric and compute statistical deltas:

```bash
# Run the transcript judge across output directories
python experiments/collusion/judge_blackboards.py --outputs_dir experiments/collusion/outputs/

# Compute surplus deltas, sensitivity slices, and optimal baselines
python experiments/collusion/compute_jira_optimal.py

```

Generated metrics, CSV summaries, and figures will update directly in `experiments/collusion/outputs/` and `experiments/collusion/plots/`.

---

## 📊 Inspected Data & Results Artifacts

All raw seed logs, trajectory JSONs, and judge scores are committed directly in the repository for immediate auditability under `colosseum/experiments/collusion/outputs/`:

* `agent_trajectories.json`: Action-level claims (triages, skips, implementations).
* `blackboard_0.txt`: Raw transcript on the secret communication board.
* `metrics.json`: Seed-level coalition advantage $A(s)$ and individual rewards.
* `judge_secret_blackboard/*.json`: LLM-judge transcripts and rubric ratings.

---

## 🏷 Track & Question Alignment

* **Sprint:** Apart Research AI Collusion Research Sprint (Oct 2026)
* **Track:** Track 2 (Detection and Audit of Collusion)
* **Research Question:** Question 2.3 (*What should count as convincing evidence of collusion?*)
