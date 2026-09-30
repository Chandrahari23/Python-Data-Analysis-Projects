# Customer Dataset Cleaning & Analysis using Python

## 📌 Project Overview

This project focuses on **cleaning, transforming, and analyzing a messy customer sales dataset using Python and Pandas**.

The dataset contains customer, product, sales, payment, salesperson, location, date, and customer-rating information. The project demonstrates a practical data-cleaning workflow starting from a raw dataset with missing values, inconsistent text formats, mixed date formats, and duplicate records, and produces a cleaner dataset ready for further analysis.

---

## 🎯 Project Objectives

- Inspect the structure and quality of the raw dataset
- Identify and handle missing values
- Detect and remove duplicate records
- Standardize text values such as customer names, cities, and payment modes
- Handle inconsistent city names
- Convert the order-date column into a usable date format
- Create a calculated `Total_Price` column
- Perform basic sales analysis using Pandas
- Export the cleaned dataset as a CSV file

---

## 🗂️ Dataset

### Input Dataset

`customer_sales_messy.csv`

The raw dataset contains **53 rows and 11 columns**.

### Columns

| Column | Description |
|---|---|
| `Order_ID` | Unique order identifier |
| `Customer_Name` | Name of the customer |
| `City` | Customer/order city |
| `Product` | Purchased product |
| `Category` | Product category |
| `Quantity` | Number of units purchased |
| `Price` | Price per unit |
| `Order_Date` | Date of the order |
| `Payment_Mode` | Payment method used |
| `Salesperson` | Salesperson associated with the order |
| `Customer_Rating` | Customer rating |

---

## 🛠️ Tools & Technologies

- **Python**
- **Pandas**
- **Google Colab / Jupyter Notebook**
- **CSV**

---

## 🔄 Data Cleaning Process

### 1. Load the Dataset

The raw CSV file was loaded using Pandas:

```python
import pandas as pd

c = pd.read_csv("customer_sales_messy.csv")
```

### 2. Initial Data Inspection

The dataset was inspected using:

- `isnull().sum()`
- `isnull().sum().sum()`
- `info()`
- `shape`
- `describe()`
- `columns`

The initial dataset contained **53 records and 11 columns**.

### 3. Missing-Value Analysis

Missing values were identified across the dataset.

The initial missing-value count was:

- `Customer_Name` – 1
- `City` – 2
- `Category` – 1
- `Price` – 2
- `Order_Date` – 1
- `Payment_Mode` – 1
- `Customer_Rating` – 2

**Total initial missing values: 10**

Missing-value percentages were also calculated to understand the extent of missing data in each column.

### 4. Missing-Value Treatment

Different approaches were used depending on the column:

- Missing `Customer_Name` values were replaced with `"Unknown"`.
- Missing `Price` values were filled using the minimum observed price.
- Rows with remaining missing values were removed using `dropna()` during the cleaning process.
- For `Order_Date`, values were converted using `pd.to_datetime(..., errors="coerce")`, and missing/unparseable dates were subsequently filled with `2026-01-01`.

> Note: The date column in the source data contained multiple date formats. The notebook converted the column to datetime and then extracted the month number for analysis.

### 5. Text Standardization

Text fields were standardized to improve consistency.

Examples include:

- Removing leading/trailing spaces
- Converting customer names to title case
- Converting cities to title case
- Converting payment modes to title case
- Standardizing `Bengaluru` to `Bangalore`

Example:

```python
c["Customer_Name"] = (
    c["Customer_Name"].str.strip().str.title()
)

c["City"] = (
    c["City"].str.strip().str.title()
)

c["Payment_Mode"] = (
    c["Payment_Mode"].str.strip().str.title()
)
```

### 6. Date Transformation

The `Order_Date` column was converted to datetime:

```python
c["Order_Date"] = pd.to_datetime(
    c["Order_Date"],
    errors="coerce"
)
```

After handling missing/unparseable values, the month was extracted:

```python
c["Order_Date"] = c["Order_Date"].dt.month
```

### 7. Data-Type Validation

The dataset's data types were checked and `Quantity` was explicitly converted to numeric format:

```python
c["Quantity"] = pd.to_numeric(
    c["Quantity"],
    errors="coerce"
)
```

