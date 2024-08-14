# SQL_sales_Analysis_Project
## Project Overview

**Project Title**: Sales Analysis  
**Level**: Beginner  
**Database**: `SQL practice_1`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `SQL practice_1`.
- **Table Creation**: A table named `sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

-- Data Analysis & Business Key Problems & Answers

-- My Analysis & Findings
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022
-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)


--creating table in database
CREATE TABLE sales 
	(
		transactions_id FLOAT PRIMARY KEY,
		sale_date DATE,
		sale_time TIME,
		customer_id INT,
		gender VARCHAR(10),
		age INT,
		category VARCHAR(20),
		quantiy INT,
		price_per_unit FLOAT,
		cogs FLOAT,
		total_sale FLOAT	
	)

SELECT * FROM sales

--counting the number of rows in the table.
SELECT count(*) from sales;

--Finding NULL values
SELECT * FROM sales 
		WHERE transactions_id IS NULL
		OR
		sale_date IS NULL
		OR
		sale_time IS NULL
		OR
		customer_id IS NULL
		OR
		gender IS NULL
		OR
		age IS NULL
		OR
		category IS NULL
		OR
		quantiy IS NULL
		OR
		price_per_unit IS NULL
		OR
		cogs IS NULL
		OR
		total_sale IS NULL
		;
--Deleting rows with NULL values (Data Cleaning)
DELETE FROM sales 
		WHERE transactions_id IS NULL
		OR
		sale_date IS NULL
		OR
		sale_time IS NULL
		OR
		customer_id IS NULL
		OR
		gender IS NULL
		OR
		age IS NULL
		OR
		category IS NULL
		OR
		quantiy IS NULL
		OR
		price_per_unit IS NULL
		OR
		cogs IS NULL
		OR
		total_sale IS NULL
		;

--Data Exploration

--Total sales in the table
SELECT COUNT(*) total_sale FROM sales 

--Finding unique customers in the table
SELECT COUNT(DISTINCT customer_id) FROM sales

--Finding types of catogory we have
SELECT DISTINCT category from sales

-- DATA ANALYSIS and solving various Business problems

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'
SELECT * FROM sales WHERE sale_date= '2022-11-05'

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than than or equal to 4 in the month of Nov-2022
--Take care of the names whether they are capital or small
	SELECT * FROM sales 
	where 
	category LIKE'Clothing' 
	AND 
	TO_CHAR(sale_date,'YYYY-MM')= '2022-11'
	AND
	quantity>=4

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

SELECT category,SUM(total_sale) as total_sales FROM sales GROUP BY category

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

SELECT category,ROUND(AVG(age),2) avg_age FROM sales WHERE category='Beauty'
GROUP BY category

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
SELECT * FROM sales WHERE total_sale>1000

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
SELECT category,gender,COUNT(transactions_id) as total_transactions FROM sales GROUP BY category,gender
order by category

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
WITH sale AS
	(
	SELECT 
		EXTRACT(YEAR FROM sale_date) as year_,
		EXTRACT(MONTH FROM sale_date) as month_,
		AVG(total_sale),
		RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) 
	ORDER BY AVG(total_sale) DESC) 	
	FROM sales 
	GROUP BY  1,2 --order by 3 desc
	) 
SELECT 
	year_,month_,avg 
FROM sale 
WHERE RANK=1

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
SELECT 
	customer_id,
	SUM(total_sale) 
FROM sales 
GROUP BY customer_id 
ORDER BY 2 DESC 
LIMIT 5

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
SELECT category,COUNT(DISTINCT customer_id) unique_customers from sales group by category

'''Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, 
Afternoon Between 12 & 17, Evening >17)'''
with sale as(
SELECT sale_time,
	case
		WHEN EXTRACT(HOUR FROM sale_time)<12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM sale_time) between 12 and 16 THEN 'Afternoon'
		ELSE 'Evening'
    END AS shift FROM sales )
SELECT shift,COUNT(*) FROM sale GROUP BY shift 

--END OF PROJECT



