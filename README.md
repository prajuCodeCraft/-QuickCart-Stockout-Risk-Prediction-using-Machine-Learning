# 📦 QuickCart: Inventory Stockout Risk Prediction

> 🚨 **Can Machine Learning predict a stockout before it happens?**

QuickCart Stockout Risk Prediction is an end-to-end **Machine Learning project** designed to predict inventory stockout risk at the **SKU × Store × Day** level.

The system classifies each inventory record into three risk categories:

🟢 **Safe**  
🟡 **At-Risk**  
🔴 **Imminent**

The project covers the complete Machine Learning workflow — from data cleaning and exploratory analysis to feature engineering, model training, evaluation, and business insights.



## 🎯 Project Objective

Inventory stockouts can lead to:

- Lost sales 💰
- Poor customer experience 😕
- Emergency replenishment 🚚
- Inefficient inventory planning 📦

The objective of this project is to identify inventory situations that are likely to experience a stockout and understand the factors contributing to stockout risk.


## 📊 Dataset Overview

The project uses **5 interconnected datasets**:

| Dataset | Records |
|---|---:|
| `dim_stores.csv` | 12 |
| `dim_skus.csv` | 60 |
| `dim_suppliers.csv` | 15 |
| `dim_events.csv` | 30 |
| `fact_inventory_daily.csv` | 21,600 |

After joining the datasets:

**Master Dataset → 21,600 rows × 39 columns**


## 🗂️ Dataset Structure

### 🏪 Stores

Contains information about stores and their locations.

### 📦 SKUs

Contains product-level information such as:

- Category
- Supplier
- Price
- Shelf life
- Perishability

### 🚚 Suppliers

Contains supplier-related information including:

- Supplier identity
- Reliability score
- Lead-time information

### 🎉 Events

Contains date-based event information such as festival periods.

### 📈 Daily Inventory

Contains daily SKU-store inventory information including:

- Opening stock
- Closing stock
- Units demanded
- Units sold
- Reorder point
- Lead time
- Days of cover
- Sales velocity
- Stockout risk



# 🔍 Exploratory Data Analysis

The dataset was explored to understand patterns behind inventory stockout risk.

### EDA Areas

📊 Stockout Risk Distribution  
🎉 Festival vs Non-Festival Periods  
🚚 Supplier Reliability  
🥦 Perishable vs Non-Perishable Products  
📈 Sales Velocity  
📦 Closing Stock  
🔄 Reorder Behaviour  
⏱️ Lead Time



## 📌 Stockout Risk Distribution

The target variable contains three classes:

| Risk Category | Records |
|---|---:|
| 🟢 Safe | 14,131 |
| 🟡 At-Risk | 5,186 |
| 🔴 Imminent | 2,283 |

This shows that the dataset is **class-imbalanced**, with Safe being the majority class.

Therefore, accuracy alone was not considered sufficient for evaluating the models.



# 🧹 Data Cleaning

The following preprocessing steps were performed:

### Date Conversion

Converted date columns into proper datetime format.

### Supplier Reliability

Supplier reliability values were converted to numeric values.

Invalid values such as `"N/A"` were treated as missing values and filled using the median reliability score.

### City Standardization

City names were cleaned and standardized using:

- Whitespace removal
- Consistent title casing

### Duplicate Check

Duplicate records were checked across all datasets.

### Missing Values

Missing values were analyzed carefully.

The `lead_time_days_actual` column contained many missing values because actual lead time was primarily recorded on reorder days.

These values were therefore **not blindly removed or imputed**.



# 🔗 Data Integration

The five datasets were merged using common keys such as:

- `store_id`
- `sku_id`
- `supplier_id`
- `date`

The final analytical dataset contained:

**21,600 records and 39 columns.**



# 🛠️ Feature Engineering

Domain-specific features were created to improve the model's ability to identify stockout risk.



## 1. 📦 Reorder Gap

```text
reorder_gap = closing_stock - reorder_point
