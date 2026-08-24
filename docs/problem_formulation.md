# Problem Formulation

> This document formally defines the research problem for the project **Early Cyberattack Prediction Using Temporal Graph Neural Networks and Explainable AI**.
>
> The formulation is intentionally conservative. Dataset-dependent decisions that have not yet been established from the complete GeNIS scenario/timeline analysis are marked **TBD**.

---

# 1. Project Information

**Project Title:**

Early Cyberattack Prediction Using Temporal Graph Neural Networks and Explainable AI

**Dataset:**

GeNIS - GECAD Network Intrusion Scenarios

**Primary Research Question:**

> Can temporal graph representations improve early cyberattack-risk prediction compared with conventional machine-learning and static graph-neural-network approaches?

---

# 2. Team

| Person | Team Member | Primary Responsibility |
|---|---|---|
| Person 1 | **Abhishek Nayanala** | Data Engineering & Graph Construction |
| Person 2 | **Y V N Divya Teja** | Baselines & Machine Learning |
| Person 3 | **Kapil Reddy Padala** | Temporal GNN & Deep Learning |
| Person 4 | **Yathin Maganti** | XAI, Evaluation & Dashboard |

All four members review major methodological decisions and experimental results.

---

# 3. Research Problem

Traditional network intrusion detection can treat individual network-flow records independently:

```text
Flow
  |
Features
  |
Classifier
  |
Attack / Benign
```

This approach does not explicitly represent how network communication changes over time.

The proposed project instead investigates a temporal graph representation:

```text
G(t-k) -> G(t-k+1) -> ... -> G(t-1) -> G(t)
```

where:

- A node represents a network endpoint/host.
- An edge represents communication between endpoints.
- Node/edge attributes represent network activity.
- Each graph corresponds to a defined temporal observation window.

The model uses historical graph observations to estimate future attack risk.

---

# 4. Scope of the Prediction

The project does **not** claim to reliably predict real-world cyberattacks.

The intended claim is:

> The model predicts future attack-risk patterns within the evaluated GeNIS cyber-range scenarios.

Therefore, all experimental conclusions must be limited to the evaluated dataset, scenarios, preprocessing choices, and experimental setting.

---

# 5. Prediction Unit

## Primary prediction unit

The proposed primary task is:

> **Node-level future attack-risk prediction.**

A node corresponds to a network endpoint/host represented in the graph.

Conceptually:

```text
Host A -> future risk
Host B -> future risk
Host C -> future risk
Host D -> future risk
```

Example output:

```text
Host A -> 0.05
Host B -> 0.87
Host C -> 0.22
Host D -> 0.63
```

These values represent predicted future risk scores/probabilities.

They do not mean that an attack is certain to occur.

---

# 6. Input

At prediction time `t`, the model receives a history of temporal graphs:

```text
G(t-k), ..., G(t-2), G(t-1), G(t)
```

Each graph represents network activity observed during a defined temporal window.

The graph sequence is processed by the temporal model to produce a future-risk prediction.

---

# 7. Graph Definition

The current candidate graph abstraction is:

```text
Node
=
Network endpoint / host
```

and:

```text
Edge
=
Communication between two endpoints
```

Based on the currently inspected GeNIS CSV fields:

```text
SrcAddr -> DstAddr
```

provides the initial candidate source-to-destination relationship.

Candidate edge attributes may include:

- Protocol
- Ports
- Flow duration
- Packet counts
- Byte counts
- Traffic rate
- Traffic load
- Loss
- Retransmission
- Timing statistics
- Jitter

The final graph representation remains **TBD** until the deeper dataset analysis is complete.

---

# 8. Temporal Representation

The project represents network activity as a sequence:

```text
G1 -> G2 -> G3 -> ... -> GT
```

A graph corresponds to activity within a defined temporal window.

The exact windowing strategy has not yet been finalized.

---

# 9. Temporal Window

## Candidate resolutions

The project will investigate temporal resolutions including:

```text
5 seconds
10 seconds
30 seconds
60 seconds
```

However:

> A flow duration of approximately 60 seconds does not automatically mean that the graph snapshot interval should be 60 seconds.

The currently inspected CSV files show flow durations reaching approximately 60 seconds. This is separate from the temporal graph-window definition.

## Final temporal window

```text
TBD
```

The final value will be selected after:

1. Scenario analysis
2. Timestamp analysis
3. Flow-duration analysis
4. Attack-timing analysis
5. Temporal-resolution experiments

---

# 10. Observation Window

The observation window defines how much past graph history is provided to the model.

Conceptually:

```text
Past
 |
G(t-k) -> ... -> G(t-1) -> G(t)
                               |
                         Prediction
```

The final observation history depends on:

- Temporal resolution
- Sequence length
- Computational feasibility
- Predictive performance
- Early-warning requirements

