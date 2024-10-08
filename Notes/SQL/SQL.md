# SQL
## Keys and contraints
### Keys
**Keys** are special columns in a database table that help identify records uniquely and establish relationships between tables.

### Types of Keys
 - Primary Key 
 - Foreign Key : 
    ```SQL
    CREATE TABLE Orders (
    OrderID INT,
    EmployeeID INT,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
    );
    ```
 - Unique Key :
 - Composite Key : A combination of two or more columns used to create a unique identifier for each row in a table.

 ### Constraints : 
  - NOT NULL
  - CHECK
  - DEFAULT
  - UNIQUE

```SQL
CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,             -- PRIMARY KEY ensures unique DepartmentID
    DepartmentName VARCHAR(100) NOT NULL,     -- NOT NULL constraint ensures DepartmentName cannot be NULL
    DepartmentCode CHAR(10) UNIQUE            -- UNIQUE constraint ensures DepartmentCode is unique
);
    CREATE TABLE Employees (
    EmployeeID IDENTITY(1,1)INT,                           -- Part of the Composite Key
    DepartmentID INT,                        -- Foreign Key referencing Departments table
    HireDate DATETIME DEFAULT GETDATE(),      -- DEFAULT constraint sets default value to current date
    Age INT CHECK (Age >= 18),                -- CHECK constraint ensures Age is 18 or older
    Email VARCHAR(100) UNIQUE,                -- UNIQUE constraint ensures unique email addresses
    JobTitle VARCHAR(100) NOT NULL,           -- NOT NULL constraint ensures JobTitle cannot be NULL
    PRIMARY KEY (EmployeeID, DepartmentID),   -- Composite Key ensures unique combination of EmployeeID and DepartmentID
    FOREIGN KEY (DepartmentID)                -- FOREIGN KEY constraint links to DepartmentID in Departments table
        REFERENCES Departments(DepartmentID)
);
```

## WHERE AND HAVING
### WHERE :
- Filter data on conditions
```SQL
-- Select employees with a salary greater than 50,000
SELECT EmployeeName, Salary
FROM Employees
WHERE Salary > 50000;

```
### HAVING :
- Filters groups of records after aggregation has been performed. 
```SQL
-- Select departments with more than 10 employees
SELECT DepartmentID, COUNT(*) AS EmployeeCount
FROM Employees
GROUP BY DepartmentID
HAVING COUNT(*) > 10;

```
## SQL Aggregate Functions
- COUNT(CLOUMN_NAME)
- MIN(CLOUMN_NAME)
- MAX(CLOUMN_NAME)
- AVG(CLOUMN_NAME)
- SUM(CLOUMN_NAME)
- GROUP_CONCAT() (MySQL specific)
   - **Description**: Returns a concatenated string of values from a group.
   - **Syntax**:
     ```sql
     SELECT GROUP_CONCAT(column_name) FROM table_name GROUP BY another_column;
     ```

- STRING_AGG() (SQL Server specific)
   - **Description**: Returns a concatenated string of values from a group, separated by a specified delimiter.
   - **Syntax**:
     ```sql
     SELECT STRING_AGG(column_name, ', ') FROM table_name GROUP BY another_column;
     ```

- VAR() / VARIANCE()
   - **Description**: Returns the variance of a numeric column (how spread out the values are).
   - **Syntax**:
     ```sql
     SELECT VAR(column_name) FROM table_name;
     ```
- STDEV() / STDDEV()
   - **Description**: Returns the standard deviation of a numeric column (measure of the amount of variation or dispersion).
   - **Syntax**:
     ```sql
     SELECT STDEV(column_name) FROM table_name;
     ```


## Nested Query 
Query inside Query
```SQL
-- Find departments with more than 5 employees
SELECT DepartmentName
FROM Departments
WHERE DepartmentID IN (
    SELECT DepartmentID
    FROM Employees
    GROUP BY DepartmentID
    HAVING COUNT(*) > 5
);
```
## Correlated Subquery
```SQL
-- Find employees who earn more than the highest salary in their department
SELECT EmployeeName, Salary, DepartmentID
FROM Employees e1
WHERE Salary > (
    SELECT MAX(Salary)
    FROM Employees e2
    WHERE e1.DepartmentID = e2.DepartmentID
);


```

## Identity : 
used to generate unique sequential numbers for a column in a table, typically used for primary key columns. The values are automatically incremented with each new row.
```SQL
 IDENTITY(seed, increment) 
 -- Reset the identity seed to 1
 DBCC CHECKIDENT ('Employees', RESEED, 0);
```
### SCOPE_IDENTITY() : 
- Returns the last identity value generated for any table in the current session and the current scope.
- it only returns the identity value generated by the last `INSERT` operation in the same scope (i.e., the same batch or stored procedure)

