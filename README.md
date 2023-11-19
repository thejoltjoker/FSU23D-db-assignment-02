# Databasutveckling 
## FSU23D Database Assignment #2

## Description
These are the replies for the second assignment of the database course in the FSU23D course.
It's an exercise in basic MySQL-queries.

## Table of contents

- [Databasutveckling](#databasutveckling)
  - [FSU23D Database Assignment #2](#fsu23d-database-assignment-2)
  - [Description](#description)
  - [Table of contents](#table-of-contents)
  - [Queries](#queries)
    - [1.](#1)
    - [2.](#2)
    - [3.](#3)
    - [4.](#4)
    - [5.](#5)
    - [6.](#6)
    - [7.](#7)
    - [8.](#8)
    - [9.](#9)
    - [10.](#10)
    - [11.](#11)
    - [12.](#12)
    - [13.](#13)
    - [14.](#14)
    - [15.](#15)
    - [16.](#16)
    - [17.](#17)
    - [18.](#18)
    - [19.](#19)
    - [20.](#20)


## Queries

### 1.
In which countries does the company have employees? Specify `Country` and do not show duplicates in the answer.
```SQL
SELECT DISTINCT Country 
FROM Employees
```

### 2.
What categories of products are there in the database? Specify `CategoryID` and `CategoryName`. Sort by `CategoryName` in descending order.
```SQL
SELECT CategoryID, CategoryName 
FROM Categories
ORDER BY CategoryName DESC
```

### 3.
Create a phone list of all German customers. Specify `CompanyName`, `ContactName`, `Phone`, and `Fax`. Sort by `CompanyName` in descending order.
```SQL
SELECT CompanyName, ContactName, Phone, Fax 
FROM Customers
WHERE Country LIKE 'Germany'
ORDER BY CompanyName DESC
```

### 4.
In which Swedish cities are there customers? Specify `City`.
```SQL
SELECT City 
FROM Customers 
WHERE Country LIKE 'Sweden'
```

### 5.
Which products contain the word 'mix' in their product name? Specify `ProductName`.
```SQL
SELECT ProductName 
FROM Products 
WHERE ProductName LIKE '%mix'
```

### 6.
Which employee was born on `1966-01-27`? Specify `FirstName` and `LastName`.
```SQL
SELECT FirstName, LastName 
FROM Employees
WHERE BirthDate = '1966-01-27'
```

### 7.
Which employee has no manager (i.e., where `ReportsTo` is `NULL`)? Specify `FirstName` and `LastName`.
```SQL
SELECT FirstName, LastName 
FROM Employees
WHERE ReportsTo IS NULL
```

### 8.
Which products need to be ordered, i.e., where the stock quantity is less than `ReorderLevel` (do not include products that have been discontinued)? Specify `ProductName`, `UnitsInStock`, and `ReorderLevel`.
```SQL
SELECT ProductName, UnitsInStock, ReorderLevel 
FROM Products
WHERE UnitsInStock < ReorderLevel
```

### 9.
Which German and French suppliers are in the database? Specify `CompanyName`, `ContactName`, `Country`, `Phone`, and `Homepage`. Sort by `CompanyName` in descending order.
```SQL
SELECT CompanyName, ContactName, Country, Phone, Homepage 
FROM Suppliers
WHERE Country IN ('Germany', 'France')
ORDER BY CompanyName DESC
```

### 10.
Which products have more than 100 in stock? Specify `ProductID`, `ProductName`, `UnitPrice`, and `UnitsInStock`. Sort first by `UnitPrice` in descending order and then by `UnitsInStock` in ascending order.
```SQL
SELECT ProductID, ProductName, UnitPrice, UnitsInStock 
FROM Products
WHERE UnitsInStock > 100
ORDER BY UnitPrice DESC, UnitsInStock ASC
```

### 11.
What is the birthdate of the oldest employee? Name the column "Birthdate."
```SQL
SELECT MIN(BirthDate) as `Birthdate`
FROM Employees
```

### 12.
What is the average price and sum for products in category 2? Name the columns "AveragePrice" and "Total."
```SQL
SELECT AVG(UnitPrice) AS `AveragePrice`, SUM(UnitPrice) as `Total`
FROM Products
WHERE CategoryID = 2
```

### 13.
What is the largest discount given among all order details? Name the column "LargestDiscount."
```SQL
SELECT MAX(Discount) as `LargestDiscount` 
FROM Order_Details
```

### 14.
How many different product categories are there? Name the column "Count."
```SQL
SELECT COUNT(*) as `Count`
FROM Categories
```

### 15.
How many customers are there in each country? Sort by the number of customers in descending order. Specify `Country` and name a column "Count."
```SQL
SELECT DISTINCT Country,
COUNT(*) as `Count`
FROM Customers
GROUP BY Country  
ORDER BY `Count` DESC
```

### 16.
What is the average price for products in categories 1, 3, 5, and 7 grouped by supplier? Specify `SupplierID` and name a column "AveragePrice."
```SQL
SELECT SupplierID, AVG(UnitPrice) AS `AveragePrice`
FROM Products
WHERE CategoryID in (1, 3, 5, 7)
GROUP BY SupplierID
```

### 17.
Specify `CategoryID` for categories that contain more than 10 products.
```SQL
SELECT CategoryID
FROM Products
GROUP BY CategoryID
HAVING COUNT(*) > 10
```

### 18.
Show `CustomerID` for customers who had more than 10 orders between January 1, 1998, and December 31, 1998. Create a column "OrderCount" to display the number of orders.
```SQL
SELECT CustomerID, COUNT(*) as `OrderCount` 
FROM Orders
WHERE OrderDate BETWEEN '1998-01-01' AND '1998-12-31'
GROUP BY CustomerID
HAVING COUNT(*) > 10
```

### 19.
Create a top ten list of products that have sold the most. Specify `ProductID` and the sum of the quantity sold as "Quantity."
```SQL
SELECT ProductID, SUM(Quantity) as `Quantity`
FROM Order_Details
GROUP BY ProductID  
ORDER BY `Quantity` DESC
LIMIT 10
```

### 20.
Show a list of the 10 most recently placed orders. Retrieve `FirstName` and `LastName` from the Employees table and `OrderDate` from the Orders table.
```SQL
SELECT Employees.FirstName, Employees.LastName, OrderDate
FROM Orders
INNER JOIN Employees ON Orders.EmployeeID=Employees.EmployeeID  
ORDER BY `Orders`.`OrderDate` DESC
LIMIT 10
```