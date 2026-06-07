# Mining Strategic Friction in Multi-Turn LLM Collaboration

This repository contains the notebooks, and analysis pipeline for the paper:

**Strategic Friction in Multi-Turn LLM Collaboration: A Structural Analysis of Interaction Trajectories**

---

## Overview

Large Language Model (LLM) interactions are commonly evaluated using output quality alone. However, output-only evaluation can conflate a policy's ability to converge within a fixed turn budget with its ability to produce high-quality work.

This project introduces a trajectory-mining framework for analyzing multi-turn collaboration. Instead of treating only the final answer as the outcome of interest, we model each interaction as a symbolic trajectory and analyze its structure using:

- Sequential Pattern Mining (PrefixSpan)
- Transition-Graph Analysis
- Entropy-Based Structural Metrics
- Density-Based Clustering (HDBSCAN)

The project compares two interaction policies:

### Seamless-Control

- Direct answers
- Minimal interaction overhead
- Fast convergence

### Strategic Friction Framework (SFF)

- Socratic prompting
- Guided revision
- Deliberate reflection
- Reduced direct answer delivery

---

## Repository Structure

```text
.
├── 01_setup_verification.ipynb
├── 02_task_pool_generation.ipynb
├── 03_orchestrator_pilot.ipynb
├── 04_analysis.ipynb
├── requirements.txt
├── Figures/
└── README.md
```

---

## Notebooks

### 01_setup_verification.ipynb

Verifies:

- Environment setup
- Package installation
- Dataset availability
- Directory structure
- API access

### 02_task_pool_generation.ipynb

Generates benchmark task pools from:

#### Python

- MBPP
- HumanEval
- APPS

#### Clinical

- MedQA
- MedMCQA
- PubMedQA

#### Historical

- World-History-1500 QA
- ArchivalQA
- ChroniclingAmericaQA

### 03_orchestrator_pilot.ipynb

Runs the interaction simulation pipeline.

Responsibilities:

- Agent orchestration
- Persona simulation
- State assignment
- Session logging
- Evaluation

### 04_analysis.ipynb

Performs:

- PrefixSpan motif mining
- Transition-graph construction
- Entropy analysis
- HDBSCAN clustering
- Statistical testing
- Figure generation

---

## Experimental Design

### Policies

#### Seamless-Control

Provides direct answers whenever possible.

Example trajectory:

```text
S3_DIRECT_ANSWER
→ S6_USER_ACCEPT
→ S9_TASK_COMPLETE
```

#### Strategic Friction Framework (SFF)

Uses Socratic prompting and revision loops.

Example trajectory:

```text
S4_SOCRATIC_PROMPT
→ S7_CONTESTED
→ S4_SOCRATIC_PROMPT
→ S7_CONTESTED
```

---

### Factorial Design

| Factor | Levels |
|----------|----------|
| Policy | 2 |
| Domain | 3 |
| Persona | 3 |
| Difficulty | 3 |

Total design:

```text
2 × 3 × 3 × 3
```

Target sessions:

```text
1800
```

Successfully analyzed sessions:

```text
1799
```

---

## State Vocabulary

```text
S0  Session Start
S1  Task Presented
S2  Initial Orientation
S3  Direct Answer
S4  Socratic Prompt
S5  User Revision
S6  User Accept
S7  Contested
S9  Task Complete
S10 Failure
```

These states are used to convert conversations into symbolic trajectories for mining.

---

## Installation

### Clone Repository

```bash
git clone https://github.com/nxxis/Mining-Strategic-Friction.git
cd Mining-Strategic-Friction
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Required API Keys

The orchestration notebook requires access to LLM APIs.

Example:

```python
import os

os.environ["OPENAI_API_KEY"] = "YOUR_KEY"
os.environ["ANTHROPIC_API_KEY"] = "YOUR_KEY"
```

Do not commit API keys.

---

## Running in Google Colab

Mount Drive:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Set project root:

```python
from pathlib import Path

