# Retail_sales_analysis_Project1

---

# 🛒 Retail Sales Analysis (SQL Project)

## 📌 Project Overview

**Project Title:** Retail Sales Analysis
**Level:** Beginner
**Database:** `retail_sales_db`

This project demonstrates fundamental SQL skills used by data analysts to **explore, clean, and analyze retail sales data**. It covers the complete workflow—from database creation to deriving business insights using SQL queries.

The goal is to simulate real-world retail analytics scenarios and build a strong foundation in **data querying, aggregation, and business analysis**.

---

## 🎯 Objectives

* Create and set up a retail sales database
* Perform **data cleaning** to ensure data quality
* Conduct **exploratory data analysis (EDA)**
* Solve real-world **business problems using SQL**
* Generate insights on **sales trends, customer behavior, and product performance**

---

## 🗂️ Project Structure

### 1️⃣ Database Setup

#### Create Database

```sql
CREATE DATABASE p1_retail_db;
```

#### Create Table

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

---

### 2️⃣ Data Exploration & Cleaning

#### 📊 Basic Exploration

```sql
-- Total records
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique categories
SELECT DISTINCT category FROM retail_sales;
```

#### 🧹 Data Cleaning (Handling Null Values)

```sql
-- Identify null values
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Delete null records
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

### 3️⃣ Data Analysis & Business Queries

#### 📅 Sales on Specific Date

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

---

#### 👕 Clothing Sales (Nov 2022, Quantity ≥ 4)

```sql
SELECT *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND quantity >= 4;
```

---

#### 💰 Total Sales by Category

```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

---

#### 👩 Average Age (Beauty Category)

```sql
SELECT
    ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

---

#### 💎 High-Value Transactions (>1000)

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

---

#### 🚻 Transactions by Gender & Category

```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

---

#### 📈 Best Selling Month Each Year

```sql
SELECT 
    year,
    month,
    avg_sale
FROM 
(    
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(
            PARTITION BY EXTRACT(YEAR FROM sale_date) 
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS ranked_data
WHERE rank = 1;
```

---

#### 🏆 Top 5 Customers by Sales

```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

---

#### 🧍 Unique Customers per Category

```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

---

#### ⏰ Sales by Time Shift

```sql
WITH hourly_sales AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders    
FROM hourly_sales
GROUP BY shift;
```

---

## 🔍 Key Findings

### 👥 Customer Demographics

* Customers span multiple age groups
* Balanced participation across genders
* Different categories attract different demographics

---

### 💰 Sales Insights

* High-value transactions (>1000) indicate **premium buying behavior**
* Certain categories contribute more revenue than others

---

### 📊 Trends & Patterns

* Monthly sales analysis reveals **seasonal trends**
* Some months consistently outperform others

---

### 🛍️ Customer Behavior

* Identified **top 5 high-value customers**
* Category-wise unique customer counts show **product popularity**

---

### ⏱️ Time-Based Insights

* Sales distribution varies across:

  * Morning
  * Afternoon
  * Evening
* Helps in optimizing **marketing & staffing decisions**

---

## 📑 Reports Generated

* **Sales Summary Report**

  * Total revenue
  * Orders per category
  * Customer distribution

* **Trend Analysis Report**

  * Monthly performance
  * Peak sales periods

* **Customer Insights Report**

  * Top customers
  * Buying patterns

---

## ✅ Conclusion

This project provides a strong foundation in SQL for data analysis by covering:

* Database design and setup
* Data cleaning techniques
* Exploratory data analysis (EDA)
* Business-driven SQL queries

The insights derived from this analysis can help businesses:

* Improve **sales strategies**
* Understand **customer behavior**
* Optimize **inventory and marketing decisions**
