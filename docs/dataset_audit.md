
# GeNIS Dataset Audit

> This document records the actual structure and properties of the GeNIS CSV files inspected for this project. It will be updated as the complete dataset and scenario structure are analyzed.

---

## 1. Audit Status

| Item | Status |
|---|---|
| Initial CSV inspection | Completed |
| Column inspection | Completed |
| Label inspection | Completed |
| Timestamp inspection | Completed |
| Flow-duration inspection | Completed |
| Endpoint inspection | Completed |
| Protocol inspection | Completed |
| Missing-value inspection | Completed |
| Duplicate inspection | Completed |
| Initial leakage analysis | Completed |
| Complete GeNIS package audit | Pending |
| Scenario structure analysis | Pending |
| Final prediction target | Not finalized |
| Final temporal-window definition | Not finalized |
| Final graph representation | Not finalized |
| Final train/validation/test strategy | Not finalized |

**Responsible:** Person 1 â€” Data Engineering & Graph Construction

**Review:** All team members

---

## 2. Dataset Information

### Dataset

**GeNIS â€” GECAD Network Intrusion Scenarios**

The project uses GeNIS as the network-intrusion dataset for investigating whether temporal graph representations can improve future attack-risk prediction.

The project documentation describes GeNIS as containing:

- Sequential attack scenarios
- Benign enterprise activity
- Labelled network flows
- Ground-truth attack information
- Multiple temporal resolutions
- More than 2.8 million filtered flows
- More than 37 million raw packets

These figures refer to the broader GeNIS dataset. The CSV files currently inspected are only a subset of the available data.

### Dataset source

https://zenodo.org/records/14919237

### Publication

https://doi.org/10.1016/j.dib.2025.111487

---

## 3. Important Audit Principle

The final prediction task must not be defined before understanding the actual dataset.

We need to determine:

1. What each CSV file represents.
2. What one row represents.
3. How files relate to scenarios.
4. What the labels mean.
5. How timestamps should be interpreted.
6. How flow duration relates to temporal graph construction.
7. What information is available before a prediction point.
8. What information could introduce future-data leakage.
9. How the inspected files relate to the complete GeNIS dataset.

---

## 4. Currently Inspected CSV Files

### Benign activity

```text
benign-user-activity.csv
benign-background-activity.csv
benign-admin-activity.csv
```

### Attack activity

```text
attack-dos-udp.csv
attack-dos-slowloris.csv
attack-dos-pushack.csv
attack-dos-icmp.csv
attack-dos-hulk.csv
attack-bruteforce-ssh.csv
attack-bruteforce-smb.csv
attack-bruteforce-ftp.csv
```

---

## 5. Current Dataset Size

The 11 inspected CSV files contain:

```text
338,820 flow records
125 columns per file
```

The currently inspected CSV files occupy approximately:

```text
195 MB
```

This is only the currently inspected subset and should not be treated as the complete GeNIS dataset.

---

## 6. File-Level Summary

| File | Rows | Binary Label | Category | Subcategory |
|---|---:|---:|---|---|
| `benign-user-activity.csv` | 12,851 | 0 | benign | benign-user |
| `benign-background-activity.csv` | 8,283 | 0 | benign | benign-background |
| `benign-admin-activity.csv` | 4,013 | 0 | benign | benign-admin |
| `attack-dos-udp.csv` | 65,536 | 1 | dos | dos-udp |
| `attack-dos-slowloris.csv` | 51,729 | 1 | dos | dos-slowloris |
| `attack-dos-pushack.csv` | 65,806 | 1 | dos | dos-pushack |
| `attack-dos-icmp.csv` | 65,536 | 1 | dos | dos-icmp |
| `attack-dos-hulk.csv` | 47,033 | 1 | dos | dos-hulk |
| `attack-bruteforce-ssh.csv` | 4,688 | 1 | bruteforce | bruteforce-ssh |
| `attack-bruteforce-smb.csv` | 10,001 | 1 | bruteforce | bruteforce-smb |
| `attack-bruteforce-ftp.csv` | 3,344 | 1 | bruteforce | bruteforce-ftp |

---

## 7. Binary Label Distribution

Across the currently inspected 338,820 flow records:

| Binary Label | Meaning | Count | Percentage |
|---:|---|---:|---:|
| 0 | Benign | 25,147 | 7.42% |
| 1 | Attack | 313,673 | 92.58% |

The current inspected files are therefore highly imbalanced toward attack-labelled flows.

This makes metrics such as the following important:

- Precision
- Recall
- F1
- PR-AUC
- ROC-AUC
- False Positive Rate

Accuracy will not be treated as the primary metric.

The final class distribution must also be checked against the complete dataset/scenario structure.

---

## 8. Category Labels

The following category labels were observed:

```text
benign
dos
bruteforce
```

Distribution:

| Category | Count | Percentage |
|---|---:|---:|
| dos | 295,640 | 87.26% |
| benign | 25,147 | 7.42% |
| bruteforce | 18,033 | 5.32% |

These are the categories observed in the currently inspected files.

The complete GeNIS taxonomy still needs to be verified.

---

## 9. Subcategory Labels

Observed subcategories include:

```text
benign-user
benign-background
benign-admin

dos-udp
dos-slowloris
dos-pushack
dos-icmp
dos-hulk

bruteforce-ssh
bruteforce-smb
bruteforce-ftp
```

Distribution:

| Subcategory | Count |
|---|---:|
| dos-pushack | 65,806 |
| dos-icmp | 65,536 |
| dos-udp | 65,536 |
| dos-slowloris | 51,729 |
| dos-hulk | 47,033 |
| benign-user | 12,851 |
| bruteforce-smb | 10,001 |
| benign-background | 8,283 |
| bruteforce-ssh | 4,688 |
| benign-admin | 4,013 |
| bruteforce-ftp | 3,344 |

These labels are observations from the current CSV files and should not automatically be assumed to represent the complete GeNIS attack taxonomy.

---

## 10. CSV Structure

All 11 inspected files contain 125 columns.

The observed columns are:

```text
FlowID
Rank
StartTime
LastTime
Proto
SrcAddr
Sport
DstAddr
Dport
Trans
Flgs
Seq
Dur
RunTime
IdleTime
Mean
StdDev
Sum
Min
Max
SrcMac
DstMac
SrcOui
DstOui
sTos
dTos
sDSb
dDSb
sCo
dCo
sTtl
dTtl
sHops
dHops
sIpId
dIpId
sMpls
dMpls
AutoId
sAS
dAS
iAS
Cause
NStrok
sNStrok
dNStrok
TotPkts
SrcPkts
DstPkts
TotBytes
SrcBytes
DstBytes
TotAppByte
SAppBytes
DAppBytes
PCRatio
Load
SrcLoad
DstLoad
Loss
SrcLoss
DstLoss
pLoss
Retrans
SrcRetra
DstRetra
pRetran
SrcGap
DstGap
Rate
SrcRate
DstRate
Dir
SIntPkt
SIntPktMin
SIntPktMax
SIntDist
SIntPktAct
SIntActDist
SIntPktIdl
SIntIdlDist
DIntPkt
DIntPktMin
DIntPktMax
DIntDist
DIntPktAct
DIntActDist
DIntPktIdl
DIntIdlDist
SrcJitter
SrcJitAct
DstJitter
DstJitAct
State
srcUdata
dstUdata
SrcWin
DstWin
sVlan
dVlan
sVid
dVid
sVpri
dVpri
SRange
ERange
SrcTCPBase
DstTCPBase
TcpRtt
SynAck
AckDat
TcpOpt
Inode
Offset
sMeanPktSz
dMeanPktSz
sMaxPktSz
dMaxPktSz
sMinPktSz
dMinPktSz
Ssaddr
Sdaddr
BinaryLabel
CategoryLabel
SubCategoryLabel
```

---

## 11. Important Feature Groups

### Flow identification

```text
FlowID
Rank
```

### Temporal information

```text
StartTime
LastTime
Dur
RunTime
IdleTime
```

### Network endpoints

```text
SrcAddr
DstAddr
Sport
Dport
```

### Protocol and flow information

```text
Proto
Trans
Flgs
Seq
Dir
State
Cause
```

### Packet and byte statistics

```text
TotPkts
SrcPkts
DstPkts
TotBytes
SrcBytes
DstBytes
TotAppByte
SAppBytes
DAppBytes
```

### Traffic statistics

