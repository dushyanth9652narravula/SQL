# Data Definition Language

- Data Definition Language is used to define the structure of the database which means it is used to define or create, alter or delete the schema or tables in the database. To define, alter or delete the tables or schema of the database, sql provides 3 clauses. Those are CREATE, ALTER, DROP

## CREATE Clause

- In SQL, CREATE clause is used to define the schema or table of the database. In this create clause, we actually need to mention the name of the table, column names, column types and constraints on each column and we provide constraints to tables in form of primary or foriegn key which are used to connect with other tables.

- **Syntax**

  ```sql

  CREATE TABLE table_name (
    col1_name type1 constraint1,
    col2_name type2 constraint2
    CONSTRAINT constraint_name constraint_type (<on which column we are applying that constraint>)
  )
  ```

- **Ex** : Create a table called persons with columns - id, person_name, birth_date, phone and primary key constraint on id.

  ```sql

  CREATE TABLE persons(
    id INT NOT NULL,
    person_name VARCHAR(50) NOT NULL,
    birth_date DATE,
    phone VARCHAR(15) NOT NULL
    CONSTRAINT pk_persons PRIMARY KEY (id)
  )

  ```

## ALTER Clause

- In SQL, ALTER Clause is used to change the structure of the database such as adding a column to a table or deleting a column from the table present in the database.

- **Syntax** : For Addding a column to a table

  ```sql

  ALTER TABLE table_name
  ADD
  col1 type_of_col1 constraint_of_col1,
  col2 type_of_col2 constraint_of_col2

  ```

- **Ex** : Add email and address column to the persons table

  ```sql

  ALTER TABLE persons
  ADD
  email VARCHAR(50) NOT NULL,
  home_address VARCHAR(50)

  ```

- **Syntax** : Deleting the columns from the table using ALTER Clause

  ```sql

  ALTER TABLE table_name
  DROP COLUMN col1, col2 

  ```

- **Ex** : Delete the birth_date and home_address column from the persons table

  ```sql

  ALTER TABLE persons
  DROP COLUMN birth_date, home_address

  ```

## DROP Clause

- In SQL, DROP Clause is used to drop entire table from the database which means it deletes specified table from the database.

- **Syntax**

  ```sql

  DROP TABLE table_name

  ```

- **Ex** : Delete the persons table from the database

  ```sql

  DROP TABLE persons

  ```