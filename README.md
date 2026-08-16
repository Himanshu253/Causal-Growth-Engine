# Causal Growth Engine: End-to-End Marketing Experimentation & Uplift Modeling


## 📑 Table of Contents
* [The Core Philosophy: Beyond Average Treatment Effects](#-the-core-philosophy-beyond-average-treatment-effects)
* [Pipeline Architecture](#-pipeline-architecture)
* [Key Technical Features](#-key-technical-features)
* [Experimental Results & Insights](#-experimental-results--insights)
* [Installation & Usage](#-installation--usage)
* [Project Structure](#-project-structure)

---

## 🎯 The Core Philosophy: Beyond Average Treatment Effects

Most data science portfolios stop at a basic A/B test (t-test) and report a blanket Average Treatment Effect (ATE). However, in real-world growth and marketing, this approach has two major flaws:
1. **The Variance Problem:** Standard experiments take too long to reach statistical significance because of high behavioral variance.
2. **The ATE Fallacy:** A positive ATE means a feature or promotion works *on average*, but it might actively harm or waste resources on users who would have converted anyway ("sure things") or users who find it annoying ("sleeping dogs").

This project implements an end-to-end causal inference and prescriptive analytics pipeline to solve both problems using real-world retail experimentation data.

---

## 🏗️ Pipeline Architecture

The pipeline executes a rigorous, production-grade analytical flow:

```text
Raw E-Commerce Logs
  │
  ├──► SQL Feature Store (DuckDB)
  │       └──► Data Wrangling & Covariate Extraction
  │
  ├──► Statistical Design & Power Analysis
  │       └──► MDE Calculation
  │
  ├──► Variance Reduction Engine
  │       └──► CUPED on Binary Conversion Metrics
  │
  ├──► Causal Machine Learning
  │       └──► GPU-Accelerated XGBoost T-Learner & X-Learner
  │
  └──► Prescriptive ROI Simulator
          └──► Blanket vs. ML-Targeted Campaign Optimization
```

## ✨ Key Technical Features

* **SQL-Driven Data Engineering:** Simulates a modern data warehouse workflow by processing raw e-commerce logs via **DuckDB** directly inside Python.
* **Statistical Power & Variance Reduction (CUPED):** Implements **Controlled-experiment Using Pre-Experiment Data (CUPED)**. Addresses zero-inflated retail data distributions by adapting linear covariate adjustments to binary site-visit metrics to tighten confidence intervals.
* **Causal Machine Learning (GPU XGBoost):** Harnesses GPU hardware acceleration (`cuda`) to train advanced meta-learners (**T-Learners** and **X-Learners** via `causalml`) to isolate individual-level Conditional Average Treatment Effects (CATE).
* **Prescriptive ROI Simulator:** Translates abstract ML outputs into bottom-line business value by optimizing marketing campaign spend and targeting only **"persuadables."**

## 🔬 Techniques & Methodologies

### 1. SQL Feature Stores via DuckDB

Pulls and transforms raw tabular transaction data on the fly using embedded analytical SQL engines.

### 2. Statistical Power Calculation (Cohen's *d*)

Evaluates sample size requirements and Minimum Detectable Effects (MDE) using independent two-sample *t*-test power modeling with `statsmodels`.

### 3. CUPED (Controlled-experiment Using Pre-Experiment Data)

Uses historical pre-experiment covariates ($X$) to strip out natural noise from post-experiment metrics ($Y$) via linear adjustment:

$$
\theta = \frac{\operatorname{Cov}(Y, X)}{\operatorname{Var}(X)}
$$

### 4. T-Learner Meta-Modeling

Fits separate GPU-accelerated XGBoost regression models for the control group ($M_0$) and treatment group ($M_1$), defining individual causal impact as:

$$
\tau(X) = M_1(X) - M_0(X)
$$

### 5. X-Learner Meta-Modeling

Utilizes advanced residual-based causal modeling through `causalml` to better capture treatment effects when treatment assignment sizes are highly imbalanced.

## 🧪 Experiments & Implementation

* **Dataset:** Evaluated on the benchmark **Kevin Hillstrom MineThatData** e-commerce dataset containing **64,000 real retail customers** subjected to a randomized email marketing split:

  * Mens email
  * Womens email
  * No email
* **Baseline A/B Test:** Grouped treatment variants (Mens/Womens email vs. No Email) and computed the standard Average Treatment Effect (ATE) and *p*-values.
* **CUPED Optimization Iteration:** Initially tested linear CUPED on continuous `post_exp_spend`, which suffered from **98% zero-inflation**. Pivoted to applying CUPED on binary `post_exp_visit` conversion metrics using historical user recency covariates to properly achieve variance reduction.
* **Prescriptive ROI Policy Simulation:** Simulated a target marketing cost of **$0.10 per email** to filter out negative-lift users and compare blanket deployment against model-targeted deployment.

## 📊 Experimental Results & Insights

### Standard A/B Test

Confirmed a positive ATE of **+$0.5968 spend per user** with high statistical significance:

$$
p < 0.00001
$$

### CUPED Variance Reduction

By adapting the covariate strategy to binary site visits, variance reduction was successfully unlocked:

* **Variance reduction:** 0.56%
* **Resulting *p*-value:** $3.28 \times 10^{-107}$

### Meta-Learner Benchmarking

Evaluated an industry-standard **X-Learner** against a **T-Learner**.

Interestingly, the simpler **T-Learner** yielded a higher simulated campaign profit:

* **T-Learner:** $35,718.77
* **X-Learner:** $33,012.60

The T-Learner achieved this by exercising stricter targeting control:

* **T-Learner targeting rate:** 91.4%
* **X-Learner targeting rate:** 95.2%

This demonstrates that complex residual modeling can sometimes **over-smooth sparse retail conversion signals**.

### ROI Strategy Comparison

| Strategy                              | Targeting Rate | Net Incremental Profit | Business Impact                                    |
| ------------------------------------- | -------------: | ---------------------: | -------------------------------------------------- |
| **Strategy 1: Blanket Campaign**      |         100.0% |             $30,989.16 | Baseline; wastes budget on non-persuadables        |
| **Strategy 2: T-Learner ML Campaign** |          91.4% |         **$35,718.77** | **+$4,729.60 value add via precision targeting**   |
| **Strategy 3: X-Learner ML Campaign** |          95.2% |             $33,012.60 | Highlights trade-offs in complex residual modeling |

## 🛠️ Installation & Usage


### 1. Install Dependencies

```bash
pip install duckdb xgboost statsmodels causalml matplotlib seaborn pandas numpy scipy
```

### 2. Run the Notebook

Open `Causal_Growth_Engine.ipynb` and ensure the runtime is configured with a **T4 GPU** for hardware-accelerated XGBoost training.


