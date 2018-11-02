# Challenge Set 9
## Part I: W3Schools SQL Lab 

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

1. Which customers are from the UK?

   ```sql
   SELECT
       * 
   FROM
       Customers
   WHERE
       City='Madrid';
   ```

2. What is the name of the customer who has the most orders?

   ```sql
   SELECT
       City, COUNT(*)
   FROM
       Customers
   GROUP BY
       City;
   ```

3. Which supplier has the highest average product price?

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

4. How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)

   ```sql
   SELECT COUNT (DISTINCT Country) FROM [Customers]
   ```

5. What category appears in the most orders?

   Dairy Products

   ```sql
   SELECT CategoryName, COUNT(OrderID) AS OrderCount  FROM [OrderDetails] 
   JOIN Products ON OrderDetails.ProductID = Products.ProductID 
   JOIN Categories ON Products.CategoryID = Categories.CategoryID 
   GROUP BY CategoryName
   ORDER BY OrderCount DESC
   ```

6. What was the total cost for each order?

   ```sql
   SELECT OrderID, SUM(Price) AS TotalCost FROM [OrderDetails] 
   JOIN Products ON OrderDetails.ProductID = Products.ProductID
   GROUP BY OrderID
   ORDER BY TotalCost DESC
   ```

7. Which employee made the most sales (by total price)?

   Peacock Margaret

   ```sql
   SELECT LastName,FirstName,SUM(Price*Quantity) AS TotalSales FROM Orders
     JOIN Employees ON Orders.EmployeeID =Employees.EmployeeID
     JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
     JOIN Products On OrderDetails.ProductID = Products.ProductID
     GROUP BY Employees.EmployeeID
     ORDER BY TotalSales DESC
   ```

8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)

   Leverling	Janet

   Buchanan	Steven

   ```sql
   SELECT * FROM [Employees] WHERE Notes Like '%BS%'
   ```

9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)

   Tokyo Traders

   ```sql
   SELECT SupplierName,COUNT(ProductID) AS NumberOfProducts, AVG(Price) AS AvgPrice FROM [Products]
   JOIN Suppliers ON Products.SupplierID = Suppliers.SupplierID
   GROUP BY SupplierName
   HAVING NumberOfProducts >= 3
   ORDER BY AvgPrice DESC
   ```