## Final observation window

```text
TBD
```

---

# 11. Prediction Horizon

The prediction horizon defines how far into the future the model is asked to predict.

Conceptually:

```text
Observed until t
       |
Prediction horizon
       |
Future target
```

For example, if the horizon were `H`:

```text
Observed:
<= t

Target:
(t, t + H]
```

The actual value of `H` must be determined from the GeNIS scenario structure and the research objective.

## Final prediction horizon

```text
TBD
```

---

# 12. Future Attack-Risk Target

The primary target is intended to answer:

> Will this host exhibit malicious activity during the defined future prediction window?

Conceptually:

```text
Input:
Network activity up to time t

Target:
Malicious activity during future window
```

Possible binary target representation:

```text
0 = no qualifying future malicious activity
1 = qualifying future malicious activity
```

However, the exact target construction must be defined after determining:

- What constitutes malicious activity
- Which label field is appropriate
- How flow labels map to host labels
- How future flows are aggregated
- How overlapping windows are handled

## Final target definition

```text
TBD
```

---

# 13. Node-Level Label Construction

The current project proposal requires converting flow-level labels into a node-level future-risk target.

Conceptually:

```text
Future flows
     |
Group by host
     |
Determine whether qualifying malicious activity occurs
     |
Future node label
```

Possible conceptual definition:

```text
Node label = 1
if the host has qualifying malicious activity
during the future prediction horizon.
```

Otherwise:

```text
Node label = 0
```

This is only the initial formulation.

The final rule must account for:

- Source and destination roles
- Attack labels
- Scenario boundaries
- Prediction horizon
- Multiple flows
- Overlapping activity

## Final node-label rule

```text
TBD
```

---

# 14. Binary vs Attack Category Prediction

The primary project task is currently intended to be:

```text
Future attack risk
```

rather than detailed attack-category prediction.

Therefore, the main target is expected to be binary:

```text
Benign future state
vs
Malicious future state
```

The observed GeNIS files also contain:

```text
BinaryLabel
CategoryLabel
SubCategoryLabel
```

These can support additional analysis.

However, the project will not automatically make multi-class attack prediction the primary task unless the dataset analysis justifies it.

---

# 15. Early-Warning Concept

The central research component is early warning.

The model should ideally raise an elevated-risk prediction before the corresponding malicious activity occurs.

Conceptual example:

```text
10:00   Normal activity
10:10   Normal activity
10:20   Suspicious activity
10:30   High-risk prediction
10:40   Attack activity
```

Potential lead time:

```text
10 minutes
```

This example is illustrative only.

No experimental lead time has been established yet.

---

# 16. High-Risk Prediction

The model produces a continuous risk score:

```text
p âˆˆ [0, 1]
```

A threshold can then be used to define a high-risk prediction:

```text
p >= threshold
```

For example:

```text
Risk score
    |
Threshold
    |
High risk / not high risk
```

The threshold must not be chosen arbitrarily after seeing test results.

It should be selected using the validation data and documented before final test evaluation.

## Final threshold

```text
TBD
```

---

# 17. Lead-Time Definition

For an attack event that occurs at time:

```text
T_attack
```

and the first qualifying high-risk prediction occurring at:

```text
T_warning
```

the lead time can be defined conceptually as:

```text
Lead Time = T_attack - T_warning
```

A positive value indicates that the warning occurred before the attack event.

A warning occurring after the attack would not count as an early warning.

The exact attack-event definition and matching procedure remain:

```text
TBD
```

---

# 18. Early-Warning Metrics

The project intends to report:

### Mean lead time

Average lead time across qualifying predicted attack events.

### Median lead time

Median lead time, which is less sensitive to extreme values.

### Percentage of attacks predicted before execution

The proportion of relevant attack events for which the model generated a qualifying warning before the event.

### False Positive Rate

The rate at which benign activity produces an incorrect high-risk warning.

Additional classification metrics:

- Precision
- Recall
- F1
- PR-AUC
- ROC-AUC

---

# 19. Baseline Hierarchy

The project will not evaluate only the final Temporal GNN.

The planned hierarchy is:

```text
Random Forest
      |
MLP
      |
GraphSAGE
      |
GAT
      |
Temporal GNN
```

A Logistic Regression baseline may also be included as a simple conventional baseline.

The purpose is to determine whether each additional modeling component provides measurable value.

---

# 20. Research Questions

## RQ1 - Conventional ML vs Deep Learning

> Does deep learning improve attack-risk prediction over conventional machine learning?

Primary comparison:

```text
Random Forest vs MLP
```

---

## RQ2 - Graph Information

> Does explicitly modeling communication relationships improve prediction?

Primary comparison:

```text
MLP vs GraphSAGE
```

---

## RQ3 - Graph Attention

