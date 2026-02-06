# Project Setup & Usage

## Quick Start

### Prerequisites
```bash
python 3.13+
pip install pandas jupyter
```

### Running the Pipeline

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd uk-retail-sales-etl
   ```

2. **Run the Jupyter notebook**
   ```bash
   jupyter notebook day1_retail_etl.ipynb
   ```

3. **Execute all cells** to run the complete ETL pipeline

4. **Query the database**
   The clean data is available in `retail_sales_day1.db`
   
   ```python
   import sqlite3
   conn = sqlite3.connect('retail_sales_day1.db')
   df = pd.read_sql("SELECT * FROM sales_clean LIMIT 10", conn)
   ```

## Sample SQL Queries

### Revenue by Channel
```sql
SELECT 
    channel,
    ROUND(SUM(revenue), 2) as total_revenue,
    COUNT(*) as transaction_count,
    ROUND(AVG(revenue), 2) as avg_revenue_per_transaction
FROM sales_clean
GROUP BY channel
ORDER BY total_revenue DESC;
```

### Top Categories
```sql
SELECT 
    category,
    ROUND(SUM(revenue), 2) as total_revenue,
    SUM(quantity) as total_units_sold,
    COUNT(DISTINCT product_name) as unique_products
FROM sales_clean
GROUP BY category
ORDER BY total_revenue DESC;
```

### Location Performance
```sql
SELECT 
    location,
    channel,
    ROUND(SUM(revenue), 2) as total_revenue,
    COUNT(*) as transactions
FROM sales_clean
GROUP BY location, channel
ORDER BY total_revenue DESC
LIMIT 5;
```

### Daily Sales Trend by Channel
```sql
SELECT 
    date,
    channel,
    ROUND(SUM(revenue), 2) as daily_revenue,
    COUNT(*) as transactions
FROM sales_clean
GROUP BY date, channel
ORDER BY date, channel;
```

## Database Schema

**Table:** `sales_clean`

| Column          | Type    | Description                           |
|-----------------|---------|---------------------------------------|
| transaction_id  | TEXT    | Unique transaction identifier         |
| date            | TEXT    | Transaction date (YYYY-MM-DD)         |
| product_name    | TEXT    | Product name                          |
| category        | TEXT    | Standardized product category         |
| quantity        | INTEGER | Units sold (0 if missing)             |
| price           | REAL    | Unit price                            |
| revenue         | REAL    | Calculated (quantity × price)         |
| location        | TEXT    | Store location or "Online"            |
| channel         | TEXT    | "in-store" or "shopify"               |
| year            | INTEGER | Extracted year                        |
| month           | INTEGER | Extracted month                       |
| day             | INTEGER | Extracted day                         |
| day_of_week     | TEXT    | Day name (Monday, Tuesday, etc.)      |

## Data Cleaning Rules Applied

### Date Standardization
- Tried formats: YYYY-MM-DD, MM-DD-YYYY, DD/MM/YYYY
- Invalid dates → None (can be filtered)
- Result: Consistent YYYY-MM-DD format

### Category Normalization
| Raw Input           | Standardized  |
|---------------------|---------------|
| "Apparel", "APPAREL"| apparel       |
| "Clothing", "Clothes"| clothing     |
| "Accessories", "Acc"| accessories   |
| "Footwear", "Shoes" | footwear      |

### Missing Value Handling
- `quantity` missing → 0
- `location` missing → "Unknown"
- Other missing → preserved as NULL for analysis

### Duplicate Removal
- Exact duplicates based on all columns removed
- Ensures accurate revenue calculations

## File Descriptions

- **day1_retail_etl.ipynb**: Complete pipeline with code, outputs, and inline documentation
- **retail_sales_day1.db**: SQLite database containing cleaned sales data
- **data/**: Folder containing raw messy CSV inputs
- **docs/**: Additional documentation and diagrams

## Next Steps for Extension

1. **Incremental Loads**: Modify pipeline to append only new records
2. **Data Quality Monitoring**: Add validation checks and alerts
3. **Automated Scheduling**: Set up cron job for daily runs
4. **Error Logging**: Implement structured logging for production
5. **Performance Optimization**: Add indexing for frequent queries

---

**Questions?** Open an issue or reach out via [LinkedIn/Email]