PROJECT_ROOT = Path(
    "/content/drive/MyDrive/DOC-ICDM"
)
```

Install dependencies:

```python
!pip install -r {PROJECT_ROOT}/requirements.txt
```

---

## Running Locally

Set project root:

```python
from pathlib import Path

PROJECT_ROOT = Path(".")
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Reproducing Results

Execute notebooks in the following order:

### Step 1

```text
01_setup_verification.ipynb
```

### Step 2

```text
02_task_pool_generation.ipynb
```

### Step 3

```text
03_orchestrator_pilot.ipynb
```

Requires:

- OpenAI API access
- Anthropic API access


### Step 4

```text
04_analysis.ipynb
```

Outputs:

```text
/Figures
```

---

## Main Results

### Structural Statistics

| Metric | Seamless | SFF | Cohen's d |
|----------|----------|----------|----------|
| Sequence Length | 4.50 | 7.99 | +2.38 |
| Completion Rate | 78.0% | 5.8% | -2.15 |
| Transition Entropy (bits) | 1.44 | 1.59 | +0.23 |
| Motif Concentration (HHI) | 0.587 | 0.357 | -0.88 |

### Dominant Interaction Patterns

#### Seamless-Control

```text
S3 → S6 → S9
```

Direct answer followed by acceptance and completion.

#### Strategic Friction Framework

```text
S4 → S7 → S4 → S7
```

Repeated Socratic-contested loop.

### Completion vs Quality

Among sessions producing a testable solution:

| Metric | Seamless | SFF |
|----------|----------|----------|
| Execution Pass Rate | 81.7% | 78.3% |

Statistical comparison:

```text
p = 0.29
```

No statistically detectable quality difference among completed solutions.

### Key Finding

The original hypothesis predicted that Strategic Friction would increase trajectory diversity.

The data showed the opposite.

Strategic Friction acts as a structural regularizer, concentrating interactions into highly recurrent revision motifs rather than producing broad structural diversity.

---

## Analysis Pipeline

### Sequential Pattern Mining

Method:

```text
PrefixSpan
```

Purpose:

- Discover recurrent motifs
- Compare policy-induced patterns

### Transition Graph Analysis

Metrics:

- Transition entropy
- State occupancy
- Absorbing-state concentration

### Clustering

Embedding Model:

```text
all-MiniLM-L6-v2
```

Clustering Method:

```text
HDBSCAN
```

Parameters:

```text
min_cluster_size = 20
```

---

## Experimental Configuration

| Component | Value |
|------------|---------|
| Total Sessions | 1800 |
| Valid Sessions | 1799 |
| Random Seed | 42 |
| Agent Model | Claude-Haiku-4.5 |
| Persona Simulator | GPT-4o-mini |
| Clustering Method | HDBSCAN |
| Sequence Mining | PrefixSpan |
| Embedding Model | all-MiniLM-L6-v2 |

---

## Generated Outputs

The analysis notebook produces:

- State-transition flow diagrams
- Motif concentration plots
- Effect-size summaries
- Completion-rate comparisons
- Time-to-completion curves
- Cluster visualizations
- Statistical summary tables

Generated figures are saved to:

```text
Figures/
```

---

## Reproducibility Notes

To ensure reproducibility:

- Fixed random seed (42)
- Frozen state vocabulary
- Frozen evaluation protocol
- Fixed clustering parameters
- Fixed statistical testing procedure
- Version-controlled notebooks
- Dependency specification via `requirements.txt`

---

## Limitations

- Synthetic personas rather than human participants
- Fixed turn budget
- Single primary LLM configuration
- Partial reliance on rubric-based judging for non-code tasks
- Results should be interpreted as structural properties of the interaction policy rather than evidence of human learning outcomes

---

## Citation

```bibtex
pending
```

---

## License

Released for research and academic use.