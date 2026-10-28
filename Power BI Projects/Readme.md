# Lotus Retail Sales Performance Dashboard

## 🛠️ TOOLS USED IN THIS PROJECT

• SQL - Data Cleaning, Data Validation, Querying, and Data Transformation

• Power BI - Data Visualization, DAX, Interactive Dashboard, KPI Reporting, and Sales Performance Analysis

## Summary

Created an interactive dashboard to analyze retail sales performance, profitability, store revenue, and customer trends using the Lotus sales dataset.

## Key Features

• Developed KPI cards for completed revenue, gross profit, gross margin, and completed orders.

• Visualized monthly revenue trends to examine changes in sales performance over time.

• Compared sales revenue across retail stores.

• Analyzed new and returning customer trends using line charts.

• Implemented a year slicer for interactive, year-based analysis.

## Data Cleaning With SQL

### Identifying duplicates

```sql
WITH duplicate_test AS(
SELECT *,
ROW_NUMBER() OVER(
PARTITION BY 
customer_id,
full_name, 
gender, 
birth_date, 
phone, 
email, 
city, 
region, 
loyalty_tier, 
registration_date) AS row_num
FROM cs_staging
)
SELECT *
FROM duplicate_test
WHERE row_num > 1;
```

### Creating a backup table

A backup table is necessary before doing any changes, it keeps the original table
intact for future revisions.

```sql
CREATE TABLE `cs_staging2` (
  `customer_id` text,
  `full_name` text,
  `gender` text,
  `birth_date` text,
  `phone` text,
  `email` text,
  `city` text,
  `region` text,
  `loyalty_tier` text,
  `registration_date` text,
  `row_num` int
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

INSERT INTO cs_staging2
SELECT *,
ROW_NUMBER() OVER(PARTITION BY customer_id, full_name, gender, birth_date, phone, email, city, region, loyalty_tier, registration_date) AS row_num
FROM cs_staging;
```

### Deleting any duplicates

```sql
DELETE
FROM cs_staging2
WHERE row_num > 1;
```

### Dropping the row_num column after the deleting process

```sql
ALTER TABLE cs_staging2
DROP COLUMN row_num;
```