### 8. Feature Engineering

A new `Total_Price` column was created:

```python
c["Total_Price"] = (
    c["Quantity"] * c["Price"]
)
```

This provides the total sales value for each order record.

### 9. Duplicate Detection and Removal

Duplicate records were checked using:

```python
c.duplicated().sum()
```

The cleaned dataset contained **2 duplicate rows**, which were removed using:

```python
c = c.drop_duplicates()
```

After removal, the duplicate count was **0**.

The final cleaned dataset therefore contains **51 records**.

---

## 📊 Analysis Performed

After cleaning, the project performed several basic business-oriented analyses.

### Sales by Product

Total sales were calculated for each product.

The resulting analysis showed:

| Product | Total Sales |
|---|---:|
| Laptop | ₹332,500 |
| Monitor | ₹72,700 |
| Office Chair | ₹44,700 |
| Desk | ₹32,200 |
| Webcam | ₹22,350 |
| Laptop Stand | ₹17,250 |
| Keyboard | ₹14,350 |
| Mouse | ₹12,300 |
| Headphones | ₹11,980 |

### Sales by City

Sales were aggregated by city:

| City | Total Sales |
|---|---:|
| Chennai | ₹216,060 |
| Kolkata | ₹127,700 |
| Hyderabad | ₹80,130 |
| Bangalore | ₹35,800 |
| Mumbai | ₹32,930 |
| Pune | ₹27,800 |
| Delhi | ₹20,250 |
| Kochi | ₹19,660 |

### City-Level Summary

The project also calculated:

- Number of orders
- Total sales
- Average customer rating

using a grouped Pandas aggregation.

---

## 📈 Key Observations

Based on the analysis performed in the notebook:

- **Laptop** generated the highest total sales among the products.
- **Chennai** recorded the highest total sales among the cities in the cleaned dataset.
- The dataset contains sales across **8 cities**.
- The dataset includes **9 products** across multiple categories.
- Customer ratings were analyzed at the city level along with order volume and sales.
- The cleaning process reduced the dataset from **53 raw records to 51 cleaned records** after duplicate removal.

These observations are descriptive results from this dataset and are not intended to represent a real-world market.

---

## 📁 Project Structure

```text
Customer-Dataset-Python-Analysis/
│
├── Customer_Dataset_Cleaning.ipynb
├── customer_sales_messy.csv
├── Customer_Dataset_Cleaned.csv
└── README.md
```

### File Description

| File | Description |
|---|---|
| `Customer_Dataset_Cleaning.ipynb` | Complete Python notebook containing the cleaning and analysis workflow |
| `customer_sales_messy.csv` | Original raw dataset |
| `Customer_Dataset_Cleaned.csv` | Cleaned dataset generated by the notebook |
| `README.md` | Project documentation |

---

## ▶️ How to Run the Project

### Option 1 — Google Colab

1. Open `Customer_Dataset_Cleaning.ipynb` in Google Colab.
2. Upload `customer_sales_messy.csv`.
3. Run the notebook cells sequentially.
4. The cleaned dataset will be exported as:

```text
Customer_Dataset_Cleaned.csv
```

### Option 2 — Jupyter Notebook

Install Pandas if required:

```bash
pip install pandas
```

Then open the notebook:

```bash
jupyter notebook
```

Make sure `customer_sales_messy.csv` is available in the same working directory as the notebook.

---

## 🧠 Skills Demonstrated

This project demonstrates practical beginner-to-intermediate data analytics skills:

- Python for Data Analysis
- Pandas
- Data Inspection
- Data Cleaning
- Missing-Value Handling
- Duplicate Detection
- Data Standardization
- Data-Type Conversion
- Date Handling
- Feature Engineering
- GroupBy Analysis
- Aggregation
- CSV Data Export

---

## 🚀 Future Improvements

Possible extensions to this project include:

- Exploratory Data Analysis (EDA) with Matplotlib or Seaborn
- Sales trend visualizations
- Product/category performance dashboards
- Customer segmentation
- Payment-mode analysis
- Salesperson performance analysis
- Interactive visualization using Power BI or Tableau
- Statistical analysis and deeper business insights

---
