# sql_project_p1 (Retail Sales Analysis SQL Project)


**Project Title: Retail Sales Analysis**
Level: Beginner
Database: sql_project_p1

This project  demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal in starting the journey in data analysis and build a solid foundation in SQL.

**Objective**
Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
**Data Cleaning:** Identify and remove any records with missing or null values.
**Exploratory Data Analysis**(EDA): Perform basic exploratory data analysis to understand the dataset.
**Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

-- **Datbase_setup**

1. Database Setup
Database Creation: The project starts by creating a database named sql_project_p1.
**Table Creation:** A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

Create database sql_project_p1;

use sql_project_p1;

'''sql
create table retail_sales (
transactions_id int primary key ,
sale_date date ,
sale_time time,
customer_id int ,
gender varchar(10),
age int ,
category varchar(15),
quantity int ,
price_per_unit int ,
cogs float ,
total_sale float
);
'''

-- **Data Exploration & Cleaning**

Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.

delete  from retail_sales
where 
transactions_id is null 
or
sale_date is null 
or
sale_time is null 
or
customer_id is null 
or
gender is null 
or
age is null 
or
category is null
or 
quantiy is null 
or
price_per_unit is null 
or
cogs is null 
or
total_sale is null ;

-- **Data_exploration**

select count(*) as Total_sales from retail_sales;
select count(distinct customer_id) from retail_sales;
select count(distinct category) from retail_sales;

-- Data analysis & business problem

**-- Write a SQL query to retrieve all columns for sales made on '2022-11-05:**

'''sql

SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';

'''

**Data Analysis & Findings**

-- **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity 
-- sold is more than 4 in the month of Nov-2022:**

SELECT 
  * 
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    date_format(sale_date, '%Y-%m') = '2022-11'
    AND
    quantiy >=4;
    
   --  Write a SQL query to calculate the total sales (total_sale) for each category.:
   
   select category , sum(price_per_unit*quantiy) as total_sale  from retail_sales
   group by  category;

**-- Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:**

with cte as 
(SELECT age
    FROM
        retail_sales
    WHERE
        category = 'Beauty')
select    ROUND(AVG(age), 2) from cte;

SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'




**-- Write a SQL query to find all transactions where the total_sale is greater than 1000.:**

select * from retail_sales where total_sale>1000;


**-- Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:**

SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1;

**-- Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:**



with cte as(
select round(avg(total_sale),2) as avg_sales , month(sale_date) as  mon , year(sale_date) as  yer,
rank() over( partition by  year(sale_date) order by round(avg(total_sale),2) desc) as rank_1 from retail_sales
group by mon,yer) 

select avg_sales, mon, yer from cte 
where rank_1 =  1;

**-- **Write a SQL query to find the top 5 customers based on the highest total sales **:**

select customer_id , sum(total_sale) as sum_total
from retail_sales group by customer_id
order by  sum_total desc
limit  5;

**-- Write a SQL query to find the number of unique customers who purchased items from each category.:**

select count(distinct(customer_id)) , category 
from retail_sales group by category ;

**-- Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
-- Soultion 1**

select count(total_sale),
case
	when hour(sale_time) < '12:00:00' then 'Morning'
    when hour(sale_time) >= '12:00:00'  and hour(sale_time) <= '17:00:00' then 'Afternoon'
   else 'Evening'
   end as Shift
   from retail_sales
   group by Shift;
   
   
  **Soultion 2**
   
   WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;
    

**Findings**
Based on the sales data analysis, electronics emerged as the most sold category.
After analyzing the sales data, the average age of customers who purchased items from the 'Beauty' category was found to be 40 years. This finding highlights that middle-aged individuals constitute a key customer segment in this category, suggesting targeted marketing efforts could be effective in reaching this demographic.
July 2022 & February 2023 had the highest average sales in 2022, indicating a peak in consumer activity and overall transactions during this period.
Evening Shift (after 17:00) consistently attracted more customers and generated higher sales compared to the Morning and Afternoon shifts.This insight suggests that extending operational hours or focusing promotions during the evening could further capitalize on customer activity.





Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.






