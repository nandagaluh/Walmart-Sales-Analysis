# Walmart-Sales-Analysis
CREATE DATABASE IF NOT EXISTS Digital; 
USE Digital;

Create table if not exists sales(
	invoice_id VARCHAR(30) NOT NULL PRIMARY KEY,
    branch VARCHAR(5) NOT NULL,
    city VARCHAR(30) NOT NULL,
    customer_type VARCHAR(30) NOT NULL,
    gender VARCHAR(10) NOT NULL,
    product_line VARCHAR(100) NOT NULL,
    unit_price DECIMAL(10, 2) NOT NULL,
    quantity INT NOT NULL,
    VAT FLOAT(6,4) NOT NULL,
    total DECIMAL(12, 4) NOT NULL,
    date DATETIME NOT NULL,
    time TIME NOT NULL,
    payment_method VARCHAR(15) NOT NULL,
    cogs DECIMAL(10, 2) NOT NULL,
    gross_margin_pct FLOAT(11, 9),
    gross_income DECIMAL(12, 4) NOT NULL,
    rating FLOAT(2,1)
);
    
-- ------------------------------------------------------
-- ------------------ FUTURE ENGINEERING -----------------
-- ---- time_of_day

SELECT 
	time,
    CASE
		WHEN time  BETWEEN "00:00:00" AND "12:00:00" THEN "Morning"
        WHEN time BETWEEN "12:01:00" AND "16:00:00" THEN "Afternoon"
        ELSE "Evening"
	END AS time_of_date
FROM sales;

ALTER TABLE sales ADD COLUMN time_of_day VARCHAR (20)

UPDATE sales
SET time_of_day = CASE
    WHEN time >= '00:00:00' AND time < '12:00:00' THEN 'Morning'
    WHEN time >= '12:00:00' AND time < '17:00:00' THEN 'Afternoon'
    ELSE 'Evening'
END;

-- day_name
SELECT
	date,
    DAYNAME(date)
FROM sales;

ALTER TABLE sales ADD COLUMN day_name VARCHAR(10);

SELECT
	date,
    DAYNAME(date) AS day_name
FROM sales;

UPDATE sales
SET day_name = DAYNAME(date) ;

-- month name
SELECT 
	date,
    MONTHNAME(date)
FROM sales;

ALTER TABLE sales ADD COLUMN month_name VARCHAR(10);

UPDATE sales 
SET month_name = MONTHNAME (date);

-- EDA
-- How many unique cities does the data have ?
SELECT
	DISTINCT city
FROM sales;

-- how many branch ?
SELECT
	DISTINCT branch
FROM sales;

SELECT
	DISTINCT city,
    branch
FROM sales;

-- product no 1
SELECT 
	COUNT(DISTINCT product_line)
FROM sales;

-- product no 2
SELECT
	payment_method,
	COUNT(payment_method) AS cnt
FROM sales
GROUP BY payment_method
ORDER BY cnt DESC;

-- what is the mosy selling product line?
SELECT 
*
FROM
Sales;

SELECT
	product_line,
	COUNT(product_line) AS cnt
FROM sales
GROUP BY product_line
ORDER BY cnt DESC;

-- what is total revenue by month ?
SELECT 
	month_name AS month,
    SUM(total) AS total_revenue
FROM sales
GROUP BY month_name
ORDER BY total_revenue DESC;

-- what month the largest COGS?
SELECT
	month_name AS month,
    SUM(cogs) AS cogs
FROM sales
GROUP BY month_name
ORDER BY cogs DESC;

-- what product line had the largest revenue?
SELECT 
	product_line,
    SUM(total) AS total_revenue
FROM sales
GROUP BY product_line
ORDER BY total_revenue DESC;

-- what is the city the largest revenue
SELECT 
	branch,
    city,
    SUM(total) AS total_revenue
FROM sales
GROUP BY city, branch
ORDER BY total_revenue DESC;

-- what product_line had the largest VAT?

SELECT 
	Product_line,
    AVG(VAT) AS AVG_Tax
FROM sales
GROUP BY product_line
ORDER BY avg_tax DESC;

-- which brand sold more product than avg product sold?
SELECT
	branch,
    SUM(quantity) AS qty
FROM sales
GROUP BY branch
HAVING SUM(quantity) > (SELECT AVG(quantity) FROM sales);
	
-- what is the most common product line by gender?
SELECT
	gender,
    product_line,
    COUNT(gender) AS total_cnt
FROM sales
GROUP BY gender, product_line
ORDER BY total_cnt DESC;

-- what is the average rating of each product_line
SELECT
	ROUND(AVG(rating), 2) AS avg_rating,
    product_line
FROM sales
GROUP BY product_line
ORDER BY avg_rating DESC;

-- ---------------------------------------------------------
-- -------------- SALES --------------------

-- Number of sales made in each time of the day per weekday
SELECT
	time_of_day,
    COUNT(*) AS total_sales
FROM SALES
WHERE day_name = "Monday"
GROUP BY time_of_day
ORDER BY total_sales DESC;

-- Which of the customer types brings the most revenue?
SELECT
	customer_type,
    SUM(total) AS total_revenue
FROM sales
GROUP BY customer_type
ORDER BY total_revenue DESC;

-- Which city has the largest tax percent/ VAT (Value Added Tax)?
SELECT 
	city,
    AVG(VAT) AS AVG_Tax
FROM sales
GROUP BY city
ORDER BY avg_tax DESC;



