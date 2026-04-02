# brazilian-hospital-process-mining
This is a master's degree project that was developed for the Business Information Systems course (A.Y 2025/26).

## 📋 Project Overview

This project applies **process mining** techniques to a real-world event log from a Brazilian hospital emergency department. The goal is to discover, analyze, and improve clinical processes by identifying bottlenecks, outliers, conformance issues, and resource imbalances.

The analysis covers:
- Data preprocessing and cleaning of the raw hospital event log
- Process discovery using the **Inductive Miner** algorithm to generate Petri nets
- Conformance checking with both token-based replay and alignment-based methods
- Statistical segmentation by case characteristics, disease type, readmission status, and physician workload

Note: the dataset is not public, so is not present in the repository.

---

## 📁 Repository Structure

```
brazilian-hospital-process-mining/
├── preprocessing.ipynb             # Data cleaning and preparation
├── process_discovery.ipynb         # Petri net discovery and conformance checking
├── segment_analysis.ipynb          # Statistical segmentation and KPI analysis
└── README.md
```

---

## 🔬 Notebooks

### 1. `preprocessing.ipynb` — Data Cleaning
Loads and cleans the raw hospital event log in preparation for process mining:
- Parses and standardizes timestamps
- Removes events with missing timestamps or critical fields
- Drops duplicate events and zero-duration cases
- Filters cases with incorrect start/end activities
- Outputs a clean event log to `generated/preprocessed_dataset.csv`

### 2. `process_discovery.ipynb` — Process Discovery & Conformance Checking
Discovers process models and evaluates conformance:
- **Global model**: Discovers a baseline Petri net for the entire dataset
- **Inlier/Outlier segmentation**: Separates normal from abnormal cases using variant frequency; discovers separate process models for each segment
- **Conformance checking**: Evaluates readmitted vs. discharged cases against the inlier model using fitness, precision, and F1-score
- **Disease-specific models**: Discovers and evaluates dedicated Petri nets for the top 3 diagnoses (Influenza J11.1, Renal Colic N23, Chest Pain R07.4)

### 3. `segment_analysis.ipynb` — Segmentation & Statistical Analysis
Performs in-depth statistical analysis of process segments:
- **Variant distribution**: Applies the Pareto principle to identify dominant variants; analyzes variant-length vs. frequency correlation
- **Case KPIs**: Computes per-case duration, case size (events), and rework count
- **Outlier analysis**: Compares KPIs between inliers and outliers (duration, size, rework)
- **Readmission analysis**: Identifies KPI differences and disease-level readmission risk (odds ratios)
- **Resource analysis**: Measures physician workload distribution using the Gini index

---

## 📊 Key Findings

| Finding | Detail |
|---|---|
| **Process heterogeneity** | ~16% of variants cover 80% of cases; long-tail variant distribution |
| **Outlier drivers** | Outlier cases average 619 min vs. 100 min for inliers; rework is the primary driver |
| **Triage deviation** | Outliers frequently skip or delay the initial triage phase |
| **Imaging bottleneck** | Diagnostic imaging doubles median case duration (57 → 101 min) |
| **Disease fit** | Standard protocol fits Influenza (J11.1) well; poor fit for Renal Colic (N23) and Chest Pain (R07.4) |
| **Readmission risk** | N23 (Renal Colic) has 5.5× higher readmission risk than the average |
| **Workload imbalance** | One physician handles 43.5% of all cases; Gini index > 0.5 |

---

## 🛠️ Technologies & Libraries

| Library | Purpose |
|---|---|
| `pm4py` | Process mining (discovery, conformance checking, Petri nets) |
| `pandas` / `numpy` | Data manipulation and numerical computation |
| `matplotlib` / `seaborn` | Visualization |
| `scipy.stats` | Statistical hypothesis testing |
| `pingouin` / `statsmodels` | Advanced statistical analysis |
| `powerlaw` | Distribution fitting and analysis |
| `inequality` | Gini index for workload inequality |

---

## 🚀 Getting Started

### Prerequisites

Install the required Python packages:

```bash
pip install pm4py pandas numpy matplotlib seaborn scipy pingouin statsmodels powerlaw inequality
```

### Running the Analysis

Execute the notebooks in order:

1. **`preprocessing.ipynb`** — cleans the raw dataset and generates `generated/preprocessed_dataset.csv`
2. **`process_discovery.ipynb`** — discovers Petri nets and performs conformance checking
3. **`segment_analysis.ipynb`** — performs statistical segmentation and KPI analysis

> **Note**: Place the raw event log at `data/dataset.csv` before running `preprocessing.ipynb`.

---

## 📌 Improvement Recommendations

Based on the analysis results:

- **Enforce mandatory triage** for all incoming cases to reduce structural deviations
- **Implement disease-specific clinical pathways** tailored to N23 and R07.4 diagnoses
- **Reduce diagnostic rework** through improved initial assessment and test planning
- **Rebalance physician workload** to reduce overconcentration on a single resource
- **Prioritize imaging resource allocation** to address the identified bottleneck