> Does graph attention improve prediction?

Primary comparison:

```text
GraphSAGE vs GAT
```

---

## RQ4 - Temporal Information

> Does temporal graph modeling improve early attack prediction over static GNNs?

Primary comparison:

```text
GAT vs Temporal GNN
```

This is the central research question.

---

## RQ5 - Temporal Resolution

> How does temporal resolution affect prediction performance and early-warning capability?

Comparison:

```text
5s
10s
30s
60s
```

where supported by the final dataset formulation.

---

## RQ6 - Explainability

> Can graph explanations identify the nodes, edges, features, and temporal observations that contribute to high-risk predictions?

---

# 21. Evaluation Metrics

Accuracy will not be the primary metric.

The main metrics are:

```text
Precision
Recall
F1
PR-AUC
ROC-AUC
False Positive Rate
Mean Lead Time
Median Lead Time
Percentage of Attacks Predicted Early
```

PR-AUC is particularly important because the class distribution may be imbalanced.

The final evaluation must also include scenario-level analysis.

---

# 22. Temporal Train / Validation / Test Split

Randomly splitting temporal network flows can introduce future information into training.

The preferred approach is a leakage-safe temporal split.

Conceptually:

```text
Earlier period
      |
TRAIN

Later period
      |
VALIDATION

Future period
      |
TEST
```

Scenario-based evaluation should also be investigated where the dataset structure supports it.

## Final split strategy

```text
TBD
```

---

# 23. Data Leakage Rules

The following rules are mandatory.

## Rule 1 - No label leakage

Do not use:

```text
BinaryLabel
CategoryLabel
SubCategoryLabel
```

as input features when predicting them.

---

## Rule 2 - No filename leakage

Filenames such as:

```text
attack-dos-udp.csv
attack-bruteforce-ssh.csv
benign-user-activity.csv
```

must not become model features.

---

## Rule 3 - No future observations

At prediction time `t`, the model may only use information available at or before `t`.

---

## Rule 4 - No future graph construction

The graph representation for the input sequence must not include future edges or node activity.

---

## Rule 5 - No future normalization

Scalers and other preprocessing transformations must be fitted using training data only.

---

## Rule 6 - No future feature engineering

Any aggregated feature must be computed only from observations available at prediction time.

---

## Rule 7 - Avoid scenario leakage

The same scenario should not unintentionally contribute highly related future activity to both training and test data.

---

# 24. Research Comparison Structure

The core experimental progression is:

```text
Conventional ML
       |
Deep Learning
       |
Static Graph Learning
       |
Graph Attention
       |
Temporal Graph Learning
```

The research question is not:

> "Is the Temporal GNN the most complicated model?"

Instead:

> "Does adding graph and temporal information provide measurable improvement under a controlled experimental setup?"

---

# 25. Temporal GNN Architecture

The initial practical architecture is:

```text
Temporal Graph Sequence
          |
GAT / GraphSAGE Encoder
          |
Node Embeddings
          |
GRU Temporal Encoder
          |
MLP Prediction Head
          |
Future Attack-Risk Score
```

The model architecture itself is not claimed to be novel.

The research contribution is intended to come from:

- Future-risk formulation
- Temporal graph representation
- Early-warning evaluation
- Controlled baselines
- Temporal-resolution comparison
- Ablation studies
- Explainability
- Failure analysis
- Reproducibility

---

# 26. Explainability Scope

For a high-risk prediction, the system should answer:

> Why did the model assign this host a high future attack risk?

Possible explanation components include:

- Important nodes
- Important edges
- Important features
- Important temporal observations

Candidate techniques include:

```text
GNNExplainer
Feature perturbation
Edge masking
Node masking
Temporal ablation
Attention analysis where appropriate
```

XAI will be interpreted as:

> Evidence that contributed to the model prediction.

It will not be interpreted as proof of attacker intent or causality.

---

# 27. Explanation Faithfulness

Where practical, explanations should be evaluated rather than only displayed.

A useful explanation should identify information whose removal or perturbation meaningfully changes the prediction.

Potential evaluation ideas include:

```text
Original prediction
      |
Remove important feature/edge
      |
Measure prediction change
```

The final explanation-faithfulness methodology is:

```text
TBD
```

---

# 28. Ablation Studies

The full model should be compared with reduced versions.

Candidate ablations:

```text
Full Temporal GNN
       |
Remove temporal component
       |
Remove attention
       |
Remove selected feature groups
       |
Reduce history length
```

The purpose is to determine which components actually contribute to performance.

---

# 29. Failure Analysis

The project must analyze cases where the model fails.

Examples:

```text
False positive
False negative
Late warning
Missed attack
Incorrectly high-risk benign host
```

For each important failure, investigate:

- Network context
- Temporal context
- Graph structure
- Feature behavior
- Model confidence
- Explanation
- Scenario characteristics

