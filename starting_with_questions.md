Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:



For city:
```
WITH city_cte AS (
	SELECT city, totaltransactionrevenue
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

For country:
```
SELECT 
    country, 
    SUM(totaltransactionrevenue) AS transaction_revenue
FROM all_sessions
WHERE totaltransactionrevenue IS NOT Null
GROUP BY country
ORDER BY transaction_revenue DESC;
```


Answer: 

Out of the cities available in this database, San Francisco, Sunnyvale and Atlanta are the top 3 cities with the highest level of transaction revenue (all 3 of them are cities in the United States). The top 3 highest countries when it comes to the level of transaction revenue are the United States, Israel and Australia.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

City
```
SELECT 
	CASE
		WHEN als.city = '(not set)' THEN Null
		WHEN als.city = 'not available in demo dataset' THEN Null
	 	ELSE city
	 END,
	ROUND(AVG(p.orderedquantity)) AS average_oq
FROM products AS p
JOIN all_sessions AS als
ON p.sku = als.productsku
GROUP BY als.city
ORDER BY average_oq DESC
```
Country
```
SELECT 
	CASE
		WHEN als.country = '(not set)' THEN Null
		ELSE country
	END,
	ROUND(AVG(p.orderedquantity)) AS average_oq
FROM products AS p
JOIN all_sessions AS als
ON p.sku = als.productsku
GROUP BY als.country
ORDER BY average_oq DESC
```

Answer:
There are 252 cities on the list, with two cities from the United States in the top 3, Council Bluffs (USA), Cork (Ireland) and Bellflower(USA).
There are 134 countries on the list, with Mali, Montenegro and Papua New Guinea in the top 3 places and Iceland and Rwanda at the bottom with 0 orders.




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

Query for country:
```
WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.country = '(not set)' THEN NULL
            ELSE als.country
        END AS country,
        CASE
            WHEN als.v2ProductCategory = '(not set)' THEN NULL
            ELSE als.v2ProductCategory
        END AS v2productcategory,
        als.productsku
    FROM all_sessions AS als
),
total_orders_by_category AS (
    SELECT 
        country, 
        v2productcategory, 
        SUM(p.orderedquantity) AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY SUM(p.orderedquantity) DESC) AS rn
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p.sku = alc.productsku
    WHERE 
        alc.v2productcategory IS NOT NULL
        AND alc.country IS NOT NULL
        AND p.orderedquantity > 0
    GROUP BY alc.country, alc.v2productcategory
)
SELECT 
    country AS "Country",
    v2productcategory AS " Product Category",
    ordered_quantity_sum AS "Sum of Ordered Quantity"
FROM total_orders_by_category
WHERE rn = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```
In top 10 countries the most popular product group was "Home/Shop by Brand/YouTube/".

Country	 |Product Category	|Sum of Ordered Quantity
|--------------|---------------------------|------|
|United States	|Home/Shop by Brand/YouTube/	|555726|
|United Kingdom	|Home/Shop by Brand/YouTube/	|151982|
|India	|Home/Shop by Brand/YouTube/	|150793|
|Germany	|Home/Shop by Brand/YouTube/	|77108|
|Canada	|Home/Shop by Brand/YouTube/	|74128|
|Australia	|Home/Shop by Brand/YouTube/	|54534|
|France	|Home/Shop by Brand/YouTube/	|28949|
|Netherlands	|Home/Shop by Brand/YouTube/	|28594|
|Italy	|Home/Shop by Brand/YouTube/	|28319|
|Japan	|Home/Accessories/Fun/	|26280|


Query for city:

```
WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.city = '(not set)' THEN NULL
            WHEN als.city = 'not available in demo dataset' THEN NULL
            ELSE als.city
        END AS city,
        CASE
            WHEN als.v2ProductCategory = '(not set)' THEN NULL
            ELSE als.v2ProductCategory
        END AS v2productcategory,
        als.productsku
    FROM all_sessions AS als
),
total_orders_by_category AS (
    SELECT 
        city, 
        v2productcategory, 
        SUM(p.orderedquantity) AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY city ORDER BY SUM(p.orderedquantity) DESC) AS rn
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p.sku = alc.productsku
    WHERE 
        alc.v2productcategory IS NOT NULL
        AND alc.city IS NOT NULL
        AND p.orderedquantity > 0
    GROUP BY alc.city, alc.v2productcategory
)
SELECT 
    city AS "City",
    v2productcategory AS " Product Category",
    ordered_quantity_sum AS "Sum of Ordered Quantity"
FROM total_orders_by_category
WHERE rn = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```

Answer:

Patern in top 10 cities by the number of orders by product category shows that the most popular category of products was "Home/Nest/Nest-USA/".

|City	 |Product Category	|Sum of Ordered Quantity|
|-------------|---------------------|---------------|
|Mountain View	|Home/Nest/Nest-USA/	|145186|
|Sunnyvale	|Home/Nest/Nest-USA/	|41280|
|Palo Alto	|Home/Nest/Nest-USA/	|39793|
|San Francisco	|Home/Nest/Nest-USA/	|34173|
|London	    |Home/Shop by Brand/YouTube/	|31792|
|Chicago	|Home/Accessories/Fun/	|31713|
|New York	|Home/Nest/Nest-USA/	|27873|
|Chennai	|Home/Shop by Brand/YouTube/	|21330|
|Minato	    |Home/Accessories/Fun/	|20725|
|San Jose	|Home/Nest/Nest-USA/	|17087|



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







