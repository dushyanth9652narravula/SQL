# Window Aggregate Functions

## Introduction

- Window Aggregate Functions aggregates all the values present in a particular window and returns a single value. The no of windows are determined by the PARTITION BY Clause. 

- We have around 5 window aggregate functions in SQL. Those are COUNT(), MIN(), MAX(), SUM(), AVG().The syntax of window aggregate functions is `<Aggregate function> Over(PARTITION BY <Column name> Order By <column name>)`. Here PARTITION BY and ORDER BY are optional. 

- All Window Aggregate function except COUNT() accepts only numeric type values or expressions holding numeric data types. But COUNT() function accepts all kind of data types.

## COUNT() Function

- COUNT() function in SQL returns the no of rows present in each window regardless of any duplicates present in a window. 

- Generally COUNT(*) returns the total no of rows present in each window regardless of any NULLs present in a row. But COUNT(Column) returns total no of non NULL rows in each window. So COUNT(Column) doesn't count rows containing NULLS present in the specified column.

- We have 4 usecases for the window COUNT() function. Those are :

  **Overall Analysis** : COUNT() function is generally used to get the quick summary of the dataset such as no of rows present in the dataset.

  **Ex** : Find the total no of orders present in the orders table.

  ```sql

  SELECT
  COUNT(*) as TotalNoOfOrders
  FROM Sales.Orders

  ```

  If you want to get additional details such as orderid, orderdate etc then we need to use this query.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  COUNT(*) OVER() as TotalNoOfOrders
  FROM Sales.Orders

  ```
 
  **Category Analysis** : COUNT() function can also be used to analyse the data categorically such as finding the no of orders made by each customer or find the no of orders we got for each product etc.

  **Ex** : Find the no of orders made by each customer.

  ```sql

  SELECT
  OrderId,
  OrderDate,
  CustomerId,
  COUNT(*) OVER(PARTITION BY CustomerId) as TotalOrdersByCustomer
  FROM Sales.Orders

  ```

  **Data Quality Check : Identifying NULLS** : COUNT() function is also used to identify NULLs in a given row. As we know COUNT(Column) return no of non-null rows present in the column then first we use COUNT(*) function to find the total no of rows and COUNT(column) function returns the non-null rows. So if there is any difference between the both results then we can say that column has NULL values.

  **Ex** : Find the total no of customer who has scores

  ```sql

  SELECT
  CustomerID,
  firstname,
  lastname,
  COUNT(*) OVER() TotalCustomers,
  COUNT(Score) OVER() TotalCustomersHavingScores
  FROM Sales.Customers

  ```

  **Data Quality Check : Identifying Duplicate Rows** : COUNT() function can also be used to find the duplicate rows present in a table. As duplicate rows can produce bad analytical results, so we need to remove those rows before performing analytics. To check whether there are any duplicate rows, we can partition the table by primary key of the table and use count() function on that partition. If any primary key count is more than 1 then we can say there are duplicate rows in it. But we say primary keys are mostly unique. But sometimes there some usecases whether we might not apply rules on primary key while creating the schema. So in these scenarios, we might get duplicate primary keys resulting duplicate rows. So we should remove those rows before performing analysis.

  **Ex** : Check whether there are any duplicate rows present in orders table.

  ```sql

  SELECT
  OrderId,
  COUNT(*) OVER(PARTITION BY OrderID) as TotalOrders
  FROM Sales.Orders

  ```

## SUM() Function

- SUM() Function returns the sum of values within a window. SUM() function accepts only numeric types. There are actually 3 major usecases of SUM() function. Those are Overall Summary, Group or Category Analysis, Comparision Analysis.

- **Overall Summary** : SUM() function can be used to get the overall summary of the dataset such as total sales made by the company etc.

  **Ex** : Find the total no of sales of the all orders.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales
  SUM(Sales) OVER() as TotalSales
  FROM Sales.Orders

  ```

- **Group or Category Analysis** : SUM() function can be used to for category or group analysis such as finding the total sales by each product or each customer etc.

  **Ex** : Find the total sales for each product.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductId,
  Sales,
  SUM(Sales) OVER(PARTITION BY ProductID) as SalesByProduct
  FROM Sales.Orders

  ```

- **Comparision Analysis** : Comparision analysis is simply comparing the current value and an aggregated value of window functions. SUM() function is used for `Part of Whole` analysis as we can find the percentage of sales for each product etc. Comparision Analysis can include comparing with extreme values such as compare current sales with highest or lowest sales, compare to average analysis such as evaluating whether a value is above or below the average.

  **Ex** : Find the percentage contribution of each product sales with total sales of the organization.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales
  SUM(Sales) OVER() as TotalSales,
  SUM(Sales) OVER(PARTITION BY ProductID) as SalesByProduct
  ROUND(SUM(Sales) OVER(PARTITION BY ProductID) / SUM(CAST(Sales As Float)) OVER(), 4) * 100 
  AS PercentageSales
  FROM Sales.Orders

  ```