```text
Mean
StdDev
Sum
Min
Max
PCRatio
Load
SrcLoad
DstLoad
Rate
SrcRate
DstRate
```

### Loss and retransmission

```text
Loss
SrcLoss
DstLoss
pLoss
Retrans
SrcRetra
DstRetra
pRetran
SrcGap
DstGap
```

### Timing statistics

```text
SIntPkt
SIntPktMin
SIntPktMax
SIntPktAct
SIntPktIdl
DIntPkt
DIntPktMin
DIntPktMax
DIntPktAct
DIntPktIdl
```

### Jitter

```text
SrcJitter
SrcJitAct
DstJitter
DstJitAct
```

### TCP / transport information

```text
SrcWin
DstWin
SrcTCPBase
DstTCPBase
TcpRtt
SynAck
AckDat
TcpOpt
```

### Packet-size statistics

```text
sMeanPktSz
dMeanPktSz
sMaxPktSz
dMaxPktSz
sMinPktSz
dMinPktSz
```

### Labels

```text
BinaryLabel
CategoryLabel
SubCategoryLabel
```

---

## 12. Timestamp Information

The CSV files contain:

```text
StartTime
LastTime
```

The values are Unix-style timestamps.

The currently inspected records cover approximately:

```text
2025-02-06 09:00:44
        â†“
2025-02-12 16:42:04
```

The exact relationship between these timestamps and individual GeNIS scenarios still needs to be established.

---

## 13. Flow Duration

The `Dur` field represents flow duration.

For the currently inspected records:

```text
Minimum: approximately 0 seconds
Mean: approximately 43.51 seconds
Median: approximately 59.41 seconds
Maximum: approximately 60 seconds
```

Many flows are therefore close to 60 seconds in duration.

### Important distinction

A flow lasting approximately 60 seconds does **not** automatically mean:

```text
flow duration = temporal graph snapshot size
```

and it does not establish:

```text
temporal graph window = 60 seconds
```

These are separate design decisions.

The temporal graph construction will be defined only after the dataset's temporal structure is fully understood.

---

## 14. Network Endpoints

The current data contains:

```text
SrcAddr
DstAddr
```

Across the currently inspected records:

```text
Unique source addresses: 11
Unique destination addresses: 273
```

This provides a natural starting point for representing network endpoints as graph nodes.

However, the final node definition is not yet finalized.

---

## 15. Protocols

The following protocol values were observed:

```text
tcp
udp
icmp
arp
ipv6-icmp
```

Protocol counts:

| Protocol | Flow Count |
|---|---:|
| tcp | 187,554 |
| udp | 83,580 |
| icmp | 65,536 |
| arp | 2,102 |
| ipv6-icmp | 48 |

The `Proto` field is categorical and will require appropriate preprocessing if used as a model feature.

---

## 16. Missing Values

Missing values are present in many columns.

The following columns were completely missing in the currently inspected data:

```text
SrcMac
DstMac
SrcOui
DstOui
sCo
dCo
sMpls
dMpls
sAS
dAS
iAS
NStrok
sNStrok
dNStrok
SIntDist
SIntActDist
SIntIdlDist
DIntDist
DIntActDist
DIntIdlDist
srcUdata
dstUdata
sVlan
dVlan
sVid
dVid
sVpri
dVpri
SRange
ERange
```

This represents 30 completely missing columns.

Other columns also have high missingness.

Examples:

| Column | Approximate Missingness |
|---|---:|
| `DstJitAct` | 75.71% |
| `TcpOpt` | 64.07% |
| `Inode` | 99.98% |

We will not automatically delete every feature containing missing values.

Before feature selection, we need to determine:

1. Why the feature is missing.
2. Whether the feature is useful.
3. Whether missingness differs by class.
4. Whether the feature should be removed.
5. Whether missingness itself contains useful information.

---

## 17. Duplicate Records

No exact duplicate rows were observed across the concatenated inspected records.

```text
Exact duplicate rows = 0
```

However, identifier values are not globally unique.

Observed:

```text
FlowID duplicates = 162,101
Rank duplicates   = 270,876
```

Therefore, `FlowID` and `Rank` should not automatically be treated as globally unique identifiers.

Their meaning must be investigated before deciding whether to use or exclude them.

---

## 18. Label Leakage

The following fields are direct labels:

```text
BinaryLabel
CategoryLabel
SubCategoryLabel
```

They must not be used as ordinary model input features when predicting the corresponding target.

---

## 19. Filename Leakage

The current filenames contain activity information.

Examples:

```text
attack-dos-udp.csv
attack-bruteforce-ssh.csv
benign-user-activity.csv
```

Therefore:

> Filenames must never be used as model features.

Otherwise the model could learn the dataset organization rather than actual network behavior.

Filenames will only be treated as metadata during data loading and analysis.

---

## 20. Temporal Leakage

The central prediction task involves predicting future risk.

Therefore, the model should only use information available at the prediction time.

Valid:

```text
Past observations
       â†“
Prediction time t
       â†“
Future target
```

Potentially invalid:

```text
Past + future observations
       â†“
Prediction time t
```

Future information must not enter:

- Input features
- Graph construction
- Temporal sequences
- Feature aggregation
- Normalization
- Target construction

---

## 21. Normalization Leakage

Feature normalization must be fitted using training data only.

Correct:

```text
Training data
     â†“
Fit scaler
     â†“
Transform train
Transform validation
Transform test
```

Incorrect:

```text
Train + validation + test
     â†“
Fit scaler
```

The second approach can leak information from future data into training.

---

## 22. Scenario Leakage

Scenario structure must be investigated before selecting the final train/test strategy.

We need to determine:

- Which files belong to each scenario.
- Whether benign and attack activity coexist within scenarios.
- Scenario duration.
- Attack start and end times.
- Whether scenarios overlap.
- Whether the same scenario can appear across splits.

Potential evaluation strategies include:

```text
Chronological split
```

and:

```text
Scenario-based split
```

The final strategy is currently:

```text
TBD
```

---

## 23. Candidate Graph Representation

Based on the currently observed fields, the initial graph abstraction is:

```text
Node
=
Network endpoint
```

and:

```text
Edge
=
Communication from source to destination
```

Conceptually:

```text
Host A â”€â”€â”€â”€â”€â”€â”€â”€â”€â†’ Host B
```

Potential edge features may be derived from flow-level information such as:

- Duration
- Packet count
- Byte count
- Protocol
- Ports
- Rate
- Loss
- Retransmission
- Timing statistics

The final graph representation is not yet finalized.

---

## 24. Candidate Temporal Graph Representation

The proposed project representation is:

```text
G(t-k) â†’ G(t-k+1) â†’ ... â†’ G(t-1) â†’ G(t)
```

Each graph should represent network activity within a defined temporal window.

The final construction must determine:

1. Temporal window size.
2. Window overlap.
3. Flow-to-window assignment.
4. Treatment of flows crossing boundaries.
5. Node feature aggregation.
6. Edge feature aggregation.
7. Node label construction.
8. Prediction horizon.

---

## 25. Candidate Prediction Task

The initial research formulation is:

> **Node-level future attack-risk prediction.**

Given network activity observed up to time `t`:

```text
G(t-k), ..., G(t-1), G(t)
```

the model predicts whether a host will exhibit malicious activity during a defined future window.

Example:

```text
Host A â†’ 0.05
Host B â†’ 0.87
Host C â†’ 0.22
Host D â†’ 0.63
```

These values represent predicted future risk probabilities/scores.

They do not represent certainty that an attack will occur.

---

## 26. Prediction Task â€” Not Finalized

The following decisions remain open:

```text
Prediction unit:
TBD

Observation window:
TBD

Prediction horizon:
TBD

Future target definition:
TBD

Node label construction:
TBD

High-risk threshold:
TBD
```

These decisions will be made after deeper temporal and scenario analysis.

---

## 27. Early-Warning Definition

The project aims to measure whether elevated risk can be identified before relevant malicious activity.

Example:

```text
10:00  Observed activity
10:10  Observed activity
10:20  Suspicious activity
10:30  Model raises high risk
10:40  Attack activity
```

Potential lead time:

```text
10 minutes
```

This is only an illustrative example.

The actual experiment must formally define:

- Attack occurrence
- High-risk threshold
- Observation window
- Prediction horizon
- Lead-time calculation

---

## 28. Temporal Resolution

The project investigates temporal resolutions such as:

```text
5 seconds
10 seconds
30 seconds
60 seconds
```

However:

```text
Flow duration
â‰ 
Temporal graph snapshot interval
```

The current CSV inspection only establishes that flow durations can reach approximately 60 seconds.

It does not by itself establish the final graph snapshot interval.

The temporal graph resolution will therefore be finalized after deeper dataset analysis.

---

## 29. Candidate Feature Selection

Potential feature groups include:

### Flow metadata

```text
Proto
Trans
Flgs
Dir
State
```

### Traffic volume

```text
TotPkts
SrcPkts
DstPkts
TotBytes
SrcBytes
DstBytes
```

### Duration

```text
Dur
RunTime
IdleTime
```

### Traffic statistics

```text
Mean
StdDev
Sum
Min
Max
Rate
SrcRate
DstRate
Load
SrcLoad
DstLoad
```

### Loss and retransmission

```text
Loss
SrcLoss
DstLoss
Retrans
SrcRetra
DstRetra
```

### Timing

```text
SIntPkt
DIntPkt
```

and related timing fields.

### Jitter

```text
SrcJitter
DstJitter
```

### TCP features

```text
TcpRtt
SynAck
AckDat
SrcWin
DstWin
```

These are candidate features only.

No final feature set has been selected.

---

## 30. Identifier Fields

Some fields may be identifiers rather than behavioral features:

```text
FlowID
Rank
SrcAddr
DstAddr
```

Their treatment depends on the final modeling design.

For example:

- `SrcAddr` and `DstAddr` are important for graph construction.
- They should not necessarily be treated as ordinary numerical features.
- `FlowID` may identify a flow rather than describe its behavior.
- `Rank` requires further investigation.

---

## 31. Required Exploratory Data Analysis

Before model training, we will analyze:

### Class distribution

- Binary labels
- Category labels
- Subcategory labels

### Temporal behavior

- Start times
- End times
- Flow duration
- Activity over time
- Attack timing

### Network behavior

- Source frequency
- Destination frequency
- Source-destination relationships
- Ports
- Protocols

### Feature behavior

- Missing values
- Constant features
- Distributions
- Outliers
- Correlations

### Graph behavior

After graph construction:

- Number of nodes
- Number of edges
- Degree distribution
- Graph density
- Connected components
- Temporal graph changes

---

## 32. Initial Findings

The current audit establishes:

### Dataset subset

```text
11 CSV files
338,820 flow records
125 columns
```

### Labels

```text
BinaryLabel
CategoryLabel
SubCategoryLabel
```

### Binary distribution

```text
Benign: 25,147
Attack: 313,673
```

### Network endpoints

```text
11 unique source addresses
273 unique destination addresses
```

### Protocols

```text
TCP
UDP
ICMP
ARP
IPv6-ICMP
```

### Timestamp range

```text
2025-02-06 09:00:44
        â†“
2025-02-12 16:42:04
```

### Flow duration

```text
Minimum: approximately 0 seconds
Mean: approximately 43.51 seconds
Median: approximately 59.41 seconds
Maximum: approximately 60 seconds
```

### Exact duplicates

```text
0 exact duplicate rows
```

### Identifier duplicates

```text
FlowID duplicates: 162,101
Rank duplicates: 270,876
```

### Completely missing columns

```text
30 columns
```

---

## 33. Research Implications

### Class imbalance

The current files are strongly attack-heavy.

Therefore, accuracy should not be the main metric.

We will emphasize:

```text
Precision
Recall
F1
PR-AUC
ROC-AUC
False Positive Rate
```

### Graph construction

The presence of:

```text
SrcAddr
DstAddr
```

provides a natural starting point for communication graphs.

### Temporal modeling

The presence of:

```text
StartTime
LastTime
Dur
```

provides flow-level temporal information.

The main unresolved question is how to aggregate these flows into temporal graph snapshots.

### Feature selection

The dataset contains many traffic statistics.

We should not simply use all 125 columns.

The feature-selection process must consider:

- Missingness
- Leakage
- Identifiers
- Correlation
- Constant features
- Predictive usefulness
- Availability at prediction time

---

## 34. Final Dataset Decisions

These will be completed after deeper analysis.

### Prediction unit

```text
TBD
```

### Node definition

```text
TBD
```

### Edge definition

```text
TBD
```

### Node features

