# Combining Data

- Combining Data is nothing but combining two or more table to get all the data in one place. In real world data of a customer is spread into multiple tables. Such as all the personal inforamtion of a customer such as Name, Age, Country are in one table and Orders that customers made in anothor table and payments that customer made in anothor table etc. While performing the analytics, analysts need all the data at one place so that they can easily build dashboards by using that data. So we have to combine all the sql tables to make one master table.

- To combine the data, SQL provides two mechanism. Those are Combining tables using SQL JOINS and second one is combining tables using SET Operators.

- **Joins** : Joins combine tables column by column which means they merge two tables side by side based on a common column or key column. If you want to combine two tables using joins then you need to have common or key column in both the tables. There are different types of Joins in SQL. Those are Right Join, LEFT Join, Inner Join, Full Join etc.

- **SET Operations** : SET operators combine tables row by row which means they actually append two tables. If you want to combine two tables using SET operators then both the tables should have same columns. There are different set operators in SQL. Those are UNION, UNION ALL, EXCEPT, INTERSECT.