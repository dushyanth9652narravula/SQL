# Window Ranking Functions

## Introduction

- Window Ranking Functions are used to rank the rows of the dataset based on a particular fields, such as rank the products based on thier sales etc. In SQL, we have two types of ranking. Those are Integer Based Ranking and Percentage Based Ranking.

- Integer Based Ranking assigns integer values to each row based on particular field. So the values of the rank are discrete. Whereas Percentage Based Ranking assigns percentages to each row based on the specified field and the values of percentage ranges from 0 to 1 only and these are continuous. 

- Integer Based Ranking are majorly used for Top / Bottom N Analysis which is finding the top 3 products based on sales etc. Whereas Percentage Based Ranking are used for Distribution Analysis which means find the products those are contributing top 20% of our sales etc.

- Functions in Integer Based Rankning are ROW_NUMBER(), RANK(), DENSE_RANK(), NTILE(). Functions for Percentage Based Ranking are CUME_DIST(), PERCENT_RANK() etc.

- The syntax of Window Ranking Functions are :

  ` <Window Function> OVER(PARTITION BY <Column> ORDER BY <Column>) `

- In the above syntax PARTITION BY Clause is optional but ORDER BY Column is required. ANd all the above mentioned window functions doesn't take single expression as input except `NTILE()` function which takes a number as input. And for ranking functions, we shouldn't change the Frame of the function. We should keep the default frame as it is.

## Interger Based Ranking

### ROW_NUMBER() Function

- ROW_NUMBER() Function assigns unique integer value to each row based on the specified field in the ORDER BY. It doesn't handle ties which  means eventhough two values are equal, ROW_NUMBER() assign different rank to those two values without skipping the ranks.

- For example, consider the sales of five products p1 : 40, p2 : 20, p3 : 80, p4 : 100, p5 : 80. Now rank the sales of the products from highest to lowest. Now SQL starts sorting the sales from highest to lowest resulting, p5 : 100, p3 : 80, p5 : 80, p1 : 40, p2 : 20 . Then it starts ranking those products as p5 : (100, 1), p3 : (80,2), p5 : (80,3), p1 : (40,4), p2 : (20,5). Here if you see that even p3 and p5 has same sales, the ROW_NUMBER() function assigns different rank for these tow products and no rank got skipped. 

- **Ex** : Rank the each products based on thier total sales

  ```sql

  SELECT
  ProductID,
  SUM(Sales) as TotalSales,
  ROW_NUMBER() OVER(Order By SUM(Sales) DESC)
  FROM Sales.Orders
  GROUP BY ProductId

  ```

### RANK() Function

- RANK() function also assigns an integer value (Rank) to each row based on the field specified in Order By clause. But RANK() function doesn't assigns a unique value to each row. It handles ties means if there is a tie between two rows it assigns same rank to those two rows and leaves the gaps in the ranking.

- Suppose, consider above example itself. If you are going to rank those products by using RANK() function, then the results could be p5 : (100 , 1) , p3 : (80, 2) , p5 : (80, 2) , p1 : (40, 4) , p2 : (20, 5). If you see the results, both products p3 and p5 has same rank as they both have same sales and RANK() function skip the rank 3 because position of p1 in table is 4 and it given its rank as 4. So Rank() function handle ties by leaving gaps in Ranks.

- **Ex** : Rank the products based on their sales by using RANK() function.

  ```sql

  SELECT
  ProductID,
  SUM(Sales) as TotalSales,
  RANK() OVER(Order By SUM(Sales)) as RankProducts
  FROM Sales.Orders
  GROUP BY ProductID

  ```

### DENSE_RANK() Function

- DENSE_RANK() Function is also similar to RANK() and ROW_NUMBER() function which assigns an integer to each row based on the field specified in the ORDER BY Clause. DENSE_RANK() handles ties. ANd DENSE_RANL() doesn't leave the gaps in ranking like RANK() function.

- For the above example the result given by DENSE_RANK() would look like : p5 : (100, 1), p3 : (80,2), p5 : (80, 2), p1 : (40, 3), p2 : (20, 4). Here we can see that, DENSE_RANK() handles ties and it doesn't leave the gaps in ranking.

