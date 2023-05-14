Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

For city:
```
SELECT 
    city, 
    SUM(totaltransactionrevenue) AS transaction_revenue
FROM all_sessions
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


Answer: Out of the cities available in this DB, San Francisco, Sunnyvale and Atlanta are the top 3 cities with the highest level of transaction revenue (all 3 of them are cities in the United States). The top 3 highest countries when it comes to the level of transaction revenue are the United States, Israel and Australia.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







