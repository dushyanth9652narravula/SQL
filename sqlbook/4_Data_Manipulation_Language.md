# Data_Manipulation_Language

- Data Manipulation Language is simply used to insert, update or delete the data present in the table or database. In order to do this, we have generally 3 clauses in SQL. Those are INSERT, UPDATE, DELETE.

## INSERT Clause

- In SQL, INSERT clause is generally used to insert the tuples or rows into the table. There are actually two ways to insert the data into database by using INSERT clause. First one is manual entry. In this method, we hardcode the values that we actually want to insert into the database in INSERT query. Second method is using SELECT Clause instead of VALUES in INSERT Query. In second method, we insert the data into database by using anothor table which we generally do in real world scenario.

- The syntax for manual entry of data to database is :

  **Synatax** : Manual Data Insertion

  ```sql

  INSERT INTO table_name (col1,col2 ,....)
  VALUES (col1_value1,col2_value1,....),
  (col1_value2,col2_value2,......)

  ```

- In the above syntax, mentioning the column names is optional. If you mention the column names then you must insert values coresponding to the column name. If you don't mention then SQL assumes that you are inserting all values for columns present in the database in same order. If you didn't given a value for a column then it fills that value as NULL. If that field is mandatory then it raises an error.

- Suppose consider an example of persons table which we have created in DDL. In persons table we have id, first_name, birth_date, phone fields. In those 4 fields id, first_name and phone are mandatory fields which means the values of those columns shouldn't be NULL. Suppose if you didn't mention the columns in INSERT query and just given 3 field values then SQL raises error for not giving value for phone. Because you didn't mention the fields then it fills the given 3 values as in the same order of columns created in database. So it won't get any value for phone field which is mandatory. So it raises an error.

- **Ex** : Insert 3 rows into persons table.

  ```sql

  INSERT INTO persons (id, first_name,phone)
  VALUES (1,'Sam','Unknown'),
  (2,'Ann','Unknown'),
  (3,'John','Unknown')

  ```

- In the above example, i didn't given the values for birth_date which means SQL fills those values with NULL in the database.

- The syntax for second method is :

  **Syntax** : INSERT + SELECT

  ```sql

  INSERT INTO table_name1
  SELECT 
  col1,
  col2, ...
  FROM table_name2

  ```

- Now lets insert the id and first_name for persons table from customer table. 

  ```sql

  INSERT INTO persons (id, first_name,phone)
  SELECT
  id,
  first_name,
  'Unknown'
  FROM customers

  ```

- In the above example, i have taken the id and first_name from the customers table but i have given 'unknown' phase to each id and first_name. This is because i need to insert the phone no also to persons table. But i doesn't have phone no of customers in customers table. So simply, i placed 'unknown' phrase for each customer.

- **Note** : We might wonder that how SELECT replacing the VALUES clause here. There is a reason for this. Both VALUES and SELECT Clause produces same result which means they both create tuples which in term called as rows. I mean `VALUES (a,b,c)` results same as `SELECT a,b,c` . So that is the reason we can replace VALUES with SELECT here.

## UPDATE Clause

- In SQL, UPDATE clause is used to update already existing data in the database which means we can update a column or multiple column values using UPDATE clause.

- **Syntax** : 

  ```sql

  UPDATE table_name
  SET column1 = value1,
  column2 = value2,
  ..............
  WHERE condition

  ```

- In UPDATE clause, WHERE clause is important. If you don't use WHERE clause in it then it update values of entire column. So we need to use the WHERE clause with UPDATE clause.

- **Ex** : Update the score of customers who has NULL to 0.

  ```sql

  UPDATE customers
  SET score = 0
  WHERE score IS NULL

  ```

## DELETE Clause

- In SQL, DELETE clause is generally used to delete the rows in the table. The basic syntax of the DELETE clause is :

  **Syntax** : 

  ```sql

  DELETE FROM table_name
  WHERE condition

  ```

- **Ex** : Delete the all the customers whose score is NULL

  ```sql

  DELETE FROM customers
  WHERE score IS NULL

  ```

- **Note** : If you don't use the WHERE clause in the DELETE clause then it will delete all the rows in the table. If you want to make the table empty then we can use `TRUNCATE` instead of `DELETE`. Because execution of `DELETE` involves a lot of logging and other stuff. So its execution becomes a little bit slower while deleting all rows. But `TRUNCATE` doesn't do all these logging and other stuff. So we can use to empty the table. The syntax for `TRUNCATE` is `TRUNCATE TABLE table_name`