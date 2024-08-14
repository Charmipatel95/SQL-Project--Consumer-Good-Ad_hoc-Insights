# CONSUMER GOODS AD-HOC INSIGHTS
## PROJECT OVERVIEW
Atliq Hardwares (imaginary company) is one of the leading computer hardware producers in India and well expanded in other countries too.
However, the management noticed that they do not get enough insights to make quick and smart data-informed decisions. They want to expand their data analytics team by adding several junior data analysts. Tony Sharma, their data analytics director wanted to hire someone who is good at both tech and soft skills. Hence, he decided to conduct a SQL challenge which will help him understand both the skills.

:link: Link to the [Challenge](https://codebasics.io/challenge/codebasics-resume-project-challenge)

:link: [LinkedIn Post](   )

Here's a rundown of everything I've learned:
## :mag: Key Skills:
- Using SQL for basic and advance queries
- PowerBi Desktop for creating visualisation and presenting meaningful insights from them
- DAX language for advanced calculations
- Using Canva and Microsoft PowerPoint for presenting the visualisation

## :bulb: SQL Concepts:
- Clauses : SELECT , WHERE, GROUPBY, ORDERBY, LIMIT
- Aggregate functions: SUM(), MIN(), MAX(),AVG()
- Windows functions: RANK(), PARTITION BY
- Common table Expressions (CTE) and Sub queries
- Joins for combining data from multiple tables

## :memo:PROBLEM STATEMENT
To generate and present meaningful insights to top management, the Data Analytics team has been given the following task.

1.    Check ‘ad-hoc-requests.pdf’ - there are 10 ad hoc requests for which the business needs insights.
2.    You need to run a SQL query to answer these requests. 
3.    The target audience of this dashboard is top-level management - hence you need to create a presentation to show the insights.
4.    Be creative with your presentation, audio/video presentation will have more weightage.

Other resources Provided:

a.    Dataset required to provide 

b.    Metadata

c.    Hints (try not to use the hints to develop your skills quicker)

d.    Sample questions and Presentation
 
## :open_file_folder:UNDERSTANDING DATA SET
The dataset provides a comprehensive overview of the tables found in the 'gdb023' (atliq_hardware_db) database. It includes information for six main tables:

1. dim_customer: contains customer-related data
2. dim_product: contains product-related data
3. fact_gross_price: contains gross price information for each product
4. fact_manufacturing_cost: contains the cost incurred in the production of each product
5. fact_pre_invoice_deductions: contains pre-invoice deductions information for each product
6. fact_sales_monthly: contains monthly sales data for each product.

## :man_in_tuxedo:Column Description for dim_customer table:
1. customer_code: The 'customer_code' column features unique identification codes for every customer in the dataset. These codes can be used to track a customer's sales 		history, demographic information, and other relevant details. For example, the codes could look like '70002017', '90005160', and '80007195' respectively.

2. customer: The 'customer' column lists the names of customers, for example 'Atliq Exclusive', 'Flipkart', and 'Surface Stores' etc.

3. platform: The 'platform' column identifies the means by which a company's products or services are sold. "Brick & Mortar" represents the physical store/location, and 			"E-Commerce" represents online platforms.

4. channel: The 'channel' column reflects the distribution methods used to sell a product. These methods include "Retailers", "Direct", and "Distributors". Retailers 				refer to physical or online stores that sell products to consumers. Direct sales refer to sales made directly to consumers through a company's website or other direct means, and distributors refer to intermediaries or middlemen between the manufacturer and retailer or end consumers.

5. market: The 'market' column lists the countries in which the customer is located.

6. region: The 'region' column categorizes countries according to their geographic location, including "APAC" (Asia Pacific), "EU" (Europe), "NA" (North America), and 			    "LATAM" (Latin America).

7. sub_zone: "The 'sub_zone' column further breaks down the regions into sub-regions, such as "India", "ROA" (Rest of Asia), "ANZ" (Australia and New Zealand), "SE" 				  Southeast Asia), "NE" (Northeast Asia), "NA" (North America), and "LATAM" (Latin America)."

## :shopping:Column Description for dim_product table:
1. product_code: The 'product_code' column features unique identification codes for each product, serving as a means to track and distinguish individual products within a 		database or system.

2. division: The 'division' column categorizes products into groups such as "P & A" (Peripherals and Accessories), "N & S" (Network and Storage) and "PC" (Personal 				 Computer).

3. segment: The 'segment' column categorizes products further within the division, such as "Peripherals" (keyboard, mouse, monitor, etc.), "Accessories" (cases, cooling 			solutions, power supplies), "Notebook" (laptops), "Desktop" (desktops, all-in-one PCs, etc), "Storage" (hard disks, SSDs, external storage), and "Networking" (routers, switches, modems, etc.).

4. category: The 'category' column classifies products into specific subcategories within the segment.

5. product: The 'product' column lists the names of individual products, corresponding to the unique identification codes found in the 'product_code' column.

6. variant: The "variant" column classifies products according to their features, prices, and other characteristics. The column includes variants such as "Standard", 				"Plus", "Premium" that represent different versions of the same product.

## :money_with_wings:Column Description for fact_gross_price table:
1. product_code: The 'product_code' column features unique identification codes for each product.

2. fiscal_year: The 'fiscal_year' column contains the fiscal period in which the product sale was recorded. A fiscal year is a 12-month period that is used for accounting 			purposes and often differs from the calendar year. For Atliq Hardware, the fiscal year starts in September. The data available in this column covers the 				fiscal years 2020 and 2021.

3. gross_price: The 'gross_price' column holds the initial price of a product, prior to any reductions or taxes. It is the original selling price of the product.

## :moneybag:Column Description for fact_manufacturing_cost:
1. product_code: The 'product_code' column features unique identification codes for each product

2. cost_year: The "cost_year" column contains the fiscal year in which the product was manufactured.

3. manufacturing_cost: The "manufacturing_cost" column contains the total cost incurred for the production of a product. This cost includes direct costs like
raw materials, labor, and overhead expenses that are directly associated with the production process.

## 📝Column Description for fact_pre_invoice_deductions:
1. customer_code: The 'customer_code' column features unique identification codes for every customer in the dataset. These codes can be used to track a customer's sales history, demographic information, and other relevant details. For example, the codes could look like '70002017', '90005160', and '80007195' respectively.

2. fiscal_year: The "fiscal_year" column holds the fiscal period when the sale of a product occurred.

3. pre_invoice_discount_pct: The "pre_invoice_discount_pct" column contains the percentage of pre-invoice deductions for each product. Pre-invoice deductions are 
discounts that are applied to the gross price of a product before the invoice is generated, and typically applied to large orders or 							     long-term contracts.

## :label:Column Description for fact_sales_monthly:
1. date: The "date" column contains the date when the sale of a product was made, in a monthly format for 2020 and 2021 fiscal years. This information can be used
to understand the sales performance of products over time.

2. product_code: The "product_code" column contains a unique identification code for each product. This code is used to track and differentiate individual 
products within a database or system.

3. customer_code: The 'customer_code' column features unique identification codes for every customer in the dataset. These codes can be used to track a customer's sales 			history, demographic information, and other relevant details. For example, the codes could look like '70002017', '90005160', and '80007195' respectively.

4. sold_quantity: The "sold_quantity" column contains the number of units of a product that were sold. This information can be used to understand the sales volume ofproducts and to compare the sales volume of different products or variants.

5. fiscal_year: The "fiscal_year" column holds the fiscal period when the sale of a product occurred.

## :bar_chart: PROVIDED MOCK-UP PRESENTATION FOR THE END RESULT
![sample pic](https://github.com/user-attachments/assets/5d1c6d7a-e8b3-4c5d-a72b-600d0ff0728e)
![sample 2](https://github.com/user-attachments/assets/afa008a2-b4af-4eea-bae6-a0739f642951)

## :chart_with_upwards_trend:	Data Model
![dm_1](https://github.com/user-attachments/assets/79613bb2-0d84-461d-b778-5c7a7a241fd0)

## 📶OVERALL OUTPUT AND INSIGHTS


## ✍🏻SOME IMPORTANT INSIGHTS FROM THE DASHBOARD

Here are some key insights gleaned from the Dashboard:

- Unique products demand increased by 36.33% from Fiscal year 2020 to 2021.
- Notebooks, accessories, and peripherals are demonstrating substantial growth in manufacturing( its 83% of our total manufactured product)
- Accessories segment shows the most increase in production in FY 2021 compared to FY 2020.
- Flipkart received the highest average pre-invoice discounts of 30.83% while Amazon received least.
- In year 2020, Quarter 1 had the highest product sold quantity where as Quarter 3 had the lowest sold quantity.
- The Retailer channel contributes 73.22% to the total gross Sales making them the largest contributor to the overall sales figure.

