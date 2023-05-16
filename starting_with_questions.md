# Answer the following questions and provide the SQL queries used to find the answer.

    
## **Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


### SQL Queries:

**Query for Country**
```
SELECT 
    country, 
    SUM(totaltransactionrevenue) AS transaction_revenue
FROM all_sessions
WHERE totaltransactionrevenue IS NOT Null
GROUP BY country
ORDER BY transaction_revenue DESC;
```

**Query for City**
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




### Answer: 

Out of the cities available in this database, San Francisco, Sunnyvale and Atlanta are the top 3 cities with the highest level of transaction revenue (all 3 of them are cities in the United States). The top 3 highest countries when it comes to the level of transaction revenue are the United States, Israel and Australia.




## **Question 2: What is the average number of products ordered from visitors in each city and country?**


### SQL Queries:

**Query for City**
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

**Query for Country**
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

### Answer:
There are 252 cities on the list, with two cities from the United States in the top 3, Council Bluffs (USA), Cork (Ireland) and Bellflower(USA).
There are 134 countries on the list, with Mali, Montenegro and Papua New Guinea in the top 3 places and Iceland and Rwanda at the bottom with 0 orders.




## **Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


### SQL Queries:

**Query for Country**
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

**Query for City**

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

### Answer:

**Country**

In top 10 countries the most popular product group was 'Home/Shop by Brand/YouTube/'.

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

**City**

Patern in top 10 cities by the number of orders by product category shows that the most popular category of products was 'Home/Nest/Nest-USA/'.

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



## **Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


### SQL Queries:

**Query for Country**
```
WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.country = '(not set)' THEN NULL
            ELSE als.country
        END AS country,
        CASE
            WHEN als.v2productname = '(not set)' THEN NULL
            ELSE als.v2productname
        END AS v2productname,
        als.productsku
    FROM all_sessions AS als
),
total_orders_by_product AS (
    SELECT 
        country, 
        v2productname, 
        SUM(p.orderedquantity) AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY SUM(p.orderedquantity) DESC) AS rn
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p.sku = alc.productsku
    WHERE 
        alc.v2productname IS NOT NULL
        AND alc.country IS NOT NULL
        AND p.orderedquantity > 0
    GROUP BY alc.country, alc.v2productname
)
SELECT 
    country AS "Country",
    v2productname AS " Product Name",
    ordered_quantity_sum AS "Sum of Ordered Quantity"
FROM total_orders_by_product
WHERE rn = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```

**Query for City**

```
WITH all_sessions_clean AS (
    SELECT 
        CASE
            WHEN als.city = '(not set)' THEN NULL
            WHEN als.city = 'not available in demo dataset' THEN NULL
            ELSE als.city
        END AS city,
        CASE
            WHEN als.v2productname = '(not set)' THEN NULL
            ELSE als.v2productname
        END AS v2productname,
        als.productsku
    FROM all_sessions AS als
),
total_orders_by_product AS (
    SELECT 
        city, 
        v2productname, 
        SUM(p.orderedquantity) AS ordered_quantity_sum,
        ROW_NUMBER() OVER (PARTITION BY city ORDER BY SUM(p.orderedquantity) DESC) AS rn
    FROM all_sessions_clean AS alc
    JOIN products AS p ON p.sku = alc.productsku
    WHERE 
        alc.v2productname IS NOT NULL
        AND alc.city IS NOT NULL
        AND p.orderedquantity > 0
    GROUP BY alc.city, alc.v2productname
)
SELECT 
    city AS "City",
    v2productname AS "Product name",
    ordered_quantity_sum AS "Sum of Ordered Quantity"
FROM total_orders_by_product
WHERE rn = 1
ORDER BY ordered_quantity_sum DESC
LIMIT 10;
```

### Answer:

**Country**

When we examine the rankings of the top ten countries based on the best-selling product, 'YouTube Custom Decals' emerges as the top-selling product in seven out of ten countries. However, it is important to note that in terms of the sheer quantity of products sold, 'Google Kick Ball' surpasses 'YouTube Custom Decals' by a significant margin (selling more than the combined sales of 'YouTube Custom Decals').