### @@IDENTITY : 
- Returns the last identity value generated for any table in the current session, regardless of scope.
- Less reliable than SCOPE_IDENTITY() because it may return an identity value inserted by a trigger or other operations, not necessarily the one you intended.

## SP VS Function 
# Difference Between Stored Procedures (SP) and Functions in SQL Server (MSSQL)

| Feature                           | Stored Procedure (SP)                                            | Function                                                    |
|-----------------------------------|------------------------------------------------------------------|-------------------------------------------------------------|
| **Purpose**                       | To perform a specific task or operation, which may involve data manipulation, calling other procedures, etc. | To compute a value and return a result, usually for a single calculation or transformation. |
| **Return Type**                   | Can return zero, one, or multiple values (using `OUTPUT` parameters or `RETURN` status). | Must return a single value (scalar or table type).           |
| **Calling Mechanism**             | Executed using the `EXEC` or `EXECUTE` keyword.                  | Invoked as part of a SQL statement like `SELECT`, `WHERE`, or `FROM` clause. |
| **Allowed Statements**            | Can contain a wide range of T-SQL statements including `INSERT`, `UPDATE`, `DELETE`, and `SELECT`. | Can only contain `SELECT` statements and cannot modify the database state. |
| **Side Effects**                  | Can change the state of the database (insert, update, delete data). | Cannot have side effects; cannot perform modifications to the database state. |
| **Error Handling**                | Supports error handling using `TRY...CATCH`.                     | Does not support structured error handling (`TRY...CATCH`).  |
| **Transaction Management**        | Can support and manage transactions using `BEGIN TRANSACTION`, `COMMIT`, or `ROLLBACK`. | Cannot manage transactions directly; operates within the context of a calling transaction. |
| **Usage in SQL Statements**       | Cannot be used in `SELECT`, `WHERE`, or `JOIN` clauses.           | Can be used directly in SQL statements such as `SELECT`, `WHERE`, or `JOIN`. |
| **Permission Level**              | Requires higher permissions due to its ability to modify the database. | Requires lower permissions, typically read-only.             |
| **Compilation**                   | Compiled once and stored in a cached plan; execution plan can be reused. | Also compiled once, and execution plan can be reused for performance. |
| **Output Parameters**             | Supports output parameters to return multiple values.            | Does not support output parameters; returns a single value directly. |
| **Execution Plan Reusability**    | Can reuse the execution plan, but may vary due to parameter sniffing or recompilation. | Typically more deterministic in reusing execution plans.     |
| **Can Call Other Routines**       | Can call other stored procedures and functions.                  | Can only call other functions; cannot call stored procedures. |
| **Performance**                   | Typically faster for complex, multi-step operations due to caching and batch processing. | Generally faster for scalar computations and reusable logic within queries. |
| **Return Keyword**                | Can use `RETURN` to return an integer status code.                | Must use `RETURN` to provide a value to the caller.          |
| **Schema Binding**                | Can be created without schema binding.                           | Can be created with `SCHEMABINDING` to prevent changes in the underlying objects. |

### Key Points to Remember

- **Stored Procedures** are ideal for performing complex tasks, such as data manipulation, batch processing, and administrative tasks. They can change the state of the database and support transactions and error handling.
- **Functions** are best suited for computations, transformations, and returning scalar or table values within SQL queries. They cannot modify the database state or handle transactions.

### Example Syntax

- **Stored Procedure:**
    ```sql
    CREATE PROCEDURE GetEmployeeInfo 
    @EmployeeID INT
    AS
    BEGIN
        SELECT * FROM Employees WHERE EmployeeID = @EmployeeID;
    END;
    ```

- **Function:**
    ```sql
    CREATE FUNCTION GetEmployeeSalary (@EmployeeID INT)
    RETURNS INT
    AS
    BEGIN
        DECLARE @Salary INT;
        SELECT @Salary = Salary FROM Employees WHERE EmployeeID = @EmployeeID;
        RETURN @Salary;
    END;
    ```

Use **Stored Procedures** when you need to perform operations that require flexibility, transaction management, or modify the database state. Use **Functions** when you need to perform calculations or return a value that can be easily integrated into SQL queries.

## User Defined Datatypes and tables : 
- **User-Defined Data Types (UDDT)** : 
 it is a custom data type based on existing SQL Server data types, used to enforce consistency and constraints across multiple database objects.
 ```SQL
  CREATE TYPE Rupee FROM CHAR(11);
 ```
- **User-Defined Table**: it is a custom table structure used to define a table-valued parameter that can be passed to stored procedures or functions.
```SQL
CREATE TYPE dbo.EmployeeType AS TABLE
(
    EmployeeID INT,
    EmployeeName VARCHAR(100),
    DepartmentID INT
);

```

