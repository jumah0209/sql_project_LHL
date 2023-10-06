What are your risk areas? Identify and describe them.
Data Inconsitency: There were lots of duplicate records in the Analytics table and
All sessions table. 

Irrelevant columns: Such as sentimentscore, sentimentmagnitude, restockingleadtime
page title, pagepathlevel, ecommerceaction

Reduntant Columns : Userid column completely null and visitstarttime, a replica
of visitid

Incomplete columns: The columns with high missing value ('total transaction Revenue'
and 'transaction' column as missing as data can introduce bias or affect the accuracy 
of analysis.


QA Process:
Productsku in the three tables it appears

SELECT COUNT(productsku) = 1092
FROM temp_products
select distinct(productsku) = 1092
FROM temp_products
--No duplicates

SELECT COUNT(productsku) = 454
FROM temp_sales_report
SELECT DISTINCT(productsku) = 454
FROM temp_sales_report
--No duplicates

SELECT COUNT(productsku) = 462
FROM temp_sales_by_sku
SELECT DISTINCT(productsku) =462
FROM temp_sales_by_sku

SELECT COUNT(productsku)  = 15134
FROM temp_all_sessions
SELECT DISTINCT(productsku) = 536
FROM temp_all_sessions
-- Productsku is duplicated in the All_sessions table

To check the distinct productsku in the 3 tables where productsku is
not duplicated
SELECT DISTINCT(tp.productsku), tsp.productsku, tsbs.productsku 
FROM temp_products tp
JOIN temp_sales_report tsp
ON tp.productsku = tsp.productsku
JOIN temp_sales_by_sku tsbs
ON tsbs.productsku = tsp.productsku     =454
	

Are there distinct productsku values in all 4 table?

SELECT DISTINCT(tas.productsku) AS psku_as, tp.productsku 
	AS psku_p, tsp.productsku AS psku_sp, tsbs.productsku AS pksu_sbs
FROM temp_products tp
JOIN temp_all_sessions tas
ON tp.productsku = tas.productsku
JOIN temp_sales_report tsp
ON tp.productsku = tsp.productsku
JOIN temp_sales_by_sku tsbs
ON tsbs.productsku = tsp.productsku

From the analysis, it shows that there are 278 distinct productsku
present in All-sessions, Sales-report, Sales-by-sku and Products tables

--------------------------------------------------------------------------
--GET THE NUMBER OF FULLVISITORID FROM ORIGINAL QUERY 
SELECT COUNT(distinct fullvisitorid) FROM (
SELECT tas.fullvisitorid AS fullvisitorid, tas.country AS country, ta.visitnumber AS visitnumber
FROM temp_all_sessions tas
JOIN temp_analytics ta
ON tas.fullvisitorid = ta.fullvisitorid
) tmp
ANSWER = 3897

SELECT COUNT(fullvisitorid) FROM temp_all_sessions
ANSWER = 15134

THERE IS A DUPLICATE OF FULLVISITORID  
The joined result tells us that there are more fullvisitorid than 
distinct fullvisitorid

WHAT TO DO?
SELECT visitid, fullvisitorid, COUNT(*) FROM (
SELECT tas.fullvisitorid AS fullvisitorid, ta.visitid AS visitid
FROM temp_all_sessions tas
JOIN temp_analytics ta
ON tas.fullvisitorid = ta.fullvisitorid
    ) tmp
    GROUP BY fullvisitorid, visitid
    HAVING COUNT(*) > 1
    ORDER BY fullvisitorid

ANSWER = 6870
so we see that visitid with a corressponding fullvisitorid do exist
---------------------------------------------------------------------------

--GET THE NUMBER OF VISITNUMBER FROM ORIGINAL QUERY 
SELECT COUNT(distinct visitnumber) FROM (
SELECT tas.fullvisitorid AS fullvisitorid, tas.country AS country, ta.visitnumber AS visitnumber
FROM temp_all_sessions tas
JOIN temp_analytics ta
ON tas.fullvisitorid = ta.fullvisitorid
) tmp
Answer=	206

--GET THE NUMBER OF VISITNUMBER IN ALL SESSIONS TABLE	
SELECT COUNT(visitnumber) FROM temp_analytics
Answer=	4301122
--THERE IS A DUPLICATE OF VISITNUMBER
The joined result tells us that there are more visitnumber than 
distinct visitnumber
	
--WHAT TO DO
SELECT country, visitnumber, COUNT(*) FROM (
SELECT tas.fullvisitorid AS fullvisitorid, tas.country AS country, ta.visitnumber AS visitnumber
FROM temp_all_sessions tas
JOIN temp_analytics ta
ON tas.fullvisitorid = ta.fullvisitorid
    ) tmp
    GROUP BY visitnumber, country
    HAVING COUNT(*) > 1
    ORDER BY visitnumber

--so we see that a city/country has a corressponding visitnumber do exist
