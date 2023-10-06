Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT tas.country, tas.city, MAX (ta.unit_price * tp.orderedquantity)
AS max_transactionrevenues 
FROM temp_all_sessions tas
JOIN temp_products tp
ON tas.productsku = tp.productsku
JOIN temp_analytics ta
ON tas.visitid = ta.visitid
WHERE ta.unit_price * tp.orderedquantity = (SELECT max (ta.unit_price * tp.orderedquantity)											
					    FROM temp_all_sessions tas
					    JOIN temp_products tp
					    ON tas.productsku = tp.productsku
					    JOIN temp_analytics ta
					    ON tas.visitid = ta.visitid)		
GROUP BY tas.country, tas.city, ta.unit_price * tp.orderedquantity;


Answer:
To get the cities and countries with the highest level of transaction revenues, the 'totaltransactionrevenue'
column in the All_sessions table could not be used because only 81 values out of 15134 were given, which would not give
a meaningful data. Therefore to  get the transaction revenue, the unit_price from Analytics Table, orderedquantity from
Products Table with the corresponding cities and countries from All_sessions table.

The subquery in the WHERE function gives the maximum transaction revenue in the database, whereby a determining
the corresponding city/country to the maximum transaction revenue gotten from the nested subquery

The country and Cities with the highest revenue are;
Country = United States of America, City = Chigaco
Country = United States of America, City = Newyork 
Highest transaction revenue = 2,199,650 USD

"country"	  "city"	"max_transactionrevenues"
"United States"	 "Chicago"	2199650
"United States"	 "New York"	2199650

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
SELECT tas.city, tas.country, CAST(AVG(tp.orderedquantity) as numeric (5,0)) 
                AS avg_orderedquantity
FROM temp_all_sessions tas 
JOIN temp_products tp
ON tp.productsku = tas.productsku
JOIN temp_analytics ta
ON tas.visitid = ta.visitid
WHERE tp.orderedquantity >0
GROUP BY tas.city, tas.country
ORDER BY tas.city, tas.country


Answer:
"city"	         "country"	    "avg_orderedquantity"
"Adelaide"	"Australia"			54
"Ahmedabad"	"India"				923
"Amsterdam"	"Netherlands"			685
"Ann Arbor"	"United States"			360
"Athens"	"Greece"			899
"Atlanta"	"United States"			427
"Austin"	"United States"			568
"Bangkok"	"Thailand"			731
"Barcelona"	"Spain"	        		296
"Beijing"	"China"	         		16

From each city and country, the average number of products ordered from visitors (visitors that
visited the page to place an order = visitid) are partitioned by each city and country, giving a 
total of 240 unique city and countries, showing the ouput to a LIMIT of 10.
The ALL_Session table, Analytics and Products table were joined using Productsku as the unique
column between all_sessions and Products table. Aswell as visitid as the unique column between
All_sessions table and analytics table.
filtering out ordered quantity < 0 (i.e no order was made).



**Question 3: Is there any pattern in the types (product categories) of products ordered 
from visitors in each city and country?**


SQL Queries:
SELECT DISTINCT(tas.productcategory), tas.city, tas.country, ta.visitid, SUM(tp.orderedquantity)
                  OVER (PARTITION BY tas.productcategory) AS sum_orderedquantity
   FROM temp_all_sessions tas
   JOIN temp_products tp
   ON tas.productsku = tp.productsku
   JOIN temp_analytics ta
   ON tas.visitid = ta.visitid
   WHERE tas.productcategory LIKE '%Home/Apparel/Men%' AND tp.orderedquantity > 0
   ORDER BY tas.country, tas.city, tas.productcategory


Answer:
The obvious pattern is the Hierachical categorization of Products, which can be determined
by viewing the data format in excel.
However, Using the LIKE function to determine the most ordered product category by indivdual visitor 
that placed an order (visitid). 
The analysis showed that Men Apparel suitable for home use were mostly ordered at 646 rows
Followed by the Women apparel at 139 and the kids apparel at 69.

MEN
"productcategory"	                  "city"	"country"	"visitid"	"sum_orderedquantity"
"Home/Apparel/Men's/"	                   "n/a"	"Albania"	 1497024296	           494130
"Home/Apparel/Men's/Men's-T-Shirts/"	   "n/a"	"Argentina"	 1499709886	           1660288
"Home/Apparel/Men's/"	                  "Rosario"	"Argentina"	 1496844423	           494130
"Home/Apparel/Men's/Men's-Outerwear/"	  "Adelaide"	"Australia"	 1496029499	            98336
"Home/Apparel/Men's/Men's-T-Shirts/"	  "Brisbane"	"Australia"	 1500871622	           1660288
"Home/Apparel/Men's/"	                   "n/a"	"Australia"	 1498221087	            494130
"Home/Apparel/Men's/Men's-Outerwear/"	   "n/a"	"Australia"	 1498976294	            98336
"Home/Apparel/Men's/"			  "Sydney"	"Australia"	 1495082657	            494130
"Home/Apparel/Men's/"			  "Sydney"	"Australia"	 1499517670	            494130
"Home/Apparel/Men's/Men's-Outerwear/"	  "Sydney"	"Australia"	 1496145631		    98336

