# SQL Practice Exercise – Basic to Intermediate

This practice exercise covers the SQL concepts from your 49 MCQs, including DDL, DML, DQL, DCL, TCL, filtering, sorting, aggregate functions, grouping, operators, and constraints.

You will practise using a real database structure with sample data, questions, and solved SQL queries.

## 1. Create the database and tables

We will use a CompanyDB database with three tables:

- `Departments` — department details
- `Employees` — employee information and salaries
- `Sales` — employee sales transactions

The SQL syntax below is suitable for MySQL.

### Step 1: Create the database

```
CREATE DATABASE CompanyDB;

USE CompanyDB;
```

### Step 2: Create the Departments table

```
CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(50) UNIQUE NOT NULL,
    Location VARCHAR(50) DEFAULT 'Mumbai'
);
```

### Step 3: Create the Employees table

```
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    EmployeeName VARCHAR(50) NOT NULL,
    Age INT CHECK (Age >= 18),
    Gender VARCHAR(10),
    Salary DECIMAL(10,2) CHECK (Salary > 0),
    DepartmentID INT,
    Email VARCHAR(100) UNIQUE,
    City VARCHAR(50) DEFAULT 'Mumbai',
    FOREIGN KEY (DepartmentID)
        REFERENCES Departments(DepartmentID)
);
```

### Step 4: Create the Sales table

```
CREATE TABLE Sales (
    SaleID INT PRIMARY KEY,
    EmployeeID INT,
    Product VARCHAR(50) NOT NULL,
    Category VARCHAR(50),
    Quantity INT CHECK (Quantity > 0),
    Amount DECIMAL(10,2) CHECK (Amount >= 0),
    SaleDate DATE,
    FOREIGN KEY (EmployeeID)
        REFERENCES Employees(EmployeeID)
);
```

## 2. Insert sample data

### Departments

```
INSERT INTO Departments
(DepartmentID, DepartmentName, Location)
VALUES
(1, 'IT', 'Mumbai'),
(2, 'HR', 'Pune'),
(3, 'Sales', 'Delhi'),
(4, 'Finance', 'Nashik'),
(5, 'Marketing', 'Mumbai');
```

### Employees

```
INSERT INTO Employees
(EmployeeID, EmployeeName, Age, Gender, Salary,
 DepartmentID, Email, City)
VALUES
(101, 'Rohit', 30, 'Male', 45000, 1,
 'rohit@gmail.com', 'Mumbai'),
(102, 'Priya', 28, 'Female', 55000, 2,
 'priya@gmail.com', 'Pune'),
(103, 'Amit', 32, 'Male', 60000, 3,
 'amit@gmail.com', 'Delhi'),
(104, 'Neha', 26, 'Female', 40000, 1,
 'neha@gmail.com', 'Mumbai'),
(105, 'Vikas', 35, 'Male', 75000, 4,
 'vikas@gmail.com', 'Nashik'),
(106, 'Sneha', 29, 'Female', 50000, 3,
 'sneha@gmail.com', 'Pune'),
(107, 'Arjun', 24, 'Male', 35000, 5,
 'arjun@gmail.com', 'Mumbai'),
(108, 'Pooja', 31, 'Female', 65000, 2,
 'pooja@gmail.com', 'Delhi'),
(109, 'Karan', 27, 'Male', 48000, 3,
 'karan@gmail.com', 'Nashik'),
(110, 'Anjali', 33, 'Female', 70000, 4,
 'anjali@gmail.com', 'Pune');
```

### Sales

```
INSERT INTO Sales
(SaleID, EmployeeID, Product, Category,
 Quantity, Amount, SaleDate)
VALUES
(1, 101, 'Laptop', 'Electronics', 2, 100000, '2026-01-05'),
(2, 102, 'Chair', 'Furniture', 5, 25000, '2026-01-10'),
(3, 103, 'Mouse', 'Electronics', 10, 5000, '2026-01-15'),
(4, 104, 'Laptop', 'Electronics', 1, 50000, '2026-02-03'),
(5, 105, 'Desk', 'Furniture', 3, 45000, '2026-02-08'),
(6, 106, 'Keyboard', 'Electronics', 6, 12000, '2026-02-12'),
(7, 101, 'Monitor', 'Electronics', 4, 40000, '2026-03-01'),
(8, 107, 'Chair', 'Furniture', 2, 10000, '2026-03-05'),
(9, 108, 'Laptop', 'Electronics', 3, 150000, '2026-03-10'),
(10, 103, 'Desk', 'Furniture', 2, 30000, '2026-03-15'),
(11, 109, 'Mouse', 'Electronics', 8, 4000, '2026-04-02'),
(12, 110, 'Keyboard', 'Electronics', 5, 10000, '2026-04-07'),
(13, 102, 'Monitor', 'Electronics', 2, 20000, '2026-04-12'),
(14, 106, 'Chair', 'Furniture', 4, 20000, '2026-05-01'),
(15, 105, 'Laptop', 'Electronics', 2, 100000, '2026-05-05');
```

