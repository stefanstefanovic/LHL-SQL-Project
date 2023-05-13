```What issues will you address by cleaning the data?

1 - Remove irrelevant data (e.g. empty columns)
2 - Check data types of columns and change it
3 - Fix structural errors (e.g. unnecessary spaces)
4 - Check if there are any outliers
5 - Link tables


Queries:
Below, provide the SQL queries you used to clean your data.```

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


