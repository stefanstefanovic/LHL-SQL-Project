## Question 1: What were top-viewed products?

### SQL Queries:

```
SELECT 
	v2productname AS "Name of the Product", 
	COUNT(DISTINCT(fullvisitorid)) AS "Number of Unique Visits"
FROM all_sessions
GROUP BY v2productname
ORDER BY "Number of Unique Visits" DESC
LIMIT 10;
```

### Answer: 

The product that received the highest number of views on this platform, with nearly 300 page visits, was the 'Google Men's Short Sleeve Hero Tee White.' Interestingly, the following six products in terms of popularity were all YouTube merchandise.

|Name of the Product	|Number of Unique Visits|
|--------------------------|------------|
|Google Men's 100% Cotton Short Sleeve Hero Tee White	|294|
|22 oz YouTube Bottle Infuser	|244|
|YouTube Twill Cap	|221|
|YouTube Custom Decals	|211|
|YouTube Men's Short Sleeve Hero Tee Black	|197|
|YouTube Leatherette Notebook Combo	|182|
|YouTube Men's Short Sleeve Hero Tee Charcoal	|179|
|Google Men's 100% Cotton Short Sleeve Hero Tee Navy	|177|
|YouTube Wool Heather Cap Heather/Black	|174|
|Google Men's 100% Cotton Short Sleeve Hero Tee Black	|173|


## Question 2: 

### SQL Queries:

### Answer:



## Question 3: 

### SQL Queries:

### Answer:



## Question 4: 

### SQL Queries:

### Answer:



Question 5: 

SQL Queries:

Answer:
