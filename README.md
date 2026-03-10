# 📦 AI-Driven Demand Forecasting & Inventory Optimization System

## 🚀 Overview

This project builds an end-to-end AI-powered demand forecasting and inventory decision engine.

It combines:
- Machine learning forecasting (XGBoost)
- Forecast diagnostics & interpretability (SHAP)
- ABC–XYZ segmentation
- Safety stock modeling
- Reorder point optimization
- Scenario-based stockout simulation

> The goal is not just to predict demand — but to convert forecasts into inventory decisions.

---

## 🎯 Business Problem

Retailers often:
- Forecast demand but use static reorder rules
- Apply the same service level across all SKUs
- Fail to stress-test inventory under uncertainty

This leads to:
- Stockouts during demand spikes
- Excess working capital tied in inventory
- No differentiation between high and low importance SKUs

---

## 🧠 Solution Architecture

```
Raw Data
   ↓
Data Cleaning & Feature Engineering
   ↓
Forecasting Models (Naive, RF, XGBoost)
   ↓
Model Evaluation & SHAP Analysis
   ↓
ABC–XYZ Segmentation
   ↓
Safety Stock & ROP Calculation
   ↓
Scenario Simulation (Demand Spike, Lead Time Variability)
   ↓
Inventory Policy Recommendations
```

---

## 📊 Key Results

- 📉 **47% reduction in RMSE** vs naive baseline (XGBoost)
- 📈 Relative RMSE: High-demand SKUs ≈ 20% / Low-demand SKUs ≈ 24%
- 🔍 Temporal features (`lag_1`, `rolling_mean_7`) dominate model importance
- 📦 Segment-based service levels implemented across ABC–XYZ classes
- 🧪 Inventory policies stress-tested under demand spikes and lead time variability

---

## 🏗 Forecasting Models Compared

| Model | RMSE | Improvement vs Naive |
|---|---|---|
| **XGBoost** | 707 | **47%** |
| Random Forest | 763 | 43% |
| Naive (lag-1) | 1,333 | — |
| Linear Regression | 250,608 | Poor fit* |

> *Linear Regression result likely reflects a data scaling issue and was excluded from downstream policy decisions.

Tree-based models significantly outperformed classical approaches on this dataset.

---

## 🔎 Feature Importance (SHAP Analysis)

Top drivers of demand:
- `lag_1` — strongest predictor by a wide margin
- `rolling_mean_7` — captures weekly demand trends
- `rolling_std_7` — captures demand volatility
- `lag_7`, `lag_14` — weekly and bi-weekly autocorrelation

Retail demand shows strong short-term autocorrelation and weekly seasonality. Promotions and calendar signals were present but secondary.

SHAP beeswarm analysis was used for interpretability over standard feature importance, as it captures both magnitude and direction of each feature's impact.

---

## 📦 Inventory Optimization Logic

### 1️⃣ Demand Distribution
Forecast mean and standard deviation estimated per Store × SKU. Normal distribution assumption used for lead-time demand.

### 2️⃣ Safety Stock
```
SS = Z × σ × √(Lead Time)
```
Where:
- `Z` = service level factor
- `σ` = forecast error standard deviation

### 3️⃣ Reorder Point (ROP)
```
ROP = (Average Demand × Lead Time) + Safety Stock
```

### 4️⃣ Economic Order Quantity (EOQ)
```
EOQ = √(2DS / H)
```
Where:
- `D` = annual demand
- `S` = ordering cost
- `H` = holding cost

---

## 🧪 Scenario Simulation

Inventory policies were stress-tested under:
- Lead time variability
- +20% demand spike
- +50% demand variance

Outputs per scenario:
- Stockout probability
- Fill rate
- Cost impact comparison

---

## 📂 Project Structure

```
├── 01_data_loading_and_merging.ipynb
├── 02_feature_engineering.ipynb
├── 03_eda.ipynb
├── 04_forecasting_models.ipynb
├── 05_inventory_optimization.ipynb
│
├── data/
│   ├── segmentation.csv
│   └── inventory_policy_recommendations.csv
│
└── README.md
```

---

## 🛠 Tech Stack

| Category | Libraries |
|---|---|
| Data processing | Pandas, NumPy |
| Machine learning | Scikit-learn, XGBoost |
| Interpretability | SHAP |
| Statistics | SciPy |
| Visualisation | Matplotlib, Seaborn |
| Environment | Python 3.x, Jupyter Notebook |

---

## 💡 Why This Project Is Different

This is not just a forecasting notebook.

It connects:

```
Forecast Accuracy → Demand Variability → Service Level → Safety Stock → Reorder Decisions → Scenario Risk
```

Most projects stop at model accuracy. This one uses the forecast as an input to a full inventory policy — segmented by SKU importance, calibrated for uncertainty, and validated under stress conditions.

---

## 🔮 Future Improvements

- Multi-echelon inventory modeling
- Dynamic service level optimization
- Real-time deployment via Streamlit
- Bayesian demand modeling
- Cost-based optimal service level tuning

---

## 📬 Author

**Vaishnavi Kulkarni**

If you'd like to discuss supply chain analytics, demand forecasting, or inventory optimization — feel free to connect on LinkedIn.
