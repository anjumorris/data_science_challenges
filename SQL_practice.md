# SQL Practice

## Guided

Let's walk through a few examples:

1) Retrieve all Customers from Madrid

```sql
SELECT
    * 
FROM
    Customers
WHERE
    City='Madrid';
```

2) How many customers are there in each city?

```sql
SELECT
    City, COUNT(*)
FROM
    Customers
GROUP BY
    City;
```

3) What is the most common city for customers? (And can you make it easy to find the correct answer?)

```sql
SELECT
    City, COUNT(*) AS count 
FROM
    Customers 
GROUP BY
    City 
ORDER BY
    count DESC;
```

4) What category has the most products?

```sql
SELECT * FROM Products
JOIN Categories ON Products.CategoryID = Categories.CategoryID

SELECT CategoryName, Count(*) AS CountVal FROM Products
JOIN Categories ON Products.CategoryID = Categories.CategoryID
GROUP BY CategoryName

SELECT CategoryName, Count(*) AS CountVal FROM Products
JOIN Categories ON Products.CategoryID = Categories.CategoryID
GROUP BY CategoryName
ORDER BY CountVal DESC
```

You can comment in SQL by using /* and */

## Practice

- Which customers are from the UK?

  ```sql
  SELECT * FROM [Customers] WHERE Country = 'UK'
  ```

- What is the name of the customer who has the most orders?

  Ernst Handel

  ```sql
  SELECT OrderID, CustomerName, COUNT() AS OrderTimes 
  FROM Orders JOIN Customers ON Orders.CustomerID == Customers.CustomerID 
  GROUP BY CustomerName
  ORDER BY OrderTimes DESC
  ```

- Which supplier has the highest average product price?

  Aux joyeux ecclésiastiques

  ```sql
  SELECT SupplierName, AVG(Price) AS Average 
  FROM Suppliers JOIN Products ON Suppliers.SupplierID == Products.SupplierID
  GROUP BY SupplierName
  ORDER BY Average DESC
  ```

- How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)

  21

  ```sql
  SELECT COUNT (DISTINCT Country) FROM [Customers]
  ```

- What category appears in the most orders?

  Dairy Products

  ```sql
  SELECT CategoryName, COUNT(OrderID) AS OrderCount  FROM [OrderDetails] 
  JOIN Products ON OrderDetails.ProductID == Products.ProductID 
  JOIN Categories ON Products.CategoryID == Categories.CategoryID 
  GROUP BY CategoryName
  ORDER BY OrderCount DESC
  ```

- What was the total cost for each order?

  ```sql
  SELECT OrderID, SUM(Price) AS TotalCost FROM [OrderDetails] 
  JOIN Products ON OrderDetails.ProductID == Products.ProductID
  GROUP BY OrderID
  ORDER BY TotalCost DESC
  ```

- Which employee made the most sales (by total cost)?

   Peacock Margaret

  ```sql
  SELECT LastName,FirstName,SUM(Price*Quantity) AS TotalSales FROM Orders
  JOIN Employees ON Orders.EmployeeID ==Employees.EmployeeID
  JOIN OrderDetails ON Orders.OrderID == OrderDetails.OrderID
  JOIN Products On OrderDetails.ProductID == Products.ProductID
  GROUP BY Employees.EmployeeID
  ORDER BY TotalSales DESC
  ```

- Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)

  Leverling	Janet

  Buchanan	Steven

  ```sql
  SELECT * FROM [Employees] WHERE Notes Like '%BS%'
  ```

- Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)

  Tokyo Traders

  ```sql
  SELECT SupplierName,COUNT(ProductID) AS NumberOfProducts, AVG(Price) AS AvgPrice FROM [Products]
  JOIN Suppliers ON Products.SupplierID == Suppliers.SupplierID
  GROUP BY SupplierName
  HAVING NumberOfProducts >= 3
  ORDER BY AvgPrice DESC
  ```
