# Day 1 – UK Retail Sales ETL Pipeline

**Cleaning messy multi-source sales data into a trustworthy database**

[![Python](https://img.shields.io/badge/Python-3.13-blue.svg)](https://www.python.org/)
[![pandas](https://img.shields.io/badge/pandas-latest-green.svg)](https://pandas.pydata.org/)
[![SQLite](https://img.shields.io/badge/SQLite-3-orange.svg)](https://www.sqlite.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📋 Problem Statement

A mid-sized UK fashion retailer (think high-street chain or growing e-commerce brand) receives daily sales data from two very different systems:

- **In-store POS system** → CSV export
- **Online Shopify store** → CSV export

**The files are messy:**

- Different column names (`Qty` vs `quantity_sold`, `Price` vs `unit_price`, etc.)
- Mixed date formats (`2026-01-21`, `01-21-2026`, `21/01/2026`)
- Missing values (especially quantities and store locations)
- Duplicates
- Inconsistent / free-text product categories (`Apparel`, `apparel `, `clothing typo`)

**Business Impact:**

Finance and marketing teams need **reliable daily sales figures** by:
- Product category
- Store location  
- Sales channel (in-store vs online)

Without clean data, they cannot accurately track performance, plan stock, or run promotions.

---

## 🎯 Business Goal

Build a **repeatable, trustworthy ETL process** that:

✅ Takes raw, messy CSVs every day  
✅ Cleans and standardizes the data  
✅ Loads it into a queryable database  
✅ Enables analysts to answer real questions using SQL or BI tools (Power BI, Tableau, Excel)

---

## 🔧 What I Built

A Python + pandas ETL pipeline that:

### 1. **Extract**
- Reads two messy CSV files from different sources

### 2. **Transform**
- Standardizes column names across sources
- Removes duplicates
- Cleans categories (lowercase, strip, remove typos)
- **Custom date parsing** to handle mixed UK/US/ISO formats
  - *This was the hardest part — automatic parsing failed on ~95% of rows*
  - Built custom logic → **0% failure rate**
- Handles missing values (quantities → 0, locations → 'Unknown')
- Adds calculated revenue check & date dimension columns

### 3. **Load**
- Saves clean data into **SQLite** database

### 4. **Consume**
- Produces business-ready SQL queries

---

## 📊 Key Results

### Revenue & Transaction Analysis

| Channel  | Total Revenue | Transactions | Avg Transaction Value |
|----------|--------------|--------------|----------------------|
| In-store | £31,343.10   | 103          | £304.30             |
| Shopify  | £23,291.46   | 100          | £232.91             |

**Insight:** In-store customers spend **31% more** per transaction — signals opportunity to enhance in-store experience and optimize online basket size.

### Top Revenue Categories

| Category    | Total Revenue | Units Sold | Unique Products |
|-------------|--------------|------------|-----------------|
| Apparel     | £18,750.17   | 342        | 8               |
| Clothing    | £16,734.17   | 283        | 8               |
| Accessories | £10,854.15   | 166        | 8               |
| Footwear    | £8,296.07    | 143        | 8               |

**Insight:** Apparel and clothing drive **65% of revenue** — clear signal for inventory investment and marketing focus.

### Location Performance

| Location    | Channel  | Revenue    | Transactions |
|-------------|----------|-----------|--------------|
| Online      | Shopify  | £23,291.46| 100          |
| London      | In-store | £7,028.92 | 21           |
| Edinburgh   | In-store | £6,875.79 | 21           |

**Insight:** While online dominates total revenue, London and Edinburgh stores show **strong per-transaction performance**.

---

## 🏗️ Architecture Diagram

```
┌─────────────────┐
│   POS System    │
│   (In-store)    │
└────────┬────────┘
         │
         │ CSV Export
         │ (messy data)
         ▼
┌─────────────────────────────────────────────┐
│                                             │
│         Python + pandas ETL Pipeline        │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  1. Extract                         │   │
│  │     • Read CSVs                     │   │
│  │     • Handle encoding issues        │   │
│  └─────────────────────────────────────┘   │
│                  │                          │
│                  ▼                          │
│  ┌─────────────────────────────────────┐   │
│  │  2. Transform                       │   │
│  │     • Standardize column names      │   │
│  │     • Custom date parsing           │   │
│  │     • Remove duplicates             │   │
│  │     • Clean categories              │   │
│  │     • Handle missing values         │   │
│  │     • Add calculated columns        │   │
│  └─────────────────────────────────────┘   │
│                  │                          │
│                  ▼                          │
│  ┌─────────────────────────────────────┐   │
│  │  3. Load                            │   │
│  │     • Save to SQLite database       │   │
│  │     • Create indexes                │   │
│  └─────────────────────────────────────┘   │
│                                             │
└─────────────────┬───────────────────────────┘
                  │
                  ▼
         ┌────────────────┐
         │ SQLite Database│
         │  sales_clean   │
         └────────┬───────┘
                  │
         ┌────────┴────────┐
         │                 │
         ▼                 ▼
┌─────────────┐   ┌─────────────┐
│  Power BI   │   │  Excel/SQL  │
│  Analysts   │   │   Queries   │
└─────────────┘   └─────────────┘

┌─────────────────┐
│ Shopify Store   │
│   (Online)      │
└────────┬────────┘
         │
         │ CSV Export
         │ (messy data)
         │
         └──────────────────────────────────┘
```

---

## 💡 Core Concepts Demonstrated

This project showcases real-world data engineering skills:

- ✅ **ETL fundamentals** (Extract → Transform → Load)
- ✅ **Handling real-world data messiness** (inconsistent schemas, mixed date formats, missing values, duplicates)
- ✅ **Defensive programming** & custom parsing logic
- ✅ **Data quality checks** and validation
- ✅ **Preparing data for analysts** / BI tools
- ✅ **Database design** for analytics workloads
- ✅ **Version control** with Git

---

## 🛠️ Tech Stack

- **Python 3.13** — Core programming language
- **pandas** — Data manipulation and transformation
- **Jupyter Notebook** — Interactive development
- **SQLite** — Lightweight local database
- **Git & GitHub** — Version control and portfolio hosting

---

## 📈 Business Questions Answered

The pipeline enables analysts to answer questions like:

### 1. Revenue & Transaction Count by Channel
```sql
SELECT 
    channel,
    ROUND(SUM(revenue), 2) as total_revenue,
    COUNT(*) as transaction_count,
    ROUND(AVG(revenue), 2) as avg_revenue_per_transaction
FROM sales_clean
GROUP BY channel;
```
**Result:** In-store has higher average transaction value (£304 vs £233 online)

### 2. Top-Selling Categories by Revenue
```sql
SELECT 
    category,
    ROUND(SUM(revenue), 2) as total_revenue,
    SUM(quantity) as total_units_sold
FROM sales_clean
GROUP BY category
ORDER BY total_revenue DESC;
```
**Result:** Apparel and clothing dominate revenue

### 3. Daily Sales Trend by Channel
```sql
SELECT 
    date,
    channel,
    ROUND(SUM(revenue), 2) as daily_revenue
FROM sales_clean
GROUP BY date, channel
ORDER BY date;
```
**Result:** Clear visibility into daily fluctuations

### 4. Performance by Store/Location
```sql
SELECT 
    location,
    channel,
    ROUND(SUM(revenue), 2) as total_revenue,
    COUNT(*) as transactions
FROM sales_clean
GROUP BY location, channel
ORDER BY total_revenue DESC;
```
**Result:** Online dominates overall, but London & Edinburgh stores perform strongly

---

## 🚀 How to Run

### Prerequisites
```bash
python 3.13+
pip install pandas jupyter
```

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/uk-retail-etl-pipeline.git
   cd uk-retail-etl-pipeline
   ```

2. **Open the Jupyter notebook**
   ```bash
   jupyter notebook day1_retail_etl.ipynb
   ```

3. **Run all cells**
   - Data generation → Cleaning → Database load → Queries

4. **Query the database**
   ```python
   import sqlite3
   import pandas as pd
   
   conn = sqlite3.connect('retail_sales_day1.db')
   df = pd.read_sql("SELECT * FROM sales_clean LIMIT 10", conn)
   print(df)
   ```

---

## 📁 Project Structure

```
uk-retail-etl-pipeline/
├── README.md                      # This file
├── SETUP.md                       # Detailed setup guide
├── day1_retail_etl.ipynb         # Main ETL pipeline notebook
├── retail_sales_day1.db          # SQLite database (output)
├── data/
│   ├── pos_sales_messy.csv       # Raw POS data
│   └── online_sales_messy.csv    # Raw Shopify data
├── docs/
│   └── architecture_diagram.png  # Visual architecture
├── .gitignore                    # Git ignore rules
└── LICENSE                       # MIT License
```

---

## 🎓 Key Learnings

### The Date Parsing Challenge

**Problem:** Standard pandas date parsing failed on 95%+ of records due to mixed formats:
- `2026-01-21` (ISO format)
- `01-21-2026` (US format)
- `21/01/2026` (UK format)

**Solution:** Built custom parsing logic that tries each format sequentially:

```python
def parse_flexible_date(date_str):
    formats = ['%Y-%m-%d', '%m-%d-%Y', '%d/%m/%Y']
    for fmt in formats:
        try:
            return pd.to_datetime(date_str, format=fmt)
        except:
            continue
    return None
```

**Result:** 0% failure rate, accurate date dimensions for time-series analysis

### Other Insights

- Real data is **always messy** — especially dates from different systems
- Automatic parsing often fails — **custom logic frequently required**
- Preparing data for analysts (clean schema + date dimensions) is **core data engineering work**
- Business context matters — understanding retail KPIs shaped the transformation logic

---

## 🔮 Future Enhancements

- [ ] Implement incremental loading for production deployment
- [ ] Add data quality monitoring and alerting
- [ ] Create automated daily reporting dashboards
- [ ] Add schema validation and error logging
- [ ] Expand to handle additional data sources (returns, inventory)
- [ ] Implement data lineage tracking
- [ ] Add unit tests for transformation logic

---

## 📝 Interview Talking Points

> *"This was a realistic UK retail data problem — messy multi-source sales files. I built an ETL pipeline that cleaned inconsistent columns, fixed mixed date formats with custom logic, removed duplicates, and loaded the result into SQLite. I then wrote SQL queries that let the business answer real questions like channel performance and top categories — all things that were impossible with the raw data."*

---

## 📫 Contact

**Rakesh Mohankumar**  
MSc Business Analytics @ University of Exeter  
Seeking UK Data/Analytics Roles

- 📧 Email: [rakesh.tiya@gmail.com]
- 💼 LinkedIn: [https://www.linkedin.com/in/rakesh-mohankumar-367915161/]
- 🐙 GitHub: [https://github.com/rakeshtiya/rakeshtiya]


---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

This project is part of a progressive data engineering portfolio series demonstrating production-ready skills for UK analytics roles.

**Part of:** Data Engineering Portfolio Series  
**Project:** Day 1 of 30  
**Focus:** ETL Fundamentals & Data Quality

---

**⭐ If you found this project helpful, please consider giving it a star!**

*Built with ❤️ for demonstrating real-world data engineering skills*
