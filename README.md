# HCL_Hackthon_NITW
# 📊 Credit Risk Scoring System

## 🧠 Project Overview

This project implements an **end-to-end Credit Risk Scoring System** using machine learning to predict the likelihood of a loan applicant defaulting. The solution is designed to help financial institutions make **data-driven, transparent, and fair lending decisions**.

The project covers the complete pipeline from **data ingestion and cleaning**, through **exploratory data analysis (EDA)** and **feature engineering**, to **model training, validation, explainability**, and finally **business-facing visualization using Power BI**.

---

## 🎯 Business Objective

The primary goals of this project are:

* Identify **high-risk loan applicants early** to reduce defaults
* Increase approval rates for **creditworthy customers**
* Provide **explainable and interpretable** risk scores
* Enable **portfolio risk monitoring** using dashboards
* Ensure **fairness and bias checks** across demographic groups

---

## 🗂️ Dataset Description

After merging customer, loan, and bureau-level data, the final dataset contains **42,003 records** with the following columns:

### Key Column Groups

**Identifiers**

* `cust_id`
* `loan_id`

**Customer Demographics**

* `age`
* `gender`
* `marital_status`
* `employment_status`
* `income`
* `number_of_dependants`
* `residence_type`
* `years_at_current_address`
* `city`, `state`, `zipcode`

**Loan Information**

* `loan_purpose`
* `loan_type`
* `sanction_amount`
* `loan_amount`
* `processing_fee`
* `gst`
* `net_disbursement`
* `loan_tenure_months`
* `principal_outstanding`
* `disbursal_date`
* `installment_start_dt`

**Credit & Behavioral Features**

* `bank_balance_at_application`
* `number_of_open_accounts`
* `number_of_closed_accounts`
* `total_loan_months`
* `delinquent_months`
* `total_dpd`
* `enquiry_count`
* `credit_utilization_ratio`

**Target Variable**

* `default` (Boolean: 1 = Default, 0 = Non-default)

---

## 🔁 Project Workflow

```
Data Ingestion
   ↓
Data Cleaning & Validation
   ↓
Exploratory Data Analysis (EDA)
   ↓
Feature Engineering
   ↓
Encoding & Feature Selection
   ↓
Model Training
   ↓
Model Evaluation (AUC, KS)
   ↓
Explainability & Fairness Checks
   ↓
Export Predictions
   ↓
Power BI Dashboard
```

---

## 🧹 Data Cleaning & Validation

* Verified data types and schema
* Confirmed **no missing values** in raw data
* Removed duplicate records
* Converted date columns to `datetime`
* Handled invalid and negative date differences
* Addressed NaNs introduced during feature engineering using **safe division** and imputation

---

## 📊 Exploratory Data Analysis (EDA)

EDA was performed **systematically using loops** to ensure full coverage:

### Univariate Analysis

* Distribution of all numerical features (histograms)
* Frequency distribution of all categorical features

### Bivariate Analysis

* Numerical features vs `default` using boxplots
* Categorical features vs `default` using stacked bar charts

### Key Insights

* Higher `total_dpd`, `delinquent_months`, and `credit_utilization_ratio` are strongly associated with default
* Lower `bank_balance_at_application` correlates with higher risk
* Certain loan purposes and employment categories show higher default rates

---

## 🏗️ Feature Engineering

Domain-driven features were created to capture financial stress and credit behavior:

* `loan_to_income_ratio`
* `balance_to_loan_ratio`
* `avg_dpd`
* `has_delinquency`
* `high_enquiry_flag`
* `long_credit_history`
* `days_to_first_installment`

All division-based features were created using **safe division** to avoid NaNs and infinities.

---

## 🔢 Feature Encoding & Selection

* One-hot encoding applied to all categorical variables
* Dropped identifier columns (`cust_id`, `loan_id`) to prevent leakage
* Final feature matrix contained only numerical, model-ready variables

---

## 🤖 Model Training

### Baseline Model

* **Logistic Regression** (with standard scaling)
* Used for interpretability and benchmarking

### Final Model

* **LightGBM Classifier**
* Selected due to:

  * Strong performance on tabular data
  * Ability to handle non-linear relationships
  * Native handling of missing values

---

## 📈 Model Evaluation

Models were evaluated on a stratified 80/20 train-test split using:

* **AUC-ROC**: Measures overall discriminatory power
* **KS Statistic**: Measures maximum separation between defaulters and non-defaulters
* **Confusion Matrix**: Evaluated classification trade-offs

> KS values above 0.30 and AUC values above 0.80 indicate strong credit risk models.

---

## 🔍 Explainability

* Feature importance extracted from LightGBM
* Top risk drivers included:

  * `total_dpd`
  * `delinquent_months`
  * `credit_utilization_ratio`
  * `loan_to_income_ratio`

This ensures **transparent and regulator-friendly** decision-making.

---

## ⚖️ Fairness & Bias Check

* Average predicted risk was compared across **gender groups**
* No disproportionate risk assignment observed
* Confirms fairness and ethical compliance

---

## 📤 Output Artifacts

* `credit_risk_model.pkl` → Trained LightGBM model
* `credit_risk_predictions.csv` → Predictions with risk scores

These outputs are directly used in **Power BI dashboards**.

---

## 📊 Power BI Dashboard

The Power BI dashboard includes:

* Portfolio KPIs (Total customers, Avg risk, Default rate)
* Risk score distribution histogram
* Risk bucket segmentation
* Model performance metrics (AUC, KS)
* Feature insights
* Fairness visualization (Risk by gender)

---

## 🛠️ Tech Stack

* **Language**: Python
* **Libraries**: Pandas, NumPy, Seaborn, Matplotlib, Scikit-learn, LightGBM
* **Visualization**: Power BI
* **Model Persistence**: Joblib

---

## 📌 Conclusion

This project demonstrates a **production-style credit risk modeling pipeline**, combining robust data processing, strong predictive modeling, explainability, and business-focused visualization. The solution is scalable, interpretable, and suitable for real-world financial risk assessment.

---

## 👤 Author

Developed as part of a **Data Science Hackathon** to demonstrate end-to-end machine learning, risk analytics, and business intelligence skills.
