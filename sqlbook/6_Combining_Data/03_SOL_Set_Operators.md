# SET Operators

## Introduction

- Set Operators is anothor mechanism in sql which is used to combine the data. Set Operators combines data row by row which means they appends each row of second table to the first table.

- Basic syntax of the set operators are :

  ```sql

  SELECT 
  col1,
  col2
  FROM table1

  SET OPERATOR

  SELECT
  col1,
  col2
  FROM table2

  ```

- So basically the syntax would be

  ```sql

  SELECT QUREY 1

  SET OPERATOR

  SELECT QUERY 2

  ```

## Rules for using Set Operators

- There are some rules which we need to follow when we are using set operators. Those are:

  1. SET Operator can be used almost in all clauses such as WHERE, JOIN, GROUP BY, HAVING etc. But ORDER BY clause must be used at end of second SELECT query only. This means we can use all other clauses such as WHERE, JOIN, GROUP BY, HAVING in both SELECT Queires but we need use ORDER BY clause only once which is at the end of second SELECT query only.

  2. The number of columns in each query must be same which means if you are selecting only two columns in first SELECT query then you need to select only two columns in the second SELECT query.

  3. Data types of columns in each query must match or compatible. This means if you are selecting two columns in both queries then data type of column1 in query1 must be same as data type of column1 in query2. Similary for column2 also.

  4. The order of columns in each query must be same. This rule is same as rule3 which means if column1 in query1 is varchar then column1 in query2 must be varchar. This automatically implies the order is same.

  5. The column names in the result set are determined by column names specified in the first query. This means first query has control on the result. Suppose if you are selecting 2 columns in both queries then sql decides data types of result set based on the data types of the 2 columns in first query. If column1 data type in first query is integer then column1 of second query is must be integer. If not sql tries to convert the data type of column1 if second query into integer. If it fails to convert then sql throws an error. This means first query controls the resultant set. So column names of resultant set is same as column names or aliases mentioned in first SELECT query only. Eventhough you mention any aliases on second query then sql doesn't consider them.

  6. Even if you met all the rules and sql shows no errors the result may be incorrect. Because incorrect column selection might leads to inaccurate results. For example, if you select first_name, last_name in first sql query and last_name , first_name in second sql query then the result would be incorrect as you changed the order in second query. Eventhough this query met all the above rules but the result would be incorrect as some of the firstnames are lastnames and some of the lastnames are firstnames.

  ```sql

  SELECT
  firstname,
  lastname
  FROM sales.customers

  UNION

  SELECT
  lastname,
  firstname
  FROM sales.employees

  ```