---
title: "Notes Taken on a SQL Course"
date: 2022-08-23T20:56:26+02:00
draft: false
---

I take notes using Jupyter Notebooks, so this blog format is ideal.

## Creating a Table Notation
```
CREATE TABLE "Table name"
(
Id char(10) PRIMARY KEY,
Brand char(10) NOT NULL,
Type char(250) NOT NULL,
Price decimal(8,2) NOT NULL,
Desc Varchar(750) NULL
);
```
Null values accept everything. Null doesn't mean empty string (Spaces, for example)

## Inserting data into table
 
You list all the columns that you'll be filling and with what.

```
INSERT INTO "Table name"
(Id
,Brand
,Type
,Price
, Desc
)
VALUES
('123456'
,'Gucci'
,'Slippers'
,'695.00'
,NULL
);
``` 

## Temporary Tables
 
Temporary tables will be deleted when the current session is terminated. They're good for when you want to create something faster than a real table and they're good for using complex queries using subsets and joins.

``` 
CREATE TEMPORARY TABLE Sandals AS
( 
SELECT * 
FROM SHOES
WHERE shoe_type = 'sandals
)
```

## Update Tables / Delete Tables
 
The UPDATE query is used to modify the existing records in a table. You can use the WHERE clause with UPDATE to update selected rows, otherwise all the rows would be updated.
 
``` 
UPDATE table_name
SET column1 = value1, column2 = value2, columnN = valueN
WHERE [condition] AND/OR [conditionN];
```
Update example: 

```
UPDATE COMPANY SET ADDRESS = 'Texas' WHERE ID = 6; 
``` 
Without the WHERE statement you just update all the records.
 
Similarly, the DELETE query deletes records.
``` 
DELETE FROM table_name 
WHERE [condition]; 
```

Using no WHERE statement would delete all records.
 
SQL - Adding comments to SQL
 
They're useful to maintain and share the code
 
- - this is a single line comment
/* This is a
multi line comment

*/
 
## Filtering Operators
 
* = - Equals. eg: WHERE ProductName = 'Tofu';
* <> - Not equal
* \> Greater than
* < Less than
* \>= Greater than or equal eg: WHERE UnitPrice >= 5;
* <= Less tha or equal
* BETWEEN - Between an inclusive range
* IS NULL - is a null value

Checking for Non-Matches sometimes useful
 
SELECT * FROM Products WHERE ProductName <> 'Pingo Doce'
 
Filtering with a range
 
SELECT ProductName, UnitPrice, SuppliedID, UnitsInStock FROM Products WHERE UnitsInStock BETWEEN 15 AND 80;
 
Filtering No Value - Useful for cleaning data and checking for missing records
 
SELECT ProductName, UnitPrice, SuppliedID, UnitsInStock FROM Products WHERE ProductName IS NULL;
 
## Advanced Filtering
 
### IN Operator
 Specifies a range of conditions Comma delimited list of values enclosed in ()
 
Example: SELECT ProductID, UnitPrice, SupplierID FROM Products WHERE SupplierID IN (9, 10, 11);
 
### OR Operator - The second condition will not be evaluated if the first condition is met!
 
Use for any rows matching the specific condition.
 
SELECT ProductName, ProductID, UnitPrice, SupplierID, ProductName FROM Products WHERE ProductName = 'Tofu' OR 'Konbu';
 
### OR with AND 
 
You can combine OR statements with AND statements:
SELECT ProductID, UnitPrice, SupplierID FROM Products WHERE SupplierID = 9 OR SupplierID = 11 AND UnitPrice > 15;
 
### Order of Operations
Can contain AND or OR operators. SQL processes AND before OR 
Use () to avoid the default order of operations
 
NOT Operator - Excludes the specified attributes:
SELECT * FROM Employees WHERE NOT City='London' AND NOT City='Seattle';
 
## Wildcards 

 A wildcard is a special character used to match parts of a value. You can use a search pattern made from literal text, wildcard characters or a combination of both. It uses LIKE as an operator (though technically a predicate). 
 
LIKE/Wildcards can only be used with strings. It cannot be used for non-text datatypes.
 
To use Wildcards use a % sign. EG:
'%Pizza' - Anything ending with the word pizza
'Pizza%' Grabs anything after the word pizza
'%Pizza%' Grabs anything before and after the word pizza.
'S%E' - Grabs anything that starts with "S" and ends with "E" (Like Sadie)
 
't%@gmail.com' - Grabs gmail addresses that start with "t"
 
Underscore (_) Wildcard - Matches a single character EG: WHERE size LIKE '_pizza'
Output: spizza, mpizza
 
Bracket [] Wildcard - Used to specify a set of characters in a specific location 
 
Downside of Wildcards - Takes longer to run (Bad performance). BEtter to use another operator if possible. Statements with wildcards will take longer to run if used at the end of search pattern.
 
## Sorting with ORDER BY

Sorting data logically helps keep information you want on top.
ORDER BY clause allows the user to sort data by particular columns 
 
SELECT something FROM database ORDER BY characteristic 
 
Rules for ORDER BY
Takes the name of one or more columns (via additional columns). 
It can sort by a column not retrieved. 
Must always be the last clause in a select statement 
 
Sorting by Column Position 
ORDER BY 2,3 (2 means 2nd column, 3 means 3rd) You could also just use the actual column name.
 
Sorting by Direction
DESC descending or ASC ascending order - Only applies to the column names it directly precedes, unless you specify a column.
 
## Math Operators 

The basic ones + - * / 
EG: SELECT Product ID, UnitsOnOrder, UnitPrice, UnitsOnOrder * UnitPrice AS Total_Order_Cost FROM PRODUCTS (AS creates an alias for a new column).
In the usual order of operations () first.
SELECT ProductID, Quantity, UnitPrice, Discount, (UnitPrice - Discount) /Quantity AS Total_Cost FROM OrderDetails;
 
## Aggregate Functions
 
These functions help summarize our data. These are:
AVG() - Rows containing NULL Values are ignored by the AVERAGE function SELECT AVG(UnitPrice) AS avg_price FROM products
COUNT() - SELECT COUNT(*) AS total_customers  FROM Customers; - this counts the number of rows in a table (Incl. NULL values) You can choose a column to count, but this excludes NULL values.
MIN() - NULL values are ignored by the MIN and MAX functions. 
MAX() SELECT MAX(UnitPrice) AS max_prod_price, MIN(UnitPrice) AS min_prod_price FROM Products
SUM() - Can use with math operators. SELECT SUM(UnitPrice*UnitsInStock) AS total_price FROM Products WHERE SupplierID = 23;
 
Using DISTINCT on Aggregate Functions
If DISTINCT is not specified ALL is assumed. Cannot use DISTINCT on COUNT(*)
 
DISTINCT returns only unique values. It's good to remove duplicate values.
EG: SELECT COUNT(DISTINCT CustomerID) FROM Customers
 
## Grouping Data with SQL
 
Grouping Example - Counts customers after group on region rather than counting the whole table.
 
SELECT Region, COUNT(CustomerID) AS total_customers FROM Customers GROUP BY Region;
 
Additional GROUP BY Information
 
GROUP BY clauses can contain multiple columns.
 
Every column in your SELECT statement must be present in a GROUP BY clause, except for aggregated calculations
 
NULLs will be grouped together if your column contains NULLs
 
Having Clause - Filtering for Groups
 
WHERE does not work for groups as it filters on ROWS, not COLUMNS
 
Instead use the HAVING clause to filter for groups 
 
Grouping example: Select all customers with more than 2 orders.
SELECT CustomerID, COUNT (*) AS orders FROM Orders GROUP BY CustomerID HAVING COUNT (*) >= 2;
 
## WHERE vs. HAVING 
 
WHERE filters before data is grouped
 
HAVING filters after data is grouped
 
Rows eliminated by the WHERE clause will not be included in the group
 
## ORDER BY with GROUP BY
 
ORDER BY sorts data, GROUP BY does not sort data. It's good practice to always use ORDER BY when using GROUP BY to avoid messy results. 
 
eg: SELECT SupplierID, COUNT(*) AS Num_Prod FROM Products WHERE UnitPrice >= 4 GROUP BY SupplierID HAVING COUNT (*) >=2;
 
Note: Filtering is Useful (Increases Performance, narrows results, helps understand data with specific values, range and blank values)
 
## Key SQL Clauses (In order) !!
```
SELECT - Columns or expressions to be returned
FROM - Table from which to retrieve data
WHERE - Row-level filtering
GROUP BY - Group specification
HAVING  - Group-level filter
ORDER BY - Output sort order
 
SELECT DISTINCT AlbumId, COUNT(TrackId) AS TrackNum
FROM Tracks
GROUP BY AlbumId
HAVING TrackNum >= 12
```

## Subqueries

### What are Subqueries?
Subqueries are queries embedded into other queries. 

Relational databases store data in multiple tables. 

Subqueries merge data from multiple sources together and they help with adding other filtering criteria.

An example would be: We need to know the region each customer is from who has had an order with freight over 100.

The issue her is that this data is in two different tables. 

Subquery example (Note - **Always perform the innermost SELECT portion first):
```
SELECT 
CustomerID
, CompanyName
, Region
FROM Customers
WHERE customerID in (SELECT 
customerID
    FROM Orders
    WHERE Freight > 100 );
```

## Subqueries Best Practices and Considerations

There is **no limit** to the number of subqueries you can have.

**Performance slows** when yuo nest too deeply

Subquery selects can only **retrieve a single column** (Hence why they're used in filtering so much)

### Subquery in Subquery

Note: Indentation is super important for readable subqueries!

1. Order numbers for toothbrushes
2. Customer ID's for those orders
3. Customer information for those orders

```
SELECT Customer_name, Customer_contact
FROM Customers
WHERE cust_id IN 
    SELECT customer_id
    FROM Orders
    WHERE order number IN (SELECT order_number
        FROM OrderItems
        WHERE prod_name = 'Toothbrush');
```
A good wesite for this is PoorSQL. This is a website that will pre-format code, uses proper indenting and this makes the code easier to read and troubleshoot.

### Subqueries for Calculations

Total number of orders placed by every customer

|Customer_name| Customer_state|Orders|
| :-: | :-: | :-: |
|Becky|IA|5|
|Nita|CA|6|
|Raj|OH|0|
|Steve|AZ|1|

```
SELECT COUNT (*) AS orders
FROM Orders
WHERE customer_id = '143569';

SELECT customer_name
    ,customer_state
    (SELECT COUNT (*) AS orders
    FROM Orders
    WHERE Orders.customer_id = Customer.customer_id) AS orders
FROM customers
ORDER BY Customer_name
```

# Joining Tables Introduction

## Benefits of Breaking Data into Tables
* Efficient storage
* Easier manipulation
* Greater scalability
* Logically models a process
* Tables are related through common values (keys)

## Joins

Associate correct records from each table on the fly 

Allows data retrieval from multiple tables in one query

Joins are not physical - they persist for the duration of the query execution (!)

Result: Ensure that duplicates aren't bloating results.

## Cartesian Joins (Cross Joins)

Coss joins: Each row from the first table joins with all the rows of another table

This is computationally taxing. 

`
SELECT product_name
, unit_price
, company_name
FROM supplies CROSS JOIN products;`

The cross join is used to generate all combinations of records in two tables. For example if you combine a meals table with a drinks table, you'll get all possible combinations of meals and drinks.

The output will be the number of joins in the first table multiplied by the number of rows in the second table.

## Inner Joins (Most Important)

The INNER JOIN keyword selects records that have matching values in both tables

`
SELECT suppliers.CompanyName
, ProductName
, UnitPrice
FROM Suppliers INNER JOIN Products 
ON Suppliers.supplierid = Products.supplierid
`

The join condition is in the FROM clause and uses the ON clause.

Joining more tables together affects overall database performance.

You can join multiple tables, no limit.

List all the tables, then define conditions.

### Inner Join with Multiple Tables

It's good to prequalify the data and pose it in a logical fashion (o = order, c = company, e = employee, etc)
`
SELECT o.OrderID, c.CompanyName, e.LastName
FROM ((Orders o INNER JOIN Customers c ON o.CustomerID = c.CustomerID)
INNER JOIN Employees e ON o.EmployeeID = e.EmployeeID);
`

### Best Practices with Joins

* Make sure you are pre-qualifying names
* Do not make unnecessary joins
* Think about the type of join you are making
* How are you connecting records?


## Aliases and Self Joins

We use aliases when aggregating a field e.g creating an alias so that we know what to call the new value. It makes the columns more readable.

An alias gives a table or a column a temporary name (duration of the query). For this we use the AS keyword.

Example:
`
SELECT vendor_name
,product_name
,product_price
FROM Vendors AS v, Products AS p
WHERE v.vendor_id = p.vendor.id;
`
### Self Joins

We want to match customers from the same city. We take the table and treat it like two separate tables.

Example:
`
SELECT A.CustomerName AS
CustomerName1, B.CustomerName AS
CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID = B.CustomerID
AND A.City = B.City
ORDER BY A.City;
`

## Advanced Joins: Left, Right, and Full Outer Joins

Note: SQL Lite only does Left Joins

### Left Join (Outer)

Note: This differs from inner joins since unmatched records return NULL!

Returns all records from the left table (table 1), and the matched records from the right table (table 2). The result is NULL from the right side, if there is no match.

An example would be a table 1 of customers and a table 2 of orders. You'd get all customers and, if they had an order, include that data there.

`
SELECT C.CustomerName, O.OrderID
FROM Customers C
LEFT JOIN Orders O ON C.CustomerID
= O.CustomerID
ORDER BY C.CustomerName;
`
`
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
`

### Right Join (Outer)

Returns all records from the right table (table 2), and the matched records from the left table. The result is NULL from the left side, if there is no match.

The table you list first is acted upon according to the type of join you use. Left joins can be turned into right joins by reversing the order of the tables. 

`
SELECT Orders.OrdedrID,
Employees.LastName,
Employees.FirstName
FROM Orders
RIGHT JOIN Employees ON 
Orders.EmployeeID = 
Employees.EmployeeID
ORDER BY Orders.OrderID;
` 
Inuition is bolstered by repetition. 

### Full Outer Join

Return all records when there is a match in either left (table 1) or right (table 2) table records.

If there is no match, they will not be returned.

`
SELECT Customers.CustomerName,
Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON 
Customers.CustomerID = 
Orders.CustomerID
ORDER BY Customers.CustomerName;
`

## Unions

A UNION is used to combine the result-set of two or more SELECT statements.

Each SELECT statement within UNION must have the same number of columns.

The columns must have **similar data types**.

The columns in each SELECT statement must be in the **same order**. 

Example 1 - Basic Union Setup:
`
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
`

Example 2 - Which German cities have suppliers
`
SELECT City, Country FROM Customers
WHERE Country='Germany' 
UNION
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City;
`

## Union All 

The UNION ALL command is equal to the UNION command, except that UNION ALL selects all values. UNION ALL will not eliminate duplicate rows, instead it just pulls all rows from all tables fitting your query specifics and combines them into a table.

## Subqueries vs Joins

Both subqueries and joins are capable of returning results from complex tables. However, they have different purposes.

Where subqueries chain statements to return a value based on its equivalent in another table, joins make a single table with all the relevant information and are faster to execute. 

Same data example in both - Taken from Part 3 Quiz
### Subquery
`
SELECT AlbumId
    ,Name
    ,UnitPrice
FROM Tracks
WHERE AlbumId IN (
        SELECT AlbumId
        FROM albums
        WHERE ArtistId IN (
                SELECT ArtistId
                FROM artists
                WHERE ArtistId = 8
                )
        )
`
### Join (Inner Basic)
`
SELECT albums.Title
	,tracks.UnitPrice
FROM albums
INNER JOIN tracks ON albums.AlbumId = tracks.AlbumId
WHERE albums.ArtistId IN (
		SELECT ArtistId
		FROM artists
		WHERE Name = 'Audioslave'
		)
`

`
SELECT customers.FirstName
	,customers.LastName
	,customers.CustomerId
	,invoices.InvoiceId
FROM customers
INNER JOIN invoices ON customers.CustomerId = invoices.CustomerId
WHERE invoices.InvoiceId IS NULL
`

`
SELECT albums.Title
	,tracks.AlbumId
	,SUM(tracks.UnitPrice)
FROM tracks
INNER JOIN albums ON tracks.AlbumId = albums.AlbumId
WHERE albums.Title = 'Big Ones'
GROUP BY tracks.AlbumId
`
        
### Join (Inner Multiple Tables)
`
SELECT artists.Name
	,albums.Title
	,tracks.UnitPrice
FROM (
	(
		albums INNER JOIN tracks ON albums.AlbumId = tracks.AlbumId
		) INNER JOIN artists ON artists.ArtistId = albums.ArtistId
	)
WHERE albums.ArtistId IN (
		SELECT ArtistId
		FROM artists
		WHERE Name = 'Audioslave'
		)
`
