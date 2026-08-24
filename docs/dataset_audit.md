# GeNIS Dataset Audit

> This document records what we have actually observed from the GeNIS CSV files available to the project.  
> It will be updated as the complete dataset structure is inspected.

---

# 1. Audit Status

**Status:** 🟡 Initial CSV inspection completed

**Dataset:** GeNIS — GECAD Network Intrusion Scenarios

**Current data inspected:** 11 CSV flow files provided to the project

**Primary responsibility:** Person 1 — Data Engineering & Graph Construction

**Review:** All team members

---

# 2. Dataset Source

## Dataset

GeNIS — GECAD Network Intrusion Scenarios

## Official Dataset

https://zenodo.org/records/14919237

## Publication

https://doi.org/10.1016/j.dib.2025.111487

---

# 3. Important Audit Rule

We will not assume the final prediction formulation before understanding the actual dataset.

In particular, we must establish:

- What one CSV file represents
- What one row represents
- How the CSV files relate to scenarios
- How timestamps should be interpreted
- How labels were generated
- Whether the provided files are complete or selected subsets
- How the 60-second flow duration relates to temporal graph construction
- What information is available before a prediction point
- What information could cause future-data leakage

---

# 4. Current CSV Files

The following files have been inspected:

## Benign Activity

```text
benign-user-activity.csv
benign-background-activity.csv
benign-admin-activity.csv
