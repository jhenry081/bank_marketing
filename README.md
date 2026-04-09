# Bank Marketing Analysis

Predicting whether a customer will subscribe to a term deposit using data from a Portuguese bank's direct marketing campaigns.

---

## Overview

This project analyses data from phone-based marketing campaigns run by a Portuguese banking institution. Each record represents one customer contact. The goal is twofold:

1. **Explore** patterns in the data to understand which customers are most likely to subscribe.
2. **Predict** the probability of subscription using machine learning models.

The analysis covers exploratory data analysis, data preprocessing, model training, and feature importance evaluation.

---

## Dataset

**Source:** [UCI Machine Learning Repository - Bank Marketing Dataset](https://archive.ics.uci.edu/ml/datasets/bank+marketing)

**File:** `bank-marketing.csv` (semicolon-delimited)

**Size:** 41,188 records, 21 columns

**Target variable:** `y` - whether the client subscribed to a term deposit (yes/no)

### Key columns

| Column | Description |
|---|---|
| `age` | Age of the customer |
| `job` | Type of employment |
| `marital` | Marital status |
| `education` | Highest level of education |
| `contact` | Contact communication type (cellular / telephone) |
| `month` | Last contact month |
| `campaign` | Number of contacts during this campaign |
| `pdays` | Days since last contact from a previous campaign (999 = never) |
| `poutcome` | Outcome of the previous campaign |
| `euribor3m` | Euribor 3-month rate |
| `nr.employed` | Number of employees (quarterly indicator) |
| `y` | Target: did the client subscribe? |

> **Note:** The `duration` column (call duration) is dropped during preprocessing to prevent data leakage, as it is only known after the call ends.

---

## Project Structure

```
bank_marketing/
├── bank-marketing.csv # dataset
├── notebook.ipynb # analysis 
└── README.md
```

---

## Analysis Outline

### 1. Exploratory Data Analysis

- **Jobs:** Students and retirees show the highest subscription rates.
- **Month:** May has the highest raw volume; March, September, October, and December have stronger conversion rates relative to campaign volume.
- **Contact frequency:** Subscription rates drop sharply after 2-3 contacts per campaign. Repeated calling is counterproductive.

### 2. Data Preprocessing

- Target variable `y` encoded as binary (1 = yes, 0 = no)
- `duration` dropped to prevent data leakage
- `pdays` converted to a binary `previously_contacted` flag
- Categorical variables one-hot encoded
- 80/20 stratified train/test split
- Features scaled with `StandardScaler` for Logistic Regression

### 3. Modelling

Two classifiers trained with `class_weight='balanced'` to handle class imbalance (~11% subscription rate):

| Model | Description |
|---|---|
| Logistic Regression | Linear baseline model |
| Random Forest | Non-linear ensemble |

### 4. Evaluation

Models evaluated on: Precision, Recall, F1-score, and ROC-AUC.

### 5. Feature Importance

Top predictors identified from the Random Forest model include:
- `euribor3m` (Euribor 3-month rate)
- `nr.employed` (number of employees)
- `cons.conf.idx` (consumer confidence index)
- `poutcome_success` (previous campaign outcome)

---

## Key Findings and Recommendations

1. **Target students and retirees** - these groups have the highest subscription rates.
2. **Run campaigns in March, September-October, and December** - these months show better conversion.
3. **Cap contacts at 2 per customer** - further calls show diminishing returns.
4. **Prioritise customers with a successful prior campaign history** - this is a strong predictor of subscription.
5. **Watch macroeconomic indicators** - lower euribor rates and higher consumer confidence align with higher subscription likelihood.

---