Check your tables:

```
SELECT * FROM Departments;
SELECT * FROM Employees;
SELECT * FROM Sales;
```

## 3. Practice questions with answers

## Exercise controls

Try each question in MySQL Workbench or your SQL editor before revealing the solutions.

All topics (35 questions)

Part A — Basic queries (1–10)

Part B — Aggregate functions (11–18)

Part C — Operators and constraints (19–26)

Part D — DDL, DML and transactions (27–35)

Hide answers

## Part A — Basic queries (1–10)

Topics: SELECT, WHERE, DISTINCT, ORDER BY, LIKE, BETWEEN, IN and NULL.

Q1. Display all records from the Employees table.

```
SELECT * FROM Employees;
```

Q2. Display only employee names and salaries.

```
SELECT EmployeeName, Salary
FROM Employees;
```

Q3. Find employees earning more than ₹50,000.

```
SELECT *
FROM Employees
WHERE Salary > 50000;
```

Q4. Display employees who live in Mumbai.

```
SELECT *
FROM Employees
WHERE City = 'Mumbai';
```

Q5. Display unique employee cities.

```
SELECT DISTINCT City
FROM Employees;
```

Q6. Display employees in descending order of salary.

```
SELECT *
FROM Employees
ORDER BY Salary DESC;
```

Q7. Find employees whose names start with 'A'.

```
SELECT *
FROM Employees
WHERE EmployeeName LIKE 'A%';
```

Q8. Find employees with salaries between ₹40,000 and ₹60,000, inclusive.

```
SELECT *
FROM Employees
WHERE Salary BETWEEN 40000 AND 60000;
```

Q9. Find employees working in department 1 or department 3.

```
SELECT *
FROM Employees
WHERE DepartmentID IN (1, 3);
```

Q10. Find employees whose email is missing.

```
SELECT *
FROM Employees
WHERE Email IS NULL;
```

The supplied data has no missing emails, so this query returns zero rows until you insert or update a record with a NULL email.

## Part B — Aggregate functions (11–18)

Topics: COUNT, SUM, AVG, MIN, MAX, GROUP BY and HAVING.

Q11. Count the total number of employees.

```
SELECT COUNT(*) AS TotalEmployees
FROM Employees;
```

Expected result: 10.

Q12. Find the total salary of all employees.

```
SELECT SUM(Salary) AS TotalSalary
FROM Employees;
```

Expected result: ₹543,000.

Q13. Find the average employee salary.

```
SELECT AVG(Salary) AS AverageSalary
FROM Employees;
```

Expected result: ₹54,300.

Q14. Find the minimum and maximum salaries.

```
SELECT
    MIN(Salary) AS MinimumSalary,
    MAX(Salary) AS MaximumSalary
FROM Employees;
```

Expected: ₹35,000 minimum and ₹75,000 maximum.

Q15. Count employees in each city.

```
SELECT City, COUNT(*) AS TotalEmployees
FROM Employees
GROUP BY City;
```

Q16. Calculate the average salary for each department.

```
SELECT DepartmentID, AVG(Salary) AS AverageSalary
FROM Employees
GROUP BY DepartmentID;
```

Q17. Display departments where the average salary exceeds ₹50,000.

```
SELECT DepartmentID, AVG(Salary) AS AverageSalary
FROM Employees
GROUP BY DepartmentID
HAVING AVG(Salary) > 50000;
```

Q18. Find total sales amount for each product category.

```
SELECT Category, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Category;
```

## Part C — Operators and constraints (19–26)

Q19. Find employees older than 25 whose salary is at least ₹50,000.

```
SELECT *
FROM Employees
WHERE Age > 25 AND Salary >= 50000;
```

Q20. Find employees whose names end with 'a'.

```
SELECT *
FROM Employees
WHERE EmployeeName LIKE '%a';
```

Q21. Find sales transactions recorded in February 2026.

```
SELECT *
FROM Sales
WHERE SaleDate BETWEEN '2026-02-01' AND '2026-02-28';
```

Q22. Find sales transactions for either Furniture or Electronics.

```
SELECT *
FROM Sales
WHERE Category IN ('Furniture', 'Electronics');
```

Q23. Display each employee's name and department name.

```
SELECT e.EmployeeName, d.DepartmentName
FROM Employees AS e
INNER JOIN Departments AS d
    ON e.DepartmentID = d.DepartmentID;
```

