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
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 

There were instances of duplicate rows within the same columns, and some tables had identical columns, such as 'sales_report' and 'sales_by_sku.' 

Establishing relationships between tables proved challenging due to missing values in key columns, such as the 'productSKU' or "fullvisitorid". As a result, foreign key constraints had to be dropped, potentially compromising data integrity. Additionally, joining the table containing demographic data (country/city) with the order information was not possible due to the absence of a suitable key to establish a proper connection between the two tables.



## Future Goals

My primary focus would be on data cleaning, including the creation of bridge tables to transform many-to-many relationships into one-to-many. Additionally, I would drop duplicate rows and columns from the tables.

Furthermore, I would assess the adequacy of the data for predicting the purchase preferences of visitors from specific countries or cities. This analysis will enable the utilization of e-commerce data to recommend similar or personalized products that align with their preferences.

