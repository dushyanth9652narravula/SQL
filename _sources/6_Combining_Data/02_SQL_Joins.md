# SQL Joins

- As we know SQL Joins are nothing but combining two or more tables column by column. In SQL joins, two tables have common column which is called as key column. When performing join on two tables, SQL first find the matching row in both tables based on key column and merge those two rows into one row by appending all the columns side by side. This is how a join actually occurs.

- SQL Joins are used in multiple usecases. It is used in `recombining all the data`. Sometimes analysts wants all data in one master table which can be easy to perform analysis and create dynamic dashboards. It is used to `enrich the data` we already have. Sometimes we actually want to add some columns to original table such as adding zip code information of country of the customers. In those cases we can just join those reference table with the original table. It is also used to `check the existence of particular data`. Suppose if you want to all the customers who didn't even placed an order yet. In this case you just perform some type of join on customers and orders table then the result will show the customers who are not placed any order yet.

- To understand the joins clearly, assume the tables as circles. When you perform the joins three scenarios would arise. Those are 

  **Matching Data** : It is common data existed in two tables.

  **All Data** : It would be all the data from two tables or all the data from any one table.

  **Un Matching Data** : It is data exists in one table but not in anothor table.

- Based on these three scenarios different types of Joins araised. Those are NO JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN, INNER JOIN, LEFT ANTI JOIN, RIGHT ANTI JOIN, FULL ANTI JOIN, CROSS JOIN.

## NO JOIN

- In SQL, NO JOIN is returning the data from two tables without combining them. The basic syntax of NO JOIN is simply two select statements of both tables returning all the columns

- **Syntax** :

  ```sql

  SELECT *
  FROM table1;

  SELECT *
  FROM table2;

  ```

- **Ex** : Return all the data of customers and orders without combining them

  ```sql

  SELECT *
  FROM Customers;

  SELECT *
  FROM Orders;

  ```

## INNER JOIN

- INNER JOIN actually returns all the matching rows from both the tables based on a common key column. It simply returns common data present in both the tables. INNER Join actually starts with key column value on left table and start finding match for the same key column value in right table. If any match found in right table then it will include in the output otherwise not. Like this it would search for entire table.

- In INNER JOIN order of table doesn't matter. If you place left with right table and right with left table the output remains same.

- **Syntax** :

  ```sql

  SELECT
  t1.col1,
  t1.col2,
  t2.col1,
  t2.col3
  FROM table1 as t1
  INNER JOIN table2 as t2
  ON t1.col1 = t2.col1

  ```

- **Ex** : Retrieve the customer id, customer name, order id and sales made by the customers.

  Here it is asked to retrieve the names and sales of the customers who placed an order.

  ```sql

  SELECT
  c.id,
  c.first_name,
  o.order_id,
  o.sales
  FROM Customers as c
  INNER JOIN Orders as o
  ON c.id = o.customer_id

  ```

- INNER JOIN is actually used to get the combined data which is common in both tables or else it can be used to check the existence of the data. As in above example we have got the customer who placed at least one order.

## LEFT JOIN

- LEFT JOIN returns all the rows from the left table and matching rows from the right table. That means for LEFT JOIN, left table should be primary table and keep all the rows of it and right table is secondary table which is used as reference table to add extra information to left table by keeping only matching rows with the right table.

- IN LEFT JOIN order of the table matters a lot. If you switch the order, the output would change.

- The basic Syntax of LEFT JOIN is :

  ```sql

  SELECT 
  t1.col1,
  t1.col2,
  t2.col1,
  t2.col3
  FROM table1 as t1
  LEFT JOIN table2 as t2
  ON t1.col = t2.col1

  ```

- **Ex** : Retrieve the data of all customers and their orders including those without any orders

  ```sql

  SELECT
  c.id,
  c.first_name,
  o.order_id,
  o.sales
  FROM Customers as c
  LEFT JOIN Orders as o
  ON c.id = o.customer_id

  ```

- In the above example, sql first make an empty table with 4 columns of id, first_name, order_id, and sales. Now it first keeps the id and first_name of first customer in customers table. Now it searches for matching id in the right table. If any customer_id present in order table matches with id in customers table then sql will keep respective order id and sales in the output. Oterwise it will keep them as NULL. This is how LEFT JOIN works.

- LEFT JOIN is mainly used to get combined data and it used for data enrichment also.

## RIGHT JOIN

- RIGHT JOIN is strictly opposite to the LEFT JOIN. It returns all the rows from the right table and the matching rows from the left table. That means for RIGHT JOIN, right table is the primary source of data and keeps all the rows from it and left table is the secondary table which is additional or reference table and keeps only the matching rows.

- In RIGHT JOIN also order of tables really matters. If you change the order, the output would change.

- The basic syntax of RIGHT JOIN is :

  ```sql

  SELECT 
  t1.col1,
  t1.col2,
  t2.col1,
  t2.col3
  FROM table1 as t1
  RIGHT JOIN table2 as t2
  ON t1.col1 = t2.col1

  ```

- **Ex** : Retireve all the customers along with thier order including orders without any customers.

  ```sql

  SELECT
  c.id,
  c.first_name,
  o.order_id,
  o.sales
  FROM Customers as c
  RIGHT JOIN Orders as o
  on c.id = o.customer_id

  ```

- As we know RIGHT JOIN is strictly opposite to LEFT JOIN, so we can just switch the order of table s and perform LEFT JOIN itself instead of using RIGHT JOIN.

## FULL JOIN

- FULL JOIN generally keeps all the rows from both tables. I mean it keeps all the rows from left table and right table and matching rows also.

- In FULL JOIN, the order of tables doesn't matter. As we are getting all rows from both tables then order of tables doesn't matter here.

- The syntax is :

  ```sql

  SELECT
  t1.col1,
  t1.col2,
  t2.col1,
  t2.col3
  FROM table1 as t1
  FULL JOIN table2 as t2
  ON t1.col1 = t2.col1

  ```

- **Ex** : Retrieve all the customers and orders eventhough there is no match.

  ```sql

  SELECT
  c.id,
  c.first_name,
  o.order_id,
  o.sales
  FROM Customers as c
  FULL JOIN Orders as o
  ON c.id = o.customer_id

  ```

- FULL JOIN is used to make a master table with all data from both the tables. It is also used to check existence but with one additional condition which we see in below joins.

