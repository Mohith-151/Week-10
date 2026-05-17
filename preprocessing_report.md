# Data Preprocessing Report: Customer Churn Prediction Pipeline

## 1. Project Overview
[cite_start]The objective of this project is to build a robust, reproducible, and automated data preprocessing pipeline to prepare customer profile records for predicting subscription attrition (churn)[cite: 2, 53]. [cite_start]The underlying dataset tracks historical metrics, billing baselines, and contract segments across unique user accounts[cite: 5, 6].

## 2. Initial Data Audit & Exploration
[cite_start]An initial diagnostic audit was conducted to analyze data types, structural integrity, and target variable distributions[cite: 6].
* [cite_start]**Dataset Shape:** 500 rows, 9 initial columns[cite: 8, 9].
* [cite_start]**Missing Values:** 0 missing entries detected across all features, as confirmed by a complete 500 non-null count across all dimensions[cite: 8, 15, 19, 23, 27, 31, 35, 38, 42, 46].
* **Target Distribution Audit:**
  * [cite_start]**Class 0 (Retained Customers):** 447 accounts [cite: 67]
  * [cite_start]**Class 1 (Churned Customers):** 53 accounts [cite: 69]
* [cite_start]**Percentage Split:** 89.4% Retained vs. 10.6% Churned [cite: 74, 76]

### Target Imbalance Warning
[cite_start]The target baseline reveals a major class imbalance of 89.4% to 10.6%[cite: 74, 76]. [cite_start]This structural variance poses a high risk where default machine learning classifiers might over-optimize for accuracy by blindly predicting the majority class[cite: 347]. [cite_start]To mitigate this risk, utilizing stratified data splits and specialized minority-class metric optimizations (such as F1-Score or Precision-Recall curves) is highly recommended for subsequent modeling cycles[cite: 347].

---

## 3. Outlier Analysis & Diagnostics
[cite_start]Numerical distributions for `TotalCharges` were subjected to outlier verification tests utilizing non-parametric statistical filters and standard distribution metrics[cite: 193, 213, 216].

### Interquartile Range (IQR) Method
[cite_start]Using a standard scaling factor of 1.5 across the calculated data parameters[cite: 202, 203]:
* [cite_start]**IQR Lower Bound:** 3807.0 [cite: 209]
* [cite_start]**IQR Upper Bound:** 12311.0 [cite: 210]
* [cite_start]**Outliers Detected via IQR:** 0 [cite: 211]

### Z-Score Method
[cite_start]Using a threshold of absolute Z-score greater than 3[cite: 218]:
* [cite_start]**Outliers Detected via Z-Score:** 0 [cite: 221]

### Diagnostic Conclusion
[cite_start]Both outlier detection methodologies returned exactly 0 extreme anomalies within the user billing structures[cite: 211, 221]. [cite_start]The calculated data indicates a structurally consistent dataset that requires no row deletion or clipping filters, preserving all 53 critical churn examples intact for training operations[cite: 69, 211, 221].

---

## 4. End-to-End Pipeline Architecture
[cite_start]To eliminate data leakage risks and ensure validation authenticity, all preprocessing transformations have been unified into a custom Scikit-Learn class (`CustomerChurnPipeline`)[cite: 441, 444, 453]. 

[cite_start]The transformation pipeline programmatically executes the following sequences upon receiving raw data strings[cite: 459, 466]:
1. [cite_start]**Feature Engineering Engine:** Derives interaction matrices and structural lifecycles natively[cite: 466, 473].
2. [cite_start]**Feature Subset Pruning:** Drops non-numeric columns and cuts the training matrix down to the top 8 features selected via Random Forest feature importance rankings[cite: 258, 435, 446].
3. [cite_start]**Feature Scaling Application:** Applies `StandardScaler` coefficients to normalize the remaining numerical elements for stable algorithmic processing[cite: 442, 451, 464].

### Final Output Validation
* [cite_start]**Pipeline Status:** Pipeline Successfully Executed! [cite: 501]
* [cite_start]**Processed Shape:** 500 Rows, 8 Features[cite: 502].