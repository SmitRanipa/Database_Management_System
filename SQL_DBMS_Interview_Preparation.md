# 🚀 SQL & DBMS Complete Interview Preparation Guide

> **Covers:** DBMS-I (SQL Fundamentals, DBMS Concepts, E-R Model, Normalization, Views & Joins)
> and DBMS-II (Functions, Triggers, Transactions, Indexes, Query Optimization, NoSQL & MongoDB)
>
> **Format:** Topic → Description → Query/Example → Tables for Functions/Methods

---

# 📘 PART 1 — DBMS-I

---

# Unit 1: SQL Fundamentals

---

## 1.1 🔹 What is DBMS & Why Do We Need It?

### Description

A **Database** is an organized collection of related data. A **Database Management System (DBMS)** is software that allows you to create, manage, and manipulate databases efficiently.

Before DBMS, data was managed using **paper-based systems** (manual files) and **file-based systems** (flat files on computers). Both had serious problems — data redundancy, inconsistency, no security, and difficulty in searching.

DBMS solves all of these problems by providing a centralized, structured, and secure way to handle data.

Every application has two parts:
- **Front-End** — What the user sees (UI/UX) — built with HTML, React, Angular, etc.
- **Back-End** — Where data lives — managed by DBMS like SQL Server, MySQL, Oracle, PostgreSQL, MongoDB

### Popular DBMS Tools

| DBMS | Type | Used By |
|------|------|---------|
| Microsoft SQL Server | RDBMS | Enterprise, .NET apps |
| MySQL | RDBMS | Web apps, WordPress, PHP |
| PostgreSQL | RDBMS | Complex queries, open source |
| Oracle | RDBMS | Banking, large enterprise |
| MongoDB | NoSQL | Real-time, document-based apps |
| SQLite | RDBMS | Mobile apps, embedded systems |

---

## 1.2 🔹 What is SQL?

### Description

**SQL (Structured Query Language)** is the standard language used to communicate with relational databases. You use SQL to create, read, update, and delete data (CRUD operations).

Key points:
- SQL is **case-insensitive** — `SELECT`, `select`, `Select` all work the same
- SQL follows the **ANSI/ISO standard**, but each DBMS may have slight variations
- SQL works with **RDBMS (Relational Database Management System)** — data is stored in tables with rows and columns, and tables can be related to each other

---

## 1.3 🔹 Database Concepts — Core Terminology

| Term | Description | Example |
|------|-------------|---------|
| **Database** | A container that holds all your tables and data | `SchoolDB`, `HospitalDB` |
| **Table** | A structured collection of data organized in rows and columns | `Students`, `Employees` |
| **Column / Attribute** | A vertical field representing a specific property | `Name`, `Age`, `Salary` |
| **Row / Record / Tuple** | A horizontal entry representing one complete data item | One student's full details |

---

## 1.4 🔹 Data Types in SQL

Data types define what kind of value a column can hold. Choosing the right data type ensures data integrity and optimizes storage.

### Numeric Data Types

| Data Type | Description | Example |
|-----------|-------------|---------|
| `INT` | Whole numbers (no decimals) | `Age INT` → 25 |
| `DECIMAL(p,s)` | Exact decimal numbers. p = total digits, s = decimal places | `Price DECIMAL(10,2)` → 99999999.99 |
| `FLOAT` | Approximate decimal numbers | `Percentage FLOAT` → 85.67 |
| `BIT` | Stores 0 or 1 (used for True/False) | `IsActive BIT` → 1 |

### String Data Types

| Data Type | Description | Example |
|-----------|-------------|---------|
| `CHAR(n)` | Fixed-length string. Always uses n bytes even if data is shorter | `Gender CHAR(1)` → 'M' |
| `VARCHAR(n)` | Variable-length string. Uses only the bytes needed + 2 | `Name VARCHAR(50)` → 'Raj' |

> **CHAR vs VARCHAR:** Use CHAR when length is always the same (like Gender 'M'/'F'). Use VARCHAR when length varies (like Name). VARCHAR saves storage space.

### Date & Time Data Types

| Data Type | Description | Example |
|-----------|-------------|---------|
| `DATE` | Only date (no time) | `2025-10-05` |
| `TIME` | Only time (no date) | `14:30:00` |
| `DATETIME` | Both date and time | `2025-10-05 14:30:00` |

### Binary Data Types

| Data Type | Description |
|-----------|-------------|
| `BINARY(n)` | Fixed-length binary data |
| `VARBINARY(n)` | Variable-length binary data |
| `IMAGE` | Used for storing images/large binary (deprecated — use VARBINARY(MAX)) |

> **NULL vs Empty:** `NULL` means "no value / unknown." An empty string `''` is a value (just empty). `NULL != ''` — they are different.

---

## 1.5 🔹 SQL Command Categories (DDL, DML, DQL, DCL, TCL)

SQL commands are grouped into 5 categories based on their purpose:

| Category | Full Form | Purpose | Commands |
|----------|-----------|---------|----------|
| **DDL** | Data Definition Language | Define/modify database structure | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` |
| **DML** | Data Manipulation Language | Manipulate data inside tables | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | Retrieve/query data | `SELECT` |
| **DCL** | Data Control Language | Control access/permissions | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Manage transactions | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

---

## 1.6 🔹 DDL Commands — CREATE, ALTER, DROP, TRUNCATE

### CREATE — Create Database and Tables

```sql
-- Create a database
CREATE DATABASE SchoolDB;
USE SchoolDB;

-- Create a table
CREATE TABLE Students (
    StudentId   INT PRIMARY KEY,
    Name        VARCHAR(50) NOT NULL,
    Age         INT CHECK (Age >= 5),
    Email       VARCHAR(100) UNIQUE,
    City        VARCHAR(30) DEFAULT 'Unknown',
    EnrollDate  DATE
);
```

### ALTER — Modify Table Structure

```sql
-- Add a new column
ALTER TABLE Students ADD Phone VARCHAR(15);

-- Drop (remove) a column
ALTER TABLE Students DROP COLUMN Phone;

-- Modify a column's data type
ALTER TABLE Students ALTER COLUMN City VARCHAR(50);
```

### DROP — Delete Database or Table Permanently

```sql
DROP TABLE Students;     -- Deletes table + data + structure
DROP DATABASE SchoolDB;  -- Deletes entire database
```

### TRUNCATE — Remove All Rows But Keep Structure

```sql
TRUNCATE TABLE Students;  -- Deletes all data, keeps table structure
```

### RENAME — Rename a Table

```sql
EXEC sp_rename 'Students', 'Learners';  -- SQL Server syntax
```

### DELETE vs TRUNCATE vs DROP

| Feature | DELETE | TRUNCATE | DROP |
|---------|--------|----------|------|
| **What it removes** | Specific rows (with WHERE) or all rows | All rows | Entire table (structure + data) |
| **WHERE clause** | ✅ Supported | ❌ Not supported | ❌ Not applicable |
| **Rollback** | ✅ Can rollback | ⚠️ Limited rollback | ❌ Cannot rollback |
| **Speed** | Slow (row by row) | Fast (deallocates pages) | Instant |
| **Triggers** | ✅ Fires triggers | ❌ Does not fire | ❌ Not applicable |
| **Identity reset** | ❌ No reset | ✅ Resets identity | N/A |

---

## 1.7 🔹 DML Commands — INSERT, UPDATE, DELETE

### INSERT — Add New Rows

```sql
-- Insert with column names (recommended)
INSERT INTO Students (StudentId, Name, Age, City)
VALUES (1, 'Raj', 20, 'Ahmedabad');

-- Insert without column names (all columns must be provided in order)
INSERT INTO Students
VALUES (2, 'Priya', 22, 'priya@mail.com', 'Mumbai', '2025-01-15');

-- Insert multiple rows
INSERT INTO Students (StudentId, Name, Age, City)
VALUES (3, 'Amit', 21, 'Delhi'),
       (4, 'Sara', 23, 'Pune');
```

### UPDATE — Modify Existing Data

```sql
-- Update specific row
UPDATE Students SET City = 'Surat' WHERE StudentId = 1;

-- Update multiple columns
UPDATE Students SET Age = 25, City = 'Jaipur' WHERE Name = 'Amit';

-- ⚠️ Without WHERE — updates ALL rows!
UPDATE Students SET City = 'Unknown';
```

### DELETE — Remove Rows

```sql
-- Delete specific row
DELETE FROM Students WHERE StudentId = 4;

-- Delete all rows (but keep table)
DELETE FROM Students;
```

---

## 1.8 🔹 DQL Command — SELECT

```sql
-- Select all columns
SELECT * FROM Students;

-- Select specific columns
SELECT Name, Age FROM Students;

-- Select with condition
SELECT * FROM Students WHERE City = 'Ahmedabad';

-- Select top N rows
SELECT TOP 5 * FROM Students;

-- Select top N percent
SELECT TOP 50 PERCENT * FROM Students;

-- Select distinct (unique) values
SELECT DISTINCT City FROM Students;
```

### SELECT INTO — Copy Data into a New Table

```sql
-- Create a new table and copy data
SELECT * INTO StudentBackup FROM Students;

-- Copy specific columns with condition
SELECT Name, Age INTO YoungStudents FROM Students WHERE Age < 21;
```

---

## 1.9 🔹 SQL Operators (WHERE Clause Filters)

| Operator | Description | Example |
|----------|-------------|---------|
| `AND` | Both conditions must be true | `WHERE Age > 18 AND City = 'Mumbai'` |
| `OR` | At least one condition must be true | `WHERE City = 'Delhi' OR City = 'Pune'` |
| `NOT` | Negates a condition | `WHERE NOT City = 'Mumbai'` |
| `IN` | Matches any value in a list | `WHERE City IN ('Mumbai', 'Delhi', 'Pune')` |
| `NOT IN` | Does not match any value in a list | `WHERE City NOT IN ('Mumbai')` |
| `BETWEEN` | Within a range (inclusive) | `WHERE Age BETWEEN 18 AND 25` |
| `NOT BETWEEN` | Outside a range | `WHERE Age NOT BETWEEN 18 AND 25` |

### LIKE Predicate — Pattern Matching

| Pattern | Description | Example |
|---------|-------------|---------|
| `%` | Zero or more characters | `WHERE Name LIKE 'R%'` → Raj, Rahul, Rohit |
| `_` | Exactly one character | `WHERE Name LIKE '_aj'` → Raj, Taj |
| `%at%` | Contains 'at' anywhere | → Atul, Prateek, Flat |
| `A%` | Starts with 'A' | → Amit, Atul |
| `%a` | Ends with 'a' | → Priya, Sara |

---

## 1.10 🔹 Aggregate Functions

Aggregate functions perform calculations on a set of values and return a **single result**.

| Function | Description | Example |
|----------|-------------|---------|
| `COUNT(*)` | Number of rows | `SELECT COUNT(*) FROM Students` |
| `COUNT(column)` | Number of non-NULL values | `SELECT COUNT(Email) FROM Students` |
| `SUM(column)` | Total of numeric values | `SELECT SUM(Salary) FROM Employees` |
| `AVG(column)` | Average of numeric values | `SELECT AVG(Age) FROM Students` |
| `MIN(column)` | Smallest value | `SELECT MIN(Salary) FROM Employees` |
| `MAX(column)` | Largest value | `SELECT MAX(Salary) FROM Employees` |

> **Note:** Aggregate functions **ignore NULL values** (except `COUNT(*)`). Use `DISTINCT` inside to count unique values: `COUNT(DISTINCT City)`.

---

## 1.11 🔹 GROUP BY & HAVING Clause

### GROUP BY — Group Rows That Share a Value

```sql
-- Count students per city
SELECT City, COUNT(*) AS StudentCount
FROM Students
GROUP BY City;

-- Average salary per department
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department;
```

### HAVING — Filter Groups (WHERE for groups)

`WHERE` filters individual rows **before** grouping. `HAVING` filters groups **after** grouping.

```sql
-- Cities with more than 5 students
SELECT City, COUNT(*) AS Total
FROM Students
GROUP BY City
HAVING COUNT(*) > 5;

-- Departments with average salary above 50000
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > 50000
ORDER BY AvgSalary DESC;
```

### ORDER BY — Sort Results

```sql
SELECT * FROM Students ORDER BY Name ASC;          -- Ascending (default)
SELECT * FROM Students ORDER BY Age DESC;           -- Descending
SELECT * FROM Students ORDER BY City ASC, Age DESC; -- Multiple columns
```

### Complete Query Execution Order

```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → TOP
```

---

## 1.12 🔹 SQL Built-in Functions

### Date & Time Functions

| Function | Description | Example | Result |
|----------|-------------|---------|--------|
| `GETDATE()` | Current date & time | `SELECT GETDATE()` | `2025-10-05 14:30:45` |
| `DAY(date)` | Extracts day | `SELECT DAY('2025-10-05')` | `5` |
| `MONTH(date)` | Extracts month | `SELECT MONTH('2025-10-05')` | `10` |
| `YEAR(date)` | Extracts year | `SELECT YEAR('2025-10-05')` | `2025` |
| `DATEPART(part, date)` | Extracts specific part | `SELECT DATEPART(WEEKDAY, GETDATE())` | `1-7` |
| `DATENAME(part, date)` | Part name as text | `SELECT DATENAME(MONTH, GETDATE())` | `October` |
| `EOMONTH(date)` | Last day of month | `SELECT EOMONTH('2025-10-05')` | `2025-10-31` |
| `DATEADD(part, n, date)` | Add n to date | `SELECT DATEADD(DAY, 10, GETDATE())` | Date + 10 days |
| `DATEDIFF(part, start, end)` | Difference between dates | `SELECT DATEDIFF(YEAR, '2000-01-01', GETDATE())` | `25` |
| `ISDATE(value)` | Checks if valid date | `SELECT ISDATE('2025-13-01')` | `0` (false) |

### String Functions

| Function | Description | Example | Result |
|----------|-------------|---------|--------|
| `LEN(str)` | Length of string | `SELECT LEN('Hello')` | `5` |
| `UPPER(str)` | Converts to uppercase | `SELECT UPPER('hello')` | `HELLO` |
| `LOWER(str)` | Converts to lowercase | `SELECT LOWER('HELLO')` | `hello` |
| `LTRIM(str)` | Removes left spaces | `SELECT LTRIM('  Hi')` | `Hi` |
| `RTRIM(str)` | Removes right spaces | `SELECT RTRIM('Hi  ')` | `Hi` |
| `TRIM(str)` | Removes both sides | `SELECT TRIM('  Hi  ')` | `Hi` |
| `SUBSTRING(str, start, len)` | Extract part | `SELECT SUBSTRING('Hello', 1, 3)` | `Hel` |
| `LEFT(str, n)` | First n characters | `SELECT LEFT('Hello', 2)` | `He` |
| `RIGHT(str, n)` | Last n characters | `SELECT RIGHT('Hello', 3)` | `llo` |
| `REPLACE(str, old, new)` | Replace text | `SELECT REPLACE('Hello', 'l', 'r')` | `Herro` |
| `REVERSE(str)` | Reverse string | `SELECT REVERSE('Hello')` | `olleH` |
| `CHARINDEX(find, str)` | Find position | `SELECT CHARINDEX('l', 'Hello')` | `3` |
| `CONCAT(a, b)` | Join strings | `SELECT CONCAT('Hi', ' ', 'there')` | `Hi there` |
| `REPLICATE(str, n)` | Repeat string | `SELECT REPLICATE('Ha', 3)` | `HaHaHa` |

### Mathematical Functions

| Function | Description | Example | Result |
|----------|-------------|---------|--------|
| `ABS(n)` | Absolute value | `SELECT ABS(-15)` | `15` |
| `CEILING(n)` | Round up | `SELECT CEILING(4.2)` | `5` |
| `FLOOR(n)` | Round down | `SELECT FLOOR(4.9)` | `4` |
| `ROUND(n, d)` | Round to d decimal places | `SELECT ROUND(4.567, 2)` | `4.570` |
| `POWER(n, p)` | n raised to power p | `SELECT POWER(2, 3)` | `8` |
| `SQRT(n)` | Square root | `SELECT SQRT(25)` | `5` |
| `SQUARE(n)` | Square | `SELECT SQUARE(5)` | `25` |

### Conversion Functions

| Function | Description | Example |
|----------|-------------|---------|
| `CAST(value AS type)` | Converts data type (ANSI standard) | `SELECT CAST(25.99 AS INT)` → `25` |
| `CONVERT(type, value)` | Converts data type (SQL Server specific) | `SELECT CONVERT(VARCHAR, GETDATE(), 103)` → `05/10/2025` |

---

## 1.13 🔹 Keys in SQL

A **Key** is a column (or set of columns) used to uniquely identify a row in a table and establish relationships between tables.

| Key Type | Description |
|----------|-------------|
| **Super Key** | Any combination of columns that uniquely identifies a row. A table can have many super keys. |
| **Candidate Key** | A minimal super key — no unnecessary columns. There can be multiple candidates. |
| **Primary Key** | The ONE candidate key chosen to uniquely identify rows. Cannot be NULL, must be unique. One per table. |
| **Alternate Key** | Candidate keys that were NOT chosen as the primary key. |
| **Unique Key** | Like primary key but allows ONE NULL value. A table can have multiple unique keys. |
| **Composite Key** | A key made up of TWO or more columns combined. |
| **Foreign Key** | A column in one table that references the Primary Key of another table. Creates a relationship. |

### Primary Key vs Unique Key

| Feature | Primary Key | Unique Key |
|---------|-------------|------------|
| NULL allowed | ❌ No | ✅ One NULL |
| Per table | Only 1 | Multiple |
| Clustered Index | ✅ Auto-created | ❌ Non-clustered |

### Foreign Key — Parent & Child Relationship

```sql
-- Parent Table
CREATE TABLE Departments (
    DeptId   INT PRIMARY KEY,
    DeptName VARCHAR(50)
);

-- Child Table — DeptId references parent
CREATE TABLE Employees (
    EmpId    INT PRIMARY KEY,
    Name     VARCHAR(50),
    DeptId   INT,
    FOREIGN KEY (DeptId) REFERENCES Departments(DeptId)
);
```

> The table with the **Primary Key** is the **Parent Table**. The table with the **Foreign Key** is the **Child Table**. You must insert into the parent first, and delete from the child first.

---

## 1.14 🔹 Constraints in SQL

Constraints are rules applied to columns to ensure data integrity — they restrict what data can be inserted.

| Constraint | Description | Example |
|-----------|-------------|---------|
| `NOT NULL` | Column cannot have NULL values | `Name VARCHAR(50) NOT NULL` |
| `UNIQUE` | All values must be different (allows 1 NULL) | `Email VARCHAR(100) UNIQUE` |
| `PRIMARY KEY` | Uniquely identifies each row (NOT NULL + UNIQUE) | `Id INT PRIMARY KEY` |
| `FOREIGN KEY` | Links to another table's primary key | `FOREIGN KEY (DeptId) REFERENCES Depts(Id)` |
| `CHECK` | Validates data against a condition | `Age INT CHECK (Age >= 18)` |
| `DEFAULT` | Provides a default value if none is given | `City VARCHAR(30) DEFAULT 'Unknown'` |

```sql
CREATE TABLE Products (
    ProductId   INT PRIMARY KEY,
    Name        VARCHAR(100) NOT NULL,
    Price       DECIMAL(10,2) CHECK (Price > 0),
    Category    VARCHAR(50) DEFAULT 'General',
    SKU         VARCHAR(20) UNIQUE
);
```

---

# Unit 2: Introduction to DBMS (Theory)

---

## 2.1 🔹 Core DBMS Terminology

| Term | Description |
|------|-------------|
| **Data** | Raw, unprocessed facts — like numbers, text, dates |
| **Information** | Processed and meaningful data — data with context |
| **Metadata** | "Data about data" — describes structure of data (column names, types, sizes) |
| **Data Dictionary** | A centralized repository of metadata — holds info about all tables, columns, constraints |
| **Data Warehouse** | A large storage system for historical data used for analysis and reporting |

> **Data vs Information:** "25" is data. "Raj's age is 25" is information. Data becomes information when given meaning and context.

---

## 2.2 🔹 Advantages & Disadvantages of DBMS

### Advantages (Short)

- **Reduced Data Redundancy** — Data is stored once, not duplicated across files
- **Data Consistency** — Since data isn't duplicated, no conflicting copies
- **Data Sharing** — Multiple users can access the same data concurrently
- **Data Integrity** — Constraints ensure data is accurate and valid
- **Data Security** — Access control via GRANT/REVOKE
- **Backup & Recovery** — Built-in mechanisms to recover from failures
- **Data Isolation** — Changes to storage don't affect how users see data
- **Atomicity** — Either all parts of a transaction complete, or none do

### Disadvantages (Short)

- **Complexity** — Requires skilled DBAs to manage
- **Cost** — License, hardware, and maintenance costs
- **Not suitable for small-scale** — Overkill for tiny applications

---

## 2.3 🔹 Types of Database Users

| User Type | Description |
|-----------|-------------|
| **Naive/End Users** | Regular users who interact through applications (e.g., bank customer using ATM) |
| **Application Programmers** | Developers who write programs that interact with the database |
| **Sophisticated Users** | Users who write their own SQL queries directly (analysts, engineers) |
| **Database Administrator (DBA)** | The person responsible for managing the entire database system |

### Roles of a DBA (Short)

Schema design, storage management, security & access control, backup & recovery, performance monitoring, and user assistance.

---

## 2.4 🔹 Three-Level ANSI-SPARC Architecture

The database system has 3 levels of abstraction to separate what users see from how data is actually stored:

| Level | Also Called | Purpose | Who Uses It |
|-------|-----------|---------|-------------|
| **External Level** | View Level | Each user sees only their relevant portion of data | End users |
| **Conceptual Level** | Logical Level | The complete logical structure of the entire database (tables, relationships, constraints) | DBA, Designers |
| **Internal Level** | Physical Level | How data is physically stored on disk (files, indexes, storage) | System/DBMS |

### Data Abstraction

Hiding the internal complexity from users. A user doesn't need to know how data is stored on disk — they just need to query it.

### Data Independence

- **Logical Data Independence** — Changing the conceptual schema (adding a column) doesn't affect external views
- **Physical Data Independence** — Changing the physical storage (moving to SSD) doesn't affect the logical schema

> Physical data independence is easier to achieve than logical data independence.

---

## 2.5 🔹 Types of Databases

| Type | Description | Example |
|------|-------------|---------|
| **Relational (RDBMS)** | Data stored in tables with rows & columns, related via keys | MySQL, SQL Server, PostgreSQL |
| **Object-Oriented** | Data stored as objects (like OOP — class, methods, variables) | db4o, ObjectDB |
| **Hierarchical** | Data organized in a tree structure (parent-child) | IBM IMS |
| **Network** | Similar to hierarchical but allows many-to-many relationships | IDS, IDMS |

---

# Unit 3: Entity-Relationship (E-R) Model

---

## 3.1 🔹 What is Database Design & E-R Model?

### Description

**Database Design** is the process of defining the structure of a database — what tables to create, what columns they'll have, and how they relate to each other.

The **E-R (Entity-Relationship) Model** is a visual tool (diagram) used to design the database before actually creating it. It shows entities (things), their attributes (properties), and relationships (connections between things).

Think of it as a **blueprint** — just like an architect draws a building plan before construction, we draw an E-R diagram before creating tables.

---

## 3.2 🔹 Entities and Entity Sets

An **Entity** is a real-world thing or concept that can be distinctly identified.

- **Physical Entity** — Physically exists: Student, Car, Book, Building
- **Conceptual Entity** — Exists as a concept: Bank Account, Course, Department

An **Entity Set** is a collection of similar entities — like "Students" is a set of all individual student entities.

> In the E-R diagram, an entity is represented as a **rectangle**.

---

## 3.3 🔹 Attributes — Types

Attributes are the **properties** of an entity. A "Student" entity might have attributes: Name, Age, Email, Phone.

| Attribute Type | Description | Example |
|---------------|-------------|---------|
| **Simple** | Cannot be divided further | `Age`, `Gender` |
| **Composite** | Can be divided into sub-parts | `Full Name` → First Name + Last Name |
| **Single-Valued** | Holds only one value | `Date of Birth` |
| **Multi-Valued** | Holds multiple values | `Phone Numbers` (can have many) |
| **Stored** | Directly stored in DB | `Date of Birth` |
| **Derived** | Calculated from another attribute | `Age` (derived from Date of Birth) |
| **Descriptive** | Attribute of a relationship | `Grade` in Student-Course relationship |

> In E-R diagrams: Simple attributes = **oval**, Multi-valued = **double oval**, Derived = **dashed oval**, Composite = **oval with sub-ovals**. Primary key attribute is **underlined**.

---

## 3.4 🔹 Relationships

A **Relationship** is an association between two or more entities.

- **Binary Relationship** — Between 2 entities (most common): Student *enrolls in* Course
- **Ternary Relationship** — Between 3 entities: Doctor *prescribes* Medicine to Patient
- **Recursive Relationship** — An entity related to itself: Employee *manages* Employee

> In E-R diagrams, a relationship is represented as a **diamond**.

### Role Labels

When an entity participates multiple times in the same relationship, we use role labels. Example: In "Employee manages Employee," one is the "Manager" role and the other is the "Subordinate" role.

---

## 3.5 🔹 Mapping Cardinality (Cardinality Constraints)

Cardinality defines how many instances of one entity can be associated with instances of another.

| Cardinality | Description | Example |
|-------------|-------------|---------|
| **One-to-One (1:1)** | One A → One B | One Person has One Passport |
| **One-to-Many (1:N)** | One A → Many B | One Department has Many Employees |
| **Many-to-One (N:1)** | Many A → One B | Many Students belong to One Department |
| **Many-to-Many (M:N)** | Many A → Many B | Many Students enroll in Many Courses |

---

## 3.6 🔹 Participation Constraints

| Type | Description | Notation |
|------|-------------|----------|
| **Total Participation** | Every entity MUST participate in the relationship | Double line (==) |
| **Partial Participation** | Some entities MAY or MAY NOT participate | Single line (—) |

> Example: Every employee MUST belong to a department (total). But not every employee manages a department (partial).

---

## 3.7 🔹 Weak Entity Set

A **Weak Entity** cannot be uniquely identified by its own attributes alone — it depends on a **Strong Entity** for identification.

- A weak entity has a **Discriminator (Partial Key)** instead of a primary key
- Its primary key = Strong Entity's PK + Discriminator
- It has **total participation** with the strong entity

> Example: A `Room` (weak) depends on `Building` (strong). Room 101 in Building A ≠ Room 101 in Building B. Room's identity depends on the building.

> In E-R diagrams: Weak entity = **double rectangle**, its relationship = **double diamond**, discriminator = **dashed underline**.

---

## 3.8 🔹 Generalization & Specialization

| Concept | Direction | Description | Example |
|---------|-----------|-------------|---------|
| **Generalization** | Bottom → Up | Combining common features of sub-entities into a super-entity | Student + Teacher → Person |
| **Specialization** | Top → Down | Breaking a super-entity into sub-entities with specific features | Person → Student, Teacher |

### Constraints on Specialization/Generalization

- **Disjoint** — An entity can belong to ONLY ONE sub-entity (Student OR Teacher, not both)
- **Overlapping** — An entity can belong to MULTIPLE sub-entities (can be Student AND Employee)
- **Total Participation** — Every super-entity MUST belong to at least one sub-entity
- **Partial Participation** — Not all super-entities need to belong to a sub-entity

---

## 3.9 🔹 Aggregation

Aggregation is used when a **relationship needs to participate in another relationship**. Since E-R diagrams can't directly show relationships between relationships, aggregation wraps a relationship into a higher-level entity.

> Example: A "Works On" relationship between Employee and Project. Now, a Manager "Monitors" the "Works On" relationship — aggregation wraps "Works On" into a single unit that "Monitors" connects to.

---

## 3.10 🔹 Reducing E-R Diagram to Database Schema

Rules for converting E-R diagram to actual tables:

| E-R Element | Becomes |
|------------|---------|
| Entity | Table |
| Attribute | Column |
| Primary Key | PRIMARY KEY constraint |
| Derived Attribute | Not stored (calculated at query time) |
| Composite Attribute | Store individual sub-attributes as columns |
| Multi-Valued Attribute | Separate table with FK referencing the entity |
| Weak Entity | Table with composite PK (Owner's PK + Discriminator) |
| 1:1 Relationship | Add FK in either table (prefer total participation side) |
| 1:N Relationship | Add FK in the "Many" side table |
| M:N Relationship | Create a new junction/bridge table with both PKs |

---

## 3.11 🔹 Relational Algebra

Relational Algebra is a **theoretical, procedural query language** — it describes WHAT operations to perform on tables to get the result. SQL is based on it.

### Core Operations

| Operator | Symbol | Description | SQL Equivalent |
|----------|--------|-------------|----------------|
| **Selection** | σ (sigma) | Filter rows by condition | `WHERE` |
| **Projection** | π (pi) | Select specific columns | `SELECT col1, col2` |
| **Cartesian Product** | × | Combine every row of A with every row of B | `CROSS JOIN` |
| **Union** | ∪ | All rows from both tables (no duplicates) | `UNION` |
| **Intersection** | ∩ | Only rows present in both tables | `INTERSECT` |
| **Set Difference** | − | Rows in A but not in B | `EXCEPT` |
| **Natural Join** | ⋈ | Join on common attribute(s) | `INNER JOIN` |
| **Division** | ÷ | Find rows in A associated with ALL rows in B | No direct SQL |
| **Rename** | ρ (rho) | Rename a table or attribute | `AS` (alias) |

### Join Types in Relational Algebra

| Join Type | Description |
|-----------|-------------|
| **Natural/Inner Join** | Only matching rows from both tables |
| **Left Outer Join** | All rows from left + matching from right (NULL if no match) |
| **Right Outer Join** | All rows from right + matching from left (NULL if no match) |
| **Full Outer Join** | All rows from both tables (NULL where no match) |

> **Set operation conditions:** Both tables must have the **same number of columns** with **compatible data types**.

---

# Unit 4: Relational Database Design (Normalization)

---

## 4.1 🔹 Functional Dependency (FD)

### Description

A Functional Dependency (FD) means: if you know the value of attribute X, you can **uniquely determine** the value of attribute Y. Written as **X → Y** (X determines Y).

> Example: `StudentId → StudentName` — if you know the StudentId, you can find exactly one StudentName. But `StudentName → StudentId` may NOT hold because two students can have the same name.

### Types of Functional Dependencies

| Type | Description | Example |
|------|-------------|---------|
| **Full FD** | Y depends on the ENTIRE key, not a part of it | `{StudentId, CourseId} → Grade` |
| **Partial FD** | Y depends on PART of a composite key | `{StudentId, CourseId} → StudentName` (Name depends only on StudentId) |
| **Transitive FD** | X → Y and Y → Z, so X → Z indirectly | `EmpId → DeptId → DeptName` |
| **Trivial FD** | Y is a subset of X | `{A, B} → A` (always true) |
| **Non-Trivial FD** | Y is NOT a subset of X | `A → B` where B ∉ A |

---

## 4.2 🔹 Armstrong's Axioms (Inference Rules)

These are rules to derive new FDs from existing ones:

| Rule | Description | If |
|------|-------------|-----|
| **Reflexivity** | If Y ⊆ X, then X → Y | `{A, B} → A` (always true) |
| **Augmentation** | If X → Y, then XZ → YZ | `A → B` then `AC → BC` |
| **Transitivity** | If X → Y and Y → Z, then X → Z | `A → B`, `B → C` then `A → C` |
| **Union** | If X → Y and X → Z, then X → YZ | `A → B`, `A → C` then `A → BC` |
| **Decomposition** | If X → YZ, then X → Y and X → Z | `A → BC` then `A → B`, `A → C` |
| **Pseudo-Transitivity** | If X → Y and WY → Z, then WX → Z | |

---

## 4.3 🔹 Closure of FDs and Attributes

### Closure of FD Set (F⁺)

The **closure of a set of FDs (F⁺)** is the complete set of ALL functional dependencies that can be derived from F using Armstrong's Axioms.

### Closure of Attribute Set (α⁺)

The **attribute closure (α⁺)** is the set of all attributes that can be determined from α using the given FDs.

**Algorithm:**
1. Start with α⁺ = α
2. For each FD X → Y in F: if X ⊆ α⁺, then add Y to α⁺
3. Repeat until no change

> **Use case:** If α⁺ contains ALL attributes of the table, then α is a **candidate key**.

---

## 4.4 🔹 Anomalies — Why Normalization is Needed

Anomalies are problems that occur due to poor database design (unnormalized tables):

| Anomaly | Description | Example |
|---------|-------------|---------|
| **Insert Anomaly** | Cannot insert data without unnecessary extra data | Can't add a new department without adding an employee |
| **Update Anomaly** | Changing one fact requires updating multiple rows | Changing a department name means updating every employee row |
| **Delete Anomaly** | Deleting data unintentionally removes other useful data | Deleting the last employee in a department loses the department info |

> **Solution:** Normalization — decomposing tables to eliminate anomalies.

---

## 4.5 🔹 Normalization & Normal Forms

**Normalization** is the process of organizing a database to reduce redundancy and eliminate anomalies by decomposing tables into smaller, related tables.

### 1NF — First Normal Form

**Rule:** Every column must hold **atomic (indivisible) values** — no multi-valued or composite attributes.

❌ Bad: `Phone: 9876543210, 1234567890` (multi-valued)
✅ Fix: Create a separate row or table for each phone number.

### 2NF — Second Normal Form

**Rule:** Must be in 1NF + no **partial dependencies** (non-key attribute should not depend on PART of a composite key).

❌ Bad: `{StudentId, CourseId} → StudentName` (Name depends only on StudentId)
✅ Fix: Move StudentName to a separate Students table.

### 3NF — Third Normal Form

**Rule:** Must be in 2NF + no **transitive dependencies** (non-key attribute should not depend on another non-key attribute).

❌ Bad: `EmpId → DeptId → DeptName` (DeptName transitively depends on EmpId via DeptId)
✅ Fix: Move DeptName to a separate Departments table.

### BCNF — Boyce-Codd Normal Form

**Rule:** Must be in 3NF + for every FD X → Y, X must be a **super key** (determinant must be a key).

BCNF is stricter than 3NF. Most 3NF tables are already in BCNF, but edge cases with overlapping candidate keys may violate it.

### 4NF — Fourth Normal Form

**Rule:** Must be in BCNF + no **non-trivial multivalued dependencies (MVDs)**.

A **Multivalued Dependency** X →→ Y means: for each value of X, there is a set of values for Y that is independent of other attributes.

❌ Bad: A table with `{Course, Book, Lecturer}` where Book and Lecturer are independent multi-valued facts about Course.
✅ Fix: Decompose into `{Course, Book}` and `{Course, Lecturer}`.

### 5NF — Fifth Normal Form

**Rule:** Must be in 4NF + no **join dependency** — the table cannot be losslessly decomposed into smaller tables and then joined back without creating spurious (extra, incorrect) rows.

5NF deals with complex cases where a table can be decomposed into 3+ tables.

### Normal Forms Summary

| NF | Eliminates | Rule |
|----|-----------|------|
| 1NF | Multi-valued/composite attributes | Atomic values only |
| 2NF | Partial dependencies | No partial FD on composite key |
| 3NF | Transitive dependencies | Non-key → Non-key removed |
| BCNF | Non-superkey determinants | Every determinant is a superkey |
| 4NF | Multivalued dependencies | No non-trivial MVDs |
| 5NF | Join dependencies | Lossless decomposition into smallest parts |

---

## 4.6 🔹 Decomposition

**Decomposition** is splitting a table into smaller tables to achieve normalization.

| Type | Description |
|------|-------------|
| **Lossless Decomposition** | When you join the decomposed tables back, you get the EXACT original data — no extra/missing rows. ✅ |
| **Lossy Decomposition** | When you join back, you get EXTRA (spurious) rows that weren't in the original — data is corrupted. ❌ |

> **Test for Lossless:** If R is decomposed into R1 and R2, it's lossless if: R1 ∩ R2 → R1 or R1 ∩ R2 → R2 (the common attributes must be a key of at least one table).

### Dependency Preservation

A decomposition is **dependency preserving** if all original FDs can be checked within individual decomposed tables — without needing to join them.

---

# Unit 5: Views, Joins & Subqueries

---

## 5.1 🔹 Views

### Description

A **View** is a **virtual/logical table** based on a stored SQL query. It doesn't store data physically — it dynamically fetches data from base tables whenever accessed.

Think of it as a **saved SELECT query** that you can treat like a table.

```sql
-- Create a simple view
CREATE VIEW ActiveStudents AS
SELECT StudentId, Name, City
FROM Students
WHERE IsActive = 1;

-- Use the view like a table
SELECT * FROM ActiveStudents;

-- Update a view
ALTER VIEW ActiveStudents AS
SELECT StudentId, Name, City, Age
FROM Students
WHERE IsActive = 1;

-- Rename a view (SQL Server)
EXEC sp_rename 'ActiveStudents', 'CurrentStudents';

-- Delete a view
DROP VIEW CurrentStudents;
```

### Types of Views

| Type | Description |
|------|-------------|
| **Simple View** | Based on a single table, no functions/groups | Updatable ✅ |
| **Complex View** | Based on multiple tables, joins, GROUP BY, aggregate functions | Usually NOT updatable ❌ |

### Advantages (Short)

- Restrict sensitive data access (show only needed columns)
- Simplify complex queries (save long JOINs as a view)
- Provide data independence (change base tables without affecting users)
- Different users can see different views of the same data

### View vs Table

| Feature | Table | View |
|---------|-------|------|
| Stores data | ✅ Physically | ❌ Virtual (no data stored) |
| DML operations | ✅ Always | ⚠️ Only on simple views |
| Storage space | Uses disk | No extra space |
| Performance | Direct access | Slight overhead (query runs each time) |

### Restrictions on Updatable Views

A view is NOT updatable if it contains: Aggregate Functions, DISTINCT, GROUP BY, HAVING, UNION, Subqueries in SELECT, or JOINs (in most cases).

---

## 5.2 🔹 SET Operators

SET operators combine results from two or more SELECT queries.

| Operator | Description | Duplicates |
|----------|-------------|-----------|
| `UNION` | Combines results from both queries | Removes duplicates |
| `UNION ALL` | Combines results from both queries | Keeps duplicates |
| `INTERSECT` | Only rows present in BOTH queries | Removes duplicates |
| `EXCEPT` | Rows in first query but NOT in second | Removes duplicates |

**Rules:** Both queries must have the same number of columns with compatible data types. `ORDER BY` can only be used with the last SELECT.

```sql
-- Students in Mumbai OR Delhi
SELECT Name FROM Students WHERE City = 'Mumbai'
UNION
SELECT Name FROM Students WHERE City = 'Delhi';

-- Students who are in BOTH CS and Math courses
SELECT StudentId FROM CS_Students
INTERSECT
SELECT StudentId FROM Math_Students;

-- Students in CS but NOT in Math
SELECT StudentId FROM CS_Students
EXCEPT
SELECT StudentId FROM Math_Students;
```

---

## 5.3 🔹 Subqueries (Nested Queries)

### Description

A **Subquery** is a query written inside another query. The inner query executes first, and its result is used by the outer query.

```sql
-- Subquery in WHERE — find employees earning more than average
SELECT Name, Salary
FROM Employees
WHERE Salary > (SELECT AVG(Salary) FROM Employees);

-- Subquery in HAVING
SELECT Department, AVG(Salary) AS AvgSal
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > (SELECT AVG(Salary) FROM Employees);

-- Subquery in SELECT
SELECT Name,
       (SELECT COUNT(*) FROM Orders O WHERE O.EmpId = E.EmpId) AS OrderCount
FROM Employees E;
```

### Types of Subqueries

| Type | Description | Example |
|------|-------------|---------|
| **Single-Row** | Returns one value (scalar) | `WHERE Salary > (SELECT MAX(Salary) ...)` |
| **Multi-Row** | Returns multiple values (list) | `WHERE DeptId IN (SELECT DeptId ...)` |
| **Correlated** | Inner query depends on outer query — runs once for each outer row | Uses outer table alias inside inner query |

> **Correlated Subquery:** The inner query references a column from the outer query, so it re-executes for every row of the outer query. Slower but powerful.

---

## 5.4 🔹 Joins

### Description

A **Join** combines rows from two or more tables based on a related column (usually FK → PK). Joins are the most important concept for retrieving data from multiple tables.

### Types of Joins

| Join Type | Description | Returns |
|-----------|-------------|---------|
| **INNER JOIN** | Only rows that have matching values in both tables | Matching rows only |
| **LEFT (OUTER) JOIN** | All rows from left table + matching from right (NULL if no match) | All left + matched right |
| **RIGHT (OUTER) JOIN** | All rows from right table + matching from left (NULL if no match) | All right + matched left |
| **FULL (OUTER) JOIN** | All rows from both tables (NULL where no match on either side) | Everything |
| **CROSS JOIN** | Every row of A combined with every row of B (Cartesian Product) | A×B rows |
| **SELF JOIN** | A table joined with itself | Used for hierarchical data |

### Join Query Examples

```sql
-- INNER JOIN — only matching rows
SELECT E.Name, D.DeptName
FROM Employees E
INNER JOIN Departments D ON E.DeptId = D.DeptId;

-- LEFT JOIN — all employees, even without a department
SELECT E.Name, D.DeptName
FROM Employees E
LEFT JOIN Departments D ON E.DeptId = D.DeptId;

-- RIGHT JOIN — all departments, even without employees
SELECT E.Name, D.DeptName
FROM Employees E
RIGHT JOIN Departments D ON E.DeptId = D.DeptId;

-- FULL OUTER JOIN — everything
SELECT E.Name, D.DeptName
FROM Employees E
FULL OUTER JOIN Departments D ON E.DeptId = D.DeptId;

-- CROSS JOIN — Cartesian Product
SELECT E.Name, D.DeptName
FROM Employees E
CROSS JOIN Departments D;

-- SELF JOIN — find employees and their managers
SELECT E.Name AS Employee, M.Name AS Manager
FROM Employees E
LEFT JOIN Employees M ON E.ManagerId = M.EmpId;
```

### Join vs Subquery

| Feature | Join | Subquery |
|---------|------|----------|
| Readability | Better for multi-table | Better for single comparisons |
| Performance | Generally faster | Can be slower (especially correlated) |
| Use case | Display columns from multiple tables | Filter based on aggregates |

---

# 📘 PART 2 — DBMS-II

---

# Unit 1: Functions, Stored Procedures & Cursors

---

## 6.1 🔹 User-Defined Functions (UDF)

### Description

A **User-Defined Function (UDF)** is a reusable block of SQL code that accepts parameters, performs an operation, and **returns a value or a table**. Unlike stored procedures, functions MUST return something and can be used inside SELECT, WHERE, and other clauses.

### Types

| Type | Description | Returns |
|------|-------------|---------|
| **Scalar Function** | Returns a single value (int, varchar, etc.) | One value |
| **Table-Valued Function** | Returns a table result set | A table |

### Scalar Function

```sql
-- Create a scalar function
CREATE FUNCTION dbo.GetFullName (@FirstName VARCHAR(50), @LastName VARCHAR(50))
RETURNS VARCHAR(100)
AS
BEGIN
    RETURN @FirstName + ' ' + @LastName;
END;

-- Use it
SELECT dbo.GetFullName('Raj', 'Patel') AS FullName;  -- Raj Patel
SELECT dbo.GetFullName(FirstName, LastName) AS FullName FROM Employees;
```

### Table-Valued Function

```sql
-- Create a table-valued function
CREATE FUNCTION dbo.GetEmployeesByDept (@DeptId INT)
RETURNS TABLE
AS
RETURN (
    SELECT EmpId, Name, Salary
    FROM Employees
    WHERE DeptId = @DeptId
);

-- Use it like a table
SELECT * FROM dbo.GetEmployeesByDept(3);
```

---

## 6.2 🔹 Stored Procedures

### Description

A **Stored Procedure** is a precompiled block of SQL statements stored in the database. It can accept input/output parameters, contain complex logic (IF/ELSE, loops), and perform DML operations. Unlike functions, procedures do NOT need to return a value and CANNOT be used in SELECT.

```sql
-- Create a stored procedure
CREATE PROCEDURE spGetEmployeeById
    @EmpId INT
AS
BEGIN
    SELECT * FROM Employees WHERE EmpId = @EmpId;
END;

-- Execute it
EXEC spGetEmployeeById @EmpId = 5;
```

### Stored Procedure with Output Parameter

```sql
CREATE PROCEDURE spGetEmployeeCount
    @DeptId INT,
    @Count INT OUTPUT
AS
BEGIN
    SELECT @Count = COUNT(*) FROM Employees WHERE DeptId = @DeptId;
END;

-- Call with OUTPUT
DECLARE @Result INT;
EXEC spGetEmployeeCount @DeptId = 3, @Count = @Result OUTPUT;
PRINT @Result;
```

### Stored Procedure for INSERT, UPDATE, DELETE

```sql
-- INSERT Procedure
CREATE PROCEDURE spInsertEmployee
    @Name VARCHAR(50),
    @Salary DECIMAL(10,2),
    @DeptId INT
AS
BEGIN
    INSERT INTO Employees (Name, Salary, DeptId) VALUES (@Name, @Salary, @DeptId);
END;

-- UPDATE Procedure
CREATE PROCEDURE spUpdateSalary
    @EmpId INT,
    @NewSalary DECIMAL(10,2)
AS
BEGIN
    UPDATE Employees SET Salary = @NewSalary WHERE EmpId = @EmpId;
END;

-- DELETE Procedure
CREATE PROCEDURE spDeleteEmployee
    @EmpId INT
AS
BEGIN
    DELETE FROM Employees WHERE EmpId = @EmpId;
END;
```

---

## 6.3 🔹 Function vs Stored Procedure

| Feature | Function | Stored Procedure |
|---------|----------|-----------------|
| **Return** | MUST return a value | May or may not return |
| **Used in SELECT** | ✅ Yes | ❌ No |
| **DML (INSERT/UPDATE/DELETE)** | ❌ Not allowed | ✅ Allowed |
| **TRY-CATCH** | ❌ Not allowed | ✅ Allowed |
| **Transaction Management** | ❌ No | ✅ Yes |
| **Call syntax** | `SELECT dbo.FnName()` | `EXEC ProcName` |
| **Parameters** | Only IN | IN, OUT, IN OUT |
| **Calling from another** | Can call function | Can call both |

---

## 6.4 🔹 Cursors

### Description

A **Cursor** allows you to process query results **row by row** instead of as a set. Normally SQL works with sets of data, but sometimes you need to process each row individually (like sending an email to each customer).

### Cursor Lifecycle

```sql
-- 1. DECLARE the cursor
DECLARE EmpCursor CURSOR FOR
    SELECT EmpId, Name, Salary FROM Employees;

-- 2. OPEN the cursor
OPEN EmpCursor;

-- 3. FETCH rows one by one
DECLARE @Id INT, @Name VARCHAR(50), @Salary DECIMAL(10,2);

FETCH NEXT FROM EmpCursor INTO @Id, @Name, @Salary;

-- 4. LOOP through rows
WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT 'Employee: ' + @Name + ', Salary: ' + CAST(@Salary AS VARCHAR);
    FETCH NEXT FROM EmpCursor INTO @Id, @Name, @Salary;
END;

-- 5. CLOSE the cursor
CLOSE EmpCursor;

-- 6. DEALLOCATE the cursor
DEALLOCATE EmpCursor;
```

> **Lifecycle:** DECLARE → OPEN → FETCH → WHILE loop → CLOSE → DEALLOCATE

> **Caution:** Cursors are slow compared to set-based operations. Use them only when row-by-row processing is absolutely necessary.

---

# Unit 2: Triggers, Exception Handling & Transactions

---

## 7.1 🔹 Triggers

### Description

A **Trigger** is a special stored procedure that **automatically executes** when a specific event (INSERT, UPDATE, DELETE) occurs on a table. You don't call it manually — it fires on its own.

### Types of Triggers

| Type | When it Fires |
|------|---------------|
| **AFTER / FOR Trigger** | Fires AFTER the DML operation completes |
| **INSTEAD OF Trigger** | Fires INSTEAD of the DML operation (replaces it) |
| **DDL Trigger** | Fires on DDL events (CREATE, ALTER, DROP) |
| **Logon Trigger** | Fires when a user logs in |

### AFTER Trigger Example

```sql
-- Audit trigger — logs every INSERT into an audit table
CREATE TRIGGER trg_AfterInsertEmployee
ON Employees
AFTER INSERT
AS
BEGIN
    INSERT INTO AuditLog (Action, TableName, ActionDate)
    SELECT 'INSERT', 'Employees', GETDATE()
    FROM INSERTED;  -- INSERTED holds newly inserted rows
END;
```

### INSTEAD OF Trigger Example

```sql
-- Prevent deletion — log instead of actually deleting
CREATE TRIGGER trg_InsteadOfDelete
ON Employees
INSTEAD OF DELETE
AS
BEGIN
    PRINT 'Deletion is not allowed! Logging attempt...';
    INSERT INTO DeleteAttempts (EmpId, AttemptDate)
    SELECT EmpId, GETDATE() FROM DELETED;  -- DELETED holds rows being deleted
END;
```

### Magic Tables in Triggers

| Table | Contains | Available In |
|-------|----------|-------------|
| `INSERTED` | Newly inserted / updated rows (new values) | INSERT, UPDATE triggers |
| `DELETED` | Deleted / updated rows (old values) | DELETE, UPDATE triggers |

### Advantages & Disadvantages (Short)

**Advantages:** Automatic enforcement of business rules, auditing, maintaining data integrity, cascading changes.

**Disadvantages:** Hidden logic (hard to debug), performance overhead, complex trigger chains, can make debugging difficult.

---

## 7.2 🔹 Exception Handling in SQL

### TRY...CATCH Block

```sql
BEGIN TRY
    -- Code that might cause an error
    INSERT INTO Employees (EmpId, Name) VALUES (1, 'Raj');  -- Duplicate PK!
END TRY
BEGIN CATCH
    -- Handle the error
    PRINT 'Error occurred!';
    PRINT ERROR_MESSAGE();
END CATCH;
```

### Error Functions

| Function | Returns |
|----------|---------|
| `ERROR_NUMBER()` | Error number |
| `ERROR_MESSAGE()` | Error description |
| `ERROR_SEVERITY()` | Severity level |
| `ERROR_STATE()` | Error state |
| `ERROR_LINE()` | Line number where error occurred |
| `ERROR_PROCEDURE()` | Name of procedure/trigger where error occurred |

### THROW vs RAISERROR

```sql
-- THROW (C# 2012+, recommended)
THROW 50001, 'Custom error message', 1;

-- RAISERROR (older method)
RAISERROR('Custom error message', 16, 1);
```

| Feature | THROW | RAISERROR |
|---------|-------|-----------|
| Error Number | Must be >= 50000 | Any number |
| Severity | Always uses state | Must specify severity |
| Simplicity | Simpler syntax | More flexible but verbose |
| Recommended | ✅ Modern approach | ⚠️ Older approach |

---

## 7.3 🔹 Transaction Management

### Description

A **Transaction** is a group of SQL operations that must either ALL succeed or ALL fail — there's no in-between. This ensures data consistency.

> Real-life example: Bank transfer — debit from Account A and credit to Account B. If debit succeeds but credit fails, the debit must also be reversed. Both must succeed or both must fail.

### ACID Properties

| Property | Description | Example |
|----------|-------------|---------|
| **Atomicity** | All operations succeed, or none do | Transfer: debit + credit = both or neither |
| **Consistency** | Database moves from one valid state to another | Total money before = total money after transfer |
| **Isolation** | Concurrent transactions don't interfere | Two users transferring simultaneously don't conflict |
| **Durability** | Once committed, data is permanent even after crash | Committed transfer survives power failure |

### Transaction Syntax

```sql
BEGIN TRANSACTION;

    UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountId = 1;
    UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountId = 2;

    IF @@ERROR <> 0
        ROLLBACK TRANSACTION;  -- Undo everything
    ELSE
        COMMIT TRANSACTION;    -- Save permanently
```

### Transaction with TRY-CATCH

```sql
BEGIN TRY
    BEGIN TRANSACTION;
        UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountId = 1;
        UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountId = 2;
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    ROLLBACK TRANSACTION;
    PRINT ERROR_MESSAGE();
END CATCH;
```

### SAVEPOINT — Partial Rollback

```sql
BEGIN TRANSACTION;
    INSERT INTO Orders VALUES (1, 'Laptop', 50000);
    SAVE TRANSACTION SavePoint1;

    INSERT INTO Orders VALUES (2, 'Phone', 20000);
    -- Something goes wrong here...
    ROLLBACK TRANSACTION SavePoint1;  -- Only undoes the Phone insert

    -- Laptop insert is still intact
COMMIT TRANSACTION;
```

### Transaction Modes

| Mode | Description |
|------|-------------|
| **Implicit** | SQL Server auto-commits each statement (default behavior) |
| **Explicit** | You manually use BEGIN, COMMIT, ROLLBACK |

---

# Unit 3: Indexes, Query Optimization & Performance Tuning

---

## 8.1 🔹 Indexes

### Description

An **Index** is a database object that speeds up data retrieval — like a book's index helps you find topics without reading every page. Without an index, SQL Server performs a **Table Scan** (reads every row), which is slow for large tables.

Indexes use a **B-Tree (Balanced Tree)** structure internally:
- **Root Node** → Top-level pointer
- **Non-Leaf Nodes** → Intermediate pointers
- **Leaf Nodes** → Actual data pointers or data itself

### Clustered Index

- **Physically sorts** the data in the table based on the index key
- Only **ONE** per table (because data can only be sorted one way)
- Primary Key automatically creates a clustered index
- Leaf nodes contain the **actual data rows**

```sql
-- Create clustered index
CREATE CLUSTERED INDEX IX_Emp_Name ON Employees(Name);

-- Drop index
DROP INDEX IX_Emp_Name ON Employees;

-- View indexes
EXEC sp_helpindex 'Employees';
SELECT * FROM sys.indexes WHERE object_id = OBJECT_ID('Employees');
```

### Non-Clustered Index

- Creates a **separate lookup structure** (like a book's index at the back)
- Can have **multiple** per table (up to 999)
- Leaf nodes contain **pointers (row locators)** to the actual data
- Data is NOT physically sorted

```sql
CREATE NONCLUSTERED INDEX IX_Emp_City ON Employees(City);
```

### Clustered vs Non-Clustered Index

| Feature | Clustered | Non-Clustered |
|---------|-----------|---------------|
| Physical sorting | ✅ Yes | ❌ No |
| Per table | Only 1 | Multiple (up to 999) |
| Speed | Faster for range queries | Faster for specific lookups |
| Leaf nodes contain | Actual data | Pointers to data |
| Auto-created by | PRIMARY KEY | UNIQUE constraint |
| Storage | No extra space (data is sorted) | Extra space (separate structure) |

---

## 8.2 🔹 Execution Plans

### Description

An **Execution Plan** shows HOW SQL Server will execute your query — what operations it will perform, in what order, and at what cost. It's essential for understanding and optimizing query performance.

| Type | Description |
|------|-------------|
| **Estimated Execution Plan** | Shows the plan WITHOUT actually running the query (Ctrl+L) |
| **Actual Execution Plan** | Shows the plan WITH actual runtime statistics (Ctrl+M, then run) |

### Common Warnings in Execution Plans

| Warning | Description | Fix |
|---------|-------------|-----|
| Missing Index | SQL suggests an index that would improve performance | Create the suggested index |
| Table Scan | Full table scan (no index used) | Create appropriate index |
| Implicit Conversion | Data type mismatch causing conversion | Use matching data types |
| Key Lookup | Extra lookup needed after index seek | Create covering index |

---

## 8.3 🔹 Query Optimization — Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|---------|
| Select only needed columns: `SELECT Name, Age` | Use `SELECT *` everywhere |
| Use `VARCHAR` for variable-length data | Use `CHAR` for everything |
| Use `WHERE` for filtering | Use `HAVING` when `WHERE` would work |
| Use `EXISTS()` to check existence | Use `COUNT()` to check existence |
| Use parameterized queries | Use string concatenation in queries |
| Create indexes on frequently searched columns | Over-index every column |
| Use `SET NOCOUNT ON` in procedures | Leave it off (extra network traffic) |
| Join from larger table to smaller | Join from smaller to larger |
| Use `UNION ALL` when duplicates are OK | Use `UNION` unnecessarily |

---

## 8.4 🔹 Temporary Tables & Table Variables

### Temporary Tables

| Type | Syntax | Scope | Stored In |
|------|--------|-------|-----------|
| **Local Temp Table** | `#TableName` | Current session only | tempdb |
| **Global Temp Table** | `##TableName` | All sessions | tempdb |

```sql
-- Create local temp table
CREATE TABLE #TempEmployees (
    Id INT, Name VARCHAR(50), Salary DECIMAL(10,2)
);

INSERT INTO #TempEmployees SELECT EmpId, Name, Salary FROM Employees WHERE DeptId = 3;
SELECT * FROM #TempEmployees;

-- Dropped automatically when session ends, or manually:
DROP TABLE #TempEmployees;
```

### Table Variable

```sql
DECLARE @EmpTable TABLE (
    Id INT, Name VARCHAR(50)
);

INSERT INTO @EmpTable SELECT EmpId, Name FROM Employees;
SELECT * FROM @EmpTable;
-- Scope: only within the current batch/procedure
```

---

# Unit 4: NoSQL & MongoDB Basics

---

## 9.1 🔹 What is NoSQL?

### Description

**NoSQL** stands for "Not Only SQL" — it's a category of databases designed for large-scale, flexible, and high-performance data storage. Unlike RDBMS (fixed schema, tables, rows), NoSQL databases handle **unstructured/semi-structured data** and scale horizontally.

### Types of NoSQL Databases

| Type | Description | Example |
|------|-------------|---------|
| **Key-Value Store** | Simple pairs like a dictionary | Redis, DynamoDB |
| **Document-Based** | Data stored as JSON/BSON documents | MongoDB, CouchDB |
| **Column-Based** | Data stored in columns instead of rows | Cassandra, HBase |
| **Graph-Based** | Data stored as nodes and edges (relationships) | Neo4j, ArangoDB |

### SQL vs NoSQL

| Feature | SQL (RDBMS) | NoSQL |
|---------|-------------|-------|
| Schema | Fixed, predefined | Dynamic, flexible |
| Data format | Tables (rows & columns) | Documents, key-value, graphs |
| Scalability | Vertical (bigger server) | Horizontal (more servers) |
| Joins | ✅ Supported | ❌ Not supported (embedded docs) |
| ACID | ✅ Full ACID | ⚠️ Eventual consistency (BASE) |
| Best for | Structured, transactional data | Unstructured, big data, real-time |
| Examples | MySQL, PostgreSQL, SQL Server | MongoDB, Redis, Cassandra |

### When to Use NoSQL

- Huge amount of data (Big Data)
- Unstructured or semi-structured data (JSON, logs)
- Frequently changing schema
- Real-time, high-performance apps (IoT, mobile, gaming)
- Horizontal scalability is needed

---

## 9.2 🔹 Introduction to MongoDB

### Description

**MongoDB** is the most popular NoSQL document-oriented database. Data is stored as **JSON-like documents** (internally **BSON — Binary JSON**), grouped into **Collections** (equivalent of SQL tables).

### RDBMS vs MongoDB Mapping

| RDBMS Term | MongoDB Term |
|-----------|-------------|
| Database | Database |
| Table | Collection |
| Row / Record | Document |
| Column | Field |
| Primary Key | _id (auto-generated ObjectId) |
| JOIN | Embedded Documents / $lookup |
| Fixed Schema | Dynamic / Schema-less |

### MongoDB Document Example

```json
{
    "_id": ObjectId("6543abc..."),
    "name": "Raj Patel",
    "age": 22,
    "city": "Ahmedabad",
    "courses": ["DBMS", "OOP", "Web Dev"],
    "address": {
        "street": "MG Road",
        "pin": "380001"
    }
}
```

---

## 9.3 🔹 MongoDB Data Types & Operators

### Data Types

| Type | Description | Example |
|------|-------------|---------|
| String | Text data | `"name": "Raj"` |
| Integer | Whole numbers (32/64 bit) | `"age": 22` |
| Boolean | true/false | `"isActive": true` |
| Double | Decimal numbers | `"gpa": 8.5` |
| Array | List of values | `"tags": ["db", "nosql"]` |
| Object | Nested document | `"address": { "city": "Mumbai" }` |
| Null | No value | `"phone": null` |
| Date | Date/time | `"created": ISODate("2025-10-05")` |
| ObjectId | Unique 12-byte ID | `"_id": ObjectId(...)` |

### Comparison Operators

| Operator | Description | SQL Equivalent |
|----------|-------------|---------------|
| `$eq` | Equal to | `=` |
| `$ne` | Not equal | `!=` |
| `$gt` | Greater than | `>` |
| `$gte` | Greater than or equal | `>=` |
| `$lt` | Less than | `<` |
| `$lte` | Less than or equal | `<=` |
| `$in` | In a list | `IN (...)` |
| `$nin` | Not in a list | `NOT IN (...)` |

### Logical Operators

| Operator | Description | SQL Equivalent |
|----------|-------------|---------------|
| `$and` | All conditions true | `AND` |
| `$or` | At least one true | `OR` |
| `$not` | Negates condition | `NOT` |
| `$nor` | None of the conditions true | `NOT (... OR ...)` |

---

## 9.4 🔹 MongoDB CRUD Operations

### Database & Collection Commands

```javascript
// Create / switch database
use SchoolDB

// Show all databases
show dbs

// Show current database
db

// Drop database
db.dropDatabase()

// Create collection
db.createCollection("students")

// Show collections
show collections

// Drop collection
db.students.drop()
```

### INSERT

```javascript
// Insert one document
db.students.insertOne({
    name: "Raj",
    age: 22,
    city: "Ahmedabad"
})

// Insert many documents
db.students.insertMany([
    { name: "Priya", age: 20, city: "Mumbai" },
    { name: "Amit", age: 23, city: "Delhi" }
])
```

### FIND (SELECT)

```javascript
// Find all documents
db.students.find()

// Pretty print
db.students.find().pretty()

// Find with filter (WHERE equivalent)
db.students.find({ city: "Mumbai" })

// Greater than
db.students.find({ age: { $gt: 21 } })

// AND condition
db.students.find({ $and: [{ age: { $gt: 20 } }, { city: "Mumbai" }] })

// OR condition
db.students.find({ $or: [{ city: "Mumbai" }, { city: "Delhi" }] })

// IN operator
db.students.find({ city: { $in: ["Mumbai", "Delhi"] } })

// Find one (first match)
db.students.findOne({ city: "Mumbai" })

// Projection (select specific fields)
db.students.find({}, { name: 1, age: 1, _id: 0 })

// Sort — 1 ascending, -1 descending
db.students.find().sort({ age: 1 })
db.students.find().sort({ age: -1 })

// Limit and Skip (pagination)
db.students.find().limit(5)
db.students.find().skip(5).limit(5)  // Page 2

// Count
db.students.countDocuments({ city: "Mumbai" })
```

### UPDATE

```javascript
// Update one document
db.students.updateOne(
    { name: "Raj" },                  // Filter
    { $set: { age: 23, city: "Surat" } }  // Update
)

// Update many documents
db.students.updateMany(
    { city: "Delhi" },
    { $set: { city: "New Delhi" } }
)

// Upsert — insert if not found
db.students.updateOne(
    { name: "Sara" },
    { $set: { age: 21, city: "Pune" } },
    { upsert: true }
)
```

### Update Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `$set` | Set field value | `{ $set: { age: 25 } }` |
| `$unset` | Remove a field | `{ $unset: { phone: "" } }` |
| `$inc` | Increment by value | `{ $inc: { age: 1 } }` |
| `$rename` | Rename a field | `{ $rename: { "nm": "name" } }` |
| `$currentDate` | Set to current date | `{ $currentDate: { updated: true } }` |

### DELETE

```javascript
// Delete one document
db.students.deleteOne({ name: "Amit" })

// Delete many documents
db.students.deleteMany({ city: "Delhi" })

// Delete all documents
db.students.deleteMany({})
```

### ObjectId Structure

Every document has a unique `_id` field. If not provided, MongoDB auto-generates an **ObjectId** — a 12-byte value:
- 4 bytes: timestamp
- 5 bytes: random value (machine + process)
- 3 bytes: incrementing counter

---

# Unit 5: Advanced MongoDB Concepts

---

## 10.1 🔹 Regex — Pattern Matching

```javascript
// Contains "raj" (case-insensitive)
db.students.find({ name: { $regex: /raj/i } })

// Starts with "R"
db.students.find({ name: { $regex: /^R/ } })

// Ends with "l"
db.students.find({ name: { $regex: /l$/ } })

// Not matching a pattern
db.students.find({ name: { $not: /^A/ } })
```

| Option | Description |
|--------|-------------|
| `i` | Case-insensitive |
| `m` | Multiline matching |
| `x` | Ignore whitespace in pattern |
| `s` | Dot matches newline |

---

## 10.2 🔹 Aggregation Pipeline

### Description

**Aggregation** is MongoDB's way of performing GROUP BY, COUNT, SUM, AVG, etc. — like SQL aggregate queries. It uses a **pipeline** — data passes through stages, each transforming it.

### Pipeline Stages

| Stage | Description | SQL Equivalent |
|-------|-------------|---------------|
| `$match` | Filter documents | `WHERE` |
| `$group` | Group by a field and aggregate | `GROUP BY` |
| `$sort` | Sort results | `ORDER BY` |
| `$project` | Select/reshape fields | `SELECT` |
| `$limit` | Limit output count | `TOP / LIMIT` |
| `$skip` | Skip documents | `OFFSET` |
| `$unwind` | Flatten arrays | N/A |
| `$lookup` | Join with another collection | `JOIN` |

```javascript
// Count students per city
db.students.aggregate([
    { $group: { _id: "$city", count: { $sum: 1 } } }
])

// Average age per city, sorted
db.students.aggregate([
    { $group: { _id: "$city", avgAge: { $avg: "$age" } } },
    { $sort: { avgAge: -1 } }
])

// Filter first, then group
db.students.aggregate([
    { $match: { age: { $gte: 20 } } },
    { $group: { _id: "$city", total: { $sum: 1 } } },
    { $sort: { total: -1 } }
])

// Distinct cities
db.students.distinct("city")
```

---

## 10.3 🔹 MongoDB Indexes

```javascript
// Create ascending index
db.students.createIndex({ name: 1 })

// Create descending index
db.students.createIndex({ age: -1 })

// Create compound index
db.students.createIndex({ city: 1, age: -1 })

// View all indexes
db.students.getIndexes()

// Drop an index
db.students.dropIndex({ name: 1 })
```

> Indexes speed up read queries but slow down writes (inserts/updates) because the index must also be updated.

---

## 10.4 🔹 Schema Validation

MongoDB is schema-less, but you can enforce rules using **Schema Validation**:

```javascript
db.createCollection("employees", {
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["name", "age", "department"],
            properties: {
                name: { bsonType: "string", description: "Must be a string" },
                age: { bsonType: "int", minimum: 18, maximum: 65 },
                department: { bsonType: "string" }
            }
        }
    }
})
```

---

## 10.5 🔹 Embedded (Nested) Documents

Instead of JOINs, MongoDB uses **embedded documents** — storing related data inside a single document.

```javascript
// Embedded document
{
    "name": "Raj",
    "address": {
        "street": "MG Road",
        "city": "Ahmedabad",
        "pin": "380001"
    },
    "courses": [
        { "name": "DBMS", "grade": "A" },
        { "name": "OOP", "grade": "B+" }
    ]
}

// Query nested field using dot notation
db.students.find({ "address.city": "Ahmedabad" })
db.students.find({ "courses.grade": "A" })
```

> **Limitations:** Maximum BSON document size is **16 MB**. Maximum nesting level is **100**.

---

## 10.6 🔹 Users & Roles in MongoDB

```javascript
// Create a user
db.createUser({
    user: "admin",
    pwd: "password123",
    roles: [{ role: "readWrite", db: "SchoolDB" }]
})

// List users
show users
db.system.users.find()

// Drop a user
db.dropUser("admin")
```

---

## 10.7 🔹 Backup & Recovery

| Command | Purpose |
|---------|---------|
| `mongodump` | Creates a backup of the database (BSON dumps) |
| `mongorestore` | Restores a database from a backup |

```bash
# Backup entire database
mongodump --db SchoolDB --out /backup/

# Restore from backup
mongorestore --db SchoolDB /backup/SchoolDB/
```

---

## 10.8 🔹 BASE Theorem (NoSQL Properties)

Unlike ACID (for RDBMS), NoSQL follows **BASE**:

| Property | Description |
|----------|-------------|
| **Basically Available** | System guarantees availability — always responds, even with stale data |
| **Soft State** | Data may change over time even without input (due to sync) |
| **Eventual Consistency** | Data will become consistent eventually, not immediately |

---

## 10.9 🔹 CAP Theorem

The CAP Theorem states that a distributed system can guarantee only **2 out of 3** properties:

| Property | Description |
|----------|-------------|
| **Consistency** | Every read gets the most recent write |
| **Availability** | Every request gets a response (even if not the latest data) |
| **Partition Tolerance** | System works even if communication between nodes fails |

> **MongoDB** follows **CP (Consistency + Partition Tolerance)** — during a network partition, it sacrifices availability to maintain consistency (primary handles writes, secondaries sync).

---

## 10.10 🔹 Replication in MongoDB

### Description

**Replication** is the process of synchronizing data across multiple servers for **redundancy**, **high availability**, and **fault tolerance**.

MongoDB uses a **Replica Set** — a group of MongoDB servers that maintain the same data:
- **Primary Node** — Receives all write operations
- **Secondary Nodes** — Replicate data from the primary, serve read operations
- If primary fails, a secondary is **automatically elected** as the new primary

---

## 10.11 🔹 Sharding in MongoDB

### Description

**Sharding** is MongoDB's strategy for **horizontal scaling** — distributing data across multiple servers (shards) when a single server can't handle the load.

| Component | Description |
|-----------|-------------|
| **Shards** | Each shard holds a portion of the total data |
| **Config Servers** | Store metadata about which data is on which shard |
| **Query Routers (mongos)** | Route queries to the correct shard(s) |

> **When to Shard:** When data volume exceeds a single server's storage/memory, when write throughput is too high for one server, when you need geographic distribution.

---

## 10.12 🔹 Time Series Collections

**Time Series** data is a sequence of measurements taken at specific time intervals — like temperature readings, stock prices, IoT sensor data.

MongoDB 5.0+ supports dedicated **Time Series Collections** optimized for this type of data:

```javascript
db.createCollection("sensorData", {
    timeseries: {
        timeField: "timestamp",
        metaField: "sensorId",
        granularity: "seconds"
    }
})
```

---

## 🎯 Quick Reference — Most Asked Topics

| # | Topic | Category | Probability |
|---|-------|----------|-------------|
| 1 | Joins (INNER, LEFT, RIGHT, FULL) | SQL | ⭐⭐⭐⭐⭐ |
| 2 | Normalization (1NF–BCNF) | Theory | ⭐⭐⭐⭐⭐ |
| 3 | Keys (Primary, Foreign, Candidate) | SQL | ⭐⭐⭐⭐⭐ |
| 4 | GROUP BY, HAVING, ORDER BY | SQL | ⭐⭐⭐⭐⭐ |
| 5 | Indexes (Clustered vs Non-Clustered) | SQL | ⭐⭐⭐⭐⭐ |
| 6 | ACID Properties | Theory | ⭐⭐⭐⭐ |
| 7 | Stored Procedures & Functions | SQL | ⭐⭐⭐⭐ |
| 8 | Triggers | SQL | ⭐⭐⭐⭐ |
| 9 | Subqueries (Correlated) | SQL | ⭐⭐⭐⭐ |
| 10 | Views | SQL | ⭐⭐⭐⭐ |
| 11 | DELETE vs TRUNCATE vs DROP | SQL | ⭐⭐⭐⭐ |
| 12 | E-R Model & E-R Diagram | Theory | ⭐⭐⭐ |
| 13 | SQL vs NoSQL | Theory | ⭐⭐⭐ |
| 14 | MongoDB CRUD | NoSQL | ⭐⭐⭐ |
| 15 | Transactions & Savepoints | SQL | ⭐⭐⭐ |
| 16 | CAP & BASE Theorems | Theory | ⭐⭐⭐ |
| 17 | Aggregate Functions | SQL | ⭐⭐⭐ |
| 18 | Replication & Sharding | NoSQL | ⭐⭐ |

---

> 💡 **Next Step:** C# file is already done! After this, we can create the **.NET / ASP.NET Core** interview preparation file covering Middleware, Routing, Entity Framework, Web API, Authentication, and more!

---

*Created for .NET Developer Interview Preparation 🚀*
*Covers DBMS-I + DBMS-II Complete Syllabus*
*Last Updated: October 2026*
