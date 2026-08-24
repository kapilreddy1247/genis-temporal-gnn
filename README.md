# Early Cyberattack Detection Using Temporal Graph Neural Networks and Explainable AI

> A research-oriented cybersecurity project investigating whether **temporal graph representations can improve future cyberattack-risk prediction compared with conventional machine learning and static Graph Neural Networks (GNNs)**.

---

## 📌 Project Status

**Status:** 🚧 Phase 0 — Project Setup

**Dataset:** GeNIS — GECAD Network Intrusion Scenarios

**Project Type:**

* Deep Learning
* Graph Neural Networks
* Temporal Modeling
* Explainable AI
* Cybersecurity
* Experimental Research

**Team Size:** 4

---

# 1. Project Overview

Modern network intrusion detection systems often analyze network flows individually:

```text
Network Flow
     ↓
Features
     ↓
Classifier
     ↓
Attack / Benign
```

This approach can miss an important characteristic of cyberattacks:

> **Network behavior changes over time and involves relationships between communicating hosts.**

Our project investigates a different representation.

Instead of treating each network flow independently, we represent network activity as a sequence of graphs:

```text
G(t1) → G(t2) → G(t3) → G(t4) → ...
```

where:

* **Node** represents a network endpoint/host.
* **Edge** represents communication between endpoints.
* **Node/edge features** represent network activity and traffic characteristics.
* **Time** represents the temporal position of network activity.
* **Labels** represent the ground-truth information available in the dataset.

The project then investigates whether incorporating both:

1. **Graph structure**, and
2. **Temporal evolution**

can improve future attack-risk prediction.

---

# 2. Main Research Question

> **Can temporal graph representations improve early cyberattack-risk prediction compared with conventional machine-learning and static graph-neural-network approaches?**

A secondary research question is:

> **Can explainable AI identify the network relationships, features, and temporal observations that contributed to high-risk predictions?**

---

# 3. Important Scope

This project does **not** claim to predict real-world cyberattacks with certainty.

The intended formulation is:

> Given network activity observed up to time `t`, predict whether a host or communication endpoint will exhibit malicious activity in a defined future time window within the evaluated GeNIS cyber-range scenarios.

For example:

```text
Observed Network Activity

G(t-3) → G(t-2) → G(t-1) → G(t)

                 ↓

          Temporal GNN

                 ↓

Future Attack-Risk Prediction

Host A → 0.12
Host B → 0.87
Host C → 0.21
Host D → 0.63
```

A prediction such as `0.87` represents **model-estimated future risk**, not certainty that an attack will occur.

The exact prediction horizon, target definition, and label construction will be finalized only after the GeNIS dataset audit.

---

# 4. Dataset — GeNIS

## GeNIS: GECAD Network Intrusion Scenarios

The project uses the **GeNIS (GECAD Network Intrusion Scenarios)** dataset.

According to the project documentation, GeNIS contains:

* Sequential attack scenarios
* Realistic benign enterprise activity
* Labelled network flows
* Ground-truth attack information
* Multiple temporal flow resolutions

  * 5 seconds
  * 10 seconds
  * 30 seconds
  * 60 seconds
* More than 2.8 million filtered flows
* More than 37 million raw packets

The dataset was created by GECAD at ISEP, Polytechnic of Porto, using the Airbus CyberRange.

### Dataset

https://zenodo.org/records/14919237

### Publication

https://doi.org/10.1016/j.dib.2025.111487

### Important

We will **not assume the exact dataset structure** before inspecting the downloaded files.

The Phase 1 dataset audit will determine:

* Available files
* Columns/features
* Timestamp fields
* Source/destination identifiers
* Labels
* Scenario identifiers
* Attack information
* Temporal structure
* Missing values
* Class distribution
* Potential sources of temporal/data leakage
* Feasibility of the proposed prediction target

---

# 5. Research Problem

Traditional flow-based detection can be represented as:

```text
Flow → Features → Classifier → Attack / Benign
```

Our proposed representation is:

```text
Network Activity
       ↓
Temporal Graph Construction
       ↓
G(t-3) → G(t-2) → G(t-1) → G(t)
       ↓
Temporal Graph Neural Network
       ↓
Future Attack-Risk Prediction
```

