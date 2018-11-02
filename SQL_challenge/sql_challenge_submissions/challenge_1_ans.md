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

5. What category appears in the most orders?

6. What was the total cost for each order?

7. Which employee made the most sales (by total price)?

8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)

9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)
