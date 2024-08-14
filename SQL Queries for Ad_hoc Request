
-- 1. Provide the list of markets in which customer "Atliq Exclusive" operates its business in the APAC region.
SELECT 
DISTINCT market 
FROM  dim_customer
WHERE region = 'APAC' AND customer = "Atliq Exclusive";

/* 2. What is the percentage of unique product increase in 2021 vs. 2020? The
 final output contains these fields,
			unique_products_2020
			unique_products_2021
			percentage_chg */
            
WITH  cte20 as
(
	SELECT COUNT(DISTINCT(product_code)) as unique_products_2020
	FROM fact_gross_price as f 
	WHERE fiscal_year=2020
),
cte21 as
(
	SELECT COUNT(DISTINCT(product_code)) as unique_products_2021
	FROM fact_gross_price as f 
	WHERE fiscal_year=2021
)
SELECT *,
	ROUND((unique_products_2021-unique_products_2020)*100/unique_products_2020,2) as percentage_chg		
FROM cte20
CROSS JOIN
cte21;


/* 3. Provide a report with all the unique product counts for each segment and
sort them in descending order of product counts. The final output contains 2 fields,
		segment
		product_count */
        
SELECT segment, count(DISTINCT product_code) AS product_count
FROM dim_product
GROUP BY segment
ORDER BY product_count DESC;

/* 4. Follow-up: Which segment had the most increase in unique products in
2021 vs 2020? The final output contains these fields,
		segment
		product_count_2020
		product_count_2021
		difference */
WITH unique_product AS
(
 SELECT
      b.segment AS segment,
      COUNT(DISTINCT
          (CASE 
              WHEN fiscal_year = 2020 THEN a.product_code END)) AS product_count_2020,
       COUNT(DISTINCT
          (CASE 
              WHEN fiscal_year = 2021 THEN a.product_code END)) AS product_count_2021        
 FROM fact_sales_monthly AS a # cz fiscal year ka data isme hai iey ye table liya and dono me product code common hai
 INNER JOIN dim_product AS b
	ON a.product_code = b.product_code
 GROUP BY b.segment # humko compare karna hai 2020 ya 2021 mai segment vise jada hai unique_product
)
SELECT segment, product_count_2020, product_count_2021, (product_count_2021-product_count_2020) AS difference
FROM unique_product
ORDER BY difference DESC;

/* 5. Get the products that have the highest and lowest manufacturing costs.
The final output should contain these fields,
		product_code
		product
		manufacturing_cost */
/* iske liye we will use dim_product and fact_manufacturing_cost_ COMMON COLUMN is product_code*/
SELECT 
	m.product_code, 
	concat(product," (",variant,")") AS product,  # with the help of concat humne isme variant vo bhi add kiya..
	cost_year,manufacturing_cost
FROM fact_manufacturing_cost m
JOIN dim_product p 
	USING (product_code) # bcz joining column name same hai
WHERE manufacturing_cost= 
	(SELECT min(manufacturing_cost) FROM fact_manufacturing_cost)
	OR 
	manufacturing_cost = 
	(SELECT max(manufacturing_cost) FROM fact_manufacturing_cost) 
ORDER BY manufacturing_cost DESC;

/* 6. Generate a report which contains the top 5 customers who received an
average high pre_invoice_discount_pct for the fiscal year 2021 and in the 
Indian market. The final output contains these fields,
		customer_code 
		customer
		average_discount_percentage */
-- two columns here will be fact_pre_invoice_deduction and dim_customer (custome_code) is joining column
-- where me two conditions ayegi i- market = india(dim_cus)  ii- fiscal_year = 2021 (pre_invoice)
SELECT 
    c.customer_code, 
    c.customer, 
    ROUND(AVG(d.pre_invoice_discount_pct)*100, 2) AS average_discount_percentage
FROM fact_pre_invoice_deductions d
JOIN dim_customer c 
    ON d.customer_code = c.customer_code
WHERE 
    c.market = 'India' 
    AND d.fiscal_year = '2021'
GROUP BY 
    c.customer_code, 
    c.customer
ORDER BY 
    average_discount_percentage DESC
LIMIT 5;

/*7. Get the complete report of the Gross sales amount for the customer “Atliq
Exclusive” for each month. This analysis helps to get an idea of low and
high-performing months and take strategic decisions.
The final report contains these columns:
		Month
		Year
		Gross sales Amount */
/* two tables fact_gross_price and dim_customer cz specific atliq exclusive ka chahiye but no common column
for gross sales we need total sales quantity.. so we join with fact_sales_monthly.. 
plus isme dono column hai customer_code and product_code. so join becomes easy.. first fact_sales ko he join karege..
CTE (temp_table): This step calculates the gross_sales for each row but does not aggregate data.
Final Query: Aggregates the gross_sales using SUM, groups by year, month_number, and months, and formats the result. */

WITH temp_table AS (
    SELECT 
        c.customer,
        MONTHNAME(s.date) AS months,
        MONTH(s.date) AS month_number,
        YEAR(s.date) AS year,
        (s.sold_quantity * g.gross_price) AS gross_sales
    FROM fact_sales_monthly s
    JOIN fact_gross_price g 
        ON s.product_code = g.product_code
    JOIN dim_customer c 
        ON s.customer_code = c.customer_code
    WHERE c.customer = 'Atliq exclusive'
)
SELECT 
    months,
    year,
    CONCAT(ROUND(SUM(gross_sales) / 1000000, 2), 'M') AS gross_sales
FROM temp_table
GROUP BY 
    year, month_number, months
ORDER BY 
    year, month_number;
    
/* 8. In which quarter of 2020, got the maximum total_sold_quantity? The final
output contains these fields sorted by the total_sold_quantity,
		Quarter
		total_sold_quantity*/
SELECT 
	(CASE
		WHEN MONTH(date) IN (9,10,11) THEN 'Q1'  /* Atliq hardware has september as it's first financial month*/
		WHEN MONTH(date) IN (12,1,2) THEN 'Q2'
		WHEN MONTH(date) IN (3,4,5) THEN 'Q3'
		ELSE 'Q4'
	END) AS quarters,
	CONCAT(CAST(ROUND(SUM(sold_quantity)/1000000, 2) AS CHAR), " M") AS total_quantity_sold
FROM fact_sales_monthly
WHERE fiscal_year = 2020
GROUP BY quarters
ORDER BY total_quantity_sold DESC;

/* 9. Which channel helped to bring more gross sales in the fiscal year 2021
and the percentage of contribution? The final output contains these fields,
		channel
		gross_sales_mln
		percentage*/

WITH channel_tbl AS (
    SELECT 
        c.channel,
        SUM(s.sold_quantity * g.gross_price) AS total_sales
    FROM fact_sales_monthly s
    JOIN fact_gross_price g 
		ON s.product_code = g.product_code
    JOIN dim_customer c 
		ON s.customer_code = c.customer_code
    WHERE 
        s.fiscal_year = 2021
    GROUP BY c.channel
)
SELECT 
    channel,
    ROUND(total_sales / 1000000, 2) AS gross_sales_in_millions,
    ROUND((total_sales / SUM(total_sales) OVER()) * 100, 2) AS percentage
FROM channel_tbl
ORDER BY total_sales DESC;
m
/* 10. Get the Top 3 products in each division that have a high
total_sold_quantity in the fiscal_year 2021? The final output contains these
fields,
		division
		product_code
		product
		total_sold_quantity
		rank_order */

WITH temp_table AS (
    SELECT p.division,
           s.product_code,
           CONCAT(p.product, '(', p.variant, ')') AS product,
           SUM(s.sold_quantity) AS total_sold_quantity,
           RANK() OVER (PARTITION BY p.division ORDER BY SUM(s.sold_quantity) DESC) AS rank_order
    FROM fact_sales_monthly s
    JOIN dim_product p
    ON s.product_code = p.product_code
    WHERE s.fiscal_year = 2021
    GROUP BY p.division, s.product_code, p.product, p.variant
)
SELECT *
FROM temp_table
WHERE rank_order IN (1, 2, 3);


