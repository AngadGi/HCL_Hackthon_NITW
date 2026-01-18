# 📊 Credit Risk Scoring System

## 🧠 Project Overview

This project implements a **full end-to-end Credit Risk Scoring System** aligned exactly with the implemented notebook/code. It covers **data ingestion from multiple sources, synthetic transaction enrichment, rigorous EDA, feature engineering, statistical feature selection (VIF, WOE/IV), multiple model families, imbalance handling, hyper-parameter optimization (RandomizedSearch & Optuna), and business-grade evaluation (ROC-AUC, KS, Gini)**, followed by Power BI consumption.

---

## 🎯 Business Objective

* Predict probability of **loan default** at customer–loan level
* Reduce credit losses while maintaining approval quality
* Build a **regulator-friendly, explainable** scorecard-style solution
* Produce outputs ready for **Power BI dashboards**

Target variable:

* `default` → 1 = defaulted, 0 = non-default

---

## 🗂️ Data Sources (Exact to Code)

### 1️⃣ Customers (`customers.csv`)

Customer demographics and residence details

### 2️⃣ Loans (`loans.csv`)

Loan characteristics, amounts, tenure, dates

### 3️⃣ Bureau (`bureau_data.csv`)

Credit behavior, delinquency, utilization

### 4️⃣ Synthetic Transactions (Generated in Code)

Created to enrich behavioral risk signals:

* `monthly_inflow`
* `monthly_spend`
* `avg_monthly_balance`
* `past_loan_count`
* `past_defaults`
* `loan_performance_rating`
* `credit_score`

Synthetic data generation is **controlled using a fixed random seed** for reproducibility.

---

## 🔁 End-to-End Workflow

```
Load Raw Data (CSV)
   ↓
Generate Synthetic Transaction Data
   ↓
Merge All Sources on cust_id
   ↓
Data Cleaning & Validation
   ↓
Outlier Detection & Treatment
   ↓
EDA (Automated, Loop-based)
   ↓
Feature Engineering
   ↓
Feature Selection (VIF + IV)
   ↓
Encoding & Scaling
   ↓
Train / Test Split
   ↓
Model Training (LR, RF, XGB, LGBM)
   ↓
Hyperparameter Optimization
   ↓
Imbalance Handling (Under / Over Sampling)
   ↓
Final Model Selection
   ↓
Evaluation (ROC, KS, Gini)
   ↓
Power BI Dashboard
```

---

## 🧹 Data Cleaning & Validation

Steps performed:

* Converted `default` to integer
* Checked missing values using `% null`
* Imputed `residence_type` with **mode**
* Verified **no duplicate rows**
* Identified continuous vs categorical features

### Outlier Treatment

* Visualized all numerical features using **loop-based boxplots**
* Business rule applied:

  * Removed records where `processing_fee > 3% of loan_amount`

---

## 📊 Exploratory Data Analysis (EDA)

EDA was performed **systematically using loops**, not cherry-picked columns.

### Continuous Variables

* KDE plots for **every numeric feature**
* Compared distributions for `default = 0` vs `default = 1`

### Key Observations

* Defaulters show:

  * Higher `total_dpd` and `delinquent_months`
  * Higher `credit_utilization_ratio`
  * Higher `loan_to_income` ratios
  * Lower balances and unstable inflow-spend patterns

---

## 🏗️ Feature Engineering (As Implemented)

| Feature                   | Business Meaning             |
| ------------------------- | ---------------------------- |
| `loan_to_income`          | Affordability risk           |
| `delinquency_ratio`       | Severity of past delinquency |
| `avg_dpd_per_delinquency` | Depth of default behavior    |
| `monthly_spent_r`         | Cash-flow stress             |

All ratios were created carefully to avoid divide-by-zero issues.

---

## 🔢 Feature Selection

### 1️⃣ Multicollinearity (VIF)

* Applied **MinMax scaling** to numeric features
* Removed high-VIF variables such as:

  * `credit_score`, `monthly_inflow`, `avg_monthly_balance`, etc.

### 2️⃣ Predictive Power (WOE & IV)

* IV calculated for **every feature**
* Numerical features were **binned** before IV calculation
* Features retained only if:

  * `IV > 0.02`

This ensures **stable and interpretable credit models**.

---

## 🔄 Encoding & Scaling

* One-hot encoding applied using `pd.get_dummies(drop_first=True)`
* Final dataset contains only **model-ready numeric features**

---

## 🤖 Model Training

Models trained and compared:

* Logistic Regression (baseline & scorecard-friendly)
* Random Forest
* XGBoost
* LightGBM

Train/Test split: **80 / 20 (stratified)**

---

## ⚖️ Class Imbalance Handling

Since default events are minority:

### Techniques Used

* Random Under Sampling
* SMOTE-Tomek (Hybrid over + clean)

Models were retrained on balanced data and evaluated on original test set.

---

## 🔍 Hyperparameter Optimization

### RandomizedSearchCV

* Logistic Regression
* XGBoost
* LightGBM

### Optuna

* Logistic Regression optimized using:

  * F1-macro score
  * Cross-validation

Final Logistic model selected based on **stability + interpretability**.

---

## 📈 Model Evaluation Metrics

| Metric             | Purpose                            |
| ------------------ | ---------------------------------- |
| Accuracy           | Overall correctness                |
| Precision / Recall | Risk trade-off                     |
| ROC-AUC            | Rank ordering ability              |
| KS Statistic       | Credit separation power            |
| Gini               | Industry standard scorecard metric |

### Final Performance

* Strong ROC-AUC
* High KS separation across deciles
* Stable coefficients with economic intuition

---

## 📊 Decile & KS Analysis

* Customers segmented into **10 risk deciles**
* Computed:

  * Event / Non-event rates
  * Cumulative distributions
  * KS statistic

This validates **business usability of the score**.

---

## 🔎 Explainability

* Logistic regression coefficients extracted
* Positive coefficients → higher default risk
* Feature importance visualized using bar plots

Ensures **audit-ready and regulator-compliant** modeling.

---

## 📤 Outputs for Power BI

Exported artifacts:

* Customer-level default probability
* Decile assignment
* KS & Gini statistics

### Power BI Dashboard Includes

* Portfolio KPIs
* Risk distribution
* Decile-wise default rates
* Feature impact summary
* Model performance visuals

---

## 🛠️ Tech Stack

* Python
* Pandas, NumPy
* Matplotlib, Seaborn
* Scikit-learn
* XGBoost, LightGBM
* Optuna
* Power BI

---

## 📌 Conclusion

This project demonstrates a **production-grade credit risk modeling pipeline**, following industry best practices used in banks and NBFCs. It balances **predictive performance, interpretability, and business relevance**, making it suitable for real-world deployment.

---

## 👤 Author

Developed as part of a **Data Science Hackathon** showcasing end-to-end credit risk analytics and machine learning expertise.
