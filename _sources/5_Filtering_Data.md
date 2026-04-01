# Filtering Data

- In SQL, we use WHERE and HAVING Clause in order to filter the data based on a condition. So the important thing in filtering data is condition. So SQL supports different types of operators for writing conditions in WHERE and HAVING clauses.

- There are actually 5 types of Operators in SQL. Those are Comparision Operators, Logical Operators, Range Operators, Membership Operators and Search Operators.

## Comparision Operators

- In SQL, Comparision operators are used to compare two things or expressions. The basic syntax of writing a condition using comaprision operators is `Expression operator Expression`. Some types of condtions using comparision operators are :

  **column1 = column2** : Comparing two columns. `first_name = last_name`

  **column = value** : Comparing a column with a static value. `first_name = 'john'`

  **Function = value** : Comparing result of a function with static value. `UPPER(first_name) = 'JOHN'`

  **Expression = value** : Comparing the result of an expression with an value. `Price*Quantity = 1000`

  **Subquery = value** : Comparing the result of the Subquery with an value. `(SELECT AVG(price) from customers) = 1000`

- There are actually 6 types of comaprision operators in SQL. Those are :

  **1.Equal to (=)** : It checks if two values are equal

  **Ex** : Retrieve the customers of Germany Country

  ```sql

  SELECT *
  FROM customers
  WHERE country = 'Germany'

  ```

  **2.Not Equal to (<> or !=)** : It checks if two values are not equal

  **Ex** : Retrieve all customers other than Germany country

  ```sql

  SELECT *
  FROM customers
  WHERE country != 'Germany'

  ```

  **3.Greater than (>)** : It checks if a value is greater than another value.

  **Ex** : Retrieve all customers whose values are greater than 500

  ```sql

  SELECT *
  FROM Customers
  WHERE score > 500

  ```

  **4. Greater than or equal to (>=)** : It checks if an value is greater than or equal to anothor value.

  **Ex** : Retrieve customers whose score is greater than or equal to 500

  ```sql

  SELECT *
  FROM customers
  WHERE score >= 500

  ```

  **5. Less than (<)** : It checks if an value is less than 500.

  **Ex** : Retrieve all customers whose score is less than 500

  ```sql

  SELECT *
  FROM Customers
  WHERE score < 500

  ```

  **6. Less than or equal to (<=)** : It checks if all customers are less than 500 or not.

  **Ex** : Retrieve all customers whose score is less than or equal to 500.

  ```sql

  SELECT *
  FROM customers
  WHERE score <= 500

  ```

## Logical Operators

- Till now we have seen filtering data with one condition. But what if you want to retireve the data that must satisfy multiple conditions. In order to do this, SQL provides Logical Operators which are generally used to combine two or more conditions. These operators generally return either TRUE or FALSE.

- In SQL, we have 3 different logical operators. Those are :

  **AND** : AND operator returns TRUE iff all the conditions must be TRUE. Otherwise it returns FALSE.

  **Ex** : Retrieve all the customers of USA and whose scores are greater than 500.

  ```sql

  SELECT * 
  FROM customers
  WHERE country = 'USA' AND score > 500

  ```

  **OR** : OR operators return TRUE if atleast on the condition returns TRUE. Otherwise it returns FALSE.

  **Ex** : Retireve the customers of USA or customers with scores greater than 500.

  ```sql

  SELECT *
  FROM Customers
  WHERE country = 'USA' or score > 500

  ```

  **NOT** : NOT operator is actually reverse operator which excludes all matching rows. It actually returns the rows which are not met the condition.

  **Ex** : Retrieve all the customers which are not less than 500.

  For this example we can write the query in two ways.

  ```sql

  SELECT *
  FROM Customers
  WHERE NOT score < 500

  ```

  or

  ```sql

  SELECT *
  FROM Customers
  WHERE score >= 500

  ```

## Range Operator

- Range Operator is generally used to define a range for a condition in WHERE clause. Sometimes we want the data of customers whose scores falls in particular range. In those cases, we use range operator.

- In SQL, we have BETWEEN operator to define a range. In BETWEEN operator, the boundaries such as lower and upper boundary are included. I mean if you want to find customers whose scores fall in 100 to 500 using BETWEEN, then it means you are including the boundaries such as 100 and 500 in that range.

- Basic syntax of BETWEEN operator is :

  ```sql

  SELECT 
  col1,col2
  FROM table_name
  WHERE col1 BETWEEN value1 AND value2

  ```

- **Ex** : Retrieve the customers whose scores falls in between 100 and 500.

  ```sql

  SELECT *
  FROM Customers
  WHERE score BETWEEN 100 AND 500

  ```

- We can actually recreate this BETWEEN logic using comparision and logical operators. 

  ```sql

  SELECT 
  col1,col2
  FROM table_name
  WHERE col1 >= value1  AND col2 <= value2

  ```

## Membership Operator

- Membership Operators are generally used to check whether any field values of the table are member of provided list or not. For example if you want to get customers from this list of countries, then we have to check whether that customers country is present in that list or not. In these situations, we use membership operators.

- In SQL, we have a membership operator which is `IN` operator. The syntax of IN operator is :

  ```sql

  SELECT 
  col1,col2
  FROM table_name
  WHERE col1 IN (value1,value2,value3)

  ```

- **Ex** : Retrieve all the customers who are from both germany, USA and UK.

  ```sql

  SELECT *
  FROM Customers
  WHERE country IN ('Germany', 'UK', 'USA')

  ```

- We have anothor membership operator in SQL which is `NOT IN`. NOT IN is quite opposite to IN operator which checks if that field is not present in the given list. The basic syntax of NOT IN is

  ```sql

  SELECT
  col1,col2
  FROM table_name
  WHERE col1 NOT IN (value1, value2, value3)

  ```

## Search Operator

- Search Operator is generally used to search for a word or a letter in a text which we generally called pattern matching. In SQL, we have LIKE operator to do pattern matching. 

- In pattern matching, we majorly use two symbols. First one is `%` which indicates anything i.e 0, 1 or any charecter. Second one is `_` which indicates single charecter.

- If you want to find a customer whose name starts with `M` then the regualr expression we use is 'M%'. If you want to find a customer whose name ends with `in` then the regular expression we use is '%in'. If you want to find a customer whose name contains `r` the we use this '%r%' regular expression. If you want to find a customer whose name has 3rd letter `r` then we use '__r%' regular expression.

- **Ex** : Retrieve the customers whose name contains `r` .

  ```sql

  SELECT *
  FROM Customers
  WHERE first_name LIKE '%r%'

  ```

- **Ex** : Retrieve the customers whose name has 3rd letter `r`.

  ```sql

  SELECT *
  FROM Customers
  WHERE first_name LIKE '__r%'

  ```