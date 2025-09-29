# 🛒 Retail Sales Analysis  

 
**Database:** `p1_retail_db`  

This project demonstrates SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. It is designed for beginners looking to build a solid foundation in SQL while working on a practical retail sales dataset.  

---

## 📌 Objectives  
- **Database Setup:** Create and populate a retail sales database.  
- **Data Cleaning:** Identify and remove missing/null records.  
- **Exploratory Data Analysis (EDA):** Understand the dataset using SQL queries.  
- **Business Analysis:** Answer real-world business questions with SQL.  

---

## 📂 Project Structure  

### 1. Database Setup  

```sql
-- Create Database
CREATE DATABASE p1_retail_db;
```

-- **Create Table**
```sql
CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```
### 2. Data Exploration & Cleaning  

```sql
-- Record Count
SELECT COUNT(*) 
FROM retail_sales;
```

-- **Unique Customers**
```sql
SELECT COUNT(DISTINCT customer_id) 
FROM retail_sales;
```

-- **Unique Product Categories**
```sql
SELECT DISTINCT category 
FROM retail_sales;
```

-- **Check for Null Values**
```sql
SELECT * 
FROM retail_sales
WHERE sale_date IS NULL 
   OR sale_time IS NULL 
   OR customer_id IS NULL 
   OR gender IS NULL 
   OR age IS NULL 
   OR category IS NULL 
   OR quantity IS NULL 
   OR price_per_unit IS NULL 
   OR cogs IS NULL;
```

-- **Remove Records with Null Values**
```sql
DELETE FROM retail_sales
WHERE sale_date IS NULL 
   OR sale_time IS NULL 
   OR customer_id IS NULL 
   OR gender IS NULL 
   OR age IS NULL 
   OR category IS NULL 
   OR quantity IS NULL 
   OR price_per_unit IS NULL 
   OR cogs IS NULL;
```
### 3. Data Analysis & Business Queries  

```sql
1. Retrieve all sales made on '2022-11-05':
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```


2. Clothing category sales with quantity >= 4 in Nov-2022:
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```


 3. Total sales for each category:
```sql
SELECT category, 
       SUM(total_sale) AS net_sale, 
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```


4. Average age of customers who purchased from 'Beauty':
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```


5. Transactions with total_sale > 1000:
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```


 6. Total number of transactions by gender in each category:
```sql
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```


7. Best-selling month in each year (based on avg sale):
```sql
SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS t1
WHERE rank = 1;
```


 8. Top 5 customers based on highest total sales:
```sql
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```


9. Number of unique customers in each category:
```sql
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```


10. Orders by shift (Morning, Afternoon, Evening):
```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

**Findings**

Customer Demographics: Sales are spread across all age groups with varied product preferences.

High-Value Transactions: Many purchases exceed 1000, highlighting premium customers.

Sales Trends: Seasonal and monthly sales variations reveal peak months.

Customer Insights: Identified top-spending customers and popular categories.

**📑 Reports**

Sales Summary: Total sales, demographics, and category performance.

Trend Analysis: Monthly and shift-based sales patterns.

Customer Insights: Top customers and unique buyers per category.

**✅ Conclusion**

This project is a beginner-friendly SQL case study for data analysts.
It covers database setup, cleaning, exploratory analysis, and business-driven SQL queries.
Insights gained from this project can help businesses understand sales trends, customer behavior, and product performance.
