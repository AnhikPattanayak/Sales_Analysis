# 🪔 Diwali Sales Analysis — Python & SQL

End-to-end retail sales analysis covering customer segmentation, regional performance, and product insights across **11,251 transactions** and **₹106M+ in revenue** — using Python for EDA and MySQL for business queries.

---

## 📌 Project Overview

This project simulates the kind of analysis a data analyst would perform at a retail or e-commerce company after a major sales event. The goal was to move beyond surface-level charts and answer real business questions: *who is buying, what are they buying, where are they buying from, and what does that mean for strategy?*

The analysis is split into two layers:

- **Python (pandas + seaborn + matplotlib)** — data cleaning, feature engineering, and exploratory visualisation
- **MySQL** — business KPI queries, window functions, CTEs, and customer segmentation

---

## 📂 Repository Structure

```
diwali-sales-analysis/
│
├── Diwali_Sales_Data.csv        # Raw dataset (11,251 rows × 15 columns)
├── README.mdSales_Analysis.ipynb         # Python EDA notebook
├── Sales_Analysis.ipynb         # Python EDA notebook       
└── Sales_Analysis_sql.sql       # MySQL business queries
```

---

## 📊 Dataset

| Attribute | Detail |
|---|---|
| Source | Diwali retail sales dataset |
| Rows | 11,251 transactions |
| Columns | 15 (User ID, Gender, Age Group, State, Zone, Occupation, Product Category, Orders, Amount) |
| Unique Customers | 3,755 |
| States Covered | 16 |
| Product Categories | 18 |
| Zones | 5 (Central, Southern, Western, Northern, Eastern) |
| Total Revenue | ₹106.2M |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3 | Data cleaning, EDA, visualisation |
| pandas | Data manipulation and feature engineering |
| seaborn / matplotlib | Charts and heatmaps |
| MySQL Workbench | Business queries and advanced SQL |
| SQLAlchemy | Python → MySQL connection via `pymysql` |
| Jupyter Notebook | Interactive analysis environment |

---

## 🔍 Python Analysis — What Was Done

### Data Cleaning
- Dropped fully empty columns (`Status`, `unnamed1`)
- Imputed null `Amount` values using **group mean** by `Product_Category × Zone` — a statistically sound method that preserves category and regional context
- Removed duplicate rows
- Snake-cased all column names for SQL compatibility

### Feature Engineering
Three new features were engineered from raw data:

| Feature | Logic | Business Use |
|---|---|---|
| `avg_order_value` | `amount / orders` | Measures spend quality per transaction |
| `purchase_count` | Count of orders per `user_id` | Identifies repeat buyers |
| `loyalty_tier` | Bucketed `purchase_count` into One-time / Occasional / Regular / Loyal | CRM segmentation |

### EDA Sections
- Zone & state revenue breakdown
- Gender × Marital Status spend analysis
- Age group × gender buyer heatmap
- Occupation: avg order value vs. total revenue
- Loyalty tier analysis (buyer count, revenue %, avg spend)
- Top 10 product categories by revenue
- Zone × Category revenue heatmap

---

## 🗄️ SQL Analysis — Business Questions Answered

The SQL file covers 20 queries across three complexity levels. Key highlights:

**Aggregation & KPIs**
- Total revenue, orders, and average transaction value
- Gender-wise, age-group-wise, and zone-wise revenue breakdown
- Top states and occupation purchasing power

**Subqueries & Filtering**
- Revenue contribution (%) by top product categories
- Products above overall average order value (`HAVING` + subquery)
- Most popular product category per occupation (inline subquery + `ROW_NUMBER`)

**Window Functions & CTEs**
- States underperforming vs. their zone average (`AVG() OVER (PARTITION BY zone)`)
- Product categories ranked by revenue using `DENSE_RANK()`
- Top 3 customers per state using `DENSE_RANK() OVER (PARTITION BY state)`
- Cumulative revenue by category — identifying which categories form the top 80% (`ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`)
- Full customer 360 summary: transactions, total spend, AOV, min/max spend per buyer

---

## 💡 Key Business Insights

| # | Insight | Business Implication |
|---|---|---|
| 1 | **Central zone = 39% of revenue (₹41.5M)**, driven by UP, Delhi, MP | Prioritise fulfilment capacity and inventory in Central zone |
| 2 | **Eastern zone = only ~7% of revenue** | Potential untapped market — investigate distribution gaps vs. demand gaps |
| 3 | **26–35 age group dominates volume**; 46–55 age group has higher AOV | Dual strategy: volume campaigns for 26–35, premium upsell for 46–55 |
| 4 | **Unmarried buyers have the highest avg spend per transaction** | Target unmarried segment for high-margin product promotions |
| 5 | **IT Sector leads total revenue**; Govt & Chemical have highest AOV | Allocate ad budget to IT for volume, Govt/Chemical for premium margin |
| 6 | **Loyal buyers (6+ purchases) drive disproportionate revenue per transaction** despite being fewer in number | Invest in retention for "Occasional" buyers to convert them to "Regular" |
| 7 | **Food is the top category at ₹34M** | Stock heavily for next sale event; explore subscription/auto-refill |
| 8 | **Northern zone is weak across all categories** | Investigate logistics/distribution issues before demand campaigns |

---

## ▶️ How to Run

### Python Notebook
```bash
# Install dependencies
pip install pandas numpy matplotlib seaborn sqlalchemy pymysql jupyter

# Launch notebook
jupyter notebook Sales_Analysis.ipynb
```

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
