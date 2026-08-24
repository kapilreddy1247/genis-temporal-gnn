# GeNIS Data Dictionary

> This document describes the important fields observed in the currently inspected GeNIS flow CSV files and records how each field is expected to be treated during the project.
>
> **Important:** This is an initial project data dictionary. Where the exact semantics of a field have not yet been verified from the official GeNIS documentation, the description is intentionally conservative.

---

## 1. Purpose

The purpose of this document is to provide a common reference for the project team when working with GeNIS flow records.

It helps answer:

- What does a field represent?
- Is it temporal, network-related, statistical, or a label?
- Could it be used for graph construction?
- Could it be used as a model feature?
- Should it be excluded because of leakage or identification?
- Does its exact meaning still require verification?

This document will be updated during the dataset audit and preprocessing phases.

---

# 2. Dataset Schema

All currently inspected CSV files contain:

```text
125 columns
```

The columns are:

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

# 3. Field Treatment Legend

The project uses the following preliminary treatment categories.

| Treatment | Meaning |
|---|---|
| `KEEP-CANDIDATE` | Candidate for model input after EDA |
| `GRAPH-KEY` | Important for graph construction |
| `TEMPORAL` | Used to establish temporal ordering/windows |
| `LABEL` | Target/label; not an input feature |
| `ID-METADATA` | Identifier or metadata; requires investigation |
| `EXCLUDE-CANDIDATE` | Likely unsuitable as a direct model feature |
| `MISSING` | Requires missing-value analysis |
| `VERIFY` | Exact semantic meaning must be confirmed |

These are preliminary classifications, not final preprocessing decisions.

---

# 4. Flow Identification Fields

## FlowID

**Type:** Identifier

**Role:** Identifies a flow record.

**Preliminary treatment:**

```text
ID-METADATA
VERIFY
```

**Model feature:** No, unless a later experiment provides a justified reason.

**Reason:** Identifier values generally should not be treated as behavioral network features.

---

## Rank

**Type:** Identifier / ordering-related field

**Role:** Present in the flow records.

**Preliminary treatment:**

```text
ID-METADATA
VERIFY
```

**Model feature:** No by default.

**Reason:** The field may encode ordering or dataset-generation information rather than network behavior.

---

# 5. Temporal Fields

## StartTime

**Type:** Timestamp

**Role:** Start time of the flow record.

**Preliminary treatment:**

```text
TEMPORAL
```

**Uses:**

- Temporal ordering
- Temporal window assignment
- Scenario timeline analysis
- Early-warning evaluation

**Important:** It must be handled carefully to avoid exposing future information.

---

## LastTime

**Type:** Timestamp

**Role:** End/last timestamp associated with the flow.

**Preliminary treatment:**

```text
TEMPORAL
VERIFY
```

**Uses:**

- Flow duration validation
- Temporal analysis
- Window assignment

**Leakage consideration:** When making a prediction at time `t`, information from `LastTime` must not expose activity occurring after the prediction point.

---

## Dur

**Type:** Numeric

**Role:** Flow duration.

**Observed behavior:**

```text
Minimum: approximately 0 seconds
Mean: approximately 43.51 seconds
Median: approximately 59.41 seconds
Maximum: approximately 60 seconds
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
```

**Potential use:**

- Flow-level feature
- Edge feature
- Aggregated temporal graph feature

---

## RunTime

**Type:** Numeric

**Role:** Flow-related timing statistic.

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

Exact semantic interpretation should be verified before final feature engineering.

---

## IdleTime

**Type:** Numeric

**Role:** Flow-related idle-time statistic.

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

---

# 6. Network Endpoint Fields

## SrcAddr

**Type:** Network address

**Role:** Source endpoint of a flow.

**Preliminary treatment:**

```text
GRAPH-KEY
```

**Potential use:**

```text
Source node
```

It is central to communication graph construction.

It should not automatically be treated as a normal numeric feature.

---

## DstAddr

**Type:** Network address

**Role:** Destination endpoint of a flow.

**Preliminary treatment:**

```text
GRAPH-KEY
```

**Potential use:**

```text
Destination node
```

It should not automatically be treated as a normal numeric feature.

---

## Sport

**Type:** Port identifier

**Role:** Source port.

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

Potentially useful as a categorical/network feature.

---

## Dport

**Type:** Port identifier

**Role:** Destination port.

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

Potentially useful as a categorical/network feature.

