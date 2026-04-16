# SQL-Project-1-Retail-Sales-
SQL Retail Sales Analysis-P1
CREATE DATABASE sql_project_p2


-- Create TABLE
DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
SELECT * FROM retail_sales
LIMIT 10
SELECT COUNT (*) FROM retail_sales
SELECT * FROM retail_sales 
WHERE transactions_id IS NULL
SELECT * FROM retail_sales 
WHERE sale_date IS NULL
SELECT * FROM retail_sales
WHERE sale_time IS NULL;

SELECT * FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;
	DELETE FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;
	-- Data Exploration

-- How many sales we have?
SELECT COUNT(*) AS total_sale FROM retail_sales;
-- How many unique customers we have ?
SELECT COUNT(DISTINCT customer_id) AS total_sale 
FROM retail_sales;
SELECT DISTINCT category AS total_sale 
FROM retail_sales;
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
  SELECT
  category,
  SUM(total_sale) AS net_sale
FROM retail_sales
GROUP BY 1;

SELECT 
  ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

SELECT * FROM retail_sales
WHERE total_sale > 1000 ;

SELECT
  category,
  gender,
  COUNT(*) AS total_trans
FROM retail_sales
GROUP BY
  category,
  gender
  ORDER BY 1;

  SELECT
  EXTRACT(YEAR FROM sale_date) AS year,
  EXTRACT(MONTH FROM sale_date) AS month,
  AVG(total_sale) AS avg_sale
FROM retail_sales
GROUP BY 1, 2
ORDER BY 1, 2 	DESC;


SELECT *
FROM (
  SELECT
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale,
    RANK() OVER (
      PARTITION BY EXTRACT(YEAR FROM sale_date)
      ORDER BY AVG(total_sale) DESC
    ) AS rank
  FROM retail_sales
  GROUP BY 1, 2
) AS t1
WHERE rank = 1
ORDER BY 1, 2 	DESC;

SELECT
  customer_id,
  SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

SELECT
  category,
  COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;

WITH hourly_sale AS (
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
FROM hourly_sale
GROUP BY shift;

SELECT EXTRACT(HOUR FROM CURRENT_TIME);
