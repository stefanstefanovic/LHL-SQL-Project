What issues will you address by cleaning the data?

1 - Remove irrelevant data (e.g. empty columns)
2 - Check data types of columns and change it
3 - Fix structural errors (e.g. unnecessary spaces)
4 - Check if there are any outliers
5 - Link tables


Queries:
Below, provide the SQL queries you used to clean your data.

1. ***************

/*This query returns rows in columns that have at least one value
*/
SELECT *
FROM analytics
WHERE userid IS NOT NULL;

/*Once I found empty columns, I used the DROP function to remove 
those columns from tables*/

--dropped column userid
ALTER TABLE all_sessions
DROP COLUMN itemquantity;

--dropped itemrevenue
ALTER TABLE all_sessions
DROP COLUMN itemrevenue;

--dropped searchkeyword
ALTER TABLE all_sessions
DROP COLUMN searchkeyword;

--dropped column userid
ALTER TABLE analytics
DROP COLUMN userid;

/*Checked columns with same name in diferent tables to see if they have same 
values in rows*/

SELECT COUNT(*)
FROM products AS p
LEFT JOIN sales_report AS sr
ON p.sku = sr.productsku
WHERE p.name = sr.name 
	AND p.stocklevel = sr.stocklevel 
	AND p.restockingLeadTime = sr.restockingLeadTime
	AND p.sentimentScore = sr.sentimentScore
	AND p.sentimentMagnitude = sr.sentimentMagnitude

/* Dropped name, stocklevel, restockingleadtime, sentimentscore and sentimentmagnitude 
from SALES_REPORT*/

ALTER TABLE sales_report
    DROP COLUMN name, 
    DROP COLUMN stocklevel, 
    DROP COLUMN restockingLeadTime, 
    DROP COLUMN sentimentScore, 
    DROP COLUMN sentimentMagnitude;


/*Checked if there is a difference beetween SALES_BY_SKU and SALES_report tables*/

--This query returns productSKU rows that don't exist in SALES_REPORT table
SELECT 
    sr.productsku, 
    sr.total_ordered, 
    sbs.productsku, 
    sbs.total_ordered
FROM sales_report AS sr
FULL OUTER JOIN sales_by_sku AS sbs
ON sr.productsku = sbs.productsku
WHERE sr.productSKU IS Null

--CHECKED tables all_sessions and Analytics for duplicate records

SELECT *
FROM all_sessions AS als
JOIN analytics AS a ON
	als.fullvisitorid = a.fullvisitorid AND
	als.visitid = a.visitid AND
	als.date = a.date AND
	als.timeonsite = a.timeonsite AND
	als.channelgrouping = a.channelgrouping AND
	als.pageviews = a.pageviews



2 ****************

--Returns data types of all columns in that table
SELECT *
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'products'

/*Changed TYPE from varchar to integer*/

ALTER TABLE analytics
ALTER column timeonsite TYPE bigint USING (timeonsite::bigint);

ALTER TABLE all_sessions
ALTER column totaltransactionrevenue TYPE bigint USING (totaltransactionrevenue::bigint);

--changed unitprice
UPDATE analytics
SET unit_price = (unit_price/1000000)

--changed unitprice
UPDATE all_sessions
SET productprice = (productprice/1000000)

--changed revenue
UPDATE analytics
SET revenue = (revenue/1000000)

--changed revenue
UPDATE all_sessions
SET totaltransactionrevenue = (totaltransactionrevenue/1000000)

3****************

--Removed extra spaces from strings

--products table
UPDATE products
SET name = TRIM(both ' ' FROM name)


4 *************
/*Checked columns for outliers or values that don't make sense*/

SELECT 
    MIN(stocklevel) as min_range, 
    MAX(stocklevel) AS max_range, 
    AVG(stocklevel)
FROM products

SELECT 
    MIN(orderedquantity) as min_range, 
    MAX(orderedquantity) AS max_range, 
    AVG(orderedquantity)
FROM products

SELECT *
FROM products
ORDER BY orderedquantity DESC


5************
/*Connected tables (added primary and foreign keys to tables)*/

--I first checked for unique values

SELECT 
    fullvisitorid, 
    visitId 
FROM all_sessions 
WHERE fullvisitorid IN
  (SELECT fullvisitorid 
   FROM all_sessions 
   GROUP BY fullvisitorid 
   HAVING COUNT(*) > 1)
ORDER BY visitid

--Added primary key to products table

ALTER TABLE products
ADD PRIMARY KEY (sku);

--Added foreign key to sales_report table

ALTER TABLE sales_report
ADD FOREIGN KEY (productsku) REFERENCES products(sku);

--Added foreign key to sales_by_sku  table

ALTER TABLE sales_by_sku 
ADD FOREIGN KEY (productsku) REFERENCES products(sku);

--to connect data i had to add productSKU that don't exist to the 
--SKU in products table

INSERT INTO products (sku)
SELECT DISTINCT productsku
FROM sales_by_sku
WHERE productsku NOT IN (SELECT sku FROM products);

--Added primary key to all_sessions table
ALTER TABLE all_sessions
ADD COLUMN session_id SERIAL;

UPDATE all_sessions
SET session_id = DEFAULT;

ALTER TABLE all_sessions
ALTER COLUMN session_id SET NOT NULL,
ADD PRIMARY KEY (session_id);

--Added primary key to analytics table

ALTER TABLE analytics
ADD COLUMN analytics_id SERIAL;

UPDATE analytics
SET analytics_id = DEFAULT;

ALTER TABLE analytics
ALTER COLUMN analytics_id SET NOT NULL,
ADD PRIMARY KEY (analytics_id);

--Added primary key to sales_by_sku table

ALTER TABLE sales_by_sku
ADD COLUMN sales_by_sku_id SERIAL;

UPDATE sales_by_sku
SET sales_by_sku_id = DEFAULT;

ALTER TABLE sales_by_sku
ALTER COLUMN sales_by_sku_id SET NOT NULL,
ADD PRIMARY KEY (sales_by_sku_id);

--added values to products table in the column SKU, 
--that can be found in all_sessions table

INSERT INTO products (sku)
SELECT DISTINCT productsku
FROM all_sessions
WHERE productsku NOT IN (SELECT sku FROM products);

--linked products and all_session

ALTER TABLE all_sessions 
ADD FOREIGN KEY (productsku) REFERENCES products(sku);