---

# 7. Protocol and Flow Metadata

## Proto

**Type:** Categorical

**Observed values:**

```text
tcp
udp
icmp
arp
ipv6-icmp
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
```

Likely requires categorical encoding.

---

## Trans

**Type:** Flow metadata

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

Exact meaning requires verification.

---

## Flgs

**Type:** Flow/network flags

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

May require categorical or multi-hot representation.

---

## Seq

**Type:** Sequence/flow metadata

**Preliminary treatment:**

```text
ID-METADATA
VERIFY
```

Should not be used until its meaning and predictive validity are established.

---

## Dir

**Type:** Direction-related flow field

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

Potentially useful for representing communication direction.

---

## State

**Type:** Flow state

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

Potentially useful as a categorical feature.

---

## Cause

**Type:** Flow/event metadata

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

Exact semantics must be verified.

---

# 8. Basic Statistical Fields

The following fields represent basic statistical properties of flow observations.

## Mean

Candidate numerical feature.

```text
KEEP-CANDIDATE
```

---

## StdDev

Candidate numerical feature.

```text
KEEP-CANDIDATE
```

---

## Sum

Candidate numerical feature.

```text
KEEP-CANDIDATE
```

---

## Min

Candidate numerical feature.

```text
KEEP-CANDIDATE
```

---

## Max

Candidate numerical feature.

```text
KEEP-CANDIDATE
```

The exact measured quantity represented by these statistics should be verified from the GeNIS documentation.

---

# 9. Network Metadata Fields

The following groups contain network metadata.

## MAC-related fields

```text
SrcMac
DstMac
```

**Preliminary treatment:**

```text
VERIFY
MISSING
```

They are completely missing in the currently inspected subset.

---

## OUI fields

```text
SrcOui
DstOui
```

**Preliminary treatment:**

```text
VERIFY
MISSING
```

They are completely missing in the currently inspected subset.

---

## Type-of-Service fields

```text
sTos
dTos
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

---

## DS-related fields

```text
sDSb
dDSb
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

---

## TTL fields

```text
sTtl
dTtl
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

---

## Hop-count fields

```text
sHops
dHops
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

---

## IP identification fields

