# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(fill in your description and goals here)
The goal of the project was to analyse and derive meaningful insights 
from an ecommerce database using postgresql
-In order to understand site visitors behaviour: Analyze the database to gain 
 insights into customer behavior, preferences, and purchase patterns.
-In order to segment the visitors based on their regions
-To identify marketing trends: Monitor market trends, industry developments, 
 and consumer preferences
-To assess Product performance: Evaluate the performance of individual 
 products or product categories based on orders, sales, revenue and region. 
-Geographic analysis: Evaluate sales performance and customer behavior across 
 different geographic regions or cities.



#Process
### STEP 1: The data based was imported manually into pgadmin, using character varying
as the default data type

### STEP 2: The data types of the tables were altered using the alter function, assigning
the correct data type to each column to ensure:
-Data Integrity
-Best query performance
-Better data presentation
-Easier Data manipulation

### STEP 3: To clean the data without altering the raw data values, sub tables were created
using CREATE TABLE function. The new tables were uniquely suffixed with "temp" to
avoid confusion.

### STEP 4: The new tables contained the clean data set
-Duplicate columns removed
-Redundant columns removed
-Corrected column names (e.g from v2productcategory to productcategory)
-Nulls removed
-Columns that had scanty values were removed
-Ajusting the values of the unit price (i.e unitprice/1,000,000)

### STEP 5: The new tables were used to answer all questions in the #Questions.txt#,
to form more questions #starting with questions# and a Quality Assurance process
was carried out and validated.

### STEP 6: ERD was generated based on the new tables created in pgadmin.



## Results
(fill in what you discovered this data could tell you and how you 
used the data to answer those questions)

-The data could tell me the most used channel grouping, which was organic search
-From the data I discovered the visitors that spent more than 2 hours on site,
 which were 9 in total 
-The dates where products were mostly sold, 2017-06-08 being the date with the
highest unit sold at 4,324
- I discovered the total product price for women's apparel suitable for home use and 
the number of items in each category which gave a summary of the selling price for each 
category, regarless the counts of items in each category. The women's t-shirts were the 
least expensive, irrespective of the number of items. However, women's outerwear is the 
most expensive which has one of the least number of items available for purchase


To get answers to the questions, it was necesary to; 
-Join tables
-Use CTE to reduce complexibilty of the queries
-Filter columns where necessary by using the WHERE operators
-Use Windowfunctions/GROUP BY to group columns
-And ORDER BY


## Challenges 
(discuss challenges you faced in the project)

-There were lots of duplicate and redundant columns
-The tables were unstable
-Limited number of days

## Future Goals
(what would you do if you had more time?)
-To derive more business insights from the database
-To carry out more quality assurance processes and validate the data
-To create a visualize representation of the data results