```text
TBD
```

### Edge features

```text
TBD
```

### Temporal window

```text
TBD
```

### Sequence length

```text
TBD
```

### Observation history

```text
TBD
```

### Prediction horizon

```text
TBD
```

### Target label

```text
TBD
```

### High-risk threshold

```text
TBD
```

### Train / validation / test strategy

```text
TBD
```

---

## 35. Data Leakage Checklist

Before training any model:

- [ ] Labels excluded from input features
- [ ] Filename information excluded from input features
- [ ] Future observations excluded from input windows
- [ ] Future labels excluded from input features
- [ ] Temporal sequences chronologically ordered
- [ ] Train/validation/test split prevents future leakage
- [ ] Normalization fitted on training data only
- [ ] Feature engineering uses only information available at prediction time
- [ ] Graph construction does not expose future information
- [ ] Scenario leakage investigated
- [ ] Duplicate identifiers investigated
- [ ] Potential indirect target leakage investigated

---

## 36. Next Audit Tasks

The next stage is deeper exploratory analysis.

### Priority 1 â€” Scenario structure

Determine:

```text
Which files belong to which scenarios?
How long is each scenario?
Do benign and attack activity coexist?
When does an attack begin?
When does an attack end?
```

### Priority 2 â€” Temporal behavior

Analyze:

```text
Flow start times
Flow end times
Flow duration
Activity per temporal window
Attack activity over time
```

### Priority 3 â€” Network behavior

Analyze:

```text
Source hosts
Destination hosts
Ports
Protocols
Source-destination relationships
```

### Priority 4 â€” Feature behavior

Analyze:

```text
Missingness
Constant features
Feature distributions
Outliers
Correlation
Categorical values
```

### Priority 5 â€” Prediction formulation

Only after the above:

```text
Observation window
        â†“
Prediction horizon
        â†“
Future target
        â†“
Node label
        â†“
High-risk threshold
```

---

## 37. Audit Conclusion

The initial inspection confirms that the currently available CSV files contain flow-level network records with:

- 125 columns
- Source and destination endpoints
- Temporal information
- Flow-duration information
- Protocol information
- Traffic statistics
- Packet and byte statistics
- Loss and retransmission information
- Binary labels
- Category labels
- Subcategory labels

The currently inspected files contain 338,820 flow records.

The data provides a reasonable basis for investigating temporal graph representations.

However, the following have not yet been finalized:

```text
Flow Records
      â†“
Temporal Window Definition
      â†“
Graph Construction
      â†“
Node / Edge Features
      â†“
Observation History
      â†“
Future Prediction Target
      â†“
Leakage-Safe Split
```

Therefore, the Temporal GNN should not yet be treated as finalized.

The next technical step is deeper EDA and scenario/timeline analysis.

---

## 38. Audit Checklist

### Completed

- [x] Inspect available CSV files
- [x] Confirm common schema
- [x] Count records
- [x] Inspect labels
- [x] Inspect timestamps
- [x] Inspect flow duration
- [x] Inspect source/destination fields
- [x] Inspect protocols
- [x] Inspect missing values
- [x] Check exact duplicate rows
- [x] Check identifier duplication
- [x] Identify initial leakage risks
- [x] Record current findings

### Still required

- [ ] Inspect complete GeNIS package
- [ ] Read and verify official dataset documentation
- [ ] Establish scenario structure
- [ ] Determine attack timing
- [ ] Perform detailed EDA
- [ ] Analyze feature distributions
- [ ] Analyze feature correlations
- [ ] Define temporal graph windows
- [ ] Define graph aggregation
- [ ] Define observation history
- [ ] Define prediction horizon
- [ ] Define future target
- [ ] Define high-risk threshold
- [ ] Define temporal/scenario split
- [ ] Finalize feature set
- [ ] Validate absence of data leakage

---

## 39. Document Metadata

**Dataset:** GeNIS â€” GECAD Network Intrusion Scenarios

**Files currently audited:** 11 CSV files

**Records currently audited:** 338,820

**Columns:** 125

**Audit owner:** Nayanala Abhishek

**Project:** Early Cyberattack Prediction Using Temporal Graph Neural Networks and Explainable AI

**Status:** Initial audit completed; deeper audit in progress

**Last updated:** 2026-08-24
