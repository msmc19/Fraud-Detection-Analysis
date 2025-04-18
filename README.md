# Fraud Detection in Digital Wallet Transactions

> **Course**: DATA 202 (University of Tennessee)  
> **Authors**: Aaron Marshall · Ashleigh Clark  
> **Date**: December 2024  

A reproducible data‑science pipeline that cleans a synthetic digital‑wallet dataset, engineers five **interpretable fraud indicators**, and surfaces high‑risk transactions/users through Python notebooks and interactive Tableau dashboards.

---

## 🗺️  Table of Contents
1. [Project Overview](#project-overview)  
2. [Fraud Indicators](#fraud-indicators)  
3. [Repository Layout](#repository-layout)  
4. [Dataset](#dataset)  
5. [Environment Setup](#environment-setup)  
6. [Quick Start](#quick-start)  
7. [Usage Guide](#usage-guide)  
8. [Key Results](#key-results)  
9. [Project Roadmap](#project-roadmap)  
10. [Contributing](#contributing)  
11. [License](#license)  

---

## 📌  Project Overview
Digital payments expose merchants to account takeovers, bot‑driven “card testing,” and other fraud vectors.  
This project analyzes **≈ 250 000 synthetic digital‑wallet transactions** and builds a lightweight rules‑based engine that:

* **Cleans & normalizes** transaction‑level data  
* **Engineers five anomaly indicators** (value, velocity, location, category, payment‑method)  
* **Flags suspicious transactions & users**, outputs summary dashboards (Tableau) and data stories for stakeholders  
* Provides a reusable blueprint that can be retrained or combined with machine‑learning models on real data  

---

## 🔍  Fraud Indicators
| # | Indicator | Rationale | Core Logic |
|---|-----------|-----------|-----------|
| 1 | **High Transaction Amount** | Fraud often involves abnormally large spends. | `amount > μ + n·σ` (category‑adjusted) |
| 2 | **Velocity / Rapid‑Fire Purchases** | Bots trigger bursts in seconds/minutes. | ≥ `k` tx per user inside `Δt` |
| 3 | **Unusual Location** | Transactions outside normal region may signal compromise. | location ≠ user’s modal region |
| 4 | **Irregular Product Category** | Sudden spend in atypical categories. | rare category vs. user history |
| 5 | **Multiple Payment Methods** | Fraudsters test many methods rapidly. | ≥ 3 distinct methods / user |

Parameters (σ, k, Δt, etc.) are tunable in the notebooks.

---

### 📊 Dataset
* **Source**: Synthetic digital‑wallet transactions (Kaggle)  
* **Size**: ≈ 250 000 rows × 19 columns  
* **Key Columns**: `transaction_id`, `user_id`, `transaction_amount`, `timestamp`, `location`, `payment_method`, `product_category`, etc.  
* **Privacy**: All records are fully synthetic; no PII  

---

### ⚙️ Environment Setup

git clone https://github.com/<your‑org>/fraud-detection-wallet.git
cd fraud-detection-wallet

# Create Python environment
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt   # pandas, numpy, seaborn, matplotlib, jupyter

### 🚀 Quick Start
1. **Run end‑to‑end pipeline**  
   * Open **`notebooks/fraud_indicators.ipynb`** and execute all cells.  
   * **Outputs**  
     * `flagged_transactions.csv` – transaction‑level flags  
     * `flagged_users.csv` – aggregated user risk scores  

2. **Explore results**  
   * Refresh data source in **`Fraud_Analysis_Tableau.twb`** to visualize KPIs.  
   * Run `visualization*.ipynb` notebooks for additional plots.  

3. **Tune thresholds**  
   * Edit parameters (σ, k, Δt) in **`fraud_indicator4.ipynb`**, rerun scoring, and compare flag counts.  

---

### 🧑‍💻 Usage Guide
| Goal | Action |
|------|--------|
| **Clean raw CSV** | Run first half of `fraud_indicators.ipynb` |
| **Generate fraud indicators** | Run entire `fraud_indicators.ipynb` or `fraud_indicator4.ipynb` |
| **Adjust indicator logic** | Edit helper functions (`high_value_flag`, `velocity_flag`, …) |
| **Package results** | Export `.xlsx` / `.pdf` from notebooks; embed Tableau screenshots |

---

### 📈 Key Results
* **1 650+ transactions** (~0.7 %) flagged across all rules  
* High‑value and multi‑payment indicators produced the strongest signals  
* Tableau dashboards enable drill‑down by user, indicator, day/hour, and geography  

---

### 🛣️ Project Roadmap
1. **Machine‑Learning Layer** – add Isolation Forest & Autoencoder anomaly scoring  
2. **Streaming Deployment** – Kafka/Flink pipeline for real‑time alerts  
3. **Feedback Loop** – ingest investigation outcomes to refine thresholds automatically  
4. **Risk Scoring API** – Flask/FastAPI microservice  