|Country	 |Product Category	|Sum of Ordered Quantity|
|----------|------------|----------|
|United States	|Google Kick Ball	|364080|
|India	|YouTube Custom Decals	|83292|
|United Kingdom	|YouTube Custom Decals	|49218|
|Germany	|YouTube Custom Decals	|34074|
|Canada	|YouTube Custom Decals	|30288|
|France	|YouTube Custom Decals	|18930|
|Australia	|YouTube Custom Decals	|18930|
|Romania	|YouTube Custom Decals	|18930|
|Ireland	|Google Kick Ball	|15170|
|Chile	|Google Kick Ball	|15170|

**City**

'Google Kick Bal' was the top-selling product in most of the cities when we look at the first ten cities buy the quantity of product ordered. However, it is noteworthy that the product with the highest overall sales volume is 'Nest Cam Indoor Security Camera - USA'.

|City	|Product name	|Sum of Ordered Quantity|
|-------------|-------------------------|------|
|Mountain View	|Nest Cam Indoor Security Camera - USA	|51336|
|New York	|Google Kick Ball	|30340|
|San Francisco	|Google Kick Ball	|30340|
|Chicago	|Google Kick Ball	|30340|
|Palo Alto	|Nest Cam Outdoor Security Camera - USA	|19033|
|Sunnyvale	|Nest Cam Outdoor Security Camera - USA	|16314|
|Kirkland	|Google Kick Ball	|15170|
|Moscow	|Google Kick Ball	|15170|
|Los Angeles	|Google Kick Ball	|15170|
|Council Bluffs	|Google Kick Ball	|15170|



## **Question 5: Can we summarize the impact of revenue generated from each city/country?**

### SQL Queries:

**Query for Country**

```
SELECT 
    country AS "Country", 
    SUM(totaltransactionrevenue) AS "Transaction Revenue",
	ROUND(SUM(totaltransactionrevenue)/SUM(SUM(totaltransactionrevenue)) OVER () * 100, 2) AS "Transaction Revenue Rate by Country"
FROM all_sessions
WHERE totaltransactionrevenue IS NOT Null
GROUP BY country
ORDER BY "Transaction Revenue" DESC;

```

**Query for City**

```
SELECT 
    city AS "City", 
    SUM(totaltransactionrevenue) AS "Transaction Revenue",
	ROUND(SUM(totaltransactionrevenue)/SUM(SUM(totaltransactionrevenue)) OVER () * 100, 2) AS "Transaction Revenue Rate by City"
FROM all_sessions
WHERE totaltransactionrevenue IS NOT Null AND city != 'not available in demo dataset'
GROUP BY city
ORDER BY "Transaction Revenue" DESC;
```

### Answer:

**Country** 

According to available data, the United States has the most significant impact on revenue from the five countries we have data for. Customers from the United States are generating 92% of the revenue.

|Country	|Transaction Revenue	|Transaction Revenue Rate by Country|
|--------|--------|-----|
|United States	|13123	|92.1|
|Israel	|602	|4.23|
|Australia	|358	|2.51|
|Canada	|149	|1.05|
|Switzerland	|16	|0.11|


**City**

Results are a bit more diverse when it comes to the cities, where San Francisco is responsible for 19% of the revenue, followed by Sunnyvale (12%) and Atlanta (10%).

|City	|Transaction Revenue	|Transaction Revenue Rate by City|
|-------|-----------|------|
|San Francisco	|1561	19.12|
|Sunnyvale	|991	|12.14|
|Atlanta	|853	|10.45|
|Palo Alto	|608	|7.45|
|Tel Aviv-Yafo	|602	|7.37|
|New York	|593	|7.26|
|Los Angeles	|479	|5.87|
|Mountain View	|479	|5.87|
|Chicago	|448	|5.49|
|Sydney	|358	|4.38|
|Seattle	|358	|4.38|
|San Jose	|262	|3.21|
|Nashville	|157	|1.92|
|Austin	|157	|1.92|
|San Bruno	|103	|1.26|
|Toronto	|82	|1|
|Houston	|38	|0.47|
|Columbus	|21	|0.26|
|Zurich	|16	|0.2|






