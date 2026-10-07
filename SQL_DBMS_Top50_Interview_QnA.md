# 1. SQL & DBMS — Interview Q&A
**Questions 1 – 50 | 50 questions**

---

### 1 &emsp; What is the difference between SQL and NoSQL?

**SQL** databases are relational — data is stored in structured tables with rows and columns, and you use SQL language to query them (e.g., MySQL, SQL Server). **NoSQL** databases are non-relational — data is stored as documents, key-value pairs, or graphs (e.g., MongoDB, Redis). SQL enforces a fixed schema and supports ACID transactions, while NoSQL is schema-flexible and designed for horizontal scaling with large, unstructured data.

---

### 2 &emsp; What is a Primary Key?

A **Primary Key** is a column (or combination of columns) that uniquely identifies each row in a table. It cannot contain `NULL` values and must be unique across all rows. Every table should have one primary key. For example, `StudentID` in a Students table — no two students can have the same ID, and it can never be blank.

---

### 3 &emsp; What is the difference between Primary Key and Unique Key?

**Primary Key** uniquely identifies a row — only one per table, no NULLs allowed. **Unique Key** also ensures uniqueness but allows one NULL value, and a table can have multiple unique keys. Think of it this way: Primary Key = your Aadhaar number (one, mandatory), Unique Key = your email or phone (unique but optional).

---

### 4 &emsp; What is a Foreign Key?

A **Foreign Key** is a column in one table that refers to the Primary Key of another table. It creates a relationship between two tables and enforces **referential integrity** — meaning you can't insert a value in the child table that doesn't exist in the parent table. For example, `DeptID` in an Employee table references `DeptID` in the Department table.

---

### 5 &emsp; What are the different types of SQL commands (DDL, DML, DCL, TCL)?

**DDL (Data Definition Language)** — defines structure: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`. **DML (Data Manipulation Language)** — manipulates data: `SELECT`, `INSERT`, `UPDATE`, `DELETE`. **DCL (Data Control Language)** — controls access: `GRANT`, `REVOKE`. **TCL (Transaction Control Language)** — manages transactions: `COMMIT`, `ROLLBACK`, `SAVEPOINT`. DDL is auto-committed; DML needs explicit commit.

---

### 6 &emsp; What is the difference between DELETE, TRUNCATE, and DROP?

**DELETE** removes specific rows using a WHERE clause, can be rolled back, fires triggers. **TRUNCATE** removes all rows at once, cannot use WHERE, faster because it doesn't log individual row deletions, and resets identity. **DROP** removes the entire table structure along with data — the table no longer exists. Delete = erase content selectively, Truncate = wipe clean, Drop = destroy the table.

---

### 7 &emsp; What is Normalization? Explain 1NF, 2NF, 3NF.

**Normalization** is the process of organizing data to reduce redundancy and dependency. **1NF** — every column must have atomic (single) values, no repeating groups. **2NF** — must be in 1NF + no partial dependency (every non-key column must depend on the entire primary key). **3NF** — must be in 2NF + no transitive dependency (non-key columns must not depend on other non-key columns). Each form builds on the previous one.

---

### 8 &emsp; What is Denormalization and when do you use it?

**Denormalization** is the opposite of normalization — you intentionally add redundancy back into tables to improve **read performance**. When you have too many joins slowing down queries, you combine tables or duplicate data. It's commonly used in data warehousing and reporting systems. Tradeoff: faster reads but slower writes and more storage.

---

### 9 &emsp; What is the difference between WHERE and HAVING?

**WHERE** filters rows before grouping — it works on individual rows and cannot use aggregate functions. **HAVING** filters groups after `GROUP BY` — it works on aggregated results and can use functions like `SUM()`, `COUNT()`, `AVG()`. Example: `WHERE salary > 50000` filters employees, `HAVING COUNT(*) > 5` filters departments with more than 5 employees.

---

### 10 &emsp; What are Joins? Explain INNER, LEFT, RIGHT, FULL JOIN.

**Joins** combine rows from two or more tables based on a related column. **INNER JOIN** returns only matching rows from both tables. **LEFT JOIN** returns all rows from the left table + matching rows from right (NULL if no match). **RIGHT JOIN** returns all rows from the right table + matching from left. **FULL JOIN** returns all rows from both tables — NULLs where there's no match. INNER is the most commonly used.

---

### 11 &emsp; What is a Self Join?

A **Self Join** is when a table is joined with itself. You use it when rows in the same table have a relationship with each other. For example, an Employee table where each employee has a `ManagerID` that refers to another `EmployeeID` in the same table. You alias the table twice: `SELECT e.Name, m.Name AS Manager FROM Employee e JOIN Employee m ON e.ManagerID = m.EmployeeID`.

---

### 12 &emsp; What is a Cross Join?

A **Cross Join** returns the Cartesian product of two tables — every row from the first table is combined with every row from the second table. If Table A has 5 rows and Table B has 3 rows, the result has 15 rows. No `ON` condition is needed. It's rarely used in practice but useful for generating all possible combinations (e.g., all sizes × all colors).

---

### 13 &emsp; What is the difference between UNION and UNION ALL?

**UNION** combines results from two SELECT queries and **removes duplicate** rows. **UNION ALL** combines results but **keeps all rows** including duplicates. UNION ALL is faster because it doesn't need to sort and compare for duplicates. Use UNION when you need distinct results; use UNION ALL when you know there are no duplicates or you want all rows.

---

### 14 &emsp; What is a Subquery? What are its types?

A **Subquery** is a query nested inside another query (also called inner query). It executes first, and its result is used by the outer query. Types: **Single-row subquery** — returns one value (`WHERE salary = (SELECT MAX(salary) FROM emp)`). **Multi-row subquery** — returns multiple values (use `IN`, `ANY`, `ALL`). **Correlated subquery** — references the outer query's column, re-executes for each outer row.

---

### 15 &emsp; What is the difference between Subquery and JOIN?

A **Subquery** is a query inside a query — good for filtering and simple lookups. A **JOIN** combines columns from multiple tables side by side — better for retrieving related data. JOINs are generally faster for large datasets because the optimizer handles them better. Subqueries are easier to read for simple conditions. Use JOIN when you need columns from both tables; use subquery when you need a value for comparison.

---

### 16 &emsp; What is an Index? Why is it used?

An **Index** is a data structure (like a book's index) that speeds up data retrieval on a table. Without an index, the database scans every row (full table scan). With an index, it goes directly to the matching row. **Clustered Index** — sorts and stores the data physically (only one per table, like a dictionary). **Non-Clustered Index** — creates a separate structure pointing to data (multiple allowed, like a book index).

---

### 17 &emsp; What is the difference between Clustered and Non-Clustered Index?

**Clustered Index** physically reorders the table data — only **one** per table. The table itself becomes the index (like pages in a dictionary are sorted by word). **Non-Clustered Index** creates a **separate** lookup structure with pointers — you can have **multiple** per table (like an index page at the back of a book). Clustered is faster for range queries; Non-Clustered is better for frequent lookups on different columns.

---

### 18 &emsp; What is a View? Why do we use it?

A **View** is a virtual table based on a SELECT query. It doesn't store data physically — every time you query a view, the underlying SQL runs. Used for **security** (hide sensitive columns), **simplicity** (simplify complex joins), and **reusability** (write once, use everywhere). Example: `CREATE VIEW ActiveEmployees AS SELECT * FROM Employees WHERE IsActive = 1`.

---

### 19 &emsp; What is the difference between a View and a Table?

A **Table** stores data physically on disk — it has actual rows and columns. A **View** is a saved SELECT query — it doesn't store data, just the query definition. When you query a view, the database executes the underlying query each time. Tables can have indexes and constraints directly; views can have indexes only in some databases (indexed/materialized views).

---

### 20 &emsp; What is a Stored Procedure?

A **Stored Procedure** is a precompiled collection of SQL statements saved in the database that you can execute by calling its name. Benefits: **reusability** (write once, call many times), **performance** (precompiled execution plan), **security** (users call the procedure instead of writing raw SQL). It can accept input/output parameters. Example: `CREATE PROCEDURE GetEmployeeByDept @DeptID INT AS SELECT * FROM Employees WHERE DeptID = @DeptID`.

---

### 21 &emsp; What is the difference between a Stored Procedure and a Function?

A **Stored Procedure** can perform actions (INSERT, UPDATE, DELETE), may or may not return a value, and is called using `EXEC`. A **Function** must return a value, can be used inside SELECT/WHERE statements, and cannot modify data (no DML in most databases). Functions are used for calculations and transformations; Procedures are used for business logic and data manipulation.

---

### 22 &emsp; What is a Trigger?

A **Trigger** is a special stored procedure that automatically executes when a specific event (INSERT, UPDATE, DELETE) happens on a table. You don't call it manually — it fires automatically. Types: **BEFORE trigger** (runs before the event), **AFTER trigger** (runs after the event), **INSTEAD OF trigger** (replaces the event). Used for auditing, validation, and maintaining data integrity.

---

### 23 &emsp; What is a Cursor? When do you use it?

A **Cursor** is a database object that lets you process query results **row by row** instead of as a set. Steps: DECLARE → OPEN → FETCH → process → CLOSE → DEALLOCATE. Use cursors when you need row-level processing that can't be done with set-based operations. **Avoid cursors when possible** — they are slow because SQL is designed for set-based operations, not row-by-row processing.

---

### 24 &emsp; What are ACID properties in a transaction?

**ACID** ensures reliable database transactions. **Atomicity** — all operations succeed or all fail (no partial). **Consistency** — database moves from one valid state to another. **Isolation** — concurrent transactions don't interfere with each other. **Durability** — once committed, data is permanently saved even if system crashes. Example: bank transfer — debit and credit must both happen, or neither.

---

### 25 &emsp; What is a Transaction? What are COMMIT and ROLLBACK?

A **Transaction** is a unit of work that contains one or more SQL statements executed as a single logical operation. **COMMIT** permanently saves all changes made during the transaction. **ROLLBACK** undoes all changes if something goes wrong. **SAVEPOINT** creates a checkpoint within a transaction so you can rollback to that specific point instead of undoing everything.

---

### 26 &emsp; What is the difference between CHAR and VARCHAR?

**CHAR** is fixed-length — `CHAR(10)` always uses 10 bytes, even if you store 'Hi' (pads with spaces). **VARCHAR** is variable-length — `VARCHAR(10)` uses only the bytes needed plus 2 bytes overhead, so 'Hi' uses 4 bytes. Use CHAR for fixed-size data like state codes, gender codes. Use VARCHAR for variable-size data like names, emails. CHAR is slightly faster; VARCHAR saves storage.

---

### 27 &emsp; What is the difference between VARCHAR and NVARCHAR?

**VARCHAR** stores **ASCII** characters — 1 byte per character. Supports English and basic characters only. **NVARCHAR** stores **Unicode** characters — 2 bytes per character. Supports **all languages** including Hindi, Chinese, Arabic, emojis. `VARCHAR(50)` = max 50 bytes. `NVARCHAR(50)` = max 100 bytes. Use NVARCHAR when your application needs to store multi-language data. The 'N' stands for National/Unicode.

**Logical Breakdown:**
- If your app is English-only → VARCHAR is enough, saves space
- If your app stores names in Hindi/Gujarati/Chinese → you MUST use NVARCHAR
- VARCHAR('Hello') = 5 bytes, NVARCHAR('Hello') = 10 bytes — double the storage
- In interviews, always say: "NVARCHAR supports international characters because it uses Unicode encoding (UTF-16)"

---

### 28 &emsp; What is the difference between COALESCE and ISNULL?

**ISNULL** takes exactly 2 arguments — if first is NULL, returns second. It's SQL Server specific. **COALESCE** takes multiple arguments — returns the first non-NULL value from the list. It's ANSI SQL standard (works across databases). COALESCE is more flexible: `COALESCE(Phone, Mobile, Email, 'No Contact')` checks each value in order. ISNULL is slightly faster in SQL Server for simple cases.

---

### 29 &emsp; What are Aggregate Functions? Name the main ones.

**Aggregate Functions** perform calculations on a set of values and return a single result. Main functions: **COUNT()** — number of rows, **SUM()** — total of numeric values, **AVG()** — average value, **MIN()** — smallest value, **MAX()** — largest value. They are used with `GROUP BY` to get results per group. NULLs are ignored by all aggregate functions except `COUNT(*)`.

---

### 30 &emsp; What is GROUP BY and how does it work?

**GROUP BY** groups rows that have the same values into summary rows. You use it with aggregate functions to get per-group results. Example: `SELECT DeptID, COUNT(*) FROM Employees GROUP BY DeptID` gives employee count per department. Rules: every column in SELECT must either be in GROUP BY or inside an aggregate function. The query execution order is: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY.

---

### 31 &emsp; What is the difference between IN and EXISTS?

**IN** compares a value against a list of values returned by a subquery — good for small result sets. **EXISTS** checks if the subquery returns any rows (TRUE/FALSE) — doesn't care about actual values. EXISTS is faster for large subqueries because it stops as soon as it finds the first match. Use IN for small lists; use EXISTS for correlated subqueries and large datasets.

---

### 32 &emsp; What is a CTE (Common Table Expression)?

A **CTE** is a temporary named result set defined using `WITH` keyword that exists only during the execution of a single query. It makes complex queries more readable. Example: `WITH TopEmp AS (SELECT * FROM Employees WHERE Salary > 50000) SELECT * FROM TopEmp`. CTEs can be recursive (for hierarchical data like org charts). Unlike temp tables, CTEs don't store data — they're just query references.

---

### 33 &emsp; What is the difference between a Temp Table and a Table Variable?

**Temp Table** (`#temp`) — stored in tempdb, can have indexes, visible in the entire session or procedure, supports statistics, good for large datasets. **Table Variable** (`@table`) — stored in memory (small data), no statistics, no explicit indexes, scoped to the batch/procedure only. Temp tables are better for large data; table variables are better for small, quick operations.

---

### 34 &emsp; What is the difference between RANK(), DENSE_RANK(), and ROW_NUMBER()?

All three are **window functions**. **ROW_NUMBER()** assigns a unique sequential number to each row — no ties. **RANK()** gives the same rank to ties but skips the next number (1, 2, 2, 4). **DENSE_RANK()** gives the same rank to ties but doesn't skip (1, 2, 2, 3). Example: if two students score 90, ROW_NUMBER gives 1, 2; RANK gives 1, 1, 3; DENSE_RANK gives 1, 1, 2.

---

### 35 &emsp; What are Window Functions?

**Window Functions** perform calculations across a set of rows related to the current row — without collapsing them like GROUP BY does. They use `OVER()` clause. Types: **Ranking** (ROW_NUMBER, RANK, DENSE_RANK), **Aggregate** (SUM, AVG, COUNT with OVER), **Value** (LAG, LEAD, FIRST_VALUE, LAST_VALUE). Example: `SELECT Name, Salary, SUM(Salary) OVER (PARTITION BY DeptID) AS DeptTotal FROM Employees`.

---

### 36 &emsp; What is the difference between CAST and CONVERT?

Both are used for **data type conversion**. **CAST** is ANSI SQL standard — simple syntax: `CAST(value AS datatype)`. **CONVERT** is SQL Server specific — has an optional style parameter for formatting: `CONVERT(datatype, value, style)`. Use CONVERT when you need date/number formatting (e.g., `CONVERT(VARCHAR, GETDATE(), 103)` for dd/mm/yyyy). For simple conversions, prefer CAST for portability.

---

### 37 &emsp; What is a Deadlock? How do you prevent it?

A **Deadlock** occurs when two transactions are waiting for each other to release locks — neither can proceed, so both are stuck forever. The database detects this and kills one transaction (the victim). Prevention: access tables in the **same order** in all transactions, keep transactions **short**, use **proper indexing** to reduce lock duration, and avoid user interaction during transactions.

---

### 38 &emsp; What are the different types of Locks in SQL Server?

**Shared Lock (S)** — used during SELECT, allows other reads but blocks writes. **Exclusive Lock (X)** — used during INSERT/UPDATE/DELETE, blocks all other access. **Update Lock (U)** — prevents deadlocks during updates, converts to exclusive when modifying. **Intent Locks** — indicate the intention to acquire locks at a lower level. **Schema Lock** — protects table structure during DDL operations. Lock granularity: Row → Page → Table → Database.

---

### 39 &emsp; What is the difference between Optimistic and Pessimistic Concurrency?

**Pessimistic Concurrency** — locks the resource when a user reads it, no one else can modify it until the lock is released. Safe but reduces performance. **Optimistic Concurrency** — doesn't lock during read; checks at update time if data was changed by someone else. If changed, the update fails and retries. Better for high-read, low-conflict scenarios. SQL Server uses row versioning for optimistic concurrency.

---

### 40 &emsp; What is Query Execution Plan? Why is it important?

A **Query Execution Plan** shows the step-by-step process the database engine uses to execute your query — which indexes it uses, how it joins tables, estimated vs actual row counts. It's crucial for **performance tuning**. Look for: table scans (bad), index seeks (good), high-cost operators, and missing index suggestions. Use `SET SHOWPLAN_ALL ON` or click "Include Actual Execution Plan" in SSMS.

---

### 41 &emsp; What is the difference between Clustered and Non-Clustered Index in terms of performance?

**Clustered Index** is best for **range queries** (`BETWEEN`, `>`, `<`) and columns used in `ORDER BY` because data is physically sorted. **Non-Clustered Index** is best for **exact lookups** (`WHERE email = 'x'`) and columns frequently used in WHERE/JOIN conditions. Too many non-clustered indexes slow down INSERT/UPDATE because each index must be updated. A table without a clustered index is called a **HEAP**.

---

### 42 &emsp; What is SQL Injection? How do you prevent it?

**SQL Injection** is a security attack where malicious SQL code is inserted through user input to manipulate the database. Example: entering `' OR 1=1 --` in a login form bypasses authentication. Prevention: use **parameterized queries** / prepared statements (never concatenate user input), use **stored procedures**, validate and sanitize input, implement **least privilege** access, and use ORM frameworks that handle parameterization.

---

### 43 &emsp; What is the difference between OLTP and OLAP?

**OLTP (Online Transaction Processing)** — handles day-to-day transactions (INSERT, UPDATE, DELETE). Optimized for fast writes, normalized tables, many concurrent users. Example: banking system, e-commerce orders. **OLAP (Online Analytical Processing)** — handles complex analytical queries (aggregations, reporting). Optimized for fast reads, denormalized tables, fewer users. Example: data warehouse, business intelligence dashboards.

---

### 44 &emsp; What are Constraints in SQL? Name the different types.

**Constraints** are rules enforced on table columns to maintain data integrity. Types: **NOT NULL** — column cannot be empty. **UNIQUE** — all values must be different. **PRIMARY KEY** — unique + not null identifier. **FOREIGN KEY** — references another table's primary key. **CHECK** — validates value against a condition (`CHECK (Age >= 18)`). **DEFAULT** — assigns a default value if none is provided.

---

### 45 &emsp; What is the difference between SCOPE_IDENTITY(), @@IDENTITY, and IDENT_CURRENT()?

**SCOPE_IDENTITY()** — returns the last identity value in the current scope and session (safest, most commonly used). **@@IDENTITY** — returns the last identity value in the current session but across all scopes (can return wrong value if a trigger inserts into another table). **IDENT_CURRENT('TableName')** — returns the last identity for a specific table regardless of session or scope. Always prefer SCOPE_IDENTITY().

---

### 46 &emsp; What is the difference between a Materialized View and a Regular View?

A **Regular View** is a virtual table — the query runs every time you access it, no data is stored. A **Materialized View** (called Indexed View in SQL Server) physically stores the query result and updates it periodically. Materialized views are faster for complex aggregations but need storage and maintenance. Use regular views for simplicity; use materialized views when the same expensive query is run frequently.

---

### 47 &emsp; What is a Schema in SQL?

A **Schema** is a logical container/namespace that groups database objects like tables, views, procedures, and functions. It helps organize objects and manage security. Default schema in SQL Server is `dbo`. Example: `dbo.Employees`, `HR.Employees`, `Sales.Orders`. You can grant permissions at the schema level instead of individual objects. Think of it like a folder that organizes your tables.

---

### 48 &emsp; What is the difference between EXCEPT and NOT IN?

**EXCEPT** returns rows from the first query that don't exist in the second query — compares entire rows and handles NULLs correctly. **NOT IN** checks if a column value is not in a list — but if the subquery returns any NULL, the entire result becomes empty (common bug!). EXCEPT is safer and more readable. Example: `SELECT Name FROM A EXCEPT SELECT Name FROM B` gives names in A but not in B.

---

### 49 &emsp; What is Referential Integrity?

**Referential Integrity** ensures that relationships between tables remain consistent. It means a foreign key value must either match a primary key value in the parent table or be NULL. It prevents **orphan records** — you can't insert a child record with a non-existing parent, and you can't delete a parent if children exist (unless CASCADE is set). It's enforced using FOREIGN KEY constraints.

---

### 50 &emsp; What is the difference between BETWEEN and IN?

**BETWEEN** selects values within a **range** (inclusive of both boundaries). Example: `WHERE Salary BETWEEN 30000 AND 60000` gets salaries from 30K to 60K. **IN** selects values that match any value in a **specific list**. Example: `WHERE DeptID IN (1, 3, 5)` gets employees in departments 1, 3, or 5. BETWEEN is for continuous ranges; IN is for discrete values. BETWEEN works with numbers, dates, and strings.

---

> 📌 **Quick Revision Tips:**
> - Primary Key = unique + not null + one per table
> - Foreign Key = references another table's PK
> - DELETE logs rows, TRUNCATE doesn't, DROP removes table
> - VARCHAR = ASCII (1 byte), NVARCHAR = Unicode (2 bytes)
> - INNER JOIN = only matches, LEFT JOIN = all left + matches
> - Clustered Index = dictionary, Non-Clustered = book index
> - ACID = Atomicity, Consistency, Isolation, Durability
> - WHERE filters rows, HAVING filters groups
> - UNION removes duplicates, UNION ALL keeps all
> - ROW_NUMBER = unique, RANK = skip, DENSE_RANK = no skip
