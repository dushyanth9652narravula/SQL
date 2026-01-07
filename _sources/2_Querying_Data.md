# Querying the Data

- Querying the data is nothing but asking a question to a database by writing a query. Whenever you execute that query, the database process that query (question) and send back a response in the form of data that we want.

- In SQL, we majorly use SELECT clause to query the data from the database. But there are other clauses that we use for querying the data. Those are DISTINCT, TOP, FROM, WHERE, JOIN, GROUPBY, HAVING and ORDERBY. In order to master the concept of querying the data we need to know the usage of these clauses and execution order of these clauses.

## SELECT and FROM Clause

- In SQL, SELECT clause is used to select the particular columns of a table in a database. Whereas the FROM clause is used to get the data (or table) from database. In simpler words, FROM clause tells sql about the table that you need to look into it and SELECT clause tells the lists of columns that we need from that table.

- **Ex** : Retrieve all columns from the customers table

  ```sql

  SELECT *
  FROM Customers

  ```

- **Ex** : Retrieve first_name, country, score columns from the customers table

  ```sql

  SELECT 
  first_name,
  country,
  score
  FROM Customers

  ```

- **Note** : If you want to get all columns from a table then we doesn't need to specify all column names from the table, we just need to use asterik (`*`) marks instead of them. And one more thing we need to know that, SQL retrieve the columns in the order we have been specified. Suppose if you listed the columns as country, first_name and score then it prints the columns in the same order.


## WHERE Clause

- In SQL, WHERE clause is used to filter the data present the table based on a condition. Suppose if we want to retrieve the customers of germany country only then we need to use the WHERE clause with the condition `country = 'Germany'`.

- We need to write the WHERE clause right after the From clause. And WHERE clause starts executing once FROM clause got executed.

- **Ex** : Retrieve the customers from the Germany Country

  ```sql

  SELECT *
  FROM Customers
  WHERE Country = 'Germany'

  ```

- **Explanation** : First SQL executes the FROM clause by loading the Customers Table from the database. Then it filters the Customers table by using `Country = 'Germany'` condition. Once the table gets filtered then it starts selecting the columns that we have listed with SELECT Clause. Here we have used the `*`, so it selects all the columns.

## ORDER BY Clause

- In SQL, ORDER BY clause is used to sort the data in Ascending or Descending order. Once the Sql gets the table (data) and filtered it, then ORDER BY clause gets executed. ORDER BY uses a column to sort the data present in the filtered table (if where clause is present). It has two keywords. Those are `ASC` and `DESC`. `ASC` sorts the data in Ascending order and `DESC` sorts the data in Descending order.

- **Ex** : Retrieve all customers and sort the score of customers from highest to lowest.

  ```sql

  SELECT *
  FROM Customers
  ORDER BY score DESC

  ```

- We can list multiple columns in ORDER BY clause to sort the data which is called as Nested Sorting. But this nested sorting is only useful if the first column we are using in ORDER BY clause has repetition. If there is no repetition then nested sorting is useless.

- For example in customer table, country column has repetition. So if you sort the countries first in ASC or DESC and then sort any column in the table then we can get sorted results related to each country. Suppose you have sorted countries in Ascending order and then sorts the scores in Descending order, then you get scores of all customers in descending order for each country only. You wont get score in descending order for entire table. ORDER BY sorts the score for each country seperately once country column gets sorted.

  ```sql

  SELECT *
  FROM Customers
  ORDER BY country ASC, score DESC

  ```

- **Note** : By default, ORDER BY sorts the data in Ascending Order if we won't use any keyword like ASC or DESC. But using the keyword increases the readability, so that we can easily understand when we see that SQL Query.

## GROUP BY Clause

- In SQL, GROUP BY Clause is used to combine the data of a column which are repeating and perfrom aggregations on the corresponding columns. In simpler words, Group By makes all repeated words into single word and perform aggregations on corresponding values of these repeated words which means it combines data of all rows of a particular category such as country, color, id and so on.

- There are mainly 5 aggregation functions in SQL. Those are SUM(), AVG(), MIN(), MAX(), COUNT().

- **Ex** : Find the total score of each country in the customers table

  ```sql

  SELECT 
  country,
  SUM(score) AS total_score
  FROM Customers
  GROUP BY country

  ```

- In the above query, SQL first runs the FROM clause to get the Customers table and perform GROUP BY Clause on the Customers table based on the Country Column which combines all the data for each country and then SUMS the scores of each country and return it.

- When we are using the GROUP BY Clause, we need to remember one thing first. That is, the columns listed in SELECT Clause (Which are not present in Aggregation functions) should be listed in GROUP BY clause also. If not SQL returns an error.

- **Ex** : Find the total score and total customers for each country in customers table.

  ```sql

  SELECT 
  country,
  SUM(score) as total_score,
  COUNT(id) as total_customers
  From Customers
  GROUP BY country

  ```

## HAVING Clause

- In SQL, HAVING Clause is used to filter the data after performing the aggregation which means if you want to filter the data after performing the GROUP BY, then we use HAVING Clause.

- **Ex** : Find the total score of each country and return the countries which has total score greater than 800.

  ```sql

  SELECT
  country,
  SUM(score) AS total_score
  FROM Customers
  GROUP BY country
  HAVING SUM(score) > 800

  ```

- If you wanna see the difference between WHERE and HAVING Clause then we can see the below example. 

- **Ex** : Find the average score of each country considering only customers not equal to score zero and return the countries only that has average score higher than 430.

  ```sql

  SELECT
  country,
  AVG(score) as avg_score
  FROM Customers
  WHERE score != 0
  GROUP BY country
  HAVING AVG(score) > 430

  ```

- In the above example, if you see, i have used both WHERE and HAVING Clause. Here SQL first executes FROM clause to get the customers table and then it executes WHERE Clause to filter th customer who has score not equal to zero (as we shouldn't include those customers while finding the average values) and then it performed GROUP BY Operation to combine the Country data (At this step it won't find the average values). And then it executes the HAVING Clause by finding average scores of each country and removes the countries whose average score is less than 430 and then it start executing the SELECT Clause which asks it to return the Country and the average score of final result.

## DISTINCT Clause

- In SQL, DISTINCT Clause is used in conjunction with SELECT Clause. It is used to find the distinct elements or values present in a coulmn or used to list the distinct values in a column by removing the duplicates.

- **Ex** : Find the unique list of countries present in customers table

  ```sql

  SELECT
  DISTINCT country
  FROM Customers

  ```

- If you want to find the distinct elements of combination of two columns then we just need to list those two columns after DISTINCT Clause

- **Ex** : Find the unique list of customers and countries from customers table

  ```sql

  SELECT
  DISTINCT first_name, country
  FROM Customers

  ```

## TOP Clause 

- In SQL, TOP Clause is used to limit the no of rows in the final output. If you want to get only top 5 customers from customer table which consisting of 1000 customers then we can use TOP Clause. It is generally used after the SELECT Clause but before the list of columns. SQL executes the TOP Clause at the last phase of the query.

- **Ex** : Return the top 3 customers from the customers table

  ```sql

  SELECT
  TOP 3
  *
  FROM Customers

  ```

- **Ex** : Return the top 3 highest score customers from customers table.

  ```sql

  SELECT 
  TOP 3
  *
  FROM Customers
  ORDER BY score DESC

  ```

- In the above example, first we have sorted the customers in descending order based on the score and then we have selected only top 3 customers from the output. From this we can say that TOP Clause is one which gets executed in the last phase of query execution by DBMS.

## Coding and Execution Order of the SQL Query

- Coding order of a SQL Query is nothing but the order of writing the SQL Query. First we need to write the SELECT clause and then we need to write DISTINCT Clause (as DISTINCT is just a mode that how we need to select the column like we need to select columns as it is or we need to select columns by removing duplicates). Third we need to write the TOP clause and then list of columns should follows the TOP Clause. At fifth we need to write the FROM Clause and then we need to Write the Where Clause. At seventh we need to write the GROUP BY Clause and at eight we need to write the HAVING Clause. And finally we need to write the ORDER BY Clause. So Coding Order is `SELECT -> DISTINCT -> TOP -> List_Of_Columns -> FROM -> WHERE -> GROUP BY -> HAVING -> ORDER BY`

- The following SQL Query is example of Coding Order.

  ```sql

  SELECT DISTINCT
  TOP n
  col1,
  SUM(col2) as total
  FROM t1
  WHERE col3 = 0
  GROUP BY col1
  HAVING SUM(col2) > 0
  ORDER BY co1 DESC

  ```

- Now lets see the Execution Order of above Query. The Execution starts at FROM Clause which gets or loads required table from the database. Then it starts executing the WHERE clause which filters the loaded table based on the condition. Then it starts executing the GROUP BY clause which generally combines rows based on the category (column) provided to GROUP BY Clause (At the stage of query parsing, sql know which aggregations that needs to perform on which columns based on the category or column). So at the stage of GROUP BY itself it peforms all aggregations provided in SELECT Clause and HAVING Clause itself. By the end of GROUP BY execution, it prepare a new table with aggregations and category. Then it starts executing the HAVING Clause by filtering the aggregated table. Now it starts executing the SELECT Clause and Distinct Clause also by selecting the list columns provided and performing DISTINCT Operation on selected columns. Now it starts executing ORDER BY Clause which sorts the final table in either Ascending or Descending Order. At final sql executes the TOP Clause by selecting the TOP n rows.

- So the Execution Order is : `FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> DISTINCT -> ORDER BY -> TOP`

## Summary of Querying Data

- Here, we have seen how to retrieve the data from the database using the 8 different clauses. Those are :

  **SELECT** : SELECT clause is used to select the list of columns.

  **DISTINCT** : DISTINCT clause is used to remove the duplicates and return the unique values from the selected columns

  **FROM** : FROM clause to get the table from the database or it says execution engine which table it needs to use for this query.

  **WHERE** : WHERE clause is used to filter the table based on a valid condition.

  **GROUP BY** : GROUP BY clause is used to combine the similar data into one row based on the column or category provided and perform aggregations listed on SELECT and HAVING clauses.

  **HAVING** : HAVING Clause is used to filter the resultant table after execution of GROUP BY clause.

  **ORDER BY** : ORDER BY clause is used to sort the data either in ascending or descending order by using ASC or DESC keywords.

  **TOP** : TOP clause is used to retrieve top n rows from the final Resultant table.