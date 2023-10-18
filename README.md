# Final-graduation-project

## Table of Contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools Used](#tools-used)
- [Data Cleaning and Preparation](#data-cleaning-preparation)
- [Exploration Data Analysis](#exploration-data-analysis)
- [Data Analytics](#data-analytics)
- [Result/ Findings](#result-findings)
- [Recommendation](#recommendation)
- [Limitations](#limitations)
## Project Overview
Global Superstore is a global online retailer based in New York, boasting a broad
product catalogue and aiming to be a one-stop-shop for its customers. Global
The superstore’s clientele, hailing from 147 different countries, can browse through an
endless offering with more than 10,000 products. This large selection comprises three
main categories: office supplies (e.g., staples), furniture (e.g., chairs), and technology
(e.g., smartphones).

The data analysis ales to provide insights into the sales performance of the above named company over the past years. By analysing various aspect of the sales data, we seek to identify trends, make data driven recommendation, and gain a deeper understanding of the company's performance.

## Data Source

The primary dataset used for this analysis is the "Order,people and Return-data.csv" file containing detailed information about each orders, sales, return made by the GlobalSuper Store.

## Tools Used 

- Excel - Data Cleaning

- SQL Server- Data Analytsis

- Power Bi - Creating reports

## Data Cleaning and Preparation

In the initail data preparation phase, i performed the following tasks.

1.	Data loading and inspection

2.	Handling the missing values 

3.	Data cleaning and formatting.

## Exploration Data Analysis (EDA)

Question 1.

a) What are the three countries that generated the highest total profit for Global
Superstore in 2014?

b) For each of these three countries, find the three products with the highest total profit.
Specifically, what are the products’ names and the total profit for each product?

Question 2.

Identify the 3 subcategories with the highest average shipping cost in the United States

Question 3.

a) Assess Nigeria’s profitability (i.e., total profit) for 2014. How does it compare to other
African countries?

b) What factors might be responsible for Nigeria’s poor performance? You might want to
investigate shipping costs and the average discount as potential root causes.

Question 4.

a) Identify the product subcategory that is the least profitable in Southeast Asia.
Note: For this question, assume that Southeast Asia comprises Cambodia,
Indonesia, Malaysia, Myanmar (Burma), the Philippines, Singapore, Thailand, and
Vietnam.

b) Is there a specific country in Southeast Asia where Global Superstore should stop
offering the subcategory identified in 4a?

Question 5.

a) Which city is the least profitable (in terms of average profit) in the United States? For
this analysis, discard the cities with less than 10 Orders.

b) Why is this city’s average profit so low?

Question 6.

a) Which product subcategory has the highest average profit in Australia?

Question 7.

a) Which customer returned items and what segment do they belong

b) Who are the most valuable customers and what do they purchase?
nalysis.

## Data Analytics
``` SQL
SELECT country, SUM(profit) AS Total_profit
FROM Orders
WHERE EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014
GROUP BY Country
ORDER BY Total_profit DESC
LIMIT 3;
```

/* The three countries that generated the highest profit for Global Superstore in 2014 were

1. United State with total profit of $65851
2. India - $32486
3. China - $29380

*/
	

---For Uinted States

```SQL
SELECT product_name, SUM(profit) AS total_profit
FROM orders 
WHERE country = 'United States' AND EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014
GROUP BY product_name
ORDER BY total_profit DESC
LIMIT 3;
```


---For India

```SQL
SELECT product_name, SUM(profit) AS total_profit
FROM orders 
WHERE country = 'India' AND EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014
GROUP BY product_name
ORDER BY total_profit DESC
LIMIT 3;
```

/* For India, the top three product with the highest total profits are 

1. Sauder Classic Bookcase, Traditional- $2419
2. Hamilton Beach Refrigerator, Red- $1440
3. Bevis Round Table, Rectangular ID - $1423

*/

-- For China 

```SQL
SELECT product_name, SUM(profit) AS total_profit
FROM orders 
WHERE country = 'China' AND EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014
GROUP BY product_name
ORDER BY total_profit DESC
LIMIT 3;
```

/* For China, the top three product with the highest toal profits are 

1. HP Copy Machine, Color - $1196
2. HP Fax and Copier, Digital - $1134
3. Samsung Smart Phone, VoIP - $1068

*/

```SQL
SELECT* FROM orders  

SELECT Sub_category, AVG(Shipping_cost) AS avg_shipping_cost
FROM Orders
WHERE Country ='United States'
GROUP BY Sub_category
ORDER BY avg_shipping_cost DESC
Limit 3;

```

