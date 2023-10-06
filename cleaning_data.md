What issues will you address by cleaning the data?
1. Changed the data types of the columns in all tables to the appropriate data type 
   The default data types for all columns after import was character varying
2. -Changed all 'not set' and 'not inluded in demo dataset' in the columns to 'n/a'
   -Changed all NULLs in the varchar datatype column to 'n/a', while NULLs in numeric
    and time data type to '0'
   -Change the column names in All_sessions table 'v2productcategory' and 'v2productprice' to 
    'productcategory' and 'productprice' respectively
   -Excluded columns with < 10 values or completely blank in the 'All_Sessions' Table 
    while creating a new table for all.
     -Columns excluded in the New All sessions are; 'sessionqualityDim', 'productRefundAmount',
      'ProductQuantity', 'ProductRevenue', 'ItemQuantity', 'ItemRevenue', 'TransactionRevenue', 
      'TransactionID', 'SearchKeyWord', 'Ecommerceaction_option', 'ecommerceaction_type'
   - In the 'Analytics' Table, excluded 'VisitStartTime' column because it is a duplicate of the 'VisitId' column. 
     Excluded the 'UserId' column because it was completely blank while creating a temp table for all.
     Divided 'Unit_Price' by 1,000,000 to give the unit price of the products.
   - In the Products table, the column 'sku' was renamed as 'productsku'
   


Queries:
Below, provide the SQL queries you used to clean your data.
To confirm that some columns had duplicates, the following queries were used
SELECT fullvisitorid
FROM all_sessions

= 15134 rows of fullvisitorid

Using DISTINCT function or GROUP BY to have a distinct value in the table
SELECT DISTINCT(fullvisitorid)
FROM all_sessions

= 14223 rows of distinct fullvisorid
Which shows that some values were repeated more than once. Basically 909
extra values

SELECT visitid
FROM analytics
=4301122 rows of visitid

SELECT DISTINCT(visitid)
FROM analytics
= 148642 rows of distinct visitid.
Which shows that many values were repeated more than once. Basically 4,152,480
extra values

1(a) Table: all_sessions
Column: 'Fullvisitorid' from varchar to numeric
ALTER TABLE all_sessions
ALTER COLUMN fullvisitorid TYPE numeric
USING fullvisitorid::numeric

Column: 'Time' from varchar to numeric to time
ALTER TABLE all_sessions
ALTER COLUMN time TYPE numeric 
USING time::numeric

ALTER TABLE all_sessions
ALTER COLUMN time TYPE time
USING '00:00'::time + (time || ' seconds')::interval;

Column: 'Totaltransactionrevenue' to integer
ALTER TABLE all_sessions
ALTER COLUMN totaltransactionrevenue TYPE integer
USING totaltransactionrevenue::integer

Column: 'Transactions' to integer
ALTER TABLE all_sessions
ALTER COLUMN transactions TYPE integer
USING transactions::integer

Column: 'TimeOnSite' to numeric to time
ALTER TABLE all_sessions
ALTER COLUMN timeonsite TYPE numeric 
USING timeonsite::numeric

ALTER TABLE all_sessions
ALTER COLUMN timeonsite TYPE time
USING '00:00'::time + (timeonsite || ' seconds')::interval;

Column: 'pageviews' to integer
ALTER TABLE all_sessions
ALTER COLUMN pageviews TYPE integer
USING pageviews::integer

Column: 'sessionqualitydim' to integer
ALTER TABLE all_sessions
ALTER COLUMN sessionqualitydim TYPE integer
USING sessionqualitydim::integer

Column: 'date' to DATE 
ALTER TABLE all_sessions
ALTER COLUMN date TYPE date 
USING date::date;

Column: 'visitid' to integer
ALTER TABLE all_sessions
ALTER COLUMN visitid TYPE integer
USING visitid::integer

Column: 'Productprice' to integer
ALTER TABLE all_sessions
ALTER COLUMN productprice TYPE integer
USING productprice::integer

