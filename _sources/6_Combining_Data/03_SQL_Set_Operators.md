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

- Now lets see the set operators that sql provides to combines tables row by row.

## UNION Operator

- UNION Operator returns all distinct rows from both queries which means it combines all rows of both tables and removes duplicate rows from the result.

- The basic syntax of UNION Operator is :

  ```sql

  SELECT QUERY 1

  UNION

  SELECT QUERY 2

  ```

- **Ex** : Combine the data from employees and customers into one table from salesdb.

  ```sql

  SELECT 
  firstname,
  lastname
  FROM Sales.Employees

  UNION

  SELECT
  firstname,
  lastname
  FROM Sales.Customers

  ```

- **Working of UNION** : SQL first extracts the column names from the first SELECT query such as firstname and lastname then it removes any duplicates present in table1 and then it adds those distinct rows from the table1 to resultant. Now it checks rows in table2 to decide whether any of the rows present in resultant or not. If that rows are not present in resultant then sql appends those rows to resultant set.


## UNION ALL Operator

- UNION ALL returns all the rows from the both queries, including duplicates. UNION ALL is faster than UNION operator as it doesn't perform any other operations like removing duplicates etc. So if you know your tables doesn't contain any duplicates and you want to combine the two tables then use UNION ALL instead of UNION as it is faster. If you want to check whether there are any duplicates present in the resultant table then we can use UNION ALL Operator.

- Basic Syntax of UNION ALL is :

  ```sql

  SELECT QUERY 1

  UNION ALL

  SELECT QUERY 2

  ```

- **Ex** : Combine the data from customers and employees into one table including the duplicates.

  ```sql

  SELECT
  firstname,
  lastname
  FROM Sales.Customers

  UNION ALL

  SELECT
  firstname,
  lastname
  FROM Sales.Employees

  ```

- **Working of UNION ALL** : In case of UNION ALL, SQL first extracts the column names from the first SELECT Query then it extracts all the rows from the table1 and append it to result set then it extracts all the rows from table2 and it appends those rows to the result set.

## EXCEPT Operator

- EXCEPT Operator returns the all unique rows from first table or query which are not present in second table or query. When we are using EXCEPT Operator Order of the tables or queries matters as it effects the final result.

- Basic Syntax of EXCEPT is :

  ```sql

  SELECT QUERY 1

  EXCEPT

  SELECT QUERY 2

  ```

- **Ex** : Find the customers who are not employees.

  ```sql

  SELECT 
  firstname,
  lastname
  FROM Sales.Customers

  EXCEPT

  SELECT
  firstname,
  lastname
  FROM Sales.Employees

  ```

- **Working of EXCEPT** :  In Case of EXCEPT, SQL first extracts the column names from first query such as firstname, lastname here. Now it checks each row in query1 (or table1) with each row in table2. If that row from table1 is not present in the table2 then it appends that row to the result set. So in case of EXCEPT, SQL uses second query as lookup table. This is how it extracts rows from query1 which are not present in query2.

## INTERSECT Operator

- INTERSECT Operator returns the rows which are common in both the queries. This means it returns the common rows from both the tables and excludes all other rows from the both tables. Here Order of queires doesn't matter.

- Basic Syntax is :

  ```sql

  SELECT QUERY 1

  INTERSECT

  SELECT QUERY 2

  ```

- **Ex** : Find the customers who are also employees.

  ```sql

  SELECT 
  firstname,
  lastname
  FROM Sales.Customers

  INTERSECT

  SELECT
  firstname,
  lastname
  FROM Sales.Employees

  ```

- **Working of INTERSECT** : SQL first extracts column names from first query and then starts checking the rows in table1 with rows in table2 to know whether any row is common between two tables. If it finds any common row then it apends it to the result set. Like this it scans entire tables.

## Usecases of Set Operators

- Set Operators like UNION, UNION ALL are used to combine similar data before analyzing the data. Suppose if you have tables such as Customers, Employees, Suppliers, Students etc and you want perform some analysis on these tables and generate reports. Generally they will write seperate queires for each table and include results of these queries in that report. Since they contain similar data such as firstname, lastname, country, gender, address etc, we can combine all these data into one data and make it as person table and then we can write single query to perform analysis and include those results in that report. This way generate more consistent results as we are writing 4 queries for 4 table and if you make changes to two queries and forgot to change the other two queries then we might see inconsistent results. So combining all data into one mega table would be better choice for analysis part.