```text
sIpId
dIpId
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

These fields require careful consideration because identifier-like network fields may have limited generalization value.

---

## MPLS fields

```text
sMpls
dMpls
```

**Preliminary treatment:**

```text
MISSING
VERIFY
```

They are completely missing in the currently inspected subset.

---

## Autonomous-system fields

```text
sAS
dAS
iAS
```

**Preliminary treatment:**

```text
MISSING
VERIFY
```

They are completely missing in the currently inspected subset.

---

# 10. Network / Event Metadata

## AutoId

**Preliminary treatment:**

```text
ID-METADATA
VERIFY
```

Do not use as a model feature until its meaning is established.

---

## NStrok

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

---

## sNStrok

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

---

## dNStrok

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

These fields require semantic verification.

---

# 11. Packet and Byte Statistics

These fields are particularly relevant candidates for traffic-behavior features.

## TotPkts

Total packet-related flow statistic.

```text
KEEP-CANDIDATE
```

---

## SrcPkts

Source packet count/statistic.

```text
KEEP-CANDIDATE
```

---

## DstPkts

Destination packet count/statistic.

```text
KEEP-CANDIDATE
```

---

## TotBytes

Total byte-related flow statistic.

```text
KEEP-CANDIDATE
```

---

## SrcBytes

Source byte statistic.

```text
KEEP-CANDIDATE
```

---

## DstBytes

Destination byte statistic.

```text
KEEP-CANDIDATE
```

---

## TotAppByte

Application-byte statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## SAppBytes

Source application-byte statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## DAppBytes

Destination application-byte statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

# 12. Traffic Statistics

## PCRatio

Traffic ratio statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## Load

Flow traffic load statistic.

```text
KEEP-CANDIDATE
```

---

## SrcLoad

Source-side traffic load statistic.

```text
KEEP-CANDIDATE
```

---

## DstLoad

Destination-side traffic load statistic.

```text
KEEP-CANDIDATE
```

---

## Rate

Traffic rate statistic.

```text
KEEP-CANDIDATE
```

---

## SrcRate

Source-side rate statistic.

```text
KEEP-CANDIDATE
```

---

## DstRate

Destination-side rate statistic.

```text
KEEP-CANDIDATE
```

These features are candidate behavioral features but require correlation and leakage analysis.

---

# 13. Loss and Retransmission

## Loss

Flow loss statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## SrcLoss

Source-side loss statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## DstLoss

Destination-side loss statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## pLoss

Loss-related percentage/statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## Retrans

Retransmission statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## SrcRetra

Source-side retransmission statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## DstRetra

Destination-side retransmission statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## pRetran

Retransmission-related percentage/statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## SrcGap

Source-side gap statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## DstGap

Destination-side gap statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

# 14. Inter-Packet Timing

The dataset contains source- and destination-side inter-packet timing features.

## Source timing fields

```text
SIntPkt
SIntPktMin
SIntPktMax
SIntDist
SIntPktAct
SIntActDist
SIntPktIdl
SIntIdlDist
```

## Destination timing fields

```text
DIntPkt
DIntPktMin
DIntPktMax
DIntDist
DIntPktAct
DIntActDist
DIntPktIdl
DIntIdlDist
```

**Preliminary treatment:**

```text
KEEP-CANDIDATE
VERIFY
```

These fields may be useful for detecting changes in communication timing.

However, the exact semantics of the derived timing fields should be verified before final use.

---

# 15. Jitter

## SrcJitter

Source-side jitter statistic.

```text
KEEP-CANDIDATE
```

---

## SrcJitAct

Source active-jitter statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## DstJitter

Destination-side jitter statistic.

```text
KEEP-CANDIDATE
```

---

## DstJitAct

Destination active-jitter statistic.

```text
KEEP-CANDIDATE
VERIFY
```

`DstJitAct` has high missingness in the currently inspected subset and therefore requires additional analysis.

---

# 16. TCP / Transport Fields

## SrcWin

Source-side TCP/window-related statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## DstWin

Destination-side TCP/window-related statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## SrcTCPBase

Source TCP base field.

```text
KEEP-CANDIDATE
VERIFY
```

---

## DstTCPBase

Destination TCP base field.

```text
KEEP-CANDIDATE
VERIFY
```

---

## TcpRtt

TCP round-trip-time-related statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## SynAck

TCP synchronization/acknowledgement-related statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## AckDat

TCP acknowledgement/data-related statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## TcpOpt

TCP options-related field.

```text
KEEP-CANDIDATE
VERIFY
```

This field has substantial missingness and requires analysis before use.

---

# 17. VLAN Fields

The dataset contains:

```text
sVlan
dVlan
sVid
dVid
sVpri
dVpri
```

These fields are currently completely missing in the inspected subset.

Preliminary treatment:

```text
MISSING
VERIFY
```

They should not be included in the initial model unless the complete dataset provides meaningful values.

---

# 18. Range Fields

```text
SRange
ERange
```

These fields are currently completely missing in the inspected subset.

Preliminary treatment:

```text
MISSING
VERIFY
```

---

# 19. User-Data Fields

```text
srcUdata
dstUdata
```

These fields are currently completely missing in the inspected subset.

Preliminary treatment:

```text
MISSING
VERIFY
```

---

# 20. Other Fields

## Inode

```text
Inode
```

Preliminary treatment:

```text
VERIFY
MISSING
```

It has extremely high missingness in the current subset.

---

## Offset

```text
Offset
```

Preliminary treatment:

```text
VERIFY
```

The exact semantic meaning must be confirmed before use.

---

## Ssaddr

```text
Ssaddr
```

Preliminary treatment:

```text
VERIFY
```

---

## Sdaddr

```text
Sdaddr
```

Preliminary treatment:

```text
VERIFY
```

---

# 21. Packet Size Statistics

## sMeanPktSz

Source-side mean packet-size statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## dMeanPktSz

Destination-side mean packet-size statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## sMaxPktSz

Source-side maximum packet-size statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## dMaxPktSz

Destination-side maximum packet-size statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## sMinPktSz

Source-side minimum packet-size statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

## dMinPktSz

Destination-side minimum packet-size statistic.

```text
KEEP-CANDIDATE
VERIFY
```

---

# 22. Labels

## BinaryLabel

**Type:** Binary target

Observed values:

```text
0 = benign
1 = attack
```

Preliminary treatment:

```text
LABEL
```

It must not be used as an input feature when predicting binary attack risk.

---

## CategoryLabel

**Type:** Categorical target/metadata

Observed categories include:

```text
benign
dos
bruteforce
```

Preliminary treatment:

```text
LABEL
```

It should not be used as an input feature when predicting the category.

---

## SubCategoryLabel

**Type:** Categorical target/metadata

Observed values include:

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

Preliminary treatment:

```text
LABEL
```

It must not be used as an input feature when predicting the corresponding target.

---

# 23. Recommended Initial Feature Groups

After leakage and missingness analysis, the first model experiments should investigate a manageable set of behavioral features.

Potential groups:

```text
Duration
Packet counts
Byte counts
Traffic rates
Traffic loads
Loss
Retransmissions
Timing statistics
Jitter
Protocol
Ports
Flow state
Direction
```

This is a candidate starting point, not a final feature list.

---

# 24. Fields Requiring Special Treatment

The following fields should receive special attention:

| Field/group | Reason |
|---|---|
| `FlowID` | Identifier |
| `Rank` | Possible ordering/metadata |
| `SrcAddr` | Graph identity |
| `DstAddr` | Graph identity |
| `StartTime` | Temporal information |
| `LastTime` | Temporal/future leakage risk |
| `BinaryLabel` | Target |
| `CategoryLabel` | Target |
| `SubCategoryLabel` | Target |
| Filename | Activity information / leakage risk |
| Completely missing fields | No useful values in current subset |
| Highly missing fields | Require missingness analysis |

---

# 25. Initial Graph Mapping

The current project concept maps the network fields as follows:

```text
SrcAddr â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€-> DstAddr
   â”‚                       â”‚
   â”‚                       â”‚
 Source node          Destination node
