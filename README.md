# UK Retail Sales ETL Pipeline

**Transforming messy multi-channel sales data into actionable business intelligence**

## The Business Problem

A mid-sized UK fashion retailer operating both physical stores and an online Shopify presence faced a critical data quality challenge. Daily sales data from their POS systems (in-store) and Shopify (online) arrived in completely inconsistent formats:

- **Inconsistent column naming** across data sources
- **Mixed date formats** (YYYY-MM-DD, MM-DD-YYYY, DD/MM/YYYY) creating parsing failures
- **Duplicate transactions** inflating revenue figures
- **Free-text categories** preventing accurate product classification
- **Missing values** across critical fields

**Business Impact:** Finance and marketing teams could not trust the numbers for daily reporting, category performance analysis, channel comparison, or stock planning decisions.

## The Solution

Built a production-ready ETL pipeline that transforms unreliable raw data into a clean, queryable database that business analysts can use directly in Power BI, Excel, and SQL.

### What This Pipeline Delivers

✅ **95%+ date parsing accuracy** through custom format detection logic  
✅ **Complete deduplication** ensuring accurate revenue reporting  
✅ **Standardized schema** across all data sources  
✅ **Business-ready SQL queries** answering real retail questions  
✅ **Repeatable automated process** for daily data loads

## Technical Implementation

### Architecture

```
[POS CSV] ──┐
            ├──> [Python + pandas ETL] ──> [SQLite DB: sales_clean]
[Shopify CSV] ─┘                                    ↓
                                      Analysts / Power BI / SQL queries
```

### ETL Pipeline Components

**1. Extract**
- Read messy CSV files from multiple sources
- Handle encoding issues and malformed records

**2. Transform**
- Unified column names across sources
- Added channel identifier (in-store vs online)
- **Custom date parsing logic** handling mixed UK/US/ISO formats
- Removed exact duplicates
- Cleaned & normalized product categories
- Handled missing quantities (filled 0) & locations (filled 'Unknown')
- Added calculated revenue validation & date dimension columns

**3. Load**
- Saved clean data into SQLite database
- Created indexed table for query performance

**4. Consume**
- Business-ready SQL queries answering real retail questions

## Key Insights Generated

### 1. Channel Performance
| Channel  | Total Revenue | Transactions | Avg Transaction Value |
|----------|--------------|--------------|---------------------|
| In-store | £31,343.10   | 103          | £304.30            |
| Shopify  | £23,291.46   | 100          | £232.91            |

**Insight:** In-store customers spend 31% more per transaction — signals opportunity to enhance in-store experience and optimize online basket size.

### 2. Top Revenue Categories
| Category    | Total Revenue | Units Sold | Unique Products |
|-------------|--------------|------------|----------------|
| Apparel     | £18,750.17   | 342        | 8              |
| Clothing    | £16,734.17   | 283        | 8              |
| Accessories | £10,854.15   | 166        | 8              |
| Footwear    | £8,296.07    | 143        | 8              |

**Insight:** Apparel and clothing drive 65% of revenue — clear signal for inventory investment and marketing focus.

### 3. Location Performance
| Location    | Channel  | Revenue    | Transactions |
|-------------|----------|-----------|--------------|
| Online      | Shopify  | £23,291.46| 100         |
| London      | In-store | £7,028.92 | 21          |
| Edinburgh   | In-store | £6,875.79 | 21          |
| London (alt)| In-store | £6,856.20 | 17          |
| Manchester  | In-store | £4,497.64 | 17          |

**Insight:** While online dominates total revenue, London and Edinburgh stores show strong per-transaction performance — worth analyzing what drives their success.

## Technical Challenges Solved

### The Date Parsing Problem
**Challenge:** 95%+ failure rate with standard pandas parsing due to mixed UK/US/ISO formats  
**Solution:** Built custom multi-format detection logic that tries formats sequentially  
**Result:** 0% failure rate, accurate date dimensions for time-series analysis

### The Data Quality Challenge
**Challenge:** Inconsistent categories, missing values, duplicates  
**Solution:** Comprehensive cleaning pipeline with standardization rules  
**Result:** Clean, trustworthy data schema ready for BI tools

## What This Demonstrates

This project showcases real-world data engineering skills that UK employers value:

- **Data quality assessment** and remediation strategies
- **ETL pipeline design** following industry best practices
- **Business-focused analytics** translating data into decisions
- **Problem-solving** for messy real-world data scenarios
- **SQL proficiency** for business intelligence
- **Python data engineering** with pandas

## Tech Stack

- **Python 3.13** + **pandas** for data processing
- **SQLite** for data storage
- **Jupyter Notebook** for development & documentation
- **SQL** for business queries

## Project Structure

```
├── day1_retail_etl.ipynb          # Full ETL pipeline with outputs
├── retail_sales_day1.db           # Final clean database
├── data/
│   ├── pos_sales_messy.csv        # Raw POS data
│   └── online_sales_messy.csv     # Raw Shopify data
├── docs/
│   └── architecture_diagram.png   # Visual pipeline flow
├── .gitignore
└── README.md
```

## Business Queries Examples

The pipeline enables analysts to answer questions like:

1. What is revenue and transaction count by channel?
2. Which product categories drive the most revenue?
3. How do daily sales trends differ by channel?
4. Which store locations perform best?

All queries run cleanly against the normalized database — impossible with raw data.

## Key Learnings

- Real data is **always messy** — especially dates from different systems
- Automatic parsing often fails — **custom logic frequently required**
- Preparing data for analysts (clean schema + date dimensions) is **core data engineering work**
- Business context matters — understanding retail KPIs shaped the transformation logic

## Future Enhancements

- Add incremental load logic for production deployment
- Implement data quality monitoring and alerting
- Create automated daily reporting dashboards
- Add schema validation and error logging
- Expand to handle additional data sources (returns, inventory)

---

## Interview Talking Points

*"This was a realistic UK retail data problem — messy multi-source sales files. I built an ETL pipeline that cleaned inconsistent columns, fixed mixed date formats with custom logic, removed duplicates, and loaded the result into SQLite. I then wrote SQL queries that let the business answer real questions like channel performance and top categories — all things that were impossible with the raw data."*

---

**Author:** Rakesh Mohankumar  
**Contact:** [https://www.linkedin.com/in/rakesh-mohankumar-367915161/] | [rakesh.tiya@gmail.com]  
**Portfolio:** [https://github.com/rakeshtiya/rakeshtiya]

*Part of a progressive data engineering portfolio series demonstrating production-ready data skills for UK analytics roles*
