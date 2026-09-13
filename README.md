# QuickCart Inventory Stockout Risk Prediction 🛒📉

## Overview
This project tackles a genuine operational problem for **QuickCart**, a quick-commerce dark-store operator. The goal is to predict the daily inventory stockout risk for 60 SKUs across 12 stores before the next supplier delivery arrives. 

Unlike standard toy datasets, this project involves multi-table relational complexity, natural class imbalances, and real-world supply chain variables like rolling sales velocity, supplier lead-time uncertainty, and festival demand spikes. 

The target variable is classified into three risk tiers:
* **Safe (65.4%)**: Days of stock cover exceeds the wait for replenishment by 3+ days.
* **At-Risk (24.0%)**: Days of cover is within 3 days of the replenishment wait.
* **Imminent (10.6%)**: High-cost scenario where stock will run out before the next delivery.

## 🛠️ Key Challenges & Methodologies Addressed

### 1. Data Integrity & Wrangling
* **Multi-Table Joins**: Merged a 21,600-row daily fact table with 4 dimension tables (Stores, SKUs, Suppliers, Events).
* **Production "Gotchas"**: Handled silent Pandas parsing errors (e.g., literal `'N/A'` strings) and standardized inconsistent string casing in categorical join keys.

### 2. Feature Engineering
Engineered operational signals that inventory planning teams use daily:
* **Inventory Buffers**: Calculated `reorder_gap` and normalized `days_of_cover_ratio`.
* **Rolling Metrics**: Applied groupby-shift operations to track recent reorder placements (`is_recent_reorder`).
* **Supplier Reliability**: Imputed missing supplier reliability scores using category medians.
* **Temporal Effects**: Created calendar features to map demand spikes leading up to major holidays (e.g., Diwali Week).

### 3. Validation Strategy
* **Time-Based Splitting**: Avoided random row-level splits which cause data leakage in daily panel data. Instead, implemented a strict chronological split: training on early October and testing on the late-October generalization challenge (which includes the back-half of a festival spike).

### 4. Modeling & Evaluation
* **Algorithms Tested**: Baseline Majority-Class Classifier, Multinomial Logistic Regression, and Tree Ensembles (Random Forest / Gradient Boosting).
* **Asymmetric Cost Optimization**: Focused heavily on **Recall for the 'Imminent' class**. In retail operations, missing a real stockout (false negative) is far more costly than a false alarm (false positive). 

## 📁 Repository Structure
* `Notebooks/`: Contains the sequential Jupyter Notebooks for data merging, feature engineering, and model training.
* `Data/`: Raw schema files including `fact_inventory_daily.csv` and the 4 `dim_*.csv` dimension tables.
* `README.md`: Project documentation.

## 🎓 Acknowledgments
Developed as part of the Artificial Intelligence technical program track at **The Unlox Academy**.