WOMEN
"productcategory"			   "city"	"country"	  "visitid"	 "sum_orderedquantity"
"Home/Apparel/Women's/"			  "Brisbane"	 "Australia"	 1501543818		    65084
"Home/Apparel/Women's/"	 		  "Montreal"	   "Canada"	  1497018135	            65084
"Home/Apparel/Women's/"			    "n/a"	   "Canada"	  1500942047	            65084
"Home/Apparel/Women's/"			    "n/a"	   "Canada"	  1496691523	            65084
"Home/Apparel/Women's/Women's-T-Shirts/"    "n/a"	   "Ecuador"	  1495134037	           117965
"Home/Apparel/Women's/"			    "n/a"	   "France"	  1495305808	            65084
"Home/Apparel/Women's/Women's-Outerwear/"   "n/a"	   "France"	  1495392656	            39128
"Home/Apparel/Women's/Women's-T-Shirts/"    "n/a"	   "Germany"	  1493969508	           117965
"Home/Apparel/Women's/Women's-T-Shirts/"   "Hong Kong"	  "Hong Kong"	  1499692702	           117965
"Home/Apparel/Women's/"			    "n/a"	   "Hong Kong"	  1496858394		   65084

KIDS
"productcategory"			   "city"	 "country"	   "visitid"	  "sum_orderedquantity"
"Home/Apparel/Kid's/"			   "n/a"	"Australia"	   1499162025	             31336
"Home/Apparel/Kid's/Kid's-Infant/"	   "n/a"	"Australia"	   1500015834	             21589
"Home/Apparel/Kid's/"			  "Sydney"	"Australia"	   1497352220	             31336
"Home/Apparel/Kid's/"			   "n/a"	"Canada"	   1494371471	             31336
"Home/Apparel/Kid's/Kids-Youth/"	   "n/a"	"Canada"	   1498434369		      5893
"Home/Apparel/Kid's/"	                 "Bengaluru"	"India"	           1499444854		     31336
"Home/Apparel/Kid's/Kid's-Toddler/"	 "Bengaluru"	"India"	           1498994304		     11630
"Home/Apparel/Kid's/Kid's-Infant/"	 "Dublin"	"Ireland"	   1501242760		     21589
"Home/Apparel/Kid's/Kid's-Infant/"	   "n/a"	"Israel"	   1499363872		     21589
"Home/Apparel/Kid's/"			   "n/a"	"Japan"	           1501155775		     31336

**Question 4: What is the top-selling product from each city/country? Can we find any 
pattern worthy of noting in the products sold?**


SQL Queries:
WITH ranked_products AS(
SELECT tas.city, tas.country, tas.productname, MAX(ta.unit_sold) AS max_unit_sold,
		RANK() OVER (PARTITION BY tas.city, tas.country ORDER BY MAX(ta.unit_sold) DESC)
		AS product_rank
FROM temp_all_sessions tas
JOIN temp_analytics ta ON tas.visitid = ta.visitid
GROUP BY tas.country, tas.city, tas.productname
)
SELECT city, country, productname, max_unit_sold, product_rank
FROM ranked_products
WHERE product_rank = 1 AND max_unit_sold != 0
ORDER BY max_unit_sold DESC 
LIMIT 10


Answer:
"city"			"country"		"productname"		"max_unit_sold"		"product_rank"
"Mountain View"		"United States"	  "Grip Highlighter Pen 3 Pack"		50			1
"Chicago"		"United States"	  "Google Alpine Style Backpack"	30			1
"n/a"			"United States"	  "Google Twill Cap"			25			1
"n/a"			"Canada"	  "Waze Pack of 9 Decal Set"		10			1
"New York"		"United States"	  "Google Alpine Style Backpack"	8			1
"Sunnyvale"		"United States"	  "SPF-15 Slim & Slender Lip Balm"	6			1
"n/a"			"Japan"		  "Rubber Grip Ballpoint Pen 4 Pack"	5			1
"Toronto"		"Canada"	  "Android Stretch Fit Hat Black"	5			1
"San Francisco"		"United States"	  "Google Women's Scoop Neck Tee White"	4			1
"Pittsburgh"		"United States"	  "YouTube Hard Cover Journal"		4			1

To get the top selling Products from each city/country the All_sessions and Analytical
tables were joined, considering Visitid column as a unique column between them because
for a product to be sold, it needed to have been ordered by a visitor who vivited the site
(visitid).
There for the Maximum unit sold was ranked for each city/country. Using CTE to to create a 
nested query of the product ranks for infering purpose.



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
SELECT DISTINCT (tas.city), tas.country, SUM (ta.revenue) OVER (PARTITION BY tas.city, tas.country) 
AS total_rvn_from_each_city
FROM temp_all_sessions tas
JOIN temp_analytics ta
ON tas.fullvisitorid = ta.fullvisitorid
where revenue != 0
ORDER BY total_rvn_from_each_city DESC

Answer:
"city"			"country"	"total_rvn_from_each_city"
"n/a"			"United States"		83024359938
"Mountain View"		"United States"		10549489996
"San Bruno"		"United States"		 4230990000
"New York"		"United States"		 3485789992
"Sunnyvale"		"United States"		 3039259992
"Chicago"		"United States"		 1383630000
"Charlotte"		"United States"		 1161389993
"Kirkland"		"United States"		 1103520000
"San Francisco"		"United States"		  987449998
"Los Angeles"		"United States"		  942999999

From the Analysis, we can deduce that United state has the highest generated revenue, compared
to the occurence of other countries in the output.

Summarizing the impact of revenue generated from each city or country, typically 
involves analyzing and understanding the significance of revenue in relation to specific 
locations. This analysis can help identify key revenue-generating areas, assess the contribution 
of different cities or countries to overall revenue, and make informed decisions regarding 
resource allocation, market strategies, and business expansion.

With this summary, businesses can gain insights into the relative performance
of different regions, identify potential growth opportunities, and optimize their 
operations accordingly. It provides a way to evaluate the effectiveness of revenue generation 
strategies and understand the role of different locations in driving overall financial outcomes.
