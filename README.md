Retail Sales Analysis Using SQL

This project explores real-world retail sales data and performs data cleaning, exploration, and analytical SQL operations.
It includes queries that help understand customer behavior, product performance, seasonal trends, and operational insights.

📁 Project Files

retail_sale_analysis.sql → Contains all SQL queries

Table creation

Data cleaning

Exploratory queries

Business analysis queries

 Database Structure
        transactions_id   INT PRIMARY KEY,
        
        sale_date         DATE,
        
        sale_time         TIME,
        
        customer_id       INT,
        
        gender            VARCHAR(20),
        
        age               INT,
        
        category          VARCHAR(20),
        
        quantity          INT,
        
        price_per_unit    FLOAT,
        
        cogs              FLOAT,
       
       total_sale        FLOAT

🧹 Data Cleaning Performed

Checked for missing values

Removed rows containing NULL in critical fields

Verified data completeness and quality

🔍 Data Exploration

Some essential exploratory insights:

✔ Total number of sales


SELECT COUNT(*) FROM retail_sales;

✔ Unique customers

SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

✔ Distinct product categories

SELECT DISTINCT category FROM retail_sales;

📈 Business Analysis & SQL Queries
1️⃣ Sales on a specific date (2022-11-05)

SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';

2️⃣ Clothing category transactions with quantity >4 in Nov 2022

SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;

3️⃣ Total sales by category

SELECT category, SUM(total_sale) AS net_sale
FROM retail_sales
GROUP BY category;

4️⃣ Average age of customers buying Beauty products

SELECT AVG(age) 
FROM retail_sales
WHERE category = 'Beauty';

5️⃣ Transactions with sale amount >1000

SELECT transactions_id, total_sale
FROM retail_sales
WHERE total_sale > 1000;

6️⃣ Number of transactions by gender and category

SELECT gender, category, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY gender, category
ORDER BY category;

7️⃣ Average sale per month & best-selling month of each year

SELECT  
    year,
    month,
    avg_sale
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
) AS t
WHERE rank = 1;

8️⃣ Top 5 customers by total sales

SELECT customer_id, SUM(total_sale) AS total_sale
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sale DESC
LIMIT 5;

9️⃣ Number of unique customers by category

SELECT category, COUNT(DISTINCT customer_id) AS custmr_id_count
FROM retail_sales
GROUP BY category;

🔟 Orders by shift (Morning, Afternoon, Evening)

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

🏁 Conclusion

This SQL project provides solid insights into sales patterns, customer demographics, product performance, and operational timing.
