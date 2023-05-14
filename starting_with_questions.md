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


Answer: Out of the cities available in this database, San Francisco, Sunnyvale and Atlanta are the top 3 cities with the highest level of transaction revenue (all 3 of them are cities in the United States). The top 3 highest countries when it comes to the level of transaction revenue are the United States, Israel and Australia.




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
	AVG(p.orderedquantity) AS average_oq
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
	AVG(p.orderedquantity) AS average_oq
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



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







