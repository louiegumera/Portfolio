# Lotus Retail Sales Performance Dashboard

![Lotus Retail Sales Performance Dashboard](https://github.com/louiegumera/Portfolio/blob/6ceab3a43d7ef1ea705d8d5364b2d565330bc674/Power%20BI%20Projects/Lotus%20Retail%20Sales%20Performance/Dashboard/2022-2024.png)

## 🛠️ Tools Used In This Project

• SQL - Data Cleaning, Data Validation, Query, Data Transformation

• Power BI - Data Visualization, DAX, Interactive Dashboard, KPI Reporting, and Sales Performance Analysis

## Summary

Transformed, cleaned, and validated retail sales data using SQL, improving data consistency and preparing the dataset for further analysis and developed an interactive Power BI dashboard to monitor key performance indicators (KPIs), including completed revenue, gross profit, gross margin, and completed orders, with visualizations of monthly revenue trends, store sales comparisons, and new versus returning customer trends

## Key Features

• Implemented data cleaning, data validation, and data transformation using SQL.

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

### Standardizing the date format
```sql
UPDATE dim_customers
SET birth_date =
CASE
	WHEN birth_date LIKE '%-%-%' THEN STR_TO_DATE(birth_date, '%Y-%m-%d')
    WHEN birth_date LIKE '%/%/%' THEN STR_TO_DATE(birth_date, '%d/%m/%Y')
END;
```

### Standardizing the datatype
```sql
ALTER TABLE dim_customers
MODIFY COLUMN birth_date DATE;
```

## Data Visualization With Power BI

### Lotus Retail Sales Performance Dashboard
![Lotus Retail Sales Performance Dashboard](https://github.com/louiegumera/Portfolio/blob/6ceab3a43d7ef1ea705d8d5364b2d565330bc674/Power%20BI%20Projects/Lotus%20Retail%20Sales%20Performance/Dashboard/2022-2024.png)

### KPI Cards
KPI cards provide a quick overview of the business's overall performance for the selected year. They help viewers understand the results before examining the detailed charts.

![KPI Cards](https://github.com/louiegumera/Portfolio/blob/9b833cd7b04a4479ee0ae22948421f80f7736de0/Power%20BI%20Projects/Lotus%20Retail%20Sales%20Performance/Dashboard/KPI%20Cards.png)

### Monthly Revenue Trends
Line chart shows how revenue changes from month to month. The horizontal axis represents the months, while the vertical axis represents revenue.

**Why it matters:**
• Identifies months with the highest and lowest revenue.
• Reveals recurring seasonal patterns or changes in sales.
• Helps management plan inventory, staffing, and promotions.

![Monthly Revenue Trends](https://github.com/louiegumera/Portfolio/blob/9b833cd7b04a4479ee0ae22948421f80f7736de0/Power%20BI%20Projects/Lotus%20Retail%20Sales%20Performance/Dashboard/Monthly%20Revenue%20Trends.png)

### New and Returning Customer Trends
Multi-series line chart compares the number of newly observed customers with returning customers across months.

**Data Series:**
• New Customers: Customers whose first recorded completed purchase occurs during the selected period.
• Returning Customers: Customers with a completed purchase during the period whose first recorded completed purchase occurred earlier.

**Why it matters:**
• Shows whether the business is attracting new customers.
• Helps monitor repeat purchasing behavior.
• Provides insight into customer acquisition and retention patterns.

![New and Returning Customer Trends](https://github.com/louiegumera/Portfolio/blob/9b833cd7b04a4479ee0ae22948421f80f7736de0/Power%20BI%20Projects/Lotus%20Retail%20Sales%20Performance/Dashboard/New%20and%20Returning%20Customers.png)

### Sales by Store
It compares revenue across individual stores. Each bar represents a store, and the bar length indicates its revenue.

**Why it matters:**
• Identifies the highest-performing stores.
• Highlights stores generating relatively low revenue.
• Helps management compare store performance and identify where further analysis may be needed.

![Sales by Store](https://github.com/louiegumera/Portfolio/blob/9b833cd7b04a4479ee0ae22948421f80f7736de0/Power%20BI%20Projects/Lotus%20Retail%20Sales%20Performance/Dashboard/Sales%20by%20Store.png)

### Year Slicer
The Year Slicer allows users to interactively filter the dashboard by year, making it easier to explore annual sales performance, revenue trends, store performance, and customer behavior.

![Year Slicer](https://github.com/louiegumera/Portfolio/blob/9b833cd7b04a4479ee0ae22948421f80f7736de0/Power%20BI%20Projects/Lotus%20Retail%20Sales%20Performance/Dashboard/Year%20Slicer.png)
