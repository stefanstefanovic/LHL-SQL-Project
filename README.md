# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The project aimed to extract, clean, and transform e-commerce data, analyze it, and develop and implement a QA process.

## Process
### Loaded CSV files into database
- Opened CSV files in Excel to check the number of columns, their names and the type of data in those columns
- Wrote SQL query to create tables and columns
```
DROP TABLE sales_report; --removed table in case I alredy had table with same name

CREATE TABLE sales_report --table name
(productSKU varchar,
total_ordered int,
 name varchar,
 stockLevel int,
 restockingLeadTime int,
 sentimentScore float,
 sentimentMagnitude float,
 ratio float
);
```
- Imported CSV file into SQL database

### Explored data
- Checked columns for data types and any unexpected values
- Checked which columns can be used to link tables and if any column can be Primary Key
- Created table in Excel with all columns
### Cleaned/transformed data
- Dropped empty columns
- Transformed data types in some columns
- Removed duplicate rows
- Added Primary and Foreign keys and linked tables
### Analyzed data
- I used different queires to analyze data

## Results

By analyzing this data, I was able to uncover valuable insights, including the most viewed products, the primary sources of traffic to the website, variations across countries and cities, as well as the number of unique visits by month.

- Q1. What were top-viewed products?
    The product that received the highest number of views on this platform, with nearly 300 page visits, was the 'Google Men's Short Sleeve Hero Tee White.' Interestingly, the following six products in terms of popularity were all YouTube merchandise.
- Q2. What were the main sources that brought traffic to the website?
    More than half of the traffic (58%) came through Organic Search, which refers to traffic from search engine results such as Google or Bing. Approximately one-fifth of the traffic originated from an unknown referrer or source. Paid opstions such as Paid Search and Affiliates accounted for only 5% of the total traffic.
- Q3. Is there any difference in the primary traffic source on the platform between countries?
    There are almost no differences between countries. Out of the 56 countries with more than ten visits, Organic Search was the primary source of traffic in all cases except for two instances—Romania and Finland. In these countries, Direct traffic emerged as the primary source, driving the majority of visits.
- Q4. What was the number of unique visits by month? Is there any pattern?
    There wasn't any visible pattern regarding the monthly visits. Other than August and October 2016, all other months had similar visits. The month with the highest number of visits was August 2016, recording 1837 unique visitors. In contrast, October of the same year had the lowest number of visits, with 966 visits.


## Challenges 

There were instances of duplicate rows within the same columns, and some tables had identical columns, such as 'sales_report' and 'sales_by_sku.' 

Establishing relationships between tables proved challenging due to missing values in key columns, such as the 'productSKU' or "fullvisitorid". As a result, foreign key constraints had to be dropped, potentially compromising data integrity. Additionally, joining the table containing demographic data (country/city) with the order information was not possible due to the absence of a suitable key to establish a proper connection between the two tables.



## Future Goals

My primary focus would be on data cleaning, including the creation of bridge tables to transform many-to-many relationships into one-to-many. Additionally, I would drop duplicate rows and columns from the tables.

Furthermore, I would assess the adequacy of the data for predicting the purchase preferences of visitors from specific countries or cities. This analysis will enable the utilization of e-commerce data to recommend similar or personalized products that align with their preferences.