Column: 'Productrevenue' to interger
ALTER TABLE all_sessions
ALTER COLUMN productrevenue TYPE integer
USING productrevenue::integer

Column: 'ecommerceaction_type' to integer
ALTER TABLE all_sessions
ALTER COLUMN ecommerceaction_type TYPE integer
USING ecommerceaction_type::integer

Column: 'ecommerce_step' to integer
ALTER TABLE all_sessions
ALTER COLUMN ecommerceaction_step TYPE integer
USING ecommerceaction_step::integer

(b) Table: Analytics
Column: 'Visitnumber' to Integer
ALTER TABLE analytics
ALTER COLUMN visitnumber TYPE integer
USING visitnumber::integer

Column: 'Visitid' to integer
ALTER TABLE analytics
ALTER COLUMN visitid TYPE integer
USING visitid::integer

Column: 'Visitstarttime' to integer
ALTER TABLE analytics
ALTER COLUMN visitstarttime TYPE integer
USING visitstarttime::integer

Column: 'data' to 'DATE'
ALTER TABLE analytics
ALTER COLUMN date TYPE date
USING date::date

Column: 'fullvisitorid' to numeric
ALTER TABLE analytics
ALTER COLUMN fullvisitorid TYPE numeric
USING fullvisitorid::numeric

Column: 'Units_sold' to integer
ALTER TABLE analytics
ALTER COLUMN units_sold TYPE integer
USING units_sold::integer

Column: 'Pageviews' to integer
ALTER TABLE analytics
ALTER COLUMN pageviews TYPE integer
USING pageviews::integer

Column: 'timeonsite' to numeric to TIME
ALTER TABLE analytics
ALTER COLUMN timeonsite TYPE numeric 
USING timeonsite::numeric

ALTER TABLE analytics
ALTER COLUMN timeonsite TYPE time
USING '00:00'::time + (timeonsite || ' seconds')::interval;

Column: 'Bounces' to integer
ALTER TABLE analytics
ALTER COLUMN bounces TYPE integer
USING bounces::integer

Column: 'revenue' to numeric
ALTER TABLE analytics
ALTER COLUMN revenue TYPE numeric
USING revenue::numeric

Column: unit_price to integer
ALTER TABLE analytics
ALTER COLUMN unit_price TYPE integer
USING unit_price::integer

(c) Table: Sales_by_sku
Column: 'Total_ordered' to integer
ALTER TABLE sales_by_sku
ALTER COLUMN total_ordered TYPE integer
USING total_ordered::integer

(d) Table: Products
Column: 'orderedquantity' to integer
ALTER TABLE products
ALTER COLUMN orderedquantity TYPE integer
USING orderedquantity::integer

Column: 'stocklevel' to integer
ALTER TABLE products
ALTER COLUMN stocklevel TYPE integer
USING stocklevel::integer

Column: 'restockingleadtime' to integer
ALTER TABLE products
ALTER COLUMN restockingleadtime TYPE integer
USING restockingleadtime::integer

Column: 'sentimentscore' to float
ALTER TABLE products
ALTER COLUMN sentimentscore TYPE float
USING sentimentscore::float

Column: 'sentimentmagnitude' to float
ALTER TABLE products
ALTER COLUMN sentimentmagnitude TYPE float
USING sentimentmagnitude::float

(e) Table: Sales_report
Column: total_ordered to integer
ALTER TABLE sales_report
ALTER COLUMN total_ordered TYPE integer
USING total_ordered::integer

Column: stocklevel to integer
ALTER TABLE sales_report
ALTER COLUMN stocklevel TYPE integer
USING stocklevel::integer

Column: 'restockingleadtime' to integer
ALTER TABLE sales_report
ALTER COLUMN restockingleadtime TYPE integer
USING restockingleadtime::integer

Column: 'sentimentscore' to float
ALTER TABLE sales_report
ALTER COLUMN sentimentscore TYPE float
USING sentimentscore::float

