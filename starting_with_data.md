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


## Question 2:  What were the main sources that brought traffic to the website?

### SQL Queries:

```
SELECT 
	channelgrouping AS "Channel Groupings",
	COUNT(DISTINCT(fullvisitorid)) AS "Number of Unique Visits",
	ROUND(COUNT(DISTINCT(fullvisitorid)) / SUM(COUNT(DISTINCT(fullvisitorid))) OVER () * 100, 2) AS "Rate of Unique Visits by Channel Grouping"
FROM all_sessions
GROUP BY channelgrouping
ORDER BY "Number of Unique Visits" DESC;
```

### Answer:

More than half of the traffic (58%) came through Organic Search, which refers to traffic from search engine results such as Google or Bing. Approximately one-fifth of the traffic originated from an unknown referrer or source. Paid Search accounted for only 3% of the total traffic.

|Channel Groupings	|Number of Unique Visits	|Rate of Unique Visits by Channel Grouping|
|--------|---------|-------|
|Organic Search	|8207	|57.51|
|Direct	|2805	|19.66|
|Referral	|2419	|16.95|
|Paid Search	|470	|3.29|
|Affiliates	|245	|1.72|
|Display	|119	|0.83|
|(Other)	|5	|0.04|


## Question 3: Is there any difference in the primary traffic source on the platform between countries?


### SQL Queries:

```
WITH total_number_of_unique_visits AS (
    SELECT 
        country, 
        channelgrouping, 
        COUNT(DISTINCT(fullvisitorid)) AS "Number of Unique Visits",
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY COUNT(DISTINCT(fullvisitorid)) DESC) AS rn
    FROM all_sessions
	WHERE country NOT IN ('(not set)')
    GROUP BY country, channelgrouping
)
SELECT 
    country AS "Country",
    channelgrouping AS "Channel Groupings",
    "Number of Unique Visits"
FROM total_number_of_unique_visits
WHERE rn = 1 AND "Number of Unique Visits" > 10
ORDER BY "Number of Unique Visits" DESC;
```

### Answer:

Out of the 56 countries with more than ten visits, Organic Search was the primary source of traffic in all cases except for two instances—Romania and Finland. In these countries, Direct traffic emerged as the primary source, driving the majority of visits.

Country	|Channel Groupings	|Number of Unique Visits
---|----|----
United States	|Organic Search	|4008
United Kingdom	|Organic Search	|509
India	|Organic Search	|489
Canada	|Organic Search	|455
Germany	|Organic Search	|232
Australia	|Organic Search	|174
France	|Organic Search	|148
Japan	|Organic Search	|123
Netherlands	|Organic Search	|108
Italy	|Organic Search	|105


## Question 4: What was the average time visitor spent on the page by Product Category?

### SQL Queries:

### Answer:



Question 5: 

SQL Queries:

Answer:
