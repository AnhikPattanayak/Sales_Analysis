# 🪔 Diwali Sales Analysis

A data analytics project exploring Diwali retail sales data to uncover insights into customer spending patterns, regional performance, product preferences, and buyer segmentation — using **Python** and **MySQL (SQL)**.

---

## 📌 Project Overview

This project analyzes **11,251 sales transactions** from a Diwali retail event. The goal is to derive actionable business insights that guide marketing strategies, regional inventory decisions, and customer retention efforts.

---

## 📂 Dataset Summary

| Property | Details |
|---|---|
| Total Rows | 11,251 |
| Total Columns | 15 |
| Missing Data | Null values in `Amount` column (imputed) |

**Key Features:**
- **Customer Demographics** — User ID, Name, Gender, Age Group, Age, Marital Status
- **Geographic Data** — State, Zone
- **Purchase Details** — Product ID, Product Category, Orders, Amount
- **Other** — Occupation

---

## 🔧 Tools & Technologies

| Tool | Purpose |
|---|---|
| Python (pandas, seaborn, matplotlib) | Data cleaning, feature engineering & EDA |
| MySQL | SQL-based business analysis |
| SQLAlchemy + PyMySQL | Python → MySQL database connection |
| Jupyter Notebook | Interactive analysis environment |

---

## 🧹 Data Preparation (Python)

- **Data Loading** — Imported dataset using `pandas` with `unicode_escape` encoding
- **Missing Data Handling** — Imputed null `Amount` values using group mean by `Product_Category × Zone` — preserves regional and category context
- **Column Standardization** — Renamed all columns to `snake_case` for SQL compatibility
- **Duplicate Removal** — Identified and dropped duplicate rows
- **Feature Engineering**
  - `avg_order_value` — derived as `amount / orders` per transaction
  - `purchase_count` — count of purchases per `user_id`
  - `loyalty_tier` — bucketed `purchase_count` into One-time / Occasional / Regular / Loyal
- **Database Integration** — Loaded cleaned DataFrame into MySQL via SQLAlchemy

---

## 🗄️ SQL Analysis (MySQL)

Twenty business questions were answered using structured SQL queries:

1. **Total Revenue & Orders** — ₹106.2M revenue across 11,251 transactions; avg order value ₹9,443
2. **Gender-wise Revenue** — Female buyers lead in total spend; male buyers have comparable transaction counts
3. **Age Group Spending** — 26–35 age group dominates volume; 46–55 age group has the highest avg order value
4. **Top 5 States by Orders**

   | Rank | State | Total Orders |
   |---|---|---|
   | 1 | Uttar Pradesh | Highest |
   | 2 | Maharashtra | — |
   | 3 | Karnataka | — |
   | 4 | Delhi | — |
   | 5 | Madhya Pradesh | — |

5. **Zone Performance** — Central zone leads at ₹41.5M (~39% of total revenue); Eastern zone at ~7%
6. **Top Product Categories** — Food (₹34M), Clothing & Apparel, and Electronics & Gadgets top the list
7. **Occupation Purchasing Power** — IT Sector leads total revenue; Govt & Chemical sectors have highest avg order value
8. **Marital Status Effect** — Unmarried buyers have the highest avg spend per transaction
9. **Top Category per Occupation** — IT → Electronics; Healthcare → Food; Govt → Clothing (via `ROW_NUMBER OVER PARTITION`)
10. **Underperforming States vs Zone Average** — Northern and Eastern zone states fall significantly below their zone average (via `AVG() OVER PARTITION BY zone`)
11. **Product Categories Ranked by Revenue** — Using `DENSE_RANK()` with revenue per unit metrics
12. **Top 3 Customers per State** — Identified using `DENSE_RANK() OVER (PARTITION BY state)`
13. **Cumulative Revenue (Pareto)** — Top 5 categories form ~65% of total revenue (`ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`)
14. **Customer 360 Summary** — Full buyer profile: transactions, total spend, AOV, max/min spend per user

---

## 💡 Key Business Insights

- **Central zone = 39% of revenue (₹41.5M)**, driven by UP, Delhi, and MP — prioritise fulfilment capacity here
- **Eastern zone = only ~7% of revenue** — signals distribution gaps rather than demand gaps
- **26–35 age group dominates in volume**; 46–55 age group has higher AOV — dual strategy needed
- **Unmarried buyers have the highest avg spend per transaction** — target with high-margin promotions
- **IT Sector leads total revenue**; Govt & Chemical have highest AOV — volume vs. margin targeting
- **Loyal buyers (6+ purchases) drive disproportionate revenue per transaction** — retention of Occasional buyers is the highest-ROI growth lever
- **Food is the top category at ₹34M** — strong candidate for subscription or auto-refill features
- **Northern zone is weak across all categories** — investigate logistics before running demand campaigns

---

## 📁 Project Structure

```
diwali-sales-analysis/
│
├── Diwali Sales Data.csv        # Raw dataset (11,251 rows × 15 columns)
├── README.md
├── Sales_Analysis.ipynb         # Python EDA notebook
└── Sales_Analysis_sql.sql       # MySQL business queries
```