The research objective is **not simply to build a Temporal GNN**.

The objective is to experimentally determine whether temporal graph information provides measurable value.

---

# 6. Proposed Graph Representation

## 6.1 Nodes

A node represents a network endpoint.

A possible node identifier may be an IP address or another endpoint identifier available in GeNIS.

Example:

```text
Host_A
Host_B
Server_1
Server_2
```

The exact node definition will be finalized after the dataset audit.

---

## 6.2 Edges

A directed edge represents communication between two endpoints.

Example:

```text
Host_A ─────────→ Server_1
```

Potential information associated with communication may include:

* Source
* Destination
* Ports
* Protocol
* Traffic statistics
* Packet statistics
* Duration
* Timestamp
* Other features available in GeNIS

We will only use fields that are actually present in the dataset.

---

## 6.3 Temporal Graphs

Instead of creating one static graph:

```text
G
```

we create a sequence:

```text
G1 → G2 → G3 → G4 → G5
```

Each graph represents network activity within a defined time window.

The project will investigate the available temporal resolutions:

```text
5s
10s
30s
60s
```

This enables an additional research question:

> How does temporal resolution affect prediction performance and early-warning capability?

---

# 7. Prediction Task

## Primary Task

The initial proposed task is:

> **Node-level future attack-risk prediction.**

At time `t`, the model observes:

```text
G(t-k), ..., G(t-2), G(t-1), G(t)
```

and predicts future malicious activity:

```text
Risk(node, t+h)
```

where the exact future horizon `h` will be defined after inspecting GeNIS.

Example:

```text
Input:

G1 → G2 → G3 → G4

Output:

Host 1 → 0.05
Host 2 → 0.87
Host 3 → 0.22
Host 4 → 0.63
```

---

## Secondary Task

If the dataset structure supports it, attack-stage or attack-category classification may be investigated as an extension.

Potential classes must **not** be assumed in advance.

The exact target classes will be determined from the actual GeNIS labels.

---

# 8. Early-Warning Concept

The project is interested in more than ordinary classification accuracy.

We want to determine whether the model can raise a high-risk prediction **before the relevant malicious activity occurs**.

Example:

```text
10:00   Normal
10:10   Normal
10:20   Suspicious activity
10:30   Model raises high risk
10:40   Attack occurs
```

Potential warning:

```text
10 minutes
```

We will investigate:

* Mean lead time
* Median lead time
* Percentage of attacks predicted before execution
* False-positive rate
* Performance at different prediction horizons

A formal definition of:

* Observation window
* Prediction horizon
* Target label
* High-risk threshold

must be established before reporting early-warning results.

---

# 9. Model Hierarchy

We will not train only the final Temporal GNN.

The project uses controlled baselines to determine whether each additional modeling component provides measurable value.

```text
Random Forest
      ↓
MLP
      ↓
GraphSAGE
      ↓
GAT
      ↓
Temporal GNN
```

This hierarchy allows us to investigate:

```text
Conventional ML
       ↓
Deep Learning
       ↓
Graph Learning
       ↓
Graph Attention
       ↓
Temporal Graph Learning
```

---

# 10. Baseline 1 — Random Forest

```text
Network Features
       ↓
Random Forest
       ↓
Prediction
```

Purpose:

Establish a conventional machine-learning baseline.

---

# 11. Baseline 2 — MLP

```text
Features
   ↓
Fully Connected Neural Network
   ↓
Prediction
```

Purpose:

Determine whether a basic deep-learning model improves over conventional ML.

---

# 12. Model 3 — GraphSAGE

```text
Graph
  ↓
GraphSAGE
  ↓
Node Embeddings
  ↓
Prediction
```

Purpose:

Determine whether explicitly modeling communication relationships improves prediction.

---

# 13. Model 4 — GAT

```text
Graph
  ↓
Graph Attention Network
  ↓
Attention-weighted aggregation
  ↓
Prediction
```

Purpose:

Determine whether graph attention provides additional value over GraphSAGE.

---

# 14. Main Model — Temporal GNN

The initial practical architecture is:

```text
Temporal Graph Sequence
        ↓
GAT / GraphSAGE Encoder
        ↓
Node Embeddings
        ↓
GRU Temporal Encoder
        ↓
MLP Prediction Head
        ↓
Future Attack-Risk Probability
```

More explicitly:

```text
Node / Edge Features
        ↓
Feature Projection
        ↓
GAT / GraphSAGE
        ↓
Node Embeddings
        ↓
Sequence of Embeddings
        ↓
GRU
        ↓
Temporal Representation
        ↓
Dropout
        ↓
Linear Prediction Head
        ↓
Future Attack Probability
```

The first implementation will favor a model that the team can understand and defend.

We will not add architectural complexity merely to make the project appear more advanced.

---

# 15. Why GNN + GRU?

A GNN is useful for modeling:

```text
Who communicates with whom?
```

A temporal model is useful for modeling:

```text
How does that behavior change over time?
```

Together:

```text
GNN
↓
Spatial / relational representation

GRU
↓
Temporal representation
```

The combination itself should **not** be described as novel.

The research contribution comes from the experimental formulation and evaluation:

* Future-risk prediction
* Temporal graph representation
* Early-warning evaluation
* Controlled baselines
* Temporal-resolution analysis
* Ablation studies
* Explainability
* Failure analysis
* Reproducibility

---

# 16. Loss Function

For binary future-risk prediction, the initial candidate is:

```text
Binary Cross Entropy with Logits
```

If the target is significantly imbalanced, we will investigate:

```text
Weighted BCE
```

or another appropriate formulation.

The loss function will be selected after inspecting the class distribution.

---

# 17. Evaluation Metrics

Accuracy will **not** be the primary metric.

We will report:

### Precision

How many predicted attacks were actually attacks?

### Recall

How many actual attacks were detected?

### F1 Score

Balance between precision and recall.

### PR-AUC

Important for evaluating performance when positive examples are relatively uncommon.

### ROC-AUC

A complementary ranking metric.

### False Positive Rate

Important for practical intrusion-detection scenarios.

### Early-Warning / Lead Time

Project-specific metric measuring how early a high-risk prediction occurs relative to the relevant attack activity.

---

# 18. Temporal Data Splitting

Temporal leakage is one of the most important risks in this project.

We should **not randomly split temporal network activity if that allows future information to enter training**.

Preferred structure:

```text
Earlier period
      ↓
TRAIN

Later period
      ↓
VALIDATION

Future period
      ↓
TEST
```

Scenario-based evaluation may also be investigated:

```text
Some scenarios
      ↓
TRAIN

Held-out / later scenarios
      ↓
TEST
```

The final strategy will depend on the actual GeNIS scenario metadata.

---

# 19. Explainable AI

XAI will be implemented **after the main prediction pipeline works**.

The goal is to answer:

> Why did the model assign this host a high future attack-risk score?

Possible explanation targets:

* Important nodes
* Important edges
* Important features
* Important temporal observations

Potential techniques:

* GNNExplainer
* Feature perturbation
* Edge masking
* Node masking
* Temporal ablation
* Attention analysis where appropriate

Example:

```text
Host B
Future attack risk: 0.91

Important evidence:

Host A → Host B edge       HIGH
Traffic volume             HIGH
Destination-port activity  MEDIUM
Previous suspicious edge   HIGH
Recent temporal pattern    HIGH
```

---

# 20. Important XAI Limitation

XAI output represents **model evidence**.

It does not prove:

* Why an attacker acted
* The attacker's intent
* Causal relationships
* The true cause of an intrusion

Therefore, we will describe explanations as:

> Features, relationships, and temporal observations that contributed to the model prediction.

---

# 21. Ablation Study

Ablation experiments will investigate which components actually contribute to performance.

Example:

```text
Full Model
    ↓
Remove Temporal Component
    ↓
Remove Attention
    ↓
Remove Selected Features
    ↓
Reduce History Length
```

The purpose is to determine whether the improvement comes from:

* Graph structure
* Attention
* Temporal information
* Specific features
* Historical context

This is more scientifically useful than simply adding more architectures.

---

# 22. Temporal-Resolution Experiments

GeNIS provides multiple temporal resolutions.

We will investigate:

```text
5 seconds
10 seconds
30 seconds
60 seconds
```

Questions:

1. Which resolution gives the best predictive performance?
2. Which resolution provides the best early-warning behavior?
3. Does finer temporal resolution always improve performance?
4. Is there a trade-off between temporal detail and computational cost?

---

# 23. Failure Analysis

A strong research project should investigate when the model fails.

We will analyze:

```text
False Positives
False Negatives
Late Warnings
Missed Attacks
Scenario-specific failures
Unstable predictions
```

Questions include:

* Which scenarios are difficult?
* Which attacks are missed?
* When does the model generate false alarms?
* Does performance change with temporal resolution?
* Does the model fail when graph structure is sparse?
* Does reducing historical context hurt prediction?

Failure analysis will be reported alongside successful results.

---

# 24. Four-Person Team Structure

## Person 1 — Data Engineering + Graph Construction

**Primary responsibility:**

```text
GeNIS
 ↓
Data Audit
 ↓
Cleaning
 ↓
Feature Processing
 ↓
Node Mapping
 ↓
Edge Construction
 ↓
Temporal Graph Snapshots
 ↓
Temporal Splitting
```

### Main deliverables

```text
src/data/
src/graph/
dataset_audit.md
graph construction notebooks
processed graph representation
```

---

## Person 2 — Baselines + Static GNN

**Primary responsibility:**

```text
Random Forest
MLP
GraphSAGE
GAT
```

### Main research responsibility

Determine whether:

```text
ML
 ↓
Deep Learning
 ↓
Graph Learning
 ↓
Attention
```

provides measurable improvements.

### Main deliverables

```text
src/models/
src/training/
baseline results
static GNN results
```

---

## Person 3 — Temporal GNN

**Primary responsibility:**

```text
GNN Encoder
      +
Temporal Module
      ↓
Future Attack-Risk Prediction
```

Initial temporal module:

```text
GRU
```

### Main research responsibility

Determine whether temporal graph modeling improves over static GNNs.

### Main deliverables

```text
src/models/temporal_gnn.py
src/training/train_temporal.py
temporal experiments
training logs
model checkpoints
```

---

## Person 4 — Evaluation + XAI

**Primary responsibility:**

```text
Metrics
 ↓
Early Warning
 ↓
XAI
 ↓
Ablation
 ↓
Failure Analysis
```

### Main research responsibility

Determine:

* How well the models perform
* How early warnings occur
* Why predictions are made
* When models fail

### Main deliverables

```text
src/evaluation/
src/xai/
evaluation plots
XAI explanations
lead-time analysis
failure analysis
```

---

# 25. Concurrent Development Strategy

The four team members will work in parallel.

```text
                 GeNIS
                   ↓
             Common Data
               Interface
                   ↓
       ┌───────────┼───────────┐
       ↓           ↓           ↓
      P1          P2          P4
     Data       Baselines   Evaluation
       │           │           │
       │           ↓           │
       │      Static GNN        │
       │           │           │
       └───────────┼───────────┘
                   ↓
                  P3
             Temporal GNN
                   ↓
                  P4
              XAI + Analysis
```

To enable parallel development:

* Use standardized interfaces.
* Use small development samples.
* Use synthetic/sample graphs when necessary.
* Do not wait for the entire pipeline to be completed.
* Integrate components progressively.

---

# 26. Development Environment

The project will use a common cloud-based training approach where possible.

### GitHub

Used for:

* Source code
* Configuration
* Documentation
* Notebooks
* Experiment definitions
* Version control

### Google Drive

Used for:

* Shared GeNIS dataset
* Selected processed data
* Development samples
* Shared results where appropriate

### Google Colab / Kaggle

Used for:

* GPU training
* Large experiments
* Temporal GNN training
* Reproducible notebook-based experimentation

### Local Computers

Used for:

* Coding
* Debugging
* Small experiments
* Data inspection
* Documentation

Hardware differences between team members should not change the project code.

---

# 27. Repository Structure

