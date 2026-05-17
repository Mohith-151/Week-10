# Feature Engineering & Selection Documentation

## 1. Categorical Encoding Strategy
[cite_start]To satisfy structural modeling constraints, three separate encoding methods were deployed to transform text attributes into numeric spaces[cite: 97, 98, 122, 154]:

* [cite_start]**PaperlessBilling (Label Encoding):** Processed via `LabelEncoder` to convert binary text components ("Yes"/"No") into clean numeric identities (1/0) without scaling dimensions[cite: 98, 100, 111].
* [cite_start]**Contract (Ordinal Encoding):** Processed via `OrdinalEncoder` using the explicit categorical hierarchy: `Month-to-month` (170 accounts), `One year` (186 accounts), and `Two year` (144 accounts)[cite: 116, 118, 120, 122, 123]. [cite_start]This ensures that the long-term structural value of contractual bounds is preserved[cite: 123].
* [cite_start]**PaymentMethod (One-Hot Encoding):** Nominal categories containing `Credit Card` (178 rows), `Electronic Check` (163 rows), and `Bank Transfer` (159 rows) were processed via `OneHotEncoder` with `drop='first'` to prevent multi-collinearity and circumvent the dummy variable trap[cite: 146, 148, 151, 154, 155].

---

## 2. Feature Scaling Evaluation
[cite_start]Numerical metrics within the dataset varied drastically in spatial range[cite: 176, 184]. [cite_start]Two scaling transformations were tested across `Tenure`, `MonthlyCharges`, and `TotalCharges`[cite: 170]:

1. [cite_start]**Min-Max Scaling (Normalization):** Scaled data limits rigidly between a fixed [0.00, 1.00] floor and ceiling baseline[cite: 181, 184].
2. [cite_start]**Standard Scaling (Standardization):** Transformed numerical metrics to center around a mean of 0.00 with a standard deviation of 1.00[cite: 172, 174, 176].

[cite_start]**Final Selection:** `StandardScaler` was chosen as the primary pipeline component[cite: 442, 451]. [cite_start]Since distance-based models and gradient-descent algorithms assume normally distributed variance, scaling data relative to standard deviations provides significantly more stable model training compared to bounded normalization[cite: 174, 176].

---

## 3. Engineered Features Architecture
[cite_start]Five new features were designed and extracted from the raw data parameters to capture complex interaction patterns and operational behaviors[cite: 224, 227, 231, 234, 245]:

* [cite_start]**Charges_Per_Month:** Calculated as `TotalCharges / (Tenure + 1)`[cite: 226]. [cite_start]Tracks historical spending velocity over time to flag accounts facing rapid cost increases[cite: 224, 225].
* [cite_start]**Tenure_Mismatch:** Calculated as `Calculated_Tenure - Tenure` (where `Calculated_Tenure = TotalCharges / (MonthlyCharges + 0.001)`)[cite: 229, 230]. [cite_start]Exposes anomalies between recorded account durations and financial contributions, which often point to administrative issues or mid-contract plan modifications[cite: 227, 228].
* [cite_start]**Is_High_Spender:** A binary indicator mapping whether a user's current `MonthlyCharges` sits above the global sample average[cite: 232, 233]. [cite_start]It identifies premium accounts that carry a high financial commitment[cite: 231, 232].
* [cite_start]**Tenure_Group_Encoded:** Segmented profiling created via `pd.cut` with logical bins bounding account longevity into `Short-Term`, `Medium-Term`, and `Long-Term` lifecycles[cite: 235, 238, 239]. [cite_start]This maps the non-linear relationship between customer onboarding cycles and churn vulnerability[cite: 234, 235].
* [cite_start]**Senior_Financial_Weight:** An interaction feature computed as `SeniorCitizen * MonthlyCharges`[cite: 247]. It highlights demographic-financial cost sensitivities by focusing on fixed-income older customer profiles carrying heavy bills[cite: 245, 246].

---

## 4. Tree-Based Feature Selection
To preserve predictive variance while trimming non-informative noise, features were evaluated via a baseline **Random Forest Classifier** configured with stratified class weights to balance the 89/11 target distribution[cite: 347, 349]. 

### Feature Importance Rankings
The complete list of all evaluated features ranked by their extraction importance scores[cite: 369, 370]:

| Feature Name | Importance Score | Status ($\ge 0.02$) |
| :--- | :--- | :--- |
| `Tenure` | 0.296499 | **Selected** [cite: 398] |
| `Tenure_Group_Encoded` | 0.280534 | **Selected** [cite: 400] |
| `Charges_Per_Month` | 0.177599 | **Selected** [cite: 402] |
| `Tenure_Mismatch` | 0.070428 | **Selected** [cite: 404] |
| `en_contract` | 0.039500 | **Selected** [cite: 405] |
| `Calculated Tenure` | 0.038365 | **Selected** [cite: 408] |
| `MonthlyCharges` | 0.030742 | **Selected** [cite: 410] |
| `TotalCharges_Z` | 0.022723 | **Selected** [cite: 412] |
| `TotalCharges` | 0.019834 | Dropped [cite: 414] |
| `Is_High_Spender` | 0.007937 | Dropped [cite: 416] |
| `Senior_Financial_Weight` | 0.006437 | Dropped [cite: 418] |
| `en_PaperlessBilling` | 0.003388 | Dropped [cite: 420] |
| `Payment Method_Credit Card` | 0.002507 | Dropped [cite: 421, 422] |
| `Payment Method_Electronic Check` | 0.002115 | Dropped [cite: 423, 424] |
| `SeniorCitizen` | 0.001392 | Dropped [cite: 425, 426] |

### Selection Summary
Applying a selective algorithmic threshold of $\ge 0.02$ narrowed the input scope down to exactly 8 top-performing features[cite: 428, 429, 436]. These 8 features were embedded into the final production deployment pipeline[cite: 437, 438, 446].