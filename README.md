# Credit_Risk_Databricks_Capstone
AI-Powered Credit Risk Scoring &amp; Loan Decision System using Databricks

## 1️⃣ Problem Statement & Objective
**Business Problem**

Banks must assess loan applications while minimizing default risk and ensuring business growth.
Traditional rule-based systems (fixed income thresholds, age limits) fail to capture complex interactions between customer behavior, credit history, and loan characteristics.

**Objective**

Build an AI-powered credit risk system that:
- Predicts loan default probability
- Explains risk drivers
- Converts predictions into actionable lending decisions

**AI Framing**

- ML Task: Binary Classification
- Target Variable: loan_status (0 = Non-Default, 1 = Default)
  
---

## 2️⃣ Dataset Understanding
**Dataset Overview**

The dataset contains 32,581 loan records with customer, credit, and loan attributes.

**Columns**

Customer details required for loan processing
- `person_age`
- `person_income`
- `person_emp_length`
- `Loan	loan_amnt`
- `loan_int_rate`
- `loan_percent_income`
- `Credit	cb_default_flag`
- `cb_person_cred_hist_length`
- `loan_intent`
- `loan_grade`
- `loan_status` (Target)


**Initial Observations**

- Missing values in `loan_int_rate` and `person_emp_length`
- Outliers in `person_emp_length`
- Unreasonable `person_age` values (e.g., 123, 144)

---

## 3️⃣ Data Architecture (Medallion Pattern)

![Data Architecture](https://github.com/JayaraniArunachalam/Credit_Risk_Databricks_Capstone/blob/main/Diagrams/Credit%20risk%20AI%20model%20dataarchitecture%20databricks.jpg)

**🟤Bronze**

- Raw CSV ingestion
- Schema-on-read
- Delta Lake storage

**⚪ Silver**

- Data cleaning & validation
- Outlier handling
- Business-rule-based imputations

**🟡 Gold**

- Feature engineering
- Analytics-ready dataset
- ML-ready dataset
- Prediction & decision outputs

---

## 4️⃣ Feature Engineering

**Cleaning & Rules**

- Removed unrealistic ages (>75)
- Removed employment length > 60
- Imputed missing interest rates using median by loan grade & intent

**Engineered Feature**

prior_default_high_risk_flag -- 1 if (previous default = YES) AND (loan_percent_income > 0.4) Else 0

**Why This Feature Matters**

Combined risk factors are more predictive than isolated attributes and closely mirror real-world banking risk assessment.

---

## 5️⃣ Insight Generation (Business Analytics)
**📈 Key Insights**

Debt Consolidation & Medical loans have the highest default rates

![](https://github.com/JayaraniArunachalam/Credit_Risk_Databricks_Capstone/blob/main/Diagrams/Loan%20Intent%20Vs%20Avg%20Default%20Rate.png)

High loan-to-income ratio significantly increases default probability

![](https://github.com/JayaraniArunachalam/Credit_Risk_Databricks_Capstone/blob/main/Diagrams/Avg%20Loan%20to%20Income%20Percent%20Vs%20Default%20risk.png)

---

## 6️⃣ Model Selection & Technical Reasoning

**Model Chosen**

Logistic Regression

**Why Logistic Regression?**

- Industry standard for credit scoring
- Highly interpretable
- Stable baseline
- Works well with linear risk factors

---

## 7️⃣ Training, Evaluation & Metrics
**Training Setup**

80/20 Train-Test Split

Features sourced directly from Gold Delta tables

MLflow is used for experiment tracking

**📏 Metrics Used**
- Metric	Purpose
- AUC	Model discrimination
- Accuracy: Overall correctness
- Precision	Controls false approvals
- Recall	identifies risky borrowers

![](https://github.com/JayaraniArunachalam/Credit_Risk_Databricks_Capstone/blob/main/Diagrams/metrics.jpg)

**Evaluation Results**

ROC Curve shows strong separation

![](https://github.com/JayaraniArunachalam/Credit_Risk_Databricks_Capstone/blob/main/Diagrams/ROC%20curve%20Credit%20Risk%20Dataset.png)


Confusion Matrix highlights the trade-off between growth and risk

![](https://github.com/JayaraniArunachalam/Credit_Risk_Databricks_Capstone/blob/main/Diagrams/Confusion%20Matrix.png)

---

## 8️⃣ Risk Scoring & Decision Logic
**Decision Framework**
Default Probability	Decision
< 0.30	APPROVE
0.30 – 0.60	REVIEW
≥ 0.60	REJECT

**🔗 Human-in-the-Loop**

High-risk loan intents (Medical, Debt Consolidation) are routed to manual review even when model confidence is moderate.

---

## 9️⃣ End-to-End AI Workflow

**Delta Tables → Feature Extraction → ML Model → Risk Scoring → Decisions → Stored Back to Delta**

This ensures full database ↔ AI integration.

---

## 🔟 Business Impact

Reduced false approvals

Improved risk transparency

Scalable loan decision pipeline

Supports responsibl