```

A flow can therefore provide an edge between two network endpoints.

Candidate edge attributes can be derived from:

```text
Dur
Proto
Sport
Dport
TotPkts
TotBytes
Rate
Load
Loss
Retrans
Timing
Jitter
```

The final graph representation will be defined after EDA.

---

# 26. Initial Temporal Mapping

Temporal fields:

```text
StartTime
LastTime
Dur
```

can be used to order flow records and assign them to temporal windows.

The project will eventually construct:

```text
G(t-k) -> ... -> G(t-1) -> G(t)
```

However, the temporal window size is currently:

```text
TBD
```

The fact that flows can last approximately 60 seconds does not by itself determine the graph window size.

---

# 27. Leakage Rules for Features

Before a feature is used, ask:

### Rule 1

Was this information available at prediction time?

### Rule 2

Could this field contain information from the future?

### Rule 3

Could this field directly or indirectly reveal the label?

### Rule 4

Is this field an identifier rather than a behavioral feature?

### Rule 5

Is this feature derived using information outside the observation window?

If any answer raises concern, the feature must be investigated before use.

---

# 28. Feature Selection Process

The final feature-selection pipeline should follow:

```text
Raw columns
     |
Remove direct labels
     |
Remove inappropriate identifiers
     |
Check missingness
     |
Check constant features
     |
Check correlations
     |
Check leakage
     |
Select candidate behavioral features
     |
Train baseline
     |
Evaluate
```

Feature selection should be based on evidence rather than simply maximizing the number of input features.

---

# 29. Final Feature List

The final feature list is currently:

```text
TBD
```

It will be recorded here after the EDA and preprocessing experiments.

---

# 30. Data Dictionary Status

### Completed

- [x] Record all 125 observed columns
- [x] Group columns conceptually
- [x] Identify labels
- [x] Identify temporal fields
- [x] Identify graph endpoint fields
- [x] Identify candidate behavioral features
- [x] Identify obvious identifier fields
- [x] Identify completely missing fields
- [x] Record initial leakage concerns

### Pending

- [ ] Verify exact field semantics using official GeNIS documentation
- [ ] Analyze all feature distributions
- [ ] Analyze feature correlations
- [ ] Analyze missingness by class
- [ ] Identify constant/near-constant features
- [ ] Finalize feature encoding
- [ ] Finalize graph features
- [ ] Finalize node features
- [ ] Finalize edge features
- [ ] Finalize preprocessing pipeline

---

# 31. Document Metadata

**Dataset:** GeNIS - GECAD Network Intrusion Scenarios

**Current schema:** 125 columns

**Document:** Data Dictionary

**Owner:** Person 1 - Data Engineering & Graph Construction

**Review:** All team members

**Status:** Initial data dictionary

**Last updated:** 2026-08-24