- **Ex** : Rank the each product based on their sales from highest to lowest.

  ```sql

  SELECT
  ProductID,
  SUM(Sales) as TotalSales,
  DENSE_RANK() OVER(Order By SUM(Sales) DESC) as RankSales_DenseRank
  FROM Sales.Orders
  GROUP BY ProductID

  ```

### Usecases of Integer Based Ranking

- **Top N Analysis** : Integer Based Ranking can be used for TOP N Analysis such as finding the top 2 products, customers or finding the highest sales for each product which are generally used to optimize the decision making for the businesses.

  **Ex** : Find the highest sales for each product.

  ```sql

  SELECT *
  (
  SELECT
  OrderID,
  ProductID,
  Sales,
  ROW_NUMBER() OVER(PARTITION BY ProductID ORDER BY Sales DESC) as RankProducts
  FROM Sales.Orders ) as t
  WHERE RankProducts = 1

  ```

- **Bottom N Analysis** : Integer Based Ranking can be used for Bottom N Analysis also such as find the bottom two customers based on sales, finding the bottom 2 products etc which are useful for cost cutting in their businesses.

  **Ex** : Find the bottom 2 customers based on their total sales.

  ```sql

  SELECT *
  (
    SELECT
    CustomerID,
    SUM(Sales) as TotalSales,
    ROW_NUMBER() OVER(Order By SUM(Sales) ASC) as RankCustomers
    FROM Sales.Orders
  ) t
  WHERE RankCustomers <=2

  ```

- **Assigning Unique IDs** : Sometimes we doesn't have primary keys to for a table which actually creates many problems for solving some tasks such as joining tables, data filtering etc. Since we don't any unique identifer for a row, it becomes difficult for SQL to filter the data as the performance decreases. If you have primary key in a table, then we can divide the large table into multiple chunks called pages based on primary key which can be processed easily. This process is called paginating

  **Ex** : Assign unique ids to the OrdersArchive table.

  ```sql

  SELECT
  ROW_NUMBER() OVER(Order By OrderID, OrderDate) as UniqueID,
  *
  FROM Sales.OrdersArchive

  ```

- **Identifying and Removing Duplicate Rows** : In real world, most of the times people or enigneers might inserting the new data of a particular order or customer whenever they got instead of updating the original data. This will create duplicates in the data which might result incorrect results in data analytical tasks. This might be a problem from source systems as they doesn't enforce rules on databases for inserting the duplicate data etc. So to identify and remove th0se duplicates we can use ROW_NUMBER() function.

  **Ex** : Identify and remove the duplicate rows from the OrdersArchive table. Here iam considering the latest inserted data as original data and all others are duplicates.

  ```sql

  SELECT
  *
  (
    SELECT
    ROW_NUMBER() OVER(Order By CreationTime DESC) as rn,
    *
    FROM Sales.OrderArchive
  ) as t
  WHERE rn = 1

  ```

  **Note** : If you want to report bad data to the source systems, then we can use rn>=2 , then we can get all the duplicate data present in the table and we can report them to the source system designers.

  
## Percentage Based Ranking

### CUME_DIST() Function

- CUME_DIST() Function is similar to those integer based ranking but it give values in percentages. I mean it actually divides current position of that row with the total no of rows. i.e `CUME_DIST = CURRENT_ROW_POSITION / TOTAL_NO_OF_ROWS`. It actually calculates what fraction of rows as extreme as this row. Suppose u have 100 sales of different products and ordered from highest to lowest. Now you when you try to calculate CUME_DIST() for fifth row i.e 5/100 => 0.05 which indicates 5% of rows have sales greater than current sale value. So until row 5, all sales comes under top 5% sales. If you arrange those sales from lowest to highest then first 5 rows comes under bottom 5% sales.