## AVG() Function

- AVG() Function is used to return the average of all values within each window. Similar to SUM() function AVG() function is also used in Overall Analysis, Group or Category Analysis, Comparision Analysis (Compare to Average).

- **Ex1 - Overall Analysis** : Find the average sales across all orders

  ```sql

  SELECT
  OrderID,
  OrderDate,
  Sales,
  AVG(Sales) OVER() AS AvgSales
  FROM Sales.Orders

  ```

- **Ex2 - Group or Category Analysis** : Find the average sales of each product.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales,
  AVG(Sales) OVER(PARTITION BY ProductID) As AvgBySales
  FROM Sales.Orders

  ```

- **Ex3 - Handling NULLS** : Find the average scores of all customers considering nulls as zeros in this business. For this scenario, we have to handle nulls first and then calculate averages which results accurate analytics.

  ```sql

  SELECT
  CustomerId,
  firstname,
  lastname,
  score,
  Avg(Score) OVER() As AvgScores
  FROM Sales.Customers

  ```

- **Ex4 - Comparision Analysis** : Find all the orders whose sales are higher than average sales across all the orders

  ```sql

  SELECT
  *
  FROM (
  SELECT
  OrderID,
  OrderDate,
  Sales,
  Avg(Sales) Over() As AvgSales
  From Sales.Orders ) As t
  Where Sales > AvgSales

  ```

## MIN() and MAX() Functions

- MIN() function returns the minimum value in each window and MAX() function returns the maximum value in each window. Similar to other aggregate functions, MIN() and MAx() can be used for OverallAnalyisis, Category or Group Analysis, and Comparision Analysis. When finding MIN() and MAX(), its better to handle NULLs first according to business scenario and then apply those functions. Otherwise we might get incorrect results.

- **Ex1 - Overall Analysis** : Show all employees who has highest salary.

  ```sql

  SELECT
  *
  FROM (
    SELECT
    EmployeeId,
    firstname,
    lastname,
    Salary,
    MAX(Salary) OVER() As HighestSalary
    FROM Sales.Employees
  ) as t

  WHERE Salary = HighestSalary

  ```

- **Ex2 - Category Analysis** : Find the minimum and maximum sales of each product.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales,
  MIN(Sales) OVER(PARTITION BY ProductID) As MinSalesByProduct,
  MAX(Sales) OVER(PARTITION BY ProductID) As MaxSalesByProduct
  FROM Sales.Orders

  ```

- **Ex3 - Comparision Analysis** : Find the deviation of sales from thier minimum and maximum sales of each order.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales,
  MIN(Sales) OVER() As MinSalesByProduct,
  MAX(Sales) OVER() As MaxSalesByProduct,
  Sales - MIN(Sales) OVER() as DeviationFromMin,
  MAX(Sales) OVER() - Sales as DeviationFromMax
  FROM Sales.Orders

  ```

## Usecases of Window Aggregate Functions

### Running and Rolling Totals

- Running ang Rolling Totals aggregates sequence of memebers and aggregation gets updated each time when a new memeber gets added. These two are useful for analysis over the time.

- **Running Total** : Running Total aggregates all values from the beginning up to current point without dropping off the older values. Running totals are simply cumulative sums which are generally used for budget tracking and target achieving scenarios.

  **Ex** : Find the Running total for sales of our products since first order.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  Sales,
  SUM(Sales) OVER(ORDER BY OrderDate ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
  FROM Sales.Orders

  ```

  we can write above code like this also as above frame is default frame if we use order by clause.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  Sales,
  SUM(Sales) OVER(ORDER BY OrderDate)
  FROM Sales.Orders

  ```

- **Rolling Totals** : Rolling Totals aggregates all values within a fixed time window (e.g total sales for last 3 months or last 30 days etc.). As new data is added, oldest data point would be dropped.

  **Ex** : Find the last 3 month total sales of our products.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  Sales,
  SUM(Sales) OVER(Order By OrderDate ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) as Last3MonthSales
  FROM Sales.Orders

  ```

### Moving Averages

- Moving Average is simply nothing finding the average of certain values over the time. It could be finding the average sales of an organization since its beginning or finding the average sales over particular period such as last 30 days or 3 months etc.

- **Ex** : Find the average sales of each product over time.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales,
  AVG(Sales) OVER(PARTITION BY ProductID ORDER BY OrderDate ) as MovingAvg
  FROM Sales.Orders

  ```

- **Ex** : Find the moving average of each product sales but just including next order.

  ```sql

  SELECT
  OrderID,
  OrderDate,
  ProductID,
  Sales,
  AVG(Sales) OVER(PARTITION BY ProductID ORDER BY OrderDate ROWS BETWEEN CURRENT ROW AND 1 PRECEDING) as RollingAvg
  FROM Sales.Orders

  ```