Column: 'sentimentmagnitude' to float
ALTER TABLE sales_report
ALTER COLUMN sentimentmagnitude TYPE float
USING sentimentmagnitude::float

Column: 'ratio' to float
ALTER TABLE sales_report
ALTER COLUMN ratio TYPE float
USING ratio::float

2.(a) For all_sessions Table
CREATE TABLE temp_all_sessions AS
(SELECT row_number() over() as unique_id, fullvisitorid, channelgrouping, time,
 pageviews, date, visitid, type, productprice, productsku,
 v2productname AS productname, pagepathlevel1, ecommerceaction_step,
	(CASE
	 	WHEN country = '(not set)' THEN 'n/a'
		ELSE country
		END) AS country,
	(CASE
		WHEN city = '(not set)' THEN 'n/a'
	 	WHEN city = 'not available in demo dataset' THEN 'n/a'
		ELSE city
		END) AS city,
 	(CASE
		WHEN v2productcategory = '(not set)' THEN 'n/a'
		ELSE v2productcategory
		END) AS productcategory,
	(CASE
		WHEN totaltransactionrevenue IS NULL THEN 0
	 	ELSE totaltransactionrevenue
	 	END) AS totaltransactionrevenue,
 	(CASE
	 	WHEN transactions IS NULL THEN 0
	 	ELSE transactions
	 	END) AS transactions,
 	(CASE 
		WHEN timeonsite is NULL THEN '00:00:00'::TIME
		ELSE timeonsite
		END) AS timeonsite,
 	(CASE
		WHEN productvariant = '(not set)' THEN 'n/a'
	 	ELSE productvariant
	 	END) AS productvariant,
 	(CASE
		WHEN currencycode IS NULL THEN 'n/a'
	 	ELSE currencycode
	 	END) AS currencycode,
 	(CASE
		WHEN pagetitle IS NULL THEN 'n/a'
	 	ELSE pagetitle
	 	END) AS pagetitle
FROM all_sessions
);

(b) For analytics Table
CREATE TABLE temp_analytics AS
(SELECT row_number() over() as unique_id, visitnumber, visitid,
 date, fullvisitorid, channelgrouping, 
 socialengagementtype, 
 	(CASE
		WHEN units_sold IS NULL THEN '0'
	 	ELSE units_sold
	 	END) AS unit_sold,
	(CASE
		WHEN pageviews IS NULL THEN '0'
	 	ELSE pageviews
	 	END) AS pageviews,
 	(CASE 
		WHEN timeonsite is NULL THEN '00:00:00'::TIME
		ELSE timeonsite
		END) AS timeonsite,
 	(CASE 
		WHEN bounces is NULL THEN '0'
		ELSE bounces
		END) AS bounces,
 	(CASE 
		WHEN revenue is NULL THEN '0'
		ELSE revenue
		END) AS revenue,
 (unit_price/1000000) AS unit_price
FROM analytics
);

(c) For Sales_by_sku Table
CREATE TABLE temp_sales_by_sku AS
(SELECT row_number() over() as unique_id, productsku,
total_ordered
FROM sales_by_sku
 );

(d) For Sales_report Table
CREATE TABLE temp_sales_report AS
(SELECT row_number() over() as unique_id, productsku,
total_ordered, name,stocklevel, sentimentscore, 
sentimentmagnitude,
 (CASE 
		WHEN ratio is NULL THEN '0'
		ELSE ratio
		END) AS ratio
FROM sales_report
 );

(e) For Products Table
CREATE TABLE temp_products AS
(SELECT row_number() over() as unique_id, sku AS productsku, name,
 orderedquantity,stocklevel,
	(CASE 
		WHEN sentimentscore is NULL THEN '0'
		ELSE sentimentscore
		END) AS sentimentscore,
	(CASE 
		WHEN sentimentmagnitude is NULL THEN '0'
		ELSE sentimentmagnitude
		END) AS sentimentmagnitude
 FROM products);