The goal is to understand model limitations rather than only reporting the best metric.

---

# 30. Assumptions

The current formulation assumes:

1. Network-flow timestamps can establish temporal ordering.
2. Source and destination endpoints can support a communication graph.
3. Future malicious activity can be defined from GeNIS labels.
4. Flow-level observations can be aggregated into temporal graph representations.
5. A node-level future-risk target can be constructed without using future information as an input.

These assumptions must be validated during implementation.

---

# 31. Known Limitations

The project should acknowledge:

### Dataset limitation

The evaluation is performed on GeNIS cyber-range scenarios and should not automatically generalize to real-world network environments.

### Scenario limitation

Performance may depend on the specific scenarios represented in the dataset.

### Temporal formulation limitation

The selected observation window and prediction horizon may influence results.

### Graph formulation limitation

Different node/edge definitions may produce different results.

### XAI limitation

Explanations indicate model evidence, not causal proof of attacker behavior.

### Generalization limitation

Strong performance on GeNIS does not guarantee deployment-level performance on unseen real-world networks.

---

# 32. Claims We Will Avoid

We will not claim:

> "The model predicts real-world cyberattacks."

Instead:

> "The model predicts future attack-risk patterns within the evaluated GeNIS cyber-range scenarios."

We will not claim:

> "XAI proves why the attacker targeted the machine."

Instead:

> "XAI identifies graph features, relationships and temporal observations that contributed to the model prediction."

We will not claim:

> "Temporal GNN causes better cybersecurity."

Instead:

> "Temporal graph modeling improved [metric] under the evaluated experimental setting."

We will not claim novelty simply because the project combines:

```text
GNN + GRU + XAI
```

---

# 33. Final Formulation Template

After the dataset audit is complete, this section will be replaced with the final numerical formulation.

Current template:

```text
Given:

Graph history:
G(t-k), ..., G(t)

Observation window:
[TBD]

Prediction horizon:
[TBD]

Node:
network endpoint

Input:
network activity available up to time t

Target:
whether the node exhibits qualifying malicious activity
during the future prediction horizon

Output:
risk score p âˆˆ [0,1]

High-risk threshold:
[TBD]
```

---

# 34. Final Decision Log

| Decision | Current Status |
|---|---|
| Prediction level | Node-level |
| Prediction type | Future attack-risk |
| Primary target | Binary future malicious activity |
| Graph node | Network endpoint/host |
| Graph edge | Communication |
| Temporal modeling | Required |
| Static GNN baseline | Required |
| Temporal GNN | Required |
| Early-warning evaluation | Required |
| XAI | Required |
| Exact temporal window | TBD |
| Observation history | TBD |
| Prediction horizon | TBD |
| Target construction | TBD |
| High-risk threshold | TBD |
| Train/validation/test split | TBD |

---

# 35. Implementation Dependency

The implementation order should follow:

```text
Dataset Audit
      |
Scenario Analysis
      |
EDA
      |
Prediction Target Definition
      |
Temporal Window Definition
      |
Graph Construction
      |
Baseline Features
      |
Baseline Models
      |
Static GNN
      |
Temporal GNN
      |
Early-Warning Evaluation
      |
XAI
      |
Ablation
      |
Failure Analysis
```

The model should not be finalized before the prediction formulation is finalized.

---

# 36. Document Status

### Completed

- [x] Define research objective
- [x] Define primary prediction level
- [x] Define candidate graph abstraction
- [x] Define candidate temporal representation
- [x] Define early-warning concept
- [x] Define research questions
- [x] Define evaluation principles
- [x] Define leakage rules
- [x] Define baseline hierarchy
- [x] Define XAI scope
- [x] Define ablation scope
- [x] Define failure-analysis scope

### Pending

- [ ] Final observation window
- [ ] Final prediction horizon
- [ ] Final temporal resolution
- [ ] Final node-label construction
- [ ] Final high-risk threshold
- [ ] Final attack-event definition
- [ ] Final lead-time calculation
- [ ] Final train/validation/test split
- [ ] Final graph construction
- [ ] Final feature set

---

# 37. Document Metadata

**Project:** Early Cyberattack Prediction Using Temporal Graph Neural Networks and Explainable AI

**Dataset:** GeNIS - GECAD Network Intrusion Scenarios

**Owner:** All team members

**Person 1:** Abhishek Nayanala - Data Engineering & Graph Construction

**Person 2:** Y V N Divya Teja - Baselines & Machine Learning

**Person 3:** Kapil Reddy Padala - Temporal GNN & Deep Learning

**Person 4:** Yathin Maganti - XAI, Evaluation & Dashboard

**Status:** Initial problem formulation; dataset-dependent decisions remain TBD

**Last updated:** 2026-08-24
