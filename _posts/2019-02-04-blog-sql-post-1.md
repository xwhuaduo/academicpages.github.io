---
title: 'SQL Summary'
date: 2019-02-04
permalink: /posts/2019/02/blog-SQL-1/
tags:
  - SQL
  - Summary
---

(Summarized from toturial https://mode.com/sql-tutorial/)

# SQL Tutorial

## Basic SQL
### 1. SQL SELECT
SELECT Everything
```
SELECT *
    FROM data
```

``` 
SELECT a, b, c
    FROM data
```

* SQL treats one space, multiple spaces, or a line break as being the same thing.

* SQL treats upper case, or lower case as being the same thing.

* Use double quotes to refer to columns with space in their names.

* To change column names, use `AS "New Name"`

```
SELECT a AS "New a"
    FROM data
```

### 2. SQL LIMIT
Limit the number of output. Could create output like what `head()` does in **R**.

e.g.
``` 
SELECT *
    FROM data
    LIMIT 100
```
### 3. SQL WHERE
To filter the data.

### 4. SQL Comparison Operators
=

<> or !=

>

<

\>=

<=

e.g.
```
SELECT *
  FROM tutorial.us_housing_units
 WHERE month_name > 'J'
```
return records with month_name starts with "j" or later in alphabet. 'Ja' is considered to be greater than 'J'.

#### Arithmetic in SQL
SQL perfrom arithmetic across columns on values in a given row.

### 5. SQL Logical Operators
LIKE
IN
BETWEEN
IS NULL
AND
OR
NOT

#### LIKE: get similar values
```
SELECT *
    FROM data
    WHERE "group" LIKE 'Snoop%'
```
**Note:** we could only use signle quotes after LIKE????
 * The double quotes are a way of indicating that you are referring to the column name, not the SQL function.
 * `%` represents any character or set of characters.
 * `_` represents an individual character.

*The keyword **ILIKE** can be used instead of **LIKE** to make the match case insensitive*

#### IN:

 ```
SELECT *
    FROM data
WHERE a IN (1,2,3)
 ```

#### BETWEEN
 `WHERE a BETWEEN 1 AND 3`
#### IS NULL
 `WHERE artist IS NULL`
#### AND

#### OR
#### NOT
#### ORDER BY 
 ` ORDER BY a`

 `ORDER By a DESC`

 `ORDER BY a, b`

`a` and `b` could be replaced by column index. May not work depending on software you use.

### 6. Comments
` SELECT * --Comments`

```
/*
comments
comments
comments
*/
SELECT *
```

## Intermediate SQL
### 7. SQL Aggregate Functions
### `COUNT` 
Count the number of rows, could be used on non-numerical columns.

#### Counting individual columns
`SELECT COUNG(high)` will provide a count of all rows in which the `high` column **is not null**.

### `SUM`
* Only use `SUM` on columns containing numerical values. 
*  `SUM` trates nulls as 0.

**NOTE:** aggregators only aggregate vertically

### `MIN/MAX`

### `AVG`
Only works on numerical columns. It **ignores nulls completely**.
### `GROUP BY`
Used when you want to  aggregate only part of a table, e.g., count the number of entries for each year.
```
SELECT year,
    COUNT(*) AS count
    FROM data
    GROUP BY year
```
Order by multiple columns:
```
SELECT year,
        month,
    COUNT(*) AS count
    FROM data
    GROUP BY year, month
```
With `LIMIT`: LIMIT will be performed after aggregation.

### 8. SQL HAVING
When aggregation is not enough, (`WHERE` clause doesn't allow you to filter on aggregate columns), we use `HAVING` clause.

e.g.
```
SELECT year, month, MAX(high) AS month_high
    FROM data
    GROUP BY year, month
    HAVING MAX(high) > 400
    ORDER BY year, month
```

#### QUERY clause order
1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. HAVING
6. ORDER BY
7. LIMIT (LIMIT should be at last?)

### 9. SQL DISTINCT
Using SQL `DISTINCT` for viewing unique values.
`SELECT DISTINCT a,b` will yield unique a-b pairs.

Useful trick `COUNT(DISTINCT year)`
**NOTE:)** `DISTINCT`, particularly in aggregations, can slow your queries down quite a bit.

### 10. SQL CASE
SQL's way of handling if/then logic, always goes in the `SELECT` caluse.
```
CASE WHEN a=b THEN c=1
      ELSE NULL END AS blala
```
`CASE` returns a new column `blala`
`CASE THEN` could be combined with `AND/OR`

```
SELECT CASE WHEN year = 'FR' THEN 'FR'
            ELSE 'Not FR' END AS year_group,
            COUNT(1) AS count
  FROM benn.college_football_players
 GROUP BY CASE WHEN year = 'FR' THEN 'FR'
               ELSE 'Not FR' END
-- Just group by year_group would still work
```
**NOTE:)** the number used in the query corresponding to the columns SELECTed.

