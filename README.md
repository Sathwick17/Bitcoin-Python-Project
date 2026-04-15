<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:F7931A,100:232526&height=160&section=header&text=Bitcoin%20Price%20Prediction&fontSize=34&fontColor=ffffff&fontAlignY=50&desc=3NF%20Normalization%20%7C%20SQLite%20%7C%20ML%20Pipeline%20%7C%20MLflow%20%2B%20DagsHub&descAlignY=72&descSize=13" width="100%"/>

</div>

<div align="center">

![Python](https://img.shields.io/badge/Python-0ea5e9?style=for-the-badge&logo=python&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-f97316?style=for-the-badge&logo=scikitlearn&logoColor=white)
![MLflow](https://img.shields.io/badge/MLflow-0194E2?style=for-the-badge&logo=mlflow&logoColor=white)

[![Live Site](https://img.shields.io/badge/View%20JupyterBook-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://bitcoin-python-project.vercel.app)

</div>

---

## Overview

An end-to-end Bitcoin price analysis and prediction pipeline built on historical OHLCV data. The project covers relational database normalization, exploratory data analysis, feature engineering, and binary classification to predict daily price direction — tracked and versioned using MLflow on DagsHub.

---

## Dataset

| Attribute | Detail |
|-----------|--------|
| Source | `coin_Bitcoin.csv` — Historical Bitcoin OHLCV data |
| Records | 2,991 daily entries |
| Features | Date, Open, High, Low, Close, Volume, Marketcap |
| Target | Binary — `1` if `Close > Open` (price went up), `0` otherwise |
| Train / Test Split | 80% / 20% (2,392 train · 599 test) |

---

## Pipeline

```
coin_Bitcoin.csv (Raw OHLCV Data)
          │
          ▼
   3NF Normalization
   ├── Crypto table     (Crypto_ID, Name, Symbol)
   └── DailyPrices table (Crypto_ID, Date, High, Low,
                          Open, Close, Volume, Marketcap)
          │
          ▼
   SQLite Database (bitcoin_data.db)
   (SQL JOIN → Pandas DataFrame)
          │
          ▼
   Exploratory Data Analysis
   ├── ydata-profiling report
   ├── Correlation heatmap (High, Low, Open, Close, Volume, Marketcap)
   ├── Volume distribution (highly right-skewed — 2,664 / 2,991 records in lowest bin)
   └── Missing values: None across all columns
          │
          ▼
   Feature Engineering & Preprocessing
   ├── Drop: SNo, Name, Symbol, Date
   ├── Target creation: Close > Open → binary label
   ├── StandardScaler  (numerical features)
   ├── MinMaxScaler    (numerical features)
   └── Log Transform   (np.log1p — handles volume skewness)
          │
          ▼
   Model Training — Logistic Regression
   (Scikit-learn Pipeline · 80/20 split · 10-fold StratifiedKFold)
          │
          ▼
   Hyperparameter Tuning (GridSearchCV)
   ├── C: [0.01, 0.1, 1, 10]
   └── Penalty: [L1, L2]
          │
          ▼
   Experiment Tracking — MLflow on DagsHub
   (F1-Score, TP, TN, FP, FN, best params, model artifact)
```

---

## Database Schema (3NF)

**Crypto Table**

| Column | Type | Description |
|--------|------|-------------|
| `Crypto_ID` | INTEGER (PK) | Unique cryptocurrency identifier |
| `Name` | TEXT | Full name (e.g., Bitcoin) |
| `Symbol` | TEXT | Ticker symbol (e.g., BTC) |

**DailyPrices Table**

| Column | Type | Description |
|--------|------|-------------|
| `Price_ID` | INTEGER (PK, AUTO) | Unique price record ID |
| `Crypto_ID` | INTEGER (FK) | References Crypto table |
| `Date` | TEXT | Trading date |
| `High / Low / Open / Close` | REAL | Daily OHLC prices |
| `Volume` | REAL | Trading volume |
| `Marketcap` | REAL | Market capitalization |

---

## EDA Highlights

- **Volume** is heavily right-skewed — 2,664 of 2,991 records fall in the lowest volume bin, necessitating log transformation
- **High, Low, Open, Close, and Marketcap** are near-perfectly correlated (expected for OHLCV data) — confirmed via heatmap
- **No missing values** across any column
- Stratified split by volume bins was attempted but skipped due to bins with fewer than 2 samples

---

## Model & Experiment Tracking

- **Model:** Logistic Regression with `liblinear` solver
- **Tuning:** GridSearchCV over `C` and `penalty` with 10-fold Stratified CV optimizing F1-Score
- **Tracking:** All runs logged to **MLflow on DagsHub** — metrics (F1, TP, TN, FP, FN), best hyperparameters, and model artifact

---

## Repository Structure

```
Bitcoin-Python-Project/
├── 2ndtry.ipynb          # Main notebook — normalization, EDA, ML pipeline
├── _images/              # Generated plot outputs
├── reports/              # Project reports
└── index.html            # JupyterBook landing page (deployed on Vercel)
```

---

## Tech Stack

![Python](https://img.shields.io/badge/Python-0ea5e9?style=flat-square&logo=python&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-f97316?style=flat-square&logo=scikitlearn&logoColor=white)
![MLflow](https://img.shields.io/badge/MLflow-0194E2?style=flat-square&logo=mlflow&logoColor=white)
![DagsHub](https://img.shields.io/badge/DagsHub-000000?style=flat-square&logoColor=white)
![ydata-profiling](https://img.shields.io/badge/ydata--profiling-7c3aed?style=flat-square&logoColor=white)
![JupyterBook](https://img.shields.io/badge/JupyterBook-F37626?style=flat-square&logo=jupyter&logoColor=white)

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:232526,100:F7931A&height=80&section=footer" width="100%"/>

</div>