```text
genis-temporal-gnn/
│
├── README.md
├── .gitignore
├── requirements.txt
│
├── configs/
│   ├── baseline.yaml
│   ├── graphsage.yaml
│   ├── gat.yaml
│   └── temporal_gnn.yaml
│
├── src/
│   ├── data/
│   │   ├── load_genis.py
│   │   ├── clean.py
│   │   ├── features.py
│   │   └── temporal_split.py
│   │
│   ├── graph/
│   │   ├── build_graph.py
│   │   ├── snapshots.py
│   │   └── node_mapping.py
│   │
│   ├── models/
│   │   ├── mlp.py
│   │   ├── graphsage.py
│   │   ├── gat.py
│   │   └── temporal_gnn.py
│   │
│   ├── training/
│   │   ├── train_baseline.py
│   │   ├── train_gnn.py
│   │   └── train_temporal.py
│   │
│   ├── evaluation/
│   │   ├── metrics.py
│   │   ├── curves.py
│   │   └── lead_time.py
│   │
│   └── xai/
│       ├── explain.py
│       ├── edge_importance.py
│       └── temporal_importance.py
│
├── notebooks/
│
├── experiments/
│
├── results/
│
├── figures/
│
└── docs/
    ├── dataset_audit.md
    ├── methodology.md
    └── experiment_log.md
```

---

# 28. Git Workflow

Each member will work primarily on their own feature branch.

```text
main
│
├── person1-data
├── person2-baselines
├── person3-temporal
└── person4-evaluation
```

Workflow:

```text
Create branch
     ↓
Implement
     ↓
Test
     ↓
Commit
     ↓
Push
     ↓
Pull Request
     ↓
Review
     ↓
Merge
```

The `main` branch should remain stable.

---

# 29. Data Policy

The raw GeNIS dataset should **not** be committed to GitHub.

Do not commit:

```text
❌ Raw GeNIS files
❌ Large model checkpoints
❌ Credentials
❌ API keys
❌ Temporary files
❌ Personal information
```

Use shared storage for the dataset and GitHub for the code.

---

# 30. Project Phases

## Phase 0 — Project Foundation

* Repository setup
* Team responsibilities
* Environment setup
* Research-question understanding
* Collaboration workflow

---

## Phase 1 — GeNIS Dataset Audit

* Download/access dataset
* Inspect files
* Inspect labels
* Inspect timestamps
* Inspect scenarios
* Inspect source/destination identifiers
* Inspect features
* Analyze class distribution
* Identify leakage risks
* Define candidate prediction targets

**Deliverable:**

```text
docs/dataset_audit.md
```

---

## Phase 2 — Data Engineering + Graph Construction

```text
Raw Data
   ↓
Cleaning
   ↓
Feature Processing
   ↓
Node Mapping
   ↓
Edge Construction
   ↓
Temporal Snapshots
   ↓
Train / Validation / Test
```

---

## Phase 3 — Baselines + Static GNN

Implement:

```text
Random Forest
MLP
GraphSAGE
GAT
```

---

## Phase 4 — Temporal GNN

Implement:

```text
GNN Encoder
      ↓
GRU
      ↓
Prediction Head
```

---

## Phase 5 — Early-Warning Experiments

Evaluate:

* Prediction horizons
* Lead time
* Detection-before-attack percentage
* False-positive rate

---

## Phase 6 — XAI + Ablation

Implement:

* Graph explanations
* Feature importance
* Edge importance
* Temporal importance
* Ablation experiments

---

## Phase 7 — Final Evaluation

Generate:

* Performance tables
* PR curves
* ROC curves
* Confusion matrices
* Lead-time analysis
* Temporal-resolution comparison
* Scenario-level results
* Failure analysis

---

## Phase 8 — Dashboard

Build:

```text
Dataset / Scenario
        ↓
Network Overview
        ↓
Risk Prediction
        ↓
Risk Timeline
        ↓
Early Warning
        ↓
XAI Explanation
        ↓
Research Evaluation
```

---

## Phase 9 — Report + Presentation

Final report structure:

1. Introduction
2. Related Work
3. Dataset
4. Problem Formulation
5. Graph Construction
6. Model Architecture
7. Experimental Setup
8. Baselines
9. Temporal GNN
10. XAI
11. Results
12. Ablation Study
13. Failure Analysis
14. Limitations
15. Conclusion

---

# 31. Project Timeline

A realistic initial schedule is:

| Week | Main Objective                        |
| ---- | ------------------------------------- |
| 1    | Dataset audit + project setup         |
| 2    | Data engineering + graph construction |
| 3    | ML baselines + EDA                    |
| 4    | GraphSAGE + GAT                       |
| 5    | Temporal GNN                          |
| 6    | Temporal experiments + ablations      |
| 7    | XAI + failure analysis                |
| 8    | Final evaluation + dashboard + report |

The schedule can be extended if experiments require additional time.

It is preferable to spend additional time on **experimental rigor** rather than adding unnecessary architectures.

---

# 32. Minimum Viable Project

If time becomes limited, the minimum research system should be:

```text
GeNIS
  ↓
Preprocessing
  ↓
Temporal Graph Construction
  ↓
Random Forest
  ↓
GraphSAGE
  ↓
Temporal GNN
  ↓
F1 / Recall / PR-AUC
  ↓
Early-Warning Analysis
  ↓
Basic XAI
```

Do not sacrifice proper data splitting, leakage prevention, or evaluation simply to add more models.

---

# 33. What This Project Is NOT

This project is **not**:

* A guaranteed real-world cyberattack predictor
* A production-ready enterprise IDS
* A claim that Temporal GNNs are universally superior
* A causal model of attacker behavior
* A claim of novelty merely because GNN + GRU + XAI are combined

---

# 34. Claims We Will Avoid

### ❌ Avoid

> The model predicts real-world cyberattacks.

### ✅ Use

> The model predicts future attack-risk patterns within the evaluated GeNIS cyber-range scenarios.

---

### ❌ Avoid

> XAI proves why the attacker targeted the machine.

### ✅ Use

> XAI identifies graph features, relationships, and temporal observations that contributed to the model prediction.

---

### ❌ Avoid

> Temporal GNN causes better cybersecurity.

### ✅ Use

> Temporal graph modeling improved [metric] under the evaluated experimental setting.

---

# 35. Research Philosophy

The project prioritizes:

```text
Understanding
     ↓
Correct Data Formulation
     ↓
Leakage Prevention
     ↓
Controlled Experiments
     ↓
Reproducibility
     ↓
Interpretability
     ↓
Failure Analysis
```

over:

```text
More Models
More Layers
More Parameters
More Complexity
```

The goal is to produce a project that the entire team can **understand, reproduce, explain, and defend**.

---

# 36. Expected Final System

```text
                 GeNIS Network-Flow Data
                           │
                           ▼
                 Data Preprocessing
                           │
                           ▼
                Temporal Graph Builder
                           │
                           ▼
             G(t-3) → G(t-2) → G(t-1) → G(t)
                           │
                           ▼
                   Temporal GNN
                           │
                           ▼
              Future Attack-Risk Scores
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
      Early-Warning Analysis            XAI
             │                           │
             └─────────────┬─────────────┘
                           ▼
                  Research Dashboard
                           │
                           ▼
             Experimental Conclusions
```

---

# 37. Final Research Question

> **Can temporal graph representations improve early cyberattack-risk prediction compared with conventional machine-learning and static graph-neural-network approaches?**

The project will answer this question through:

```text
Conventional ML
      ↓
Deep Learning
      ↓
Static GNN
      ↓
Graph Attention
      ↓
Temporal GNN
      ↓
Early-Warning Evaluation
      ↓
Ablation
      ↓
XAI
      ↓
Failure Analysis
```

The final conclusion will be based on **measured experimental evidence**, not assumptions.

---

# 38. References

### GeNIS Dataset

GECAD/ISEP, Polytechnic of Porto.

https://zenodo.org/records/14919237

### GeNIS Publication

*GeNIS: A modular dataset for network intrusion detection and classification.*

Data in Brief, 2025.

https://doi.org/10.1016/j.dib.2025.111487

---

## ⚠️ Current Next Step

**Phase 0 is currently in progress.**

The next technical milestone is:

```text
PHASE 1 — GeNIS DATASET AUDIT
```

Before implementing the final prediction task or Temporal GNN, we must inspect the actual GeNIS files and establish:

* Dataset structure
* Available features
* Labels
* Timestamps
* Scenarios
* Host identifiers
* Temporal structure
* Prediction target
* Train/validation/test strategy
* Potential leakage
