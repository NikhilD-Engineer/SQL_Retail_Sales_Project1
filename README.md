# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL. This project is done using MySQL Workbench.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure
```sql
-- SQL Retail Sales Analysis - P1
CREATE DATABASE sql_project_p1;
```
-- Create TABLE 
-- DROP TABLE IF EXISTS retail_sales;
```sql
CREATE TABLE retail_sales (
	transactions_id INT,	
    sale_date	DATE,
    sale_time	TIME,
    customer_id	INT,
    gender	VARCHAR(15),
    age	INT,
    category VARCHAR(15),	
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,	
    total_sale FLOAT
    )
```
-- Cheking the 10 entries from the data to verify
```sql
SELECT * FROM retail_sales
LIMIT 10
```
-- Checking the count (1987)
```sql
SELECT COUNT(*) FROM retail_sales;
```
-- DATA cleaning
```sql
SELECT * FROM retail_sales
WHERE 
	transactions_id IS NULL
    OR
    sale_date IS NULL
    OR
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR 
    price_per_unit IS NULL
    OR 
    cogs IS NULL
    OR
    total_sale IS NULL; 
```
-- DATA exploration
```sql
-- How many sales we have?
SELECT COUNT(*) as total_sales FROM retail_sales
```
-- How many unique customers we have?
```sql
SELECT COUNT(DISTINCT customer_id) as total_sales FROM retail_sales
```
-- Which are the categories we have?
```sql
SELECT DISTINCT category as total_sales FROM retail_sales
```
**DATA analysis & Business key problems & answers**

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```
2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022**:
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing' 
	AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
    AND quantity >= 3
```    
3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT category,
		SUM(total_sale) AS Total_sales,
        COUNT(total_sale) AS Total_orders
FROM retail_sales
GROUP BY category
```
4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT ROUND(AVG(age), 2) AS Average_age,
	   category
FROM retail_sales
WHERE category = 'Beauty'
```
5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT *
FROM retail_sales
WHERE total_sale >1000;
```
6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT category,
		gender,
        COUNT(transactions_id) AS Total_transactions
FROM retail_sales
GROUP BY category, 
		 gender
ORDER BY category
```
7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
WITH monthly_sales AS (
	SELECT
		YEAR(sale_date) AS sale_year,
        MONTH(sale_date) AS sale_month,
        ROUND(AVG(total_sale), 2) AS avg_monthly_sale,
        SUM(total_sale) AS total_monthly_sale
	FROM retail_sales
    GROUP BY sale_year, sale_month
    )
SELECT 
	sale_year, 
    sale_month, 
    avg_monthly_sale, 
    total_monthly_sale
FROM monthly_sales
WHERE (sale_year, total_monthly_sale) 
	IN (
		SELECT sale_year,
				MAX(total_monthly_sale)
		FROM monthly_sales
        GROUP BY sale_year
        )
ORDER BY sale_year, sale_month;
```
8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT customer_id,
		SUM(total_sale) AS total_sale,
        count(*)
FROM retail_sales
GROUP BY customer_id, total_sale 
ORDER BY total_sale DESC
LIMIT 5;
```
9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
	category,
	COUNT(DISTINCT customer_id) AS unique_customer_id		
FROM retail_sales
GROUP BY category
```
10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sales AS	
    (
		SELECT *,
			CASE
				WHEN HOUR(sale_time) < 12 THEN 'Morning'
				WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
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

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across various categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project is a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behaviour, and product performance.

## Author - Nikhil Dagale

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, or feedback, or would like to collaborate, feel free to get in touch!


