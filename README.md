# Customer Churn Prediction: Data Preprocessing Pipeline

## 🎯 Project Overview
This repository contains a robust, end-to-end data preprocessing and feature engineering pipeline designed to prepare customer profile records for churn prediction. Operating on a dataset of 500 customers, the pipeline automates handling categorical data, scaling numerical components, and extracting behavioral interaction features while strictly preventing data leakage.

## 📁 Repository Structure
* `churn_prediction_pipeline.ipynb` - Core Jupyter Notebook containing data auditing, visualizations, feature selection, and pipeline testing.
* `customer_churn.csv` - The raw source dataset (500 rows, 9 columns).
* `requirements.txt` - Python environment package dependencies.
* [Preprocessing Report](preprocessing_report.md) - Detailed analysis of class distributions, target imbalance configurations, and outlier diagnostics.
* [Feature Engineering & Selection Documentation](feature_engineering_documentation.md) - Complete documentation of the categorical encoding strategies, custom feature math, and Random Forest feature importance rankings.

## 🛠️ Setup Instructions
To set up the environment and run the preprocessing pipeline locally, execute the following commands in your terminal:

```bash
# Clone the repository directly from GitHub
git clone [https://github.com/Mohith-151/Week-10.git](https://github.com/Mohith-151/Week-10.git)
cd Week-10

# Install required dependencies
pip install -r requirements.txt
```
## 🚀 Pipeline Performance Summary
The custom `CustomerChurnPipeline` class unifies all transformations and narrows the data scope down to the **top 8 features** based on tree-based importance rankings using a strict significance selection threshold of >= 0.02.

* **Input Raw Shape:** (500, 9)
* **Pipeline Output Shape:** (500, 8)

### 📋 Selected Features Ready for Machine Learning Models:
1. **Tenure** (Lifecycle Duration)
2. **Tenure_Group_Encoded** (Loyalty Segment)
3. **Charges_Per_Month** (Spending Velocity Tracking)
4. **Tenure_Mismatch** (Billing Volatility Indicator)
5. **en_contract** (Contractual Tier)
6. **Calculated_Tenure** (Derived Historical Footprint)
7. **MonthlyCharges** (Current Cost Commitment)
8. **TotalCharges_Z** (Standardized Cumulative Value)