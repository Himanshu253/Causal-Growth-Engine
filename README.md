# Causal Growth Engine: End-to-End Experimentation, Causal Inference & Treatment Policy Optimization

A production-style experimentation framework that moves beyond traditional A/B testing by combining rigorous experimental design, variance reduction, heterogeneous treatment effect estimation, and treatment policy optimization to maximize business impact.

---

## 📑 Table of Contents

* [Overview](#-overview)
* [Why This Project?](#-why-this-project)
* [Pipeline Architecture](#-pipeline-architecture)
* [Technical Highlights](#-technical-highlights)
* [Methodology](#-methodology)
* [Experimental Workflow](#-experimental-workflow)
* [Key Results](#-key-results)
* [Tech Stack](#-tech-stack)
* [Installation & Usage](#-installation--usage)

---

# 🎯 Overview

Most experimentation projects stop after reporting a statistically significant Average Treatment Effect (ATE).

Real-world experimentation is considerably more challenging.

Questions such as

* Is the randomization valid?
* Was the experiment sufficiently powered?
* Can variance be reduced?
* Which customers actually benefit?
* Who should receive treatment?
* What deployment policy maximizes profit?

are often left unanswered.

This project implements an end-to-end experimentation pipeline inspired by production workflows used in Growth, Product, and Marketing Data Science teams.

Rather than stopping at statistical significance, the pipeline estimates individual treatment effects and converts them into business decisions through policy optimization.

---

# 🚀 Why This Project?

Traditional A/B testing suffers from three major limitations.

### 1. Average Treatment Effects Hide User Heterogeneity

A positive experiment average does **not** imply every customer benefits.

Users typically fall into four groups:

* Persuadables
* Sure Things
* Lost Causes
* Sleeping Dogs

Treating everyone wastes marketing budget.

---

### 2. High Variance Makes Experiments Expensive

Retail data is noisy.

The project applies **CUPED** variance reduction using historical behavioral covariates to increase statistical efficiency without collecting additional samples.

---

### 3. Statistical Significance ≠ Business Value

Even statistically significant experiments can destroy ROI.

Instead of asking

> "Did treatment work?"

this project asks

> **"Who should actually receive treatment?"**

---

# 🏗 Pipeline Architecture

```text
                     Raw E-Commerce Dataset
                               │
                               ▼
                  DuckDB SQL Feature Engineering
                               │
                               ▼
                 Randomization & SRM Validation
                               │
                               ▼
              Statistical Power & MDE Analysis
                               │
                               ▼
             Baseline A/B Test (ATE Estimation)
                               │
                               ▼
                 CUPED Variance Reduction
                               │
                               ▼
        Pre-Registered Subgroup Analysis
                               │
                               ▼
      CATE Estimation (T-Learner / X-Learner)
                               │
                               ▼
           Model Validation & Calibration
                               │
                               ▼
          Treatment Policy Optimization
                               │
                               ▼
          Business ROI Simulation & Decision
```

---

# ✨ Technical Highlights

### SQL-Based Feature Engineering

Uses DuckDB as an embedded analytical database to perform production-style SQL transformations directly from Python.

---

### Experiment Design Validation

Before estimating treatment effects, the project validates experimental integrity through

* Sample Ratio Mismatch (SRM) testing
* Covariate balance checks
* Randomization diagnostics

---

### Statistical Power Analysis

Evaluates experiment sensitivity using

* Minimum Detectable Effect (MDE)
* Statistical power
* Sample size calculations

to determine whether an experiment is capable of detecting meaningful business effects.

---

### CUPED Variance Reduction

Implements Controlled Experiments Using Pre-Experiment Data (CUPED) to reduce estimator variance using historical customer behavior.

Rather than increasing sample size, the pipeline improves statistical efficiency through covariate adjustment.

---

### Causal Machine Learning

Estimates Conditional Average Treatment Effects (CATE) using meta-learners including

* T-Learner
* X-Learner

built with GPU-accelerated XGBoost.

Instead of predicting outcomes, these models estimate **incremental causal impact** for each individual customer.

---

### Treatment Policy Optimization

Transforms estimated treatment effects into actionable business decisions by targeting only customers with positive expected uplift.

The resulting deployment policy is compared against a blanket marketing strategy to quantify incremental business value.

---

# 🔬 Methodology

The project follows a modern experimentation workflow.

## 1. Experimental Design

* Randomized treatment assignment
* Power analysis
* MDE estimation
* SRM detection

---

## 2. Classical Experiment Analysis

* Average Treatment Effect (ATE)
* Hypothesis testing
* Confidence intervals

---

## 3. Variance Reduction

CUPED adjustment

[
Y_{CUPED}=Y-\theta(X-\bar X)
]

where

[
\theta=\frac{Cov(Y,X)}{Var(X)}
]

---

## 4. Heterogeneous Treatment Effects

Estimate

[
\tau(x)=E[Y(1)-Y(0)\mid X=x]
]

using

* T-Learner
* X-Learner

to identify users most likely to respond positively.

---

## 5. Policy Learning

Rather than maximizing prediction accuracy,

the optimization objective becomes

[
\max \sum_i \tau_i - Cost_i
]

producing an economically optimal treatment policy.

---

# 🧪 Experimental Workflow

The framework is demonstrated using the **Kevin Hillstrom MineThatData** marketing dataset.

Pipeline stages include

* SQL feature engineering
* Experiment validation
* Power analysis
* Baseline A/B testing
* CUPED adjustment
* Subgroup analysis
* CATE estimation
* Treatment policy simulation
* Business ROI comparison

The notebook mirrors the analytical workflow commonly followed in experimentation teams at technology companies.

---

# 📊 Key Results

The framework demonstrates that:

* Experiment validation should precede causal estimation.
* CUPED reduces estimator variance and increases experimental efficiency.
* Significant heterogeneity exists across customers.
* Individual treatment effects outperform blanket campaign deployment.
* Treatment policies generate greater expected ROI than treating every customer.

Rather than answering

> "Did the campaign work?"

the pipeline answers

> "Which customers should receive the campaign?"

---

# 🛠 Tech Stack

### Data Engineering

* DuckDB
* Pandas
* NumPy

### Statistics

* SciPy
* Statsmodels

### Machine Learning

* XGBoost (GPU)
* CausalML

### Visualization

* Matplotlib
* Seaborn

---

# ⚙ Installation & Usage

### Install Dependencies

```bash
pip install duckdb pandas numpy scipy statsmodels xgboost causalml matplotlib seaborn
```

### Run

Open

```text
causal_growth_engine_v3.ipynb
```

Run all notebook cells.

---

## ⭐ Skills Demonstrated

* Experimental Design
* A/B Testing
* Statistical Inference
* Power Analysis
* CUPED
* Causal Inference
* Heterogeneous Treatment Effects (CATE)
* Uplift Modeling
* Treatment Policy Optimization
* SQL Analytics (DuckDB)
* XGBoost
* Business Decision Science
* Reproducible Data Science Pipelines