- Generally, Database developers divide the data into multiple table to optimize the performance and archive old data. So many organizations use seperate tables to store the order data for each year. Because, they want to archive thier older data for furture analysis. I mean some analytics need only current year data and some analytics need entire data. So to improve the performance they seperate data like this. So when we are performing analytics on entire order data then its better to combine all the order data using UNION or UNION ALL operator.

- For example we have current year order data as orders and older order data as orderarchive. To combine this we use this query.

  ```sql

  SELECT
  *
  FROM Sales.Orders

  UNION

  SELECT
  *
  FROM Sales.OrderArchive

  ```

- Since we know both orders and orderarchive have same schema we have used `*` instead of mentioning all the columns in the query. But its not best way to do it. Suppose , for coming year by mistake they kept productId at first and then orderID as second column, then when you try to combine the older table with newer order table then you get incorrect results by mixing order and product Ids. So its better to mention all the column names in the both queries instead of `*` symbol.

  ```sql

  SELECT 
  'Orders' As SourceTable,
  [OrderID],
  [ProductID],
  [CustomerID],
  [SalesPersonID],
  [OrderDate],
  [ShipDate],
  [OrderStatus],
  [ShipAddress],
  [BillAddress],
  [Quantity],
  [Sales],
  [CreationTime]
  FROM Sales.Orders

  UNION

  SELECT
  'OrdersArchive' AS SourceTable,
  [OrderID],
  [ProductID],
  [CustomerID],
  [SalesPersonID],
  [OrderDate],
  [ShipDate],
  [OrderStatus],
  [ShipAddress],
  [BillAddress],
  [Quantity],
  [Sales],
  [CreationTime]
  FROM Sales.OrdersArchive
  ORDER BY OrderID

  ```

- So by mentioning the column names we might know that we need to change the order of columns (change the order of ProductID and OrderID) while combining the new table with old table. And one more thing, its better to add new column such as sourcetable to know from which table this column has came from. These are the usecases of UNION and UNION ALL.

- SET Operators such as EXCEPT is used majorly in DELTA detection. Delta detection is nothing but identifying the changes between two batches of data. Suppose you are shifting data from source systems to data warehouse. On day 1 , you shifted data of john doe and jan doe data from source systems to data warehouse. Now on day 2 you got john doe and alice data in source system. But we have already sent john doe data to data warehouse. So we have to send only alice data to data warehouse today. So what we actually do is we perform EXCEPT Operation on day 2 and day 1 data, so that we get only unique data from day2 which are not present in day1. Then we can add these unique data to data warehouse.

- This EXCEPT operated might be used in case of data migration. Some times organizations migrates data from Database1 to Database2 for backups and any other usecases. So in these case they have migrate all the data from Database1 to Database2 so that both databases should be identical. To check all data gets migrated from Database1 to Database2 then perform EXCEPT operation on Database1 and Database2. This will give unique rows from Database1 which are not present on Database2. If it gives empty set then we can tell all the data present in Database1 is migrated to Database2. Now we need to perform reverse operation also which means we need perform EXCEPT Operation on Database2 and Database1 so that we get unique rows from Database2 that are not present in Database1. If we get any rows they we can say that new rows are added to Database2. So they can delete those new rows from Database2 to make both Databases identical. If we get empty set then we can say both databases are identical.

- These is how SET Operators are used in real world use cases.

## Summary

- Set Operators are used to combine the tables row by row into a single resultant set. We have 4 major set operators those are UNION, UNION ALL, EXCEPT, INTERSECT.

- UNION is used to return all the rows from the both tables by removing the duplicates from them. UNION ALL returns all the rows from both the tables including the duplicates.

- EXCEPT returns the unique rows from the first query or table which are not found in second query or table. INTERSECT returns the common rows between both tables.

- Major rules of SET Operators are same no of columns, data types, order of columns and column names or aliases are determined by the first query only.

- Major use cases are Combine Similar Data (UNION or UNION ALL), DELTA Detection (EXCEPT) and Data Completeness check or Data Migration (EXCEPT).