- **Ex** : Find the top 5% sales of our products and get corresponding customers who made those sales.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  CustomerID,
  ProductID,
  Sales,
  PercentSales
  FROM (
    SELECT
    *
    CUME_DIST() OVER(Order By Sales DESC) as PercentSales
    FROM Sales.Orders
  ) t
  WHERE PercentSales <= 0.05

  ```

**Note** : Suppose row 5 and row 6 has same value then CUME_DIST() function consider highest postion for that value and use it for finding percentile which means for row 5 it give 6/100 -> 0.06 instead of 0.05. Like this if row 5 to row 10 has same value then it gives 10/100 -> 0.1 for row 5 to row 10. So it takes highest

### PERCENT_RANK() Function

- PERCENT_RANK() Function actually gives ranks in form of percentages. It is similar to RANK() function which gives ranks as integer numbers whereas PERCENT_RANK() gives ranks as percentages. The formula for `PERCENT_RANK is (Rank - 1) / (Total no of Rows - 1)`. This function actually gives relative position of each row with respective to total no of rows. Suppose if percent_rank of particular row is 0.25, then this means you are at 25% down the ranking ladder from top or we can say 25% ranking positions are above you and 75% ranking positions are at or below you. It doesn't mean there are 25% of people better than you and 75% people worse than you.

- PERCENT_RANK() is basically useful for finding normalized ranks and comapring the ranks between 2 sessions like JEE exams percentile finding etc.

- **Ex** : Find the Percent_Ranks for the based on the sales of the products.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales,
  PercentRankBySales
  FROM (
    SELECT
    *
    PERCENT_RANK() OVER(ORDER BY Sales DESC) as PercentRankBySales
    FROM Sales.Orders
  )

  ```

### NTILE() Function

- NTILE() Function acutally divides the dataset into equal groups approximately. It takes a number as input which is generally called as buckets and divides the database table into that many no of groups. The formula that NTILE() uses to divide the table is `No of Rows / No of Buckets `.

- Suppose we have 10 rows and we have divide that table into 2 groups then NTILE(2) divides the table into two equal groups with each group having 5 rows. If assigns 1 to the rows in group 1 and 2 to the rows in group 2.

- Suppose we want to divide this table into 4 groups, then NTILE(4) gives 2.5 as result which is a decimal. So SQL actually makes sure that first group must be big followed by smaller groups. SInce it resulted 2.5, so sql puts first 3 rows as group1. Now the remaining rows are 7 and we have make them into 3 groups. Now NTILE(3) results 2.33 which is a decimal. So Sql puts first 3 rows in those 7 rows as group2. Now remaining rows are 4 and we have make them into 2 groups. Now NTILE(2) results 2 which is a integer, so sql divides remaining 4 rows into 2 equal groups where both group having 2 rows each. This is how NTILE() function divides the dataset into specified butckets. 

- NTILE() actually has 2 usecases. It can be used for Data Segmentation and Equalizing the Load when we are transferring data. So this function can be useful for both Data Analysts for Data Segmentation and Data Engineers for Loading the Data in ETL Process. The reason for equalizing the data is sometimes we cannot able to tansfer a big table to other databases in one go. Because of network issues or by any other reason the transfer might be failed. If this transfer is failed then we have to redo the task from start itself. So insetad of transfer whole table we can divide the table into multiple chuncks and transfer these multiple chuncks one at a time and apply union operation to combine all the data at the other end once you get all the data.

- **Ex** : Data Segmentation - Segment the orders into 3 groups such as High, Medium and Low based on their sales.

  ```sql

  SELECT
  OrderID,
  Sales
  CASE 
    WHEN Buckets = 1 THEN 'High'
    WHEN Buckets = 2 THEN 'Medium'
    ELSE 'Low'
  END as SalesSegmentation
  FROM (
    SELECT
    OrderID,
    Sales,
    NTILE(3) OVER(ORDER BY Sales DESC) Buckets
    FROM Sales.Orders
  ) t

  ```

- **Ex** : Equalizing the Load - Divide the dataset into 5 groups. Since we doesn't have any field for dataset division, we can use the primary key to divide the dataset as it is the one which can differentiate rows.

  ```sql

  SELECT
  NTILE(5) OVER(Order By OrderID) as Buckets,
  *
  FROM Sales.Orders

  ```
