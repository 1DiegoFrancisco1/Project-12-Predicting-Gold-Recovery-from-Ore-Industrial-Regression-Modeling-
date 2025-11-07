# 🪙 Project 12 — Predicting Gold Recovery from Ore (Industrial Regression Modeling)

### 🏭 Project Context
You work as a **data scientist** for a mining company developing models to predict **gold recovery efficiency** during the flotation and purification processes.  
The goal is to estimate two key metrics for operational optimization:

1. **Rougher recovery** — intermediate gold extraction efficiency.  
2. **Final recovery** — overall efficiency after full purification.

These predictions help maximize yield while minimizing operational waste.

---

## ⚙️ Technological Process Overview

### 🧪 Main Stages
1. **Flotation (Rougher Process)** — initial gold separation from ore.  
2. **Primary Purification** — first cleaning of the rougher concentrate.  
3. **Secondary Purification** — final cleaning stage.

### 🧮 Recovery Formula
Recovery = (C × (F - T)) / (F × (C - T)) × 100%
Where:  
- **C** → proportion of gold in the concentrate  
- **F** → proportion of gold in the feed (input)  
- **T** → proportion of gold in the tails (waste)

---

## 📏 Evaluation Metric — Symmetric Mean Absolute Percentage Error (sMAPE)

sMAPE = (1/N) × Σ(|y - ŷ| / ((|y| + |ŷ|) / 2)) × 100%

### Combined Metric
The project’s final score combines both targets:

sMAPE_total = 0.25 × sMAPE(rougher) + 0.75 × sMAPE(final)
This weighting emphasizes the **final stage**, which has the greatest business importance.

---

## 🧹 Step 1 — Data Validation and Preprocessing

- Verified dataset integrity and feature consistency between `train`, `test`, and `full`.  
- Removed rows with missing targets.  
- Applied proper **train-test split** (time-based) to avoid leakage.  
- Imputed and standardized numerical features using parameters from the training set only.  
- Aligned feature sets across all datasets to ensure consistency.

---

## 🤖 Step 2 — Model Training and Evaluation

Three models were compared via **Cross-Validation** using the **combined sMAPE** metric.

| Model | Mean sMAPE | Std. Dev. | Observations |
|--------|-------------|-----------|---------------|
| **Linear Regression** | 14.5 % | 1.82 | Struggles with nonlinear relationships — underfits. |
| **Random Forest** | 14.1 % | 1.90 | Better but with higher variance; mild overfitting in temporal folds. |
| **Gradient Boosting** | **12.1 %** | **1.09** | Best performer — stable and captures nonlinear interactions effectively. |

✅ **Selected model:** `GradientBoostingRegressor`

---

## 🧪 Step 3 — Final Model and Test Evaluation

- Retrained the Gradient Boosting model using the **entire training dataset** (with targets).  
- Predicted `rougher.output.recovery` and `final.output.recovery` for the **test dataset**.  
- Aligned predictions with real test targets from `gold_recovery_full.csv`.  
- Computed sMAPE for both recovery stages and the combined metric.

| Metric | sMAPE (%) | Weight |
|---------|------------|--------|
| Rougher Recovery | 10.63 | 0.25 |
| Final Recovery | 10.34 | 0.75 |
| **Weighted (Final)** | **10.41** | ✅ |

🧠 **Interpretation**
- Excellent alignment between **validation (~10.69%)** and **test (10.41%)** results.  
- Improvement over CV baseline (~12.14%) demonstrates **good generalization**.  
- Indicates a stable, reliable predictive pipeline for industrial use.

---

## 🧠 Insights and Conclusions

### ✅ Objective Achieved
A functional ML pipeline was built to **predict gold recovery at two process stages**, achieving a final combined **sMAPE ≈ 10.4%** — surpassing the acceptable threshold for process control.

### 🔍 Key Takeaways
- A **10% relative error** implies **moderate-to-high precision** for predicting gold recovery.  
- The **final stage** (weighted 75%) is modeled with strong reliability.  
- Consistency between validation and test shows robust handling of **covariate shift** (e.g., `feed_size`).  
- Nonlinear ensemble methods (like Gradient Boosting) outperform linear baselines in capturing process dynamics.

---

## 💡 Business and Operational Implications

- The model allows engineers to **monitor process efficiency in real time**, anticipate deviations, and fine-tune parameters.  
- Enables **data-driven optimization** of metallurgical processes.  
- Reduces experimental costs by simulating expected recovery under different conditions.  
- Provides a **reproducible pipeline** (data cleaning, standardization, cross-validation, metric computation).

---

## 🧰 Tools and Libraries
- **Python:** pandas, numpy, scikit-learn  
- **Models:** LinearRegression, RandomForestRegressor, GradientBoostingRegressor  
- **Evaluation:** sMAPE, cross-validation  
- **Visualization:** matplotlib, seaborn  

---

## 🧾 Deliverables
- Validated datasets and preprocessed features.  
- Comparative model evaluation.  
- Gradient Boosting model achieving ~10.4% combined sMAPE.  
- Full test-phase predictions aligned with real outcomes.  
- Written summary of industrial and business insights.

---

## 👨‍💻 Author
**Diego Francisco Domínguez Aguilar**  
_Data Science Bootcamp – TripleTen (2025)_  
