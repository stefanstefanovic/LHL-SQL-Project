## What are your risk areas? Identify and describe them.

I have identified several risk areas in the dataset, including the following:
- Duplicate values were prevalent across multiple tables, particularly in the Analytics table.
- Certain columns contained either empty values or only a few populated values.
- Several tables exhibited duplicate columns with identical values.
- The absence of suitable primary key columns posed risks when joining and integrating data.

## QA Process:
Describe your QA process and include the SQL queries used to execute it.

1. Checked if data is complete, ensuring that all required fields are present and populated appropriately
- Are there are any columns with Null values
```
SELECT *
FROM fullvisitorId
WHERE fullvisitorId IS Null
```
- Are there any empty columns
```
SELECT *
FROM analytics
WHERE userid IS NOT NULL
```
2. Verified data uniqueness, identifying and addressing any instances of duplicate values within the dataset
- Are there any duplicate values left
```
WITH city_cte AS (
	SELECT 
        city, 
        totaltransactionrevenue
	FROM all_sessions
	WHERE city NOT IN ('(not set)', 'not available in demo dataset'))

SELECT 
    city,
    SUM(totaltransactionrevenue) AS transaction_revenue
FROM city_cte
WHERE totaltransactionrevenue IS NOT Null
GROUP BY city
ORDER BY transaction_revenue DESC;
```

Checked with output results from this query

```
SELECT 
    city,
    SUM(totaltransactionrevenue) AS transaction_revenue
FROM call_sessions
GROUP BY city
ORDER BY transaction_revenue DESC;
```

3. Ensured data consistency, validating data types, formatting, and decimal placements for accuracy and standardization
- I carefully reviewed the outputs by utilizing the ORDER BY clause and visually inspected them for any unusual or unexpected patterns.

4. Conducted outlier analysis, identifying and investigating any data points that deviate significantly from the expected patterns or ranges

```
SELECT 
    MIN(orderedquantity) as min_range, 
    MAX(orderedquantity) AS max_range, 
    AVG(orderedquantity)
FROM products


SELECT 
    MIN(timeOnSite) as min_range, 
    MAX(timeOnSite) AS max_range, 
    AVG(timeOnSite)
FROM all_sessions
```


### If I had a bit more time, I would have also conducted the following steps:
5. Conducted data integrity checks, examining relationships and dependencies between tables to ensure data coherence and adherence to defined constraints.
6. Performed data validation against predefined business rules or reference data, verifying that the data aligns with expected criteria.
7. Conducted data reconciliation, comparing the transformed data against the raw data to verify accuracy during the transformation process.