#### With multiple conditions in one query
`WHERE` clause only allows you to count one condition. `CASE WHEN...` will solve this problem.
```
SELECT CASE WHEN year = 'FR' THEN 'FR'
            WHEN year = 'SO' THEN 'SO'
            WHEN year = 'JR' THEN 'JR'
            WHEN year = 'SR' THEN 'SR'
            ELSE 'No Year Data' END AS year_group,
            COUNT(1) AS count
  FROM benn.college_football_players
 GROUP BY 1
 ```

#### Using CASE inside of aggregate functions
 ```
SELECT COUNT(CASE WHEN year = 'FR' THEN 1 ELSE NULL END) AS fr_count,
       COUNT(CASE WHEN year = 'SO' THEN 1 ELSE NULL END) AS so_count,
       COUNT(CASE WHEN year = 'JR' THEN 1 ELSE NULL END) AS jr_count,
       COUNT(CASE WHEN year = 'SR' THEN 1 ELSE NULL END) AS sr_count
  FROM benn.college_football_players
 ```

**NOTE:)** In the last practice problem of https://mode.com/sql-tutorial/sql-case/, if you use condition like `CASE WHEN school_name < 'M' THEN 1 ELSE NULL END AS aaa`, Then you will get incorrect answer, since 'MINNESOTA' > 'M'. The correct way is to set `school_name <'n'`. Upper case or not doesn't matter.


### 11. SQL JOIN
#### Aliases in SQL
Give a table an alias by adding a space after the table name nd typing the intended name of the alias`FROM benn.college_football_players players`
#### JOIN and ON
ON clause set the indeces we will use to merge the datasets.

* SELECT all columns of two table: `SELECT *`
* SELECT all columns of table players: `SELECT players.*`

### 12. SQL INNER JOIN
`INNER JOIN` eliminate rows from both tables that do not satisfy the join condition set forth in the `ON` statement.

### 13. SQL Outer Joins
* LEFT JOIN: returns everything from the left table.
* RIGHT JOIN
* FULL OUTER JOIN

### 14. SQL Joins Using WHERE or ON
When you want filter one or both of the tables before joining them.

e.g. with `AND`. **`AND` condition works before we join tables.** Return 27355 records.
```
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink

   AND acquisitions.company_permalink != '/company/1000memories'

 ORDER BY 1
 ```

 e.g. with 'WHERE'. **`WHERE` works after we join tables.** Return 27354 records. That's why we have the `OR` clause after `WHERE`.
 ```
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink
 
 WHERE acquisitions.company_permalink != '/company/1000memories'
    OR acquisitions.company_permalink IS NULL
 
 ORDER BY 1
 ```

### 15. SQL FULL OUTER JOIN
 e.g.
 ```
     SELECT COUNT(CASE WHEN companies.permalink IS NOT NULL AND investments.company_permalink IS NULL
                      THEN companies.permalink ELSE NULL END) AS companies_only,
           COUNT(CASE WHEN companies.permalink IS NOT NULL AND investments.company_permalink IS NOT NULL
                      THEN companies.permalink ELSE NULL END) AS both_tables,
           COUNT(CASE WHEN companies.permalink IS NULL AND investments.company_permalink IS NOT NULL
                      THEN investments.company_permalink ELSE NULL END) AS investments_only
      FROM tutorial.crunchbase_companies companies
      FULL JOIN tutorial.crunchbase_investments_part1 investments
        ON companies.permalink = investments.company_permalink
```
### 16. SQL UNION
Allows you to stack one dataset on top of the other. (Allows you to write two separate `SELECT` statements).

when you use `UNION`, the dataset is appended, and any rows in the appended table that are exactly identical to rows in the first table are **dropped**. If you’d like to append all the values from the second table, use **`UNION ALL`**.

#### Rules for appending data:
1. Both tables must have the same number of columns
2. The columns must have the same data types in the same order as the first table. (Column names don't necessarily have to be the same)

**NOTE:)** For the last practice https://mode.com/sql-tutorial/sql-union/, I should conduct `JOIN` on each `SELECT` statement, then `UNION ALL`.
**!!!**

### 17. SQL Joins with Comparison Operators
See the e.g. of `AND` and `WHERE` in **14**.

### 18. SQL Joins on Multiple Keys
It can occasionally make your query run faster to join on multiple fields.

e.g. the results of the following query will be the same with or without the last line. However, it is possible to optimize the database such that the query runs more quickly with the last line included:
```
SELECT companies.permalink,
       companies.name,
       investments.company_name,
       investments.company_permalink
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_investments_part1 investments
    ON companies.permalink = investments.company_permalink
   AND companies.name = investments.company_name
```

### 19. SQL Self Joins
Used for complicated filtering. 

e.g. you wanted to identify companies that received an investment from Great Britain following an investment from Japan.
```
SELECT DISTINCT japan_investments.company_name,
       japan_investments.company_permalink
  FROM tutorial.crunchbase_investments_part1 japan_investments
  JOIN tutorial.crunchbase_investments_part1 gb_investments
    ON japan_investments.company_name = gb_investments.company_name
  
   AND gb_investments.investor_country_code = 'GBR'
  
   AND gb_investments.funded_at > japan_investments.funded_at
 
 WHERE japan_investments.investor_country_code = 'JPN'
 
 ORDER BY 1
```

## Advanced SQL
### Data Types
#### Changing a column's data type
