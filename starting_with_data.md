Question 1: 
What is the most effective channel groups each visitor used to order products

SQL Queries:
SELECT COUNT(channelgrouping) AS channelgrouping_count, 
MAX(channelgrouping) OVER (PARTITION BY channelgrouping)
AS max_channelgrouping
FROM temp_analytics
GROUP BY channelgrouping
ORDER BY channelgrouping_count DESC
LIMIT 1

Answer: 
"channelgrouping_count"		"max_channelgrouping"
	2071832			   "Organic Search"

Organic search is the most used channel grouping which means that a 
significant portion of website traffic originates from search engines 
through unpaid, natural search results. This indicates that users are 
finding the website by conducting searches on search engines and clicking 
on the organic search listings


Question 2: 
Detect unique visitors that spent more than 2 hours on the site

SQL Queries:
SELECT DISTINCT (visitid), timeonsite
FROM temp_analytics
WHERE timeonsite > '02:00:00' ::time
ORDER BY timeonsite DESC

Answer:

"visitid"	"timeonsite"
1501207944	"03:08:36"
1500599111	"02:37:59"
1500449096	"02:30:51"
1498573175	"02:22:50"
1501537217	"02:19:29"
1501346674	"02:17:10"
1501106670	"02:12:23"
1495473851	"02:06:15"
1500674987	"02:00:13"

9 visitors spent more than 2 hours on site

Question 3: 
Which dates had the most numbers of units sold >10

SQL Queries:
SELECT DISTINCT(date), MAX(unit_sold) as max_unit_sold
FROM temp_analytics
WHERE unit_sold > 0
GROUP BY unit_sold, date
ORDER BY max_unit_sold desc
LIMIT 10

Answer:
date	     max_unit_sold
2017-06-08	4324
2017-07-21	1594
2017-05-25	1121
2017-05-22	1111
2017-06-10	1002
2017-06-21	1000
2017-06-28	880
2017-06-05	825
2017-06-08	825
2017-05-11	531

This summarizes that the maximum units of products were sold on the 8th of June, 2017

Question 4: 
what is the total product price for women's apparel suitable for home use? Specify the number of
items in each category.

SQL Queries:
WITH productcat_total_productprice AS
	(SELECT DISTINCT(productcategory), SUM(productprice) OVER (PARTITION BY productcategory)
	AS total_productprice, COUNT(productcategory) AS count_product
	FROM temp_all_sessions
	WHERE productcategory LIKE '%Home/Apparel/Women%'
	GROUP BY productcategory, productprice)
	
SELECT productcategory, total_productprice, SUM(count_product) as sum_of_counts
FROM productcat_total_productprice
GROUP BY productcategory, total_productprice
ORDER BY total_productprice DESC;

Answer:
productcategory	                                 total_productprice	sum_of_counts
Home/Apparel/Women's/Women's-Outerwear/	             1363820000	             95
Home/Apparel/Women's/	                              898720000	            146
Home/Apparel/Women's/Women's-Performance Wear/	      612920000	             46
Home/Apparel/Women's/Women's-T-Shirts/	              410250000	            360

The category with the hihest number of items is Women's T-shirts at 360


Question 5: 
Give a detailed explanation on the the output in question 4 and why determining the total product 
price in each category is necessary for businesses.

SQL Queries:
--
Answer:
The output from determining the total product price for women's apparel suitable for home use and the number of
items in each category basically gives a summary of the selling price for each category, regarless the counts
of items in each category. 
The output shows that women's t-shirts are the least expensive, irrespective of the number of 
items. However, women's outerwear is the most expensive which has one of the least number of items available for
purchase

Summarizing the total product price of a product category can provide several benefits and insights for a business. 
Here are some reasons why it is necessary:

1. Financial Analysis: Summarizing the total product price allows businesses to analyze the financial performance 
of specific product categories. By understanding the total revenue generated from each category, businesses can 
identify the most profitable categories and make informed decisions about resource allocation, inventory management, 
and pricing strategies.

2. Pricing Strategy: Analyzing the total product price by category helps businesses evaluate the pricing strategy 
for different product categories. It enables them to identify categories with higher or lower average prices, 
compare pricing across categories, and make adjustments to optimize profitability and competitiveness.

3. Product Mix Analysis: Summarizing the total product price by category provides insights into the composition 
of a business's product mix. It helps identify the contribution of each category to the overall revenue and assess 
the popularity and demand for different types of products. This analysis can guide decisions related to product 
development, marketing campaigns, and future product investments.

4. Performance Evaluation: By summarizing the total product price of a category, businesses can evaluate the 
performance of specific product lines or categories. It helps identify underperforming or overperforming categories, 
monitor trends over time, and take appropriate actions such as adjusting marketing efforts, improving product offerings, 
or discontinuing unprofitable categories.

5. Decision Making: Summarizing the total product price assists in informed decision making. 
Businesses can use the information to prioritize product categories, allocate resources effectively, 
identify opportunities for growth, and align their strategies with customer preferences and market trends.

6. Benchmarking: Summarizing the total product price by category allows businesses to benchmark their performance 
against competitors or industry standards. It helps identify areas where the business may be lagging or excelling 
in terms of pricing and revenue generation within specific categories.

Overall, summarizing the total product price of a product category provides valuable insights that can drive 
strategic decision making, improve financial performance, optimize pricing strategies, and align the business 
with customer needs and market dynamics.