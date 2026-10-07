# 📘 SQL — Quick Revision & Practice Queries

---

# PART 1 — Functions & Methods Quick Revision Tables

---

## 🔹 1. Aggregate Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `COUNT()` | `SELECT COUNT(column) FROM table` | Returns the number of rows (ignores NULLs unless `COUNT(*)`) |
| `SUM()` | `SELECT SUM(column) FROM table` | Returns the total sum of a numeric column |
| `AVG()` | `SELECT AVG(column) FROM table` | Returns the average value of a numeric column |
| `MIN()` | `SELECT MIN(column) FROM table` | Returns the smallest value in a column |
| `MAX()` | `SELECT MAX(column) FROM table` | Returns the largest value in a column |
| `COUNT(DISTINCT)` | `SELECT COUNT(DISTINCT column) FROM table` | Returns count of unique non-NULL values |
| `STRING_AGG()` | `STRING_AGG(column, ', ')` | Concatenates values into a single string with a separator (SQL Server 2017+) |
| `GROUPING()` | `GROUPING(column)` | Returns 1 if the row is a subtotal/grand total row (used with ROLLUP/CUBE) |

---

## 🔹 2. Window / Ranking Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `ROW_NUMBER()` | `ROW_NUMBER() OVER (ORDER BY col)` | Assigns a unique sequential number to each row — no ties, always 1, 2, 3... |
| `RANK()` | `RANK() OVER (ORDER BY col)` | Same rank for ties, **skips** next number — e.g., 1, 2, 2, **4** |
| `DENSE_RANK()` | `DENSE_RANK() OVER (ORDER BY col)` | Same rank for ties, **no skip** — e.g., 1, 2, 2, **3** |
| `NTILE(n)` | `NTILE(4) OVER (ORDER BY col)` | Divides rows into `n` equal groups and assigns group number (1 to n) |
| `LAG()` | `LAG(col, offset, default) OVER (ORDER BY col)` | Returns value from the **previous** row (offset rows back) |
| `LEAD()` | `LEAD(col, offset, default) OVER (ORDER BY col)` | Returns value from the **next** row (offset rows forward) |
| `FIRST_VALUE()` | `FIRST_VALUE(col) OVER (ORDER BY col)` | Returns the **first** value in the window frame |
| `LAST_VALUE()` | `LAST_VALUE(col) OVER (ORDER BY col ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)` | Returns the **last** value in the window frame (need full frame clause) |
| `SUM() OVER` | `SUM(col) OVER (PARTITION BY grp ORDER BY col)` | Running total / cumulative sum within a partition |
| `AVG() OVER` | `AVG(col) OVER (PARTITION BY grp)` | Average within a partition without collapsing rows |
| `COUNT() OVER` | `COUNT(*) OVER (PARTITION BY grp)` | Count within a partition without collapsing rows |

> **PARTITION BY** = divides data into groups (like GROUP BY but keeps all rows)
> **ORDER BY** inside OVER = defines the order for ranking/running calculations

---

## 🔹 3. String Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `LEN()` | `LEN('Hello')` → 5 | Returns the length of a string (excludes trailing spaces) |
| `DATALENGTH()` | `DATALENGTH('Hello')` → 5 (VARCHAR) / 10 (NVARCHAR) | Returns actual bytes used (detects VARCHAR vs NVARCHAR) |
| `LEFT()` | `LEFT('Hello', 3)` → 'Hel' | Returns first N characters from the left |
| `RIGHT()` | `RIGHT('Hello', 3)` → 'llo' | Returns last N characters from the right |
| `SUBSTRING()` | `SUBSTRING('Hello', 2, 3)` → 'ell' | Extracts characters from position for given length |
| `CHARINDEX()` | `CHARINDEX('l', 'Hello')` → 3 | Finds the position of a substring (first occurrence) |
| `PATINDEX()` | `PATINDEX('%pattern%', string)` | Finds position using wildcard pattern matching |
| `REPLACE()` | `REPLACE('Hello', 'l', 'r')` → 'Herro' | Replaces all occurrences of a substring |
| `STUFF()` | `STUFF('Hello', 2, 3, 'XY')` → 'HXYo' | Deletes characters at position, inserts new string |
| `CONCAT()` | `CONCAT('Hi', ' ', 'There')` → 'Hi There' | Joins strings together (NULL-safe, treats NULL as '') |
| `CONCAT_WS()` | `CONCAT_WS('-', '2024', '01', '15')` → '2024-01-15' | Joins strings with a separator (skips NULLs) |
| `UPPER()` | `UPPER('hello')` → 'HELLO' | Converts to uppercase |
| `LOWER()` | `LOWER('HELLO')` → 'hello' | Converts to lowercase |
| `LTRIM()` | `LTRIM('  Hello')` → 'Hello' | Removes leading spaces |
| `RTRIM()` | `RTRIM('Hello  ')` → 'Hello' | Removes trailing spaces |
| `TRIM()` | `TRIM('  Hello  ')` → 'Hello' | Removes both leading and trailing spaces (SQL 2017+) |
| `REVERSE()` | `REVERSE('Hello')` → 'olleH' | Reverses the string |
| `REPLICATE()` | `REPLICATE('Ha', 3)` → 'HaHaHa' | Repeats a string N times |
| `SPACE()` | `SPACE(5)` → '     ' | Returns N spaces |
| `FORMAT()` | `FORMAT(1234567, 'N2')` → '1,234,567.00' | Formats a value with a specified format string |
| `STRING_SPLIT()` | `SELECT * FROM STRING_SPLIT('a,b,c', ',')` | Splits a string by delimiter into rows (table-valued) |

---

## 🔹 4. DateTime Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `GETDATE()` | `SELECT GETDATE()` | Returns current date and time (datetime) |
| `SYSDATETIME()` | `SELECT SYSDATETIME()` | Returns current date and time with higher precision (datetime2) |
| `GETUTCDATE()` | `SELECT GETUTCDATE()` | Returns current UTC date and time |
| `DATEADD()` | `DATEADD(DAY, 7, GETDATE())` | Adds an interval to a date (DAY, MONTH, YEAR, HOUR, etc.) |
| `DATEDIFF()` | `DATEDIFF(YEAR, '2000-01-01', GETDATE())` | Returns the difference between two dates in specified unit |
| `DATEPART()` | `DATEPART(MONTH, GETDATE())` → 10 | Extracts a specific part as integer (YEAR, MONTH, DAY, HOUR, etc.) |
| `DATENAME()` | `DATENAME(MONTH, GETDATE())` → 'October' | Extracts a part as string (returns name for MONTH, WEEKDAY) |
| `YEAR()` | `YEAR(GETDATE())` → 2026 | Shortcut: returns year from a date |
| `MONTH()` | `MONTH(GETDATE())` → 10 | Shortcut: returns month number from a date |
| `DAY()` | `DAY(GETDATE())` → 7 | Shortcut: returns day of month from a date |
| `EOMONTH()` | `EOMONTH(GETDATE())` | Returns the last day of the month for a given date |
| `ISDATE()` | `ISDATE('2024-13-01')` → 0 | Checks if a value is a valid date (returns 1 or 0) |
| `CONVERT()` | `CONVERT(VARCHAR, GETDATE(), 103)` → '07/10/2026' | Converts date to string with format style |
| `FORMAT()` | `FORMAT(GETDATE(), 'dd-MMM-yyyy')` → '07-Oct-2026' | Formats date using .NET-style format strings |
| `CAST()` | `CAST('2024-01-15' AS DATE)` | Converts string to date type |
| `SWITCHOFFSET()` | `SWITCHOFFSET(SYSDATETIMEOFFSET(), '+05:30')` | Converts datetimeoffset to a different timezone |

**Common CONVERT Style Codes:**

| Style | Format | Example |
|-------|--------|---------|
| 101 | mm/dd/yyyy | 10/07/2026 |
| 103 | dd/mm/yyyy | 07/10/2026 |
| 104 | dd.mm.yyyy | 07.10.2026 |
| 108 | HH:mm:ss | 23:15:30 |
| 112 | yyyymmdd | 20261007 |
| 120 | yyyy-mm-dd HH:mm:ss | 2026-10-07 23:15:30 |
| 126 | ISO 8601 | 2026-10-07T23:15:30 |

---

## 🔹 5. Mathematical Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `ABS()` | `ABS(-15)` → 15 | Returns the absolute (positive) value |
| `CEILING()` | `CEILING(4.2)` → 5 | Rounds up to the nearest integer |
| `FLOOR()` | `FLOOR(4.8)` → 4 | Rounds down to the nearest integer |
| `ROUND()` | `ROUND(4.567, 2)` → 4.57 | Rounds to specified decimal places |
| `POWER()` | `POWER(2, 3)` → 8 | Returns value raised to a power |
| `SQRT()` | `SQRT(25)` → 5 | Returns the square root |
| `SIGN()` | `SIGN(-10)` → -1 | Returns -1, 0, or 1 based on sign |
| `RAND()` | `RAND()` → 0.7134... | Returns a random float between 0 and 1 |
| `LOG()` | `LOG(10)` → 2.302... | Returns the natural logarithm |
| `LOG10()` | `LOG10(100)` → 2 | Returns the base-10 logarithm |

---

## 🔹 6. NULL Handling Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `ISNULL()` | `ISNULL(Phone, 'N/A')` | Replaces NULL with a specified value (SQL Server only, 2 args) |
| `COALESCE()` | `COALESCE(Phone, Mobile, 'N/A')` | Returns the first non-NULL value from a list (ANSI, multiple args) |
| `NULLIF()` | `NULLIF(a, b)` | Returns NULL if `a = b`, otherwise returns `a` (useful to avoid divide-by-zero) |
| `IIF()` | `IIF(condition, true_val, false_val)` | Inline IF — returns one of two values based on condition |
| `CASE WHEN` | `CASE WHEN x > 10 THEN 'High' ELSE 'Low' END` | Multi-condition branching, can be used in SELECT, WHERE, ORDER BY |
| `IS NULL` | `WHERE col IS NULL` | Checks if a value is NULL (never use `= NULL`, it doesn't work!) |
| `IS NOT NULL` | `WHERE col IS NOT NULL` | Checks if a value is NOT NULL |

---

## 🔹 7. Conversion Functions

| Function | Syntax | Description |
|----------|--------|-------------|
| `CAST()` | `CAST(25.67 AS INT)` → 25 | Converts one data type to another (ANSI standard) |
| `CONVERT()` | `CONVERT(VARCHAR, 123)` → '123' | Converts with optional style code (SQL Server specific) |
| `TRY_CAST()` | `TRY_CAST('abc' AS INT)` → NULL | Returns NULL instead of error if conversion fails |
| `TRY_CONVERT()` | `TRY_CONVERT(INT, 'abc')` → NULL | Same as TRY_CAST but SQL Server specific with style support |
| `PARSE()` | `PARSE('07/10/2026' AS DATE USING 'en-GB')` | Converts string to date/number using culture settings |
| `TRY_PARSE()` | `TRY_PARSE('abc' AS INT)` → NULL | Returns NULL instead of error if parse fails |
| `STR()` | `STR(123.45, 8, 2)` → '  123.45' | Converts numeric to string with length and decimal control |

---

## 🔹 8. Set Operators

| Operator | Syntax | Description |
|----------|--------|-------------|
| `UNION` | `SELECT ... UNION SELECT ...` | Combines results, removes duplicates (slower) |
| `UNION ALL` | `SELECT ... UNION ALL SELECT ...` | Combines results, keeps all rows including duplicates (faster) |
| `INTERSECT` | `SELECT ... INTERSECT SELECT ...` | Returns only rows that exist in BOTH queries |
| `EXCEPT` | `SELECT ... EXCEPT SELECT ...` | Returns rows from first query that don't exist in second query |

> All set operators require: same number of columns, compatible data types, same column order.

---

## 🔹 9. JOIN Types Quick Reference

| JOIN Type | What It Returns | NULL Behavior |
|-----------|----------------|---------------|
| `INNER JOIN` | Only matching rows from both tables | No NULLs — unmatched rows excluded |
| `LEFT JOIN` | All rows from left + matching from right | Right side fills with NULL if no match |
| `RIGHT JOIN` | All rows from right + matching from left | Left side fills with NULL if no match |
| `FULL JOIN` | All rows from both tables | NULLs on both sides where no match |
| `CROSS JOIN` | Cartesian product (every × every) | No ON clause needed, no NULLs |
| `SELF JOIN` | Table joined with itself (uses aliases) | Depends on join type used |

---

## 🔹 10. Useful Clauses & Keywords

| Clause | Syntax | Description |
|--------|--------|-------------|
| `TOP` | `SELECT TOP 10 * FROM table` | Returns first N rows |
| `TOP WITH TIES` | `SELECT TOP 5 WITH TIES * FROM t ORDER BY sal DESC` | Includes tied rows beyond TOP limit |
| `DISTINCT` | `SELECT DISTINCT col FROM table` | Removes duplicate rows |
| `ORDER BY` | `ORDER BY col ASC/DESC` | Sorts results (ASC = ascending default, DESC = descending) |
| `GROUP BY` | `GROUP BY col1, col2` | Groups rows for aggregate functions |
| `HAVING` | `HAVING COUNT(*) > 5` | Filters groups after GROUP BY (like WHERE for groups) |
| `OFFSET FETCH` | `OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY` | Pagination — skip 10, take next 5 (SQL Server 2012+) |
| `IN` | `WHERE col IN (1, 2, 3)` | Matches any value in a list |
| `BETWEEN` | `WHERE col BETWEEN 10 AND 50` | Range filter, inclusive of both ends |
| `LIKE` | `WHERE name LIKE 'A%'` | Pattern matching (% = any chars, _ = one char) |
| `EXISTS` | `WHERE EXISTS (subquery)` | Returns TRUE if subquery returns any rows |
| `ANY / ALL` | `WHERE col > ANY (subquery)` | Compares value against subquery results |
| `WITH (CTE)` | `WITH cte AS (SELECT ...) SELECT * FROM cte` | Temporary named result set for one query |
| `PIVOT` | `PIVOT (AGG(col) FOR col2 IN (...))` | Rotates rows to columns (crosstab) |
| `UNPIVOT` | `UNPIVOT (val FOR col IN (...))` | Rotates columns to rows (reverse of PIVOT) |
| `MERGE` | `MERGE target USING source ON ... WHEN MATCHED / NOT MATCHED` | Insert, Update, Delete in a single statement |
| `OUTPUT` | `INSERT INTO t OUTPUT inserted.ID VALUES (...)` | Returns affected rows from INSERT/UPDATE/DELETE |
| `APPLY` | `CROSS APPLY / OUTER APPLY (table-valued function)` | Applies a function to each row (like correlated subquery as JOIN) |

---

## 🔹 11. Wildcard Characters (for LIKE)

| Wildcard | Meaning | Example |
|----------|---------|---------|
| `%` | Any sequence of characters | `'A%'` → starts with A |
| `_` | Any single character | `'_ello'` → Hello, Jello |
| `[]` | Any single character in the set | `'[ABC]%'` → starts with A, B, or C |
| `[^]` | Any single character NOT in the set | `'[^ABC]%'` → does NOT start with A, B, or C |
| `[-]` | Character range | `'[A-F]%'` → starts with A through F |

---

---

# PART 2 — 50 Practice Queries (Schema-Wise, Mixed Concepts)

---

## 📋 Schema 1: Employee Management System

```sql
CREATE TABLE Departments (
    DeptID    INT PRIMARY KEY,
    DeptName  VARCHAR(50),
    Location  VARCHAR(50)
);

CREATE TABLE Employees (
    EmpID      INT PRIMARY KEY,
    EmpName    VARCHAR(100),
    Salary     DECIMAL(10,2),
    DeptID     INT FOREIGN KEY REFERENCES Departments(DeptID),
    ManagerID  INT NULL,
    HireDate   DATE,
    City       VARCHAR(50)
);

-- Sample Data:
-- Departments: (1,'IT','Ahmedabad'), (2,'HR','Mumbai'), (3,'Sales','Delhi'), (4,'Finance','Pune')
-- Employees: Multiple employees with varying salaries, departments, managers, hire dates
```

---

### Q1. Find the second highest salary from Employees.

```sql
SELECT MAX(Salary) AS SecondHighest
FROM Employees
WHERE Salary < (SELECT MAX(Salary) FROM Employees);
```

**Logic:** Inner query finds the max salary. Outer query finds the max among the rest — which is the 2nd highest.

---

### Q2. Find the Nth highest salary (e.g., 3rd highest) using a subquery.

```sql
SELECT DISTINCT Salary
FROM Employees e1
WHERE 3 = (
    SELECT COUNT(DISTINCT Salary)
    FROM Employees e2
    WHERE e2.Salary >= e1.Salary
);
```

**Logic:** For each salary, count how many distinct salaries are ≥ it. If that count is N, it's the Nth highest. This is a correlated subquery — it re-runs for each row.

---

### Q3. Find the Nth highest salary using DENSE_RANK (cleaner approach).

```sql
WITH RankedSalary AS (
    SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS Rnk
    FROM Employees
)
SELECT DISTINCT Salary
FROM RankedSalary
WHERE Rnk = 3;
```

**Logic:** DENSE_RANK assigns ranks without gaps (ties get same rank). Filter where rank = N. This is the **interview-preferred** approach.

---

### Q4. Find employees who earn more than the average salary of their department.

```sql
SELECT e.EmpName, e.Salary, e.DeptID
FROM Employees e
WHERE e.Salary > (
    SELECT AVG(Salary)
    FROM Employees
    WHERE DeptID = e.DeptID
);
```

**Logic:** Correlated subquery calculates avg salary per department. Outer query picks employees whose salary beats their department average.

---

### Q5. List each department with the number of employees and average salary. Show only departments with more than 2 employees.

```sql
SELECT d.DeptName, COUNT(e.EmpID) AS TotalEmp, AVG(e.Salary) AS AvgSalary
FROM Departments d
LEFT JOIN Employees e ON d.DeptID = e.DeptID
GROUP BY d.DeptName
HAVING COUNT(e.EmpID) > 2
ORDER BY AvgSalary DESC;
```

**Logic:** LEFT JOIN to include all departments. GROUP BY to aggregate. HAVING to filter groups (not WHERE — because we're filtering on COUNT). ORDER BY to sort the result.

---

### Q6. Find employees who don't belong to any department.

```sql
SELECT EmpName
FROM Employees
WHERE DeptID IS NULL;

-- OR using NOT EXISTS:
SELECT e.EmpName
FROM Employees e
WHERE NOT EXISTS (
    SELECT 1 FROM Departments d WHERE d.DeptID = e.DeptID
);
```

**Logic:** First approach checks for NULL foreign key. Second uses NOT EXISTS — if no matching department exists, the employee is included.

---

### Q7. Find the employee(s) with the highest salary in each department.

```sql
WITH DeptMax AS (
    SELECT DeptID, EmpName, Salary,
           RANK() OVER (PARTITION BY DeptID ORDER BY Salary DESC) AS Rnk
    FROM Employees
)
SELECT DeptID, EmpName, Salary
FROM DeptMax
WHERE Rnk = 1;
```

**Logic:** RANK() with PARTITION BY DeptID ranks employees within each department. Rnk = 1 gives the highest. RANK is used over ROW_NUMBER to handle ties (multiple employees with same top salary).

---

### Q8. Find the manager name for each employee (Self Join).

```sql
SELECT e.EmpName AS Employee, m.EmpName AS Manager
FROM Employees e
LEFT JOIN Employees m ON e.ManagerID = m.EmpID;
```

**Logic:** Self Join — same table aliased twice. LEFT JOIN because top-level managers have NULL ManagerID (no manager above them).

---

### Q9. Find departments that have no employees.

```sql
SELECT d.DeptName
FROM Departments d
LEFT JOIN Employees e ON d.DeptID = e.DeptID
WHERE e.EmpID IS NULL;
```

**Logic:** LEFT JOIN returns all departments. If no employee matches, EmpID is NULL. Filtering on NULL gives empty departments. This is a classic interview pattern.

---

### Q10. Find employees hired in the last 6 months.

```sql
SELECT EmpName, HireDate
FROM Employees
WHERE HireDate >= DATEADD(MONTH, -6, GETDATE())
ORDER BY HireDate DESC;
```

**Logic:** DATEADD subtracts 6 months from today. Employees hired after that date are selected.

---

## 📋 Schema 2: E-Commerce System

```sql
CREATE TABLE Customers (
    CustomerID   INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    City         VARCHAR(50),
    Country      VARCHAR(50)
);

CREATE TABLE Products (
    ProductID    INT PRIMARY KEY,
    ProductName  VARCHAR(100),
    Category     VARCHAR(50),
    Price        DECIMAL(10,2)
);

CREATE TABLE Orders (
    OrderID      INT PRIMARY KEY,
    CustomerID   INT FOREIGN KEY REFERENCES Customers(CustomerID),
    OrderDate    DATE,
    TotalAmount  DECIMAL(10,2)
);

CREATE TABLE OrderDetails (
    OrderDetailID INT PRIMARY KEY,
    OrderID       INT FOREIGN KEY REFERENCES Orders(OrderID),
    ProductID     INT FOREIGN KEY REFERENCES Products(ProductID),
    Quantity      INT,
    UnitPrice     DECIMAL(10,2)
);
```

---

### Q11. Find customers who have placed more than 5 orders.

```sql
SELECT c.CustomerName, COUNT(o.OrderID) AS TotalOrders
FROM Customers c
INNER JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerName
HAVING COUNT(o.OrderID) > 5
ORDER BY TotalOrders DESC;
```

**Logic:** JOIN to connect customers to orders. GROUP BY customer. HAVING filters groups with count > 5.

---

### Q12. Find the top 3 most expensive products in each category.

```sql
WITH RankedProducts AS (
    SELECT ProductName, Category, Price,
           ROW_NUMBER() OVER (PARTITION BY Category ORDER BY Price DESC) AS Rnk
    FROM Products
)
SELECT ProductName, Category, Price
FROM RankedProducts
WHERE Rnk <= 3;
```

**Logic:** ROW_NUMBER partitioned by Category ranks products within each category by price. Filter top 3.

---

### Q13. Find customers who have never placed an order.

```sql
SELECT c.CustomerName
FROM Customers c
WHERE c.CustomerID NOT IN (
    SELECT DISTINCT CustomerID FROM Orders
);

-- Better approach (handles NULLs safely):
SELECT c.CustomerName
FROM Customers c
WHERE NOT EXISTS (
    SELECT 1 FROM Orders o WHERE o.CustomerID = c.CustomerID
);
```

**Logic:** NOT IN checks if customer has no orders. But NOT EXISTS is safer — if CustomerID has NULLs in Orders, NOT IN returns empty results (common bug!).

---

### Q14. Find the total revenue generated per month in the current year.

```sql
SELECT DATENAME(MONTH, OrderDate) AS MonthName,
       MONTH(OrderDate) AS MonthNum,
       SUM(TotalAmount) AS Revenue
FROM Orders
WHERE YEAR(OrderDate) = YEAR(GETDATE())
GROUP BY DATENAME(MONTH, OrderDate), MONTH(OrderDate)
ORDER BY MonthNum;
```

**Logic:** YEAR() filters current year. GROUP BY month. DATENAME gives month name for readability. ORDER BY month number for chronological order.

---

### Q15. Find the most popular product (highest total quantity sold).

```sql
SELECT TOP 1 p.ProductName, SUM(od.Quantity) AS TotalSold
FROM Products p
INNER JOIN OrderDetails od ON p.ProductID = od.ProductID
GROUP BY p.ProductName
ORDER BY TotalSold DESC;
```

**Logic:** JOIN products with order details. SUM quantity per product. TOP 1 with ORDER BY DESC gives the most sold product.

---

### Q16. Find the running total of order amounts per customer.

```sql
SELECT c.CustomerName, o.OrderDate, o.TotalAmount,
       SUM(o.TotalAmount) OVER (PARTITION BY c.CustomerID ORDER BY o.OrderDate) AS RunningTotal
FROM Customers c
INNER JOIN Orders o ON c.CustomerID = o.CustomerID
ORDER BY c.CustomerName, o.OrderDate;
```

**Logic:** SUM() OVER with PARTITION BY customer and ORDER BY date calculates cumulative sum. Each row shows the running total up to that order.

---

### Q17. Find customers who ordered all products from a specific category (e.g., 'Electronics').

```sql
SELECT c.CustomerName
FROM Customers c
WHERE NOT EXISTS (
    SELECT p.ProductID
    FROM Products p
    WHERE p.Category = 'Electronics'
    AND NOT EXISTS (
        SELECT 1
        FROM OrderDetails od
        INNER JOIN Orders o ON od.OrderID = o.OrderID
        WHERE o.CustomerID = c.CustomerID
          AND od.ProductID = p.ProductID
    )
);
```

**Logic:** This is a **relational division** query. Outer NOT EXISTS says "there is no product in Electronics that this customer hasn't ordered." Double NOT EXISTS = "for all" logic.

---

### Q18. Find the difference in order amount from the previous order for each customer.

```sql
SELECT c.CustomerName, o.OrderDate, o.TotalAmount,
       LAG(o.TotalAmount) OVER (PARTITION BY c.CustomerID ORDER BY o.OrderDate) AS PrevAmount,
       o.TotalAmount - LAG(o.TotalAmount) OVER (PARTITION BY c.CustomerID ORDER BY o.OrderDate) AS Difference
FROM Customers c
INNER JOIN Orders o ON c.CustomerID = o.CustomerID
ORDER BY c.CustomerName, o.OrderDate;
```

**Logic:** LAG() gets the previous row's value. Subtract it from current to get the difference. First order for each customer will show NULL (no previous).

---

### Q19. Find categories where total revenue exceeds ₹1,00,000 and has at least 50 orders.

```sql
SELECT p.Category,
       SUM(od.Quantity * od.UnitPrice) AS TotalRevenue,
       COUNT(DISTINCT od.OrderID) AS TotalOrders
FROM Products p
INNER JOIN OrderDetails od ON p.ProductID = od.ProductID
GROUP BY p.Category
HAVING SUM(od.Quantity * od.UnitPrice) > 100000
   AND COUNT(DISTINCT od.OrderID) >= 50
ORDER BY TotalRevenue DESC;
```

**Logic:** Multiple HAVING conditions combined with AND. COUNT(DISTINCT OrderID) avoids counting same order multiple times. Revenue = Quantity × UnitPrice.

---

### Q20. Find customers whose total spending is above the overall average customer spending.

```sql
WITH CustomerSpending AS (
    SELECT CustomerID, SUM(TotalAmount) AS TotalSpent
    FROM Orders
    GROUP BY CustomerID
)
SELECT c.CustomerName, cs.TotalSpent
FROM CustomerSpending cs
INNER JOIN Customers c ON cs.CustomerID = c.CustomerID
WHERE cs.TotalSpent > (SELECT AVG(TotalSpent) FROM CustomerSpending)
ORDER BY cs.TotalSpent DESC;
```

**Logic:** CTE calculates total spending per customer. Then filter where individual total > average of all totals. CTE is reused twice — once in main query, once in subquery.

---

## 📋 Schema 3: Student & Exam System

```sql
CREATE TABLE Students (
    StudentID   INT PRIMARY KEY,
    StudentName VARCHAR(100),
    Class       VARCHAR(20),
    Section     CHAR(1)
);

CREATE TABLE Subjects (
    SubjectID   INT PRIMARY KEY,
    SubjectName VARCHAR(50)
);

CREATE TABLE Marks (
    MarkID     INT PRIMARY KEY,
    StudentID  INT FOREIGN KEY REFERENCES Students(StudentID),
    SubjectID  INT FOREIGN KEY REFERENCES Subjects(SubjectID),
    MarksObt   INT,
    ExamDate   DATE
);
```

---

### Q21. Find students who scored above 90 in all subjects.

```sql
SELECT s.StudentName
FROM Students s
INNER JOIN Marks m ON s.StudentID = m.StudentID
GROUP BY s.StudentName
HAVING MIN(m.MarksObt) > 90;
```

**Logic:** If the **minimum** marks of a student is > 90, it means ALL their marks are above 90. MIN trick avoids checking each subject individually. Classic interview logic!

---

### Q22. Find the topper (highest total marks) in each class.

```sql
WITH ClassRank AS (
    SELECT s.StudentName, s.Class, SUM(m.MarksObt) AS TotalMarks,
           RANK() OVER (PARTITION BY s.Class ORDER BY SUM(m.MarksObt) DESC) AS Rnk
    FROM Students s
    INNER JOIN Marks m ON s.StudentID = m.StudentID
    GROUP BY s.StudentName, s.Class
)
SELECT StudentName, Class, TotalMarks
FROM ClassRank
WHERE Rnk = 1;
```

**Logic:** SUM marks per student. RANK partitioned by class. Rnk = 1 gives the topper. RANK handles ties (both get rank 1 if tied).

---

### Q23. Find subjects where the class average is below 50.

```sql
SELECT sub.SubjectName, AVG(m.MarksObt) AS AvgMarks
FROM Subjects sub
INNER JOIN Marks m ON sub.SubjectID = m.SubjectID
GROUP BY sub.SubjectName
HAVING AVG(m.MarksObt) < 50
ORDER BY AvgMarks;
```

**Logic:** GROUP BY subject, calculate AVG, filter with HAVING. Simple GROUP BY + HAVING pattern.

---

### Q24. Find students who appeared in fewer subjects than the total number of subjects offered.

```sql
SELECT s.StudentName, COUNT(DISTINCT m.SubjectID) AS SubjectsAttempted
FROM Students s
INNER JOIN Marks m ON s.StudentID = m.StudentID
GROUP BY s.StudentName
HAVING COUNT(DISTINCT m.SubjectID) < (SELECT COUNT(*) FROM Subjects);
```

**Logic:** Count distinct subjects each student attempted vs total subjects. HAVING compares per-student count with total count from subquery.

---

### Q25. Show each student's marks alongside the subject average and department average.

```sql
SELECT s.StudentName, sub.SubjectName, m.MarksObt,
       AVG(m.MarksObt) OVER (PARTITION BY m.SubjectID) AS SubjectAvg,
       AVG(m.MarksObt) OVER (PARTITION BY s.Class) AS ClassAvg
FROM Students s
INNER JOIN Marks m ON s.StudentID = m.StudentID
INNER JOIN Subjects sub ON m.SubjectID = sub.SubjectID
ORDER BY s.StudentName;
```

**Logic:** Window functions with different PARTITION BY clauses. SubjectAvg partitions by subject, ClassAvg partitions by class — both without collapsing rows.

---

## 📋 Schema 4: Hospital Management System

```sql
CREATE TABLE Doctors (
    DoctorID    INT PRIMARY KEY,
    DoctorName  VARCHAR(100),
    Speciality  VARCHAR(50),
    Experience  INT
);

CREATE TABLE Patients (
    PatientID   INT PRIMARY KEY,
    PatientName VARCHAR(100),
    Age         INT,
    Gender      CHAR(1)
);

CREATE TABLE Appointments (
    AppointmentID INT PRIMARY KEY,
    PatientID     INT FOREIGN KEY REFERENCES Patients(PatientID),
    DoctorID      INT FOREIGN KEY REFERENCES Doctors(DoctorID),
    AppointmentDate DATE,
    Diagnosis     VARCHAR(200),
    Fee           DECIMAL(10,2)
);
```

---

### Q26. Find doctors who have treated more than 100 unique patients.

```sql
SELECT d.DoctorName, d.Speciality, COUNT(DISTINCT a.PatientID) AS UniquePatients
FROM Doctors d
INNER JOIN Appointments a ON d.DoctorID = a.DoctorID
GROUP BY d.DoctorName, d.Speciality
HAVING COUNT(DISTINCT a.PatientID) > 100
ORDER BY UniquePatients DESC;
```

**Logic:** COUNT(DISTINCT PatientID) ensures same patient visiting multiple times is counted once. HAVING filters doctors with 100+ unique patients.

---

### Q27. Find patients who visited more than one doctor for the same diagnosis.

```sql
SELECT p.PatientName, a.Diagnosis, COUNT(DISTINCT a.DoctorID) AS DoctorCount
FROM Patients p
INNER JOIN Appointments a ON p.PatientID = a.PatientID
GROUP BY p.PatientName, a.Diagnosis
HAVING COUNT(DISTINCT a.DoctorID) > 1;
```

**Logic:** GROUP BY patient and diagnosis. If DISTINCT doctor count > 1, the patient visited multiple doctors for the same problem.

---

### Q28. Find the doctor with the highest total revenue (fees) per speciality.

```sql
WITH DoctorRevenue AS (
    SELECT d.DoctorName, d.Speciality, SUM(a.Fee) AS TotalRevenue,
           ROW_NUMBER() OVER (PARTITION BY d.Speciality ORDER BY SUM(a.Fee) DESC) AS Rnk
    FROM Doctors d
    INNER JOIN Appointments a ON d.DoctorID = a.DoctorID
    GROUP BY d.DoctorName, d.Speciality
)
SELECT DoctorName, Speciality, TotalRevenue
FROM DoctorRevenue
WHERE Rnk = 1;
```

**Logic:** CTE calculates revenue per doctor. ROW_NUMBER partitioned by speciality ranks them. Rnk = 1 = top earner per speciality.

---

### Q29. Find the month-over-month growth in total appointments.

```sql
WITH MonthlyCount AS (
    SELECT YEAR(AppointmentDate) AS Yr, MONTH(AppointmentDate) AS Mn,
           COUNT(*) AS Total
    FROM Appointments
    GROUP BY YEAR(AppointmentDate), MONTH(AppointmentDate)
)
SELECT Yr, Mn, Total,
       LAG(Total) OVER (ORDER BY Yr, Mn) AS PrevMonth,
       Total - LAG(Total) OVER (ORDER BY Yr, Mn) AS Growth,
       CASE WHEN LAG(Total) OVER (ORDER BY Yr, Mn) IS NOT NULL
            THEN CAST((Total - LAG(Total) OVER (ORDER BY Yr, Mn)) * 100.0 
                  / LAG(Total) OVER (ORDER BY Yr, Mn) AS DECIMAL(5,2))
            ELSE NULL END AS GrowthPct
FROM MonthlyCount
ORDER BY Yr, Mn;
```

**Logic:** CTE groups by year-month. LAG gets previous month. Growth = current - previous. GrowthPct = percentage change. CASE handles NULL for the first month.

---

### Q30. Find patients older than 60 who haven't visited in the last 1 year.

```sql
SELECT p.PatientName, p.Age, MAX(a.AppointmentDate) AS LastVisit
FROM Patients p
INNER JOIN Appointments a ON p.PatientID = a.PatientID
WHERE p.Age > 60
GROUP BY p.PatientName, p.Age
HAVING MAX(a.AppointmentDate) < DATEADD(YEAR, -1, GETDATE())
ORDER BY LastVisit;
```

**Logic:** MAX(AppointmentDate) = last visit. HAVING filters those whose last visit was more than 1 year ago. WHERE filters age first, HAVING filters aggregated date.

---

## 📋 Schema 5: Banking System

```sql
CREATE TABLE Accounts (
    AccountID   INT PRIMARY KEY,
    AccountName VARCHAR(100),
    AccountType VARCHAR(20),  -- 'Savings', 'Current'
    Balance     DECIMAL(15,2),
    BranchID    INT
);

CREATE TABLE Transactions (
    TxnID       INT PRIMARY KEY,
    AccountID   INT FOREIGN KEY REFERENCES Accounts(AccountID),
    TxnDate     DATE,
    TxnType     VARCHAR(10),  -- 'Credit', 'Debit'
    Amount      DECIMAL(15,2)
);

CREATE TABLE Branches (
    BranchID   INT PRIMARY KEY,
    BranchName VARCHAR(100),
    City       VARCHAR(50)
);
```

---

### Q31. Find the total credit and debit for each account using conditional aggregation.

```sql
SELECT a.AccountName,
       SUM(CASE WHEN t.TxnType = 'Credit' THEN t.Amount ELSE 0 END) AS TotalCredit,
       SUM(CASE WHEN t.TxnType = 'Debit' THEN t.Amount ELSE 0 END) AS TotalDebit,
       SUM(CASE WHEN t.TxnType = 'Credit' THEN t.Amount ELSE -t.Amount END) AS NetAmount
FROM Accounts a
INNER JOIN Transactions t ON a.AccountID = t.AccountID
GROUP BY a.AccountName
ORDER BY NetAmount DESC;
```

**Logic:** CASE inside SUM = conditional aggregation. It's like doing separate queries for credit and debit, but in one pass. NetAmount shows profit/loss.

---

### Q32. Find accounts that had no transactions in the last 3 months.

```sql
SELECT a.AccountName, a.Balance
FROM Accounts a
WHERE NOT EXISTS (
    SELECT 1 FROM Transactions t
    WHERE t.AccountID = a.AccountID
      AND t.TxnDate >= DATEADD(MONTH, -3, GETDATE())
);
```

**Logic:** NOT EXISTS checks if there's no recent transaction. More efficient than LEFT JOIN for existence checks on large tables.

---

### Q33. Find the top 3 branches by total deposits (credit transactions).

```sql
SELECT TOP 3 b.BranchName, b.City, SUM(t.Amount) AS TotalDeposits
FROM Branches b
INNER JOIN Accounts a ON b.BranchID = a.BranchID
INNER JOIN Transactions t ON a.AccountID = t.AccountID
WHERE t.TxnType = 'Credit'
GROUP BY b.BranchName, b.City
ORDER BY TotalDeposits DESC;
```

**Logic:** Multi-table JOIN chain: Branch → Account → Transaction. WHERE filters credits only. TOP 3 with ORDER BY DESC gives top branches.

---

### Q34. Find duplicate transactions (same account, same amount, same date).

```sql
SELECT AccountID, TxnDate, Amount, COUNT(*) AS Occurrences
FROM Transactions
GROUP BY AccountID, TxnDate, Amount
HAVING COUNT(*) > 1;
```

**Logic:** GROUP BY the combination of columns that should be unique. HAVING COUNT > 1 identifies duplicates. This is the **standard duplicate detection pattern**.

---

### Q35. Calculate the daily running balance for each account.

```sql
SELECT a.AccountName, t.TxnDate, t.TxnType, t.Amount,
       SUM(CASE WHEN t.TxnType = 'Credit' THEN t.Amount ELSE -t.Amount END) 
           OVER (PARTITION BY a.AccountID ORDER BY t.TxnDate, t.TxnID) AS RunningBalance
FROM Accounts a
INNER JOIN Transactions t ON a.AccountID = t.AccountID
ORDER BY a.AccountName, t.TxnDate;
```

**Logic:** CASE converts debits to negative. SUM OVER with ORDER BY gives cumulative sum = running balance. PARTITION BY ensures each account is calculated separately.

---

## 📋 Schema 6: Company Project Tracker

```sql
CREATE TABLE Projects (
    ProjectID    INT PRIMARY KEY,
    ProjectName  VARCHAR(100),
    StartDate    DATE,
    EndDate      DATE,
    Budget       DECIMAL(15,2),
    Status       VARCHAR(20)  -- 'Active', 'Completed', 'Cancelled'
);

CREATE TABLE Tasks (
    TaskID       INT PRIMARY KEY,
    ProjectID    INT FOREIGN KEY REFERENCES Projects(ProjectID),
    TaskName     VARCHAR(100),
    AssignedTo   INT,  -- EmpID
    StartDate    DATE,
    DueDate      DATE,
    Status       VARCHAR(20),  -- 'Pending', 'In Progress', 'Done'
    Priority     VARCHAR(10)   -- 'High', 'Medium', 'Low'
);
```

---

### Q36. Find projects that are overdue (EndDate has passed but Status is not 'Completed').

```sql
SELECT ProjectName, EndDate, Status,
       DATEDIFF(DAY, EndDate, GETDATE()) AS DaysOverdue
FROM Projects
WHERE EndDate < GETDATE() AND Status != 'Completed'
ORDER BY DaysOverdue DESC;
```

**Logic:** EndDate < today = past deadline. Status check excludes completed ones. DATEDIFF shows how many days overdue.

---

### Q37. Find employees (AssignedTo) who have more than 3 high-priority tasks pending.

```sql
SELECT AssignedTo, COUNT(*) AS HighPriorityPending
FROM Tasks
WHERE Priority = 'High' AND Status = 'Pending'
GROUP BY AssignedTo
HAVING COUNT(*) > 3
ORDER BY HighPriorityPending DESC;
```

**Logic:** WHERE filters high priority + pending. GROUP BY employee. HAVING count > 3 identifies overloaded employees.

---

### Q38. Find the percentage of completed tasks per project.

```sql
SELECT p.ProjectName,
       COUNT(*) AS TotalTasks,
       SUM(CASE WHEN t.Status = 'Done' THEN 1 ELSE 0 END) AS CompletedTasks,
       CAST(SUM(CASE WHEN t.Status = 'Done' THEN 1 ELSE 0 END) * 100.0 
            / COUNT(*) AS DECIMAL(5,2)) AS CompletionPct
FROM Projects p
INNER JOIN Tasks t ON p.ProjectID = t.ProjectID
GROUP BY p.ProjectName
ORDER BY CompletionPct DESC;
```

**Logic:** CASE counts completed tasks. Divide by total and multiply by 100 for percentage. CAST ensures decimal division (not integer).

---

### Q39. Find projects where budget utilization is below 50% but have active tasks.

```sql
SELECT p.ProjectName, p.Budget, p.Status,
       COUNT(t.TaskID) AS ActiveTasks
FROM Projects p
INNER JOIN Tasks t ON p.ProjectID = t.ProjectID
WHERE t.Status = 'In Progress'
GROUP BY p.ProjectName, p.Budget, p.Status
HAVING COUNT(t.TaskID) > 0
ORDER BY p.Budget DESC;
```

**Logic:** JOIN with tasks, filter active ones. GROUP BY project shows how many active tasks exist per project.

---

### Q40. Rank projects by the number of overdue tasks.

```sql
SELECT p.ProjectName,
       COUNT(CASE WHEN t.DueDate < GETDATE() AND t.Status != 'Done' THEN 1 END) AS OverdueTasks,
       DENSE_RANK() OVER (ORDER BY COUNT(CASE WHEN t.DueDate < GETDATE() AND t.Status != 'Done' THEN 1 END) DESC) AS OverdueRank
FROM Projects p
LEFT JOIN Tasks t ON p.ProjectID = t.ProjectID
GROUP BY p.ProjectName
ORDER BY OverdueRank;
```

**Logic:** CASE inside COUNT identifies overdue incomplete tasks. DENSE_RANK over that count ranks projects. LEFT JOIN includes projects with zero tasks too.

---

## 📋 Schema 7: Miscellaneous Tricky Queries (No New Schema Needed)

*These use schemas already defined above.*

---

### Q41. Write a query to delete duplicate rows keeping only the one with the lowest ID.

```sql
WITH CTE AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY EmpName, Salary, DeptID ORDER BY EmpID) AS RowNum
    FROM Employees
)
DELETE FROM CTE WHERE RowNum > 1;
```

**Logic:** ROW_NUMBER partitions by the columns that define a "duplicate." First occurrence gets RowNum = 1, rest get 2, 3... Delete where RowNum > 1 keeps the first and removes the rest. **Very common interview question!**

---

### Q42. Find employees whose salary is the same (find all salary duplicates).

```sql
SELECT e1.EmpName, e1.Salary
FROM Employees e1
WHERE EXISTS (
    SELECT 1 FROM Employees e2
    WHERE e2.Salary = e1.Salary AND e2.EmpID != e1.EmpID
)
ORDER BY e1.Salary;

-- Alternative:
SELECT Salary, STRING_AGG(EmpName, ', ') AS Employees
FROM Employees
GROUP BY Salary
HAVING COUNT(*) > 1;
```

**Logic:** First approach: EXISTS finds employees with at least one other person having the same salary. Second approach: GROUP BY salary, HAVING COUNT > 1, STRING_AGG lists names together.

---

### Q43. Swap gender values ('M' to 'F' and 'F' to 'M') in a single UPDATE.

```sql
UPDATE Patients
SET Gender = CASE 
    WHEN Gender = 'M' THEN 'F'
    WHEN Gender = 'F' THEN 'M'
    ELSE Gender
END;
```

**Logic:** CASE in UPDATE swaps values in one statement. Without CASE, you'd need a temp value to avoid overwriting. This is a classic interview trick question.

---

### Q44. Find consecutive login dates for each user (gap and island problem).

```sql
-- Assuming a Logins table: UserID, LoginDate
WITH Numbered AS (
    SELECT UserID, LoginDate,
           LoginDate - ROW_NUMBER() OVER (PARTITION BY UserID ORDER BY LoginDate) * INTERVAL '1 day' AS GroupID
           -- For SQL Server:
           -- DATEADD(DAY, -ROW_NUMBER() OVER (PARTITION BY UserID ORDER BY LoginDate), LoginDate) AS GroupID
    FROM (SELECT DISTINCT UserID, LoginDate FROM Logins) t
),
Streaks AS (
    SELECT UserID, MIN(LoginDate) AS StreakStart, MAX(LoginDate) AS StreakEnd,
           COUNT(*) AS ConsecutiveDays
    FROM Numbered
    GROUP BY UserID, GroupID
)
SELECT * FROM Streaks WHERE ConsecutiveDays >= 3
ORDER BY UserID, StreakStart;
```

**Logic:** This is the **Gaps and Islands** pattern. Subtract ROW_NUMBER from date — consecutive dates produce the same GroupID. GROUP BY that GroupID finds streaks. Filter for streaks of 3+ days.

---

### Q45. PIVOT: Show monthly sales as columns.

```sql
SELECT *
FROM (
    SELECT MONTH(OrderDate) AS Mn, TotalAmount
    FROM Orders
    WHERE YEAR(OrderDate) = 2026
) AS SourceTable
PIVOT (
    SUM(TotalAmount)
    FOR Mn IN ([1],[2],[3],[4],[5],[6],[7],[8],[9],[10],[11],[12])
) AS PivotTable;
```

**Logic:** PIVOT rotates rows into columns. The source has month numbers as values. PIVOT creates one column per month with SUM of amounts. Great for reporting.

---

### Q46. Find employees who earn the top 10% salaries.

```sql
WITH PercentileRank AS (
    SELECT EmpName, Salary,
           PERCENT_RANK() OVER (ORDER BY Salary) AS PctRank
    FROM Employees
)
SELECT EmpName, Salary, CAST(PctRank * 100 AS DECIMAL(5,2)) AS Percentile
FROM PercentileRank
WHERE PctRank >= 0.90
ORDER BY Salary DESC;
```

**Logic:** PERCENT_RANK gives 0 to 1 value. 0.90+ means top 10%. Alternative: NTILE(10) and filter where group = 10.

---

### Q47. Find the year-over-year growth in revenue.

```sql
WITH YearlyRevenue AS (
    SELECT YEAR(OrderDate) AS Yr, SUM(TotalAmount) AS Revenue
    FROM Orders
    GROUP BY YEAR(OrderDate)
)
SELECT Yr, Revenue,
       LAG(Revenue) OVER (ORDER BY Yr) AS PrevYear,
       Revenue - LAG(Revenue) OVER (ORDER BY Yr) AS Growth,
       CAST((Revenue - LAG(Revenue) OVER (ORDER BY Yr)) * 100.0 
            / NULLIF(LAG(Revenue) OVER (ORDER BY Yr), 0) AS DECIMAL(5,2)) AS GrowthPct
FROM YearlyRevenue
ORDER BY Yr;
```

**Logic:** CTE sums revenue per year. LAG gets previous year. Growth = difference. NULLIF prevents divide-by-zero for the first year.

---

### Q48. Write a recursive CTE to generate numbers 1 to 100.

```sql
WITH Numbers AS (
    SELECT 1 AS Num                 -- Anchor member
    UNION ALL
    SELECT Num + 1 FROM Numbers     -- Recursive member
    WHERE Num < 100                 -- Termination condition
)
SELECT Num FROM Numbers
OPTION (MAXRECURSION 100);
```

**Logic:** Anchor starts at 1. Recursive member adds 1 each time. WHERE Num < 100 stops it. OPTION (MAXRECURSION) prevents infinite loop. Useful for generating sequences, date ranges, and hierarchical data.

---

### Q49. Find the median salary of employees.

```sql
-- Method 1: Using PERCENTILE_CONT (SQL Server 2012+)
SELECT DISTINCT 
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY Salary) OVER () AS MedianSalary
FROM Employees;

-- Method 2: Manual approach
WITH Ordered AS (
    SELECT Salary,
           ROW_NUMBER() OVER (ORDER BY Salary) AS RowAsc,
           COUNT(*) OVER () AS TotalRows
    FROM Employees
)
SELECT AVG(Salary) AS MedianSalary
FROM Ordered
WHERE RowAsc IN (TotalRows / 2, TotalRows / 2 + 1)
   OR (TotalRows % 2 = 1 AND RowAsc = (TotalRows + 1) / 2);
```

**Logic:** PERCENTILE_CONT(0.5) directly gives median. Manual approach: find the middle row(s) — if odd count, it's the center row; if even, average of two center rows. Interviewers love the manual approach.

---

### Q50. Write a query to display a comma-separated list of employees per department.

```sql
-- SQL Server 2017+ (STRING_AGG)
SELECT d.DeptName,
       STRING_AGG(e.EmpName, ', ') WITHIN GROUP (ORDER BY e.EmpName) AS EmployeeList
FROM Departments d
INNER JOIN Employees e ON d.DeptID = e.DeptID
GROUP BY d.DeptName;

-- SQL Server 2016 and below (FOR XML PATH)
SELECT d.DeptName,
       STUFF((
           SELECT ', ' + e.EmpName
           FROM Employees e
           WHERE e.DeptID = d.DeptID
           ORDER BY e.EmpName
           FOR XML PATH(''), TYPE
       ).value('.', 'NVARCHAR(MAX)'), 1, 2, '') AS EmployeeList
FROM Departments d;
```

**Logic:** STRING_AGG is the modern, clean approach. FOR XML PATH is the old method — STUFF removes the leading comma. Both concatenate multiple rows into one string. Very frequently asked!

---

> 📌 **Interview Query Patterns to Remember:**
> - **Nth Salary** → DENSE_RANK() with CTE
> - **Duplicates** → GROUP BY + HAVING COUNT > 1 (find) | ROW_NUMBER + DELETE (remove)
> - **Empty Departments** → LEFT JOIN + WHERE IS NULL
> - **Running Total** → SUM() OVER (PARTITION BY ... ORDER BY ...)
> - **Previous/Next Row** → LAG() / LEAD()
> - **Top N Per Group** → ROW_NUMBER() with PARTITION BY
> - **Percentage** → SUM(CASE) * 100.0 / COUNT(*)
> - **Growth** → LAG() for previous + subtraction
> - **All subjects / All products** → Double NOT EXISTS (relational division)
> - **Gaps & Islands** → ROW_NUMBER subtraction trick
