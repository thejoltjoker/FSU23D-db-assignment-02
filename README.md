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
I vilka länder har företaget anställda? Ange `Country` samt visa inte dubbletter i svaret.

```SQL
SELECT DISTINCT Country 
FROM Employees
```

### 2. 
Vilka kategorier av produkter finns det i databasen? Ange `CategoryID` och `CategoryName`. Sortera efter `CategoryName` i fallande ordning.

```SQL
SELECT CategoryID, CategoryName 
FROM Categories
ORDER BY CategoryName DESC
```

### 3. 
Gör en telefonlista över samtliga tyska kunder. Ange `CompanyName`, `ContactName`, `Phone` samt `Fax`. Sortera efter `CompanyName` i fallande ordning.
```SQL
SELECT CompanyName, ContactName, Phone, Fax 
FROM Customers
WHERE Country LIKE 'Germany'
ORDER BY CompanyName DESC
```

### 4. 
I vilka svenska städer finns det kunder? Ange `City`.
```SQL
SELECT City 
FROM Customers 
WHERE Country LIKE 'Sweden'
```

### 5. 
Vilka produkter innehåller ordet 'mix' i sitt produktnamn? Ange `ProductName`.
```SQL
SELECT ProductName 
FROM Products 
WHERE ProductName LIKE '%mix'
```

### 6. 
Vilken anställd föddes `1966-01-27`? Ange `FirstName` och `LastName`.
```SQL
SELECT FirstName, LastName 
FROM Employees
WHERE BirthDate = '1966-01-27'
```

### 7. 
Vilken anställd har ingen chef (dvs där `ReportsTo` är `NULL`)? Ange `FirstName` och `LastName`.
```SQL
SELECT FirstName, LastName 
FROM Employees
WHERE ReportsTo IS NULL
```

### 8. 
Vilka produkter behöver beställas, dvs där lagerantalet är mindre än ReorderLevel (ta inte med produkter som har utgått ur sortimentet) Ange `ProductName`, `UnitsInStock` samt `ReorderLevel`.
```SQL
SELECT ProductName, UnitsInStock, ReorderLevel 
FROM Products
WHERE UnitsInStock < ReorderLevel
```

### 9. 
Vilka tyska och franska leverantörer finns i databasen? Ange `CompanyName`, `ContactName`, `Country`, `Phone` samt `Homepage`. Sortera efter `CompanyName` i fallande ordning.
```SQL
SELECT CompanyName, ContactName, Country, Phone, Homepage 
FROM Suppliers
WHERE Country IN ('Germany', 'France')
ORDER BY CompanyName DESC
```

### 10. 
Vilka produkter finns det mer än 100 av i lager? Ange `ProductID`, `ProductName`, `UnitPrice` samt `UnitsInStock`. Sortera först efter `UnitPrice` i fallande ordning och sedan efter `UnitsInStock` i stigande ordning.
```SQL
SELECT ProductID, ProductName, UnitPrice, UnitsInStock 
FROM Products
WHERE UnitsInStock > 100
ORDER BY UnitPrice DESC, UnitsInStock ASC
```

### 11. 
Vilket födelsedatum har den anställd som är äldst? Namnge kolumnen ”Födelsedag”.
```SQL
SELECT MIN(BirthDate) as `Födelsedag`
FROM Employees
```

### 12. 
Vad är snittpriset samt summan för produkter i kategori 2? Namnge kolumnerna ”SnittPris” och ”Summa”.
```SQL
SELECT AVG(UnitPrice) AS `SnittPris`, SUM(UnitPrice) as `Summa`
FROM Products
WHERE CategoryID = 2
```

### 13. 
Vad är den största rabatten som ges bland samtliga orderrader? Namnge kolumnen ”StörstRabatt”.
```SQL
SELECT MAX(Discount) as `StörstRabatt` 
FROM Order_Details
```

### 14. 
Hur många olika produktkategorier finns det? Namnge kolumnen ”Antal”
```SQL
SELECT COUNT(*) as `Antal`
FROM Categories
```

### 15. 
Hur många kunder finns det i varje land? Sortera efter antal kunder i fallande ordning. Ange Country och namnge en kolumn ”Antal”.
```SQL
SELECT DISTINCT Country,
COUNT(*) as `Antal`
FROM Customers
GROUP BY Country  
ORDER BY `Antal` DESC
```

### 16. 
Vad är det genomsnittliga priset för produkter i kategorierna 1, 3, 5, och 7 grupperade efter leverantör? Ange SupplierID och namnge en kolumn ”SnittPris”.
```SQL
SELECT SupplierID, AVG(UnitPrice) AS `SnittPris`
FROM Products
WHERE CategoryID in (1, 3, 5, 7)
GROUP BY SupplierID
```

### 17. 
Ange CategoryID för de kategorier som innehåller fler än 10 produkter.
```SQL
SELECT CategoryID
FROM Products
GROUP BY CategoryID
HAVING COUNT(*) > 10
```

### 18. 
Visa CustomerID för de kunder som hade fler än 10 ordrar under perioden 1998-01- 01 och 1998-12-31. Skapa en kolumn ”Antal Ordrar” för att visa antalet ordrar.
```SQL
SELECT CustomerID, COUNT(*) as `Antal Ordrar` 
FROM Orders
WHERE OrderDate BETWEEN '1998-01-01' AND '1998-12-31'
GROUP BY CustomerID
HAVING COUNT(*) > 10
```

### 19. 
Skapa en tio-i-top lista över de produkter som det har sålts mest av. Ange ProductID samt summan av antalet sålda enheter som ”Antal”.
```SQL
SELECT ProductID, SUM(Quantity) as `Antal`
FROM Order_Details
GROUP BY ProductID  
ORDER BY `Antal` DESC
```

### 20. 
<!-- TODO -->
Visa en lista med de 10 senaste lagda ordrarna. Hämta FirstName och LastName från Employees tabellen och OrderDate från Orders tabellen.
```SQL
SELECT Employees.FirstName, Employees.LastName, OrderDate
FROM Orders
INNER JOIN Employees ON Orders.EmployeeID=Employees.EmployeeID  
ORDER BY `Orders`.`OrderDate` DESC
LIMIT 10
```