This introduces INNER JOIN to connect related tables using a foreign key.

Q24. Display all departments, including departments without employees.

```
SELECT d.DepartmentName, e.EmployeeName
FROM Departments AS d
LEFT JOIN Employees AS e
    ON d.DepartmentID = e.DepartmentID;
```

Q25. Create a test table with a primary key, a unique email, and a NOT NULL name.

```
CREATE TABLE TestEmployees (
    EmployeeID INT PRIMARY KEY,
    EmployeeName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE
);
```

Q26. Create a table that allows only ages of 18 or above and assigns Mumbai as the default city.

```
CREATE TABLE TestStudents (
    StudentID INT PRIMARY KEY,
    Age INT CHECK (Age >= 18),
    City VARCHAR(50) DEFAULT 'Mumbai'
);
```

## Part D — DDL, DML and transactions (27–35)

Q27. Add a new column called `Phone` to the Employees table.

```
ALTER TABLE Employees
ADD Phone VARCHAR(15);
```

Q28. Increase Rohit's salary to ₹48,000.

```
UPDATE Employees
SET Salary = 48000
WHERE EmployeeName = 'Rohit';
```

Always check the WHERE condition before running UPDATE.

Q29. Delete the employee whose ID is 107.

```
DELETE FROM Employees
WHERE EmployeeID = 107;
```

This will fail while employee 107 has a related sales record, unless you first handle that dependent record. This is a foreign-key restriction in the sample schema.

Q30. Create a view showing employees earning at least ₹60,000.

```
CREATE VIEW HighSalaryEmployees AS
SELECT EmployeeID, EmployeeName, Salary
FROM Employees
WHERE Salary >= 60000;
```

Q31. Display employees from highest to lowest salary, showing only the first three.

```
SELECT EmployeeName, Salary
FROM Employees
ORDER BY Salary DESC
LIMIT 3;
```

MySQL uses LIMIT to restrict the number of returned rows.

Q32. Find employees who have made at least one sale.

```
SELECT e.EmployeeID, e.EmployeeName
FROM Employees AS e
WHERE EXISTS (
    SELECT 1
    FROM Sales AS s
    WHERE s.EmployeeID = e.EmployeeID
);
```

Q33. Start a transaction, update Rohit's salary, and undo the change.

```
START TRANSACTION;

UPDATE Employees
SET Salary = 50000
WHERE EmployeeID = 101;

ROLLBACK;
```

ROLLBACK undoes the transaction's uncommitted change. Run this in a transactional table engine such as InnoDB.

Q34. Start a transaction, update Rohit's salary, and save the change permanently.

```
START TRANSACTION;

UPDATE Employees
SET Salary = 50000
WHERE EmployeeID = 101;

COMMIT;
```

Q35. Create a savepoint, make two updates, then undo only the second update.

```
START TRANSACTION;

UPDATE Employees
SET Salary = 47000
WHERE EmployeeID = 101;

SAVEPOINT FirstUpdate;

UPDATE Employees
SET Salary = 58000
WHERE EmployeeID = 102;

ROLLBACK TO SAVEPOINT FirstUpdate;

COMMIT;
```

The first update is committed. The second update is undone because it occurred after the savepoint.

## 4. Important concepts to remember

| Concept           | Command or rule                                         | Purpose                              |
| ----------------- | ------------------------------------------------------- | ------------------------------------ |
| DDL               | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`                   | Define or change database structures |
| DML               | `INSERT`, `UPDATE`, `DELETE`                            | Modify data                          |
| DQL               | `SELECT`                                                | Retrieve data                        |
| DCL               | `GRANT`, `REVOKE`                                       | Manage permissions                   |
| TCL               | `COMMIT`, `ROLLBACK`, `SAVEPOINT`                       | Manage transactions                  |
| Filtering         | `WHERE`                                                 | Filter individual rows               |
| Group filtering   | `HAVING`                                                | Filter aggregated groups             |
| Duplicate removal | `DISTINCT`                                              | Return unique results                |
| Constraints       | `PRIMARY KEY`, `UNIQUE`, `NOT NULL`, `CHECK`, `DEFAULT` | Enforce data rules                   |

Three corrections worth remembering from your MCQ test:

1. `ALTER` changes table structure, while `UPDATE` changes existing data.
2. `DELETE` removes selected rows, `TRUNCATE` removes all rows while retaining the table structure, and `DROP` removes the table itself.
3. In a basic SQL query, `SELECT` specifies the output columns and `FROM` specifies the source table. In ordinary `SELECT ... FROM ...` queries, both are required.

Note: `GRANT` and `REVOKE` are DCL statements, but their execution depends on your database privileges. Transaction behaviour and constraint support can also vary by database system.
