# String Functions

- String Functions are type of single row functions which are used to clean, manipulate and transform the text data.

- Since we have so many stirng functions in SQL, we are gonna divide them into 3 catergories which are shown in below image

  <p align="center">
  <img src="../_static/Stringfunctions.png" alt="String Functions" width="1000"/>
  <br>
  <em>String Functions</em>
  </p>

## String Manipulation Functions

### 1. CONCAT

- CONCAT function is generally used to combine multiple strings into one. The basic syntax of CONCAT is :

  `CONCAT('Maria', ' ', 'Begam') -> 'Maria Begam'`

- **Ex** : Combine the customer firstnames with their country names

  ```sql

  SELECT
  firstname,
  country
  CONCAT(firstname, ' ', country) AS name_country
  FROM Customers

  ```

### 2. UPPER

- UPPER function is used to convert all the charecters into uppercase. The basic syntax of UPPER function is :

  `UPPER('Maria') -> 'MARIA'`

- **Ex** : Convert all the firstnames of the customers to uppercase

  ```sql

  SELECT
  UPPER(firstname),
  country
  FROM Customers

  ```

### 3. LOWER

- LOWER function is used to convert all the charecters into lowercase. The basic syntax of the function is :

  `LOWER('Maria') -> 'maria' `

- **Ex** : Convert all the charecters of firstnames of customers into lowercase

  ```sql

  SELECT
  LOWER(firstname),
  country
  FROM Customers

  ```

### 4. TRIM

- TRIM function is generally used to trim the leading and tailing whitespaces. the basic syntax of TRIM is :

  `TRIM(' John') -> 'John'`

- **Ex** : Detect whether there are any whitespace present in firstnames of customers.

  ```sql

  SELECT
  firstname
  FROM Customers
  WHERE firstname != TRIM(firstname)

  ```

### 5. REPLACE

- REPLACE function is used to replace a charecter with a new charecter. We can use this REPLACE function to remove a charecter from the string by replacing it with blank string (''). The basic syntax is :

  `REPLACE(<originalString>, <oldcharecter>, <newcharecter>)`

  `REPLACE('123-456-7890','-','/') -> '123/456/7890'`

  `REPLACE('123-456-7890','-','') -> '1234567890'`

- **Ex** : Remove the dashes from the phone no and make it clean.

  ```sql

  SELECT
  phone_no,
  REPLACE(phone_no,'-','') as cleaned_phone_no
  FROM Customers

  ```

## String Calculation Functions

- SQL has only one string calculation function which is LEN(). LEN() function is used to calculate how many charecters that are present in a string. It is used to calculate no of charecters for numbers, Date and any other data types also. The basic syntax for LEN() function is :

  `LEN(<value>)`

  `LEN('Maria') -> 5`

  `LEN(350) -> 3`

  `LEN(01-10-2026) -> 10`

- **Ex** : Find the no of charecters in customers first_name

  ```sql

  SELECT
  firstname,
  LEN(firstname) as len_name
  FROM Customers

  ```

## String Extraction Functions

### 1. LEFT

- LEFT() function is used to extract the specific number of charecters from the start of the string. The basic syntax of LEFT is :

  `LEFT(<string>,<no_of_charecters>)`

  `LEFT('Maria', 2) -> 'Ma'`

- **Ex** : Extract the first 2 charecters from firstnames of the customers

  ```sql

  SELECT
  firstname,
  LEFT(TRIM(firstname),2) as first_2_charecters
  FROM Customers

  ```

### 2. RIGHT

- RIGHT() function is used to extract the specific no of characters from the end of the string. The basic syntax of the RIGHT() function is :

  `RIGHT(<string>, <no_of_characters>)`

  `RIGHT('Maria',2) -> 'ia'`

- **Ex** : Extract the last 2 characters from the customers firstname.

  ```sql

  SELECT 
  firstname,
  RIGHT(TRIM(firstname),2)
  FROM Customers

  ```

### 3. SUBSTRING

- SUBSTRING() function is used to extract certain part of the string from the specified position. The basic syntax of SUBSTRING() is :

  `SUBSTRING(<string_value>, <start>, <length_of_substring>)`

  `SUBSTRING('Maria', 2, 3) -> 'ari'`

- **Ex** : Extract all characters of customers firstname after removing the first charecter.

  ```sql

  SELECT
  firstname,
  SUBSTRING(TRIM(firstname),2, LEN(firstname)-1) as name_substring
  FROM Customers

  ```