/* The three sub categories with the highest average shipping cost in the United States are 

1. Copiers - avg of 165
2. Machines - avg of 132
3. Tables - avg of 69

```SQL
SELECT country, SUM(profit) AS Nigeria_total_profit_2014
FROM Orders
WHERE Country ='Nigeria' AND EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014
GROUP BY country 
ORDER BY Nigeria_total_profit_2014 DESC
```
--Nigeria made a total profit of -14968.. 


/* To compare Nigeria to other African Countries, I created a table for all the African countries, ordered 
the total profit of 2014 to return in a decending order in order to see who toped the list and who
made the least profit,Nigerai appeared to made the least profit with $-14968. Then used Nigeria (-14968) 
to ran a query to see those greater than Nigeria. 
South Africa, DR Congo, Morocco, Egypt, Senegal and Cote d'Ivoire toped the African Region 
with the highest profit in 2014 */ 


```SQL
CREATE TABLE African_Country_total_profit_2014 AS ( SELECT Country, SUM (profit)AS Total_profit_2014
FROM Orders
WHERE EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014 
AND Region = 'Africa'
GROUP BY Country
ORDER BY Total_profit_2014 DESC			   
				)
```
								  

``` SQL
SELECT * FROM African_Country_total_profit_2014 


SELECT Country, total_profit_2014
FROM African_Country_total_profit_2014
WHERE Total_profit_2014 >-14968 --Nigeria's total_profit for 2014
ORDER BY Total_profit_2014 DESC;

SELECT * FROM orders
```


--Checking numbers of months each African countries operated 

```SQL
SELECT DISTINCT country, ship_date FROM orders
WHERE EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014 AND Region='Africa'
```

-- For Each Country

```SQL
SELECT DISTINCT country, ship_date FROM orders
WHERE EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014 AND Country = 'Cote d''Ivoire'
--Change for each counrty
```


```SQL
SELECT Country, AVG (shipping_cost) AS Avg_shipping_cost, AVG(discount) AS avg_discount
FROM orders
WHERE EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014 AND Region ='Africa'
GROUP BY country
ORDER by avg_shipping_cost DESC
```

```SQL
SELECT Country, AVG (shipping_cost) AS Avg_shipping_cost, AVG(discount) AS avg_discount
FROM orders
WHERE EXTRACT (YEAR FROM TO_DATE(Order_date, 'M/D/YYYY'))= 2014 AND Region ='Africa'
GROUP BY country
ORDER by avg_discount  DESC
```

/* From the above analysis, Nigeria shipped more than other countries in the space of 9 months
but reciveced a higher avg discount of o.70002 which probably affected her market in cause of doller 
rate at the time of payment. The higher discount indicates a greater level of risk associated with an 
invesment and its future cash flow. */

```SQL
SELECT * FROM Orders

SELECT Sub_category, MIN(profit) AS Total_Min_profit_South_Asia
FROM orders
WHERE Region = 'Southeast Asia' --IN('Cambodia', 'Indonesia', 'Malaysia', 'Myanmar (Burma)', 
'Philippines',--'Singapore', ' Thailand', 'Vietnam' )
GROUP BY Sub_category
ORDER BY Total_Min_profit_South_Asia DESC
LIMIT 1;
```
-- Answer-  The sub category with the least profitable is Labels with least total profit of-35.568
 
```SQL
SELECT Country, MIN(profit) AS Labels_least_profit
FROM orders
WHERE region = 'Southeast Asia' AND sub_category ='Labels'
GROUP BY Country
ORDER BY Labels_least_profit ASC
```

--Answer
/* From the above analysis
Global SuperStore should consinder stopping the shipping of Labels to Thialand due it least profit 
generated */

```SQL
SELECT City, AVG(profit) AS City_least_Avg_Profit_USA
FROM orders
WHERE Country='United States'
GROUP BY City
ORDER BY City_least_Avg_Profit_USA ASC
LIMIT 1;
```

--The city with least Average profit is Bethlehem with the average profit of -200.61916


--Why Bethlehem had a lower sale 

```SQL
SELECT City,AVG (profit) AS Avg_city_profit, SUM(sales) Sum_of_city_sales, SUM(shipping_cost) AS
Sum_of_shipping_cost, COUNT (quantity) AS Count_quantity
FROM Orders
WHERE Country = 'United States'AND city = 'Bethlehem'
GROUP BY City
ORDER BY Sum_of_city_sales DESC
```

/*From the analysis above it was observed that
Bethlehem with the average profit of -200.61916  made a total sale of 1689.634 with and average discount of
351.75 for just 5 quantities of items compared to other cities who got more more quantities of goods.
thus their average profit above other top cities */

-- Cities with less than 10 orders are 530 in the to united states.. Below is a table which discards them all

```SQL
CREATE TABLE Cities_with_less_than_10_orders_USA AS (SELECT City, AVG(profit) AS least_Avg_Profit
FROM orders
WHERE Country='United States' AND Quantity < 10
GROUP BY City
--HAVING COUNT (*) <10
ORDER BY least_Avg_Profit ASC )

```
										
											
```SQL
SELECT * FROM Cities_with_less_than_10_orders_USA 



/*SELECT City --AVG (profit) AS Cities_With_Less_than_10_orders_USA
FROM  Cities_with_less_than_10_orders_USA
WHERE country = 'United State' AND quantity < 10
GROUP BY City
--HAVING COUNT(*) <10
ORDER BY Cities_With_Less_than_10_orders_USA ASC */

```
```SQL
SELECT Sub_category, AVG (profit) AS Avg_profit_Australia
FROM Orders
WHERE Country = 'Australia'
GROUP BY Sub_category
ORDER BY Avg_profit_Australia DESC
LIMIT 1;
```

/*Answer - Appliances has the the highest average profit in Australia with an average profit of 
139.  */ 

---7.
```SQL
SELECT * FROM Returns
SELECT * FROM People
SELECT * FROM Orders
```

/* So to get the customers that returned itmes ( probabily the highest returned) I joined 
the orders table and the return table in other to see the custusmer names, segment and the product they 
returned. After joining both tables i turned them to a new table to enable me count the most returned 
items and the customer who returned them. I called my new table Count returns*/

```SQL
CREATE TABLE Count_returns  AS (SELECT o.customer_name, o.product_name, o.segment, r.returned  --, p.person
FROM Orders AS o
INNER JOIN Returns AS r
ON o.order_id = r.order_id
--LEFT JOIN People AS p
--ON o.region = p.region 
WHERE Returned = 'Yes')
```


```SQL
SELECT * FROM  Count_returns
```

--From my new  table returns, i ran an analysis to see the customer with most returns 

```SQL
SELECT Customer_name, COUNT(Returned) AS Total_product_returned
FROM Count_returns
GROUP BY Customer_name
ORDER BY Total_Product_returned DESC 
LIMIT 1;
```

-- Ted Butterfield happend to be the customer who returned more items 31times  than others

--TO find his/her segment

```SQL
SELECT Customer_name, Segment
FROM Count_returns
WHERE Customer_name LIKE 'Ted%'
```

/*From the above analysis, Ted Butterfield who belong to the Consumer segment returned 
about 31 items in total */


```SQL
SELECT customer_name, product_name, COUNT(*) AS purchase_count
FROM orders
GROUP BY customer_name, product_name
ORDER BY purchase_count DESC
LIMIT 18;
```



## Result/ Findings.

The analysis results are summerized as follows:

The three countries that generated the highest profit for Global Superstore in 2014 were

1. United State with total profit of $65851
2. India - $32486
3. China - $29380

The three sub categories with the highest average shipping cost in the United States are 

1. Copiers - avg of 165
2. Machines - avg of 132
3. Tables - avg of 69

To compare Nigeria to other African Countries, I created a table for all the African countries, ordered 
the total profit of 2014 to return in a decending order in order to see who toped the list and who
made the least profit,Nigerai appeared to make the least profit with $-14968. Then used Nigeria (-14968) 
to ran a query to see those greater than Nigeria. 
South Africa, DR Congo, Morocco, Egypt, Senegal and Cote d'Ivoire toped the African Region 
with the highest profit in 2014. The access the caused of the lower sales in i comparaed Number of times and months top 5 African countries shipped compared to Nigeria

- Angola - 6 months they shipped 14 times
- Algeria - 9 months and 24 times
- Benin 4 months and 7 times 
- Cote d'Ivoire - 8 months and 15 times 
- Democratic Republic of the Congo - 9 months and 43 times 
- Egypt - 9 months 55 times 
- Nigeria - 10 months and 82 times 
 
From the above analysis, Nigeria shipped more than other countries in the space of 9 months
but reciveced a higher avg discount of o.70002 which probably affected her market in cause of doller 
rate at the time of payment. The higher discount indicates a greater level of risk associated with an 
invesment and its future cash flow.

- In Southeast Asia The sub category with the least profitable is Labels with least total profit of-35.568, A need for promotion of label is needed else the supply should be reduced pending when the demand becomes higher.


## Recommendation 

Based on the analysis, i recommend the following:

Global Supperstore should consider pumping more products into the market of the top three countries namely USA, India and China.

- As Southeast the the product Label should be less supplied mainly to Thialand cos of its lowest profit.

- Appliances should be supplied more to Australia for highest average profit made previously.

- In the Africa region least products should bve suplied to the countries with less profit for the main, keep a track on their marketing proress.

- From returned, Staples has more number of sales from the top 18 customers more than other products. The product should be supplied more.

- In the USA From the analysis above 
Bethlehem with the average profit of -200.61916  made a total sale of 1689.634 with and average discount of
351.75 for just 5 quantities of items compared to other cities who got more more quantities of goods.
thus their average profit above other top cities.

- The company should focus on expanding and promoting in  AFrica and Asia more.
- Implement a customer segmentation strategy to terget high LTV customers efffectively.


## Limitations.

- Had to change the file type, date etc to load into PostgreSQL
- Loaded the data into SQl in three parts. Orders, people, Return
- for my analysis to correspond.
- filterd out some columns but even then we can still see that there is a positive correlation betwwen the tables.
- During my anaylis i created seperate tables to eneble me solve some casese.
- During report on my Bi i connected these several tables through data modeling.












