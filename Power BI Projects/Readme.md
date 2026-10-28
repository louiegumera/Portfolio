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

### Identifying Duplicates

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
