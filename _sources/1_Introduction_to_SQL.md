# Introduction to SQL

## Introduction to Databases and SQL

- In today's world, many companies such as banking, insurance, ecommerce companies will generate a lot of data everyday. At start they actually used to store in text files. Later they started using Excel spreadsheets, to store data in a formatted way.

- But as days going on, companies are generating massive amount of data (such as customer data, product data, sales data etc). In order to store these massive amount of data, companies needs to replicate same kind of files multiple times. So storing the data, isn't a problem for them. When they are trying to retrieve or perform analysis on that data it becoming a problem for them. Because they have to lookup for many files to get the data they actually need.

- So to overcome these issues with files, companies stated using Databases to store their data. A Database is nothing but a container to store data in an organized way. A Database stores data in well organized way and makes the data more secure. In filesystem, if you want to analyze or query the data we have to lookup all the files and aggreates them manually. But in Database, we have a seperate language called SQL (Structured Query Language) to query the data stored in the database.

## DBMS and SQL Server

- DBMS stands for Database Management System which is a software system used to manage, create and retrieve the data from the database in a structured formatted way. So when you are interacting with the database means that you are interacting with DBMS software. DBMS is actually bridge between the Database and Users or Applications. Users or Applications uses APIs to interacts with DBMS and DBMS responds to the query of the User or Application.

- Generally, people say if DBMS is the heart of the system which controls everything then what generally a database is ?. Database is actually data + Schema created by DBMS. It is actually a secondary storage which contains data, schema and Metadata of the data.

- But we cannot maintain a live database in our PC. We need a more powerful PC to handle massive amount of data and make the data available to everyone. So the only option we have is Server which has enough computing capacity to handle massive amount of data and can make data available to everyone.

- So we can simply summarize that A database is an container which stores data in a structured format, SQL is language used to speak with database, DBMS is a software used to manage the database and Server is the computing power where database lives.

## Types of Databases 

- **Relational Databases** : Relational Databases are the types of databases which stores data in tabular form, which can be joined by using fields known as foriegn keys. These relational databases are designed in 1970. MySQL, Microsoft SQL Server and Oracle Server are examples of relational Databases.

- **Key-Value Databases** : Key-Value Database is a type of database where data is tored in key-value pairs. These databases are highly used in login sessions, shopping carts etc. Redis, DynamoDB are the examples of Key-Value Databases.

- **[Columnar Databases](https://stackoverflow.com/questions/2133017/what-is-a-columnar-database)** : Columnar Databases stores data in columns which means each column are stored in disk seperately without having the relationship with other fields or columns. Whereas RDBMS stores data in rows in the disk which means all data related to an user is stored in single block [In RDBMS all fields are related to each other]. So Columnar Database is majorly used for analytics because for analysis we majorly uses two or three rows. But adding a row to the columnar database is difficult. Cassandra and Hbase are the examples of columnar database.

- **Documnet Databases** : A Document Database is a type of database which store data in JSON like documents. The main purpose of this database is to store the semi structured data. If there is no relationship between the fields of the data or if we cannot know the exact schema of our data then we cannot able to use our traditional relational database. In these situations we can use these document database which offers at most flexibility in terms of schema. So whenever your data is variable, non uniform and semi-structured then we can use Documnet Databases. Major usecases of documnet database is product catlogs (where we store all the product information. We usually cannot store all the product information in tables because each product has different attributes or features which leads to null values when we store in relational database. This is the reason why amazon uses the dynamodb for storing product information.), Social media etc. Mongodb and Dynamodb are the examples of document databases. For more reference use this stackoverflow discussion [Document Databases](https://stackoverflow.com/questions/441441/why-should-i-use-document-based-database-instead-of-relational-database)

- **Graph Databases** : Graph Databases stores data as Nodes which are entities, Edges which are relationship with nodes and the properties of nodes. Graph databases are used handle highly connected data with millions or billions of relationships. Traditional databases like relational databases cannot handle these kind of relationships. Because relationships in RDBMS are handled by foriegn keys and join operations. If we more and more join operations, then RDBMS becomes slow, difficult to query and hard to scale. To overcome these issues we use Graph databases. Neo4j and Amazon Neptune are the examples of graph databases.

## Types of SQL Commands

- There are mainly three types of commands in sql. Those are :

  **Data Definition Language** : Data Definition Language (DDL) is used to define the database such as creation, deletion and altering the database. The basic commands in DDL are CREATE, ALTER, DROP. With these commands, we can just create, drop or alter the database but we doesn't have actual data in database which means we can create schema of the database with DDL commands.

  **Data Manipulation Language** : Once database is created, now its time to insert data into the database. To insert the data into the database, we use Data Manipulation Language. The major commands we use in Data Manipulation Language is INSERT, UPDATE and DELETE commands.

  **Data Query Language** : Once data gets inserted into the database, we now can perform some analytics on these database. Inorder to perform analytics on these databases, we use Data Query Language. The major command we use to query the data is SELECT command. 