## Joins
- **OUTER JOIN** : 
    - ***Left Outer Join/Left join*** : 
        all entries from left table and matching from right
    - ***Right outer join/Right Join*** :
        all entries from right table and matching from left
    - ***Full outer join*** : 
        Returns all rows when there is a match in either left or right table records.
    - ***Exclusive left/right join*** :  
    The purpose of an Exclusive Left Join is to find records in the left table that have no corresponding entries in the right table. 
    ```SQL
        SELECT table1.*
        FROM table1
        LEFT JOIN table2 ON table1.common_column = table2.common_column
        WHERE table2.common_column IS NULL;
    ```
- **INNER JOIN** : 
    Retrieves only the rows with matching values in both tables.
- **CROSS JOIN:** : 
    Cartesian product of the two tables.meaning every row in the first table is paired with every row in the second table.
- **SELF JOIN**
    A join in which a table is joined with itself.

**Key Points to Remember**
- JOIN without any keyword usually implies INNER JOIN.
- LEFT JOIN and RIGHT JOIN are called OUTER JOINS because they retrieve data beyond the inner join matches.
- Cross Join is rarely used due to the potentially large result set, but it can be helpful in generating combinations.
- Self Join is used to join a table to itself for hierarchical or comparative data.



## SQL Triggers in MSSQL

### What is a Trigger?
 **Trigger** is a special type of stored procedure in SQL Server that automatically executes (or "fires") in response to certain events on a table or view. Triggers can be used to enforce business rules, maintain data integrity, and perform automated tasks.

### Types of Triggers
1. **AFTER Trigger**: Executes after an `INSERT`, `UPDATE`, or `DELETE` operation is completed. It is commonly used for auditing, logging, or enforcing complex business rules.
2. **INSTEAD OF Trigger**: Executes in place of an `INSERT`, `UPDATE`, or `DELETE` operation, allowing custom behavior or conditional logic before the actual modification occurs.
3. **DDL Trigger**: Executes in response to Data Definition Language (DDL) events like `CREATE`, `ALTER`, or `DROP` (e.g., table, view, database changes).

### Syntax to Create a Trigger
```sql
CREATE TRIGGER trigger_name
ON table_name
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    -- Trigger logic
    PRINT 'Trigger executed!';
END;
```

## Specialized Tables in SQL

### 1. Magic Table
- **Description**: internal tables created during the execution of triggers. These tables are not physical tables but exist temporarily during the trigger execution.
- **Purpose**: Used to hold the `INSERTED` and `DELETED` data within triggers.
- **Example**:
  - `INSERTED` table: Contains the new values being inserted or updated.
  - `DELETED` table: Contains the old values being deleted or updated.
- **Use Case**: Commonly used in `AFTER INSERT`, `AFTER UPDATE`, and `AFTER DELETE` triggers to compare old and new values or maintain audit logs.

### 2. Temporal Table (System-Versioned Table)
- **Description**: Temporal tables are tables that automatically track historical data changes over time. Introduced in SQL Server 2016 and supported in other databases like Oracle.
- **Purpose**: To keep a full history of data changes and easily perform queries on data at any point in time.
- **Syntax**:
  ```sql
  CREATE TABLE table_name
  (
    column1 datatype,
    column2 datatype,
    ...
    PERIOD FOR SYSTEM_TIME (start_time_column, end_time_column)
  )
  WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = history_table_name));
  ```

### Pivot Table : 
- **Description**: A table that reshapes the data from rows to columns, allowing for summary and analysis. Not a specific table type, but a result of the PIVOT operation in SQL.
- **Purpose**: To summarize data and provide aggregated views of data, such as sums or averages grouped by specific columns.
 ```SQL
    SELECT * 
    FROM (SELECT columns FROM table_name)
    PIVOT (AGGREGATE_FUNCTION(value_column) FOR pivot_column IN (value1, value2, ...)) AS pivot_table;

  ```

### Partitioned Table : 
- **Description**: A table whose data is divided into multiple partitions based on specified column values, improving query performance on large datasets.
- **Purpose**: To manage and query large datasets more efficiently by dividing them into smaller, manageable pieces.

```SQL
CREATE TABLE table_name
(
  column1 datatype,
  column2 datatype,
  ...
)
PARTITION BY RANGE (partition_column) 
(
  PARTITION partition_name VALUES LESS THAN (value1),
  PARTITION partition_name VALUES LESS THAN (value2)
  ...
);

```

## CTE(Comman Table Expression) : 
```SQL
WITH CTE_name (column1, column2, ...)
AS
(
  SELECT columns FROM table_name
)
SELECT * FROM CTE_name;

```