# SQL Commands

This chapter covers the **5 major categories of SQL commands**:

1. **DDL** — Data Definition Language
2. **DML** — Data Manipulation Language
3. **DQL** — Data Query Language
4. **DCL** — Data Control Language
5. **TCL** — Transaction Control Language

We will use **MySQL syntax** and one common database throughout the chapter.

---

# 1. SQL Command Categories

| Category | Full Form                    | Main Purpose                  | Important Commands                              |
| -------- | ---------------------------- | ----------------------------- | ----------------------------------------------- |
| **DDL**  | Data Definition Language     | Structure of database objects | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` |
| **DML**  | Data Manipulation Language   | Add/change/delete data        | `INSERT`, `UPDATE`, `DELETE`                    |
| **DQL**  | Data Query Language          | Retrieve data                 | `SELECT`                                        |
| **DCL**  | Data Control Language        | Control permissions           | `GRANT`, `REVOKE`                               |
| **TCL**  | Transaction Control Language | Manage transactions           | `COMMIT`, `ROLLBACK`, `SAVEPOINT`               |

### Easy way to remember

```text
DDL → Structure
DML → Data
DQL → Query
DCL → Permission
TCL → Transaction
```

---

# 2. Create Our Database

We will use a database called `company_db`.

```sql
CREATE DATABASE company_db;
```

Select the database:

```sql
USE company_db;
```

Check available databases:

```sql
SHOW DATABASES;
```

Check the currently selected database:

```sql
SELECT DATABASE();
```

---

# PART A — DDL

# 3. DDL — Data Definition Language

**DDL is used to create and modify the structure of database objects.**

Common DDL commands:

```text
CREATE
ALTER
DROP
TRUNCATE
RENAME
```

Think:

> **DDL = Structure of the database**

---

# 3.1. CREATE

`CREATE` is used to create database objects.

It can create:

* Database
* Table
* View
* Index
* etc.

## Create Table

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    department VARCHAR(30),
    salary DECIMAL(10,2),
    city VARCHAR(30)
);
```

Our table structure:

| Column        | Data Type     |
| ------------- | ------------- |
| employee_id   | INT           |
| employee_name | VARCHAR(50)   |
| department    | VARCHAR(30)   |
| salary        | DECIMAL(10,2) |
| city          | VARCHAR(30)   |

---

# 3.2. DESCRIBE

`DESCRIBE` is used to see the structure of a table.

```sql
DESCRIBE employees;
```

You can also use:

```sql
DESC employees;
```

---

# 3.3. ALTER TABLE

`ALTER` modifies the structure of an existing table.

## Add a Column

Suppose we want to add `email`.

```sql
ALTER TABLE employees
ADD email VARCHAR(100);
```

Now the table contains:

```text
employee_id
employee_name
department
salary
city
email
```

---

## 3.4. Add Multiple Columns

```sql
ALTER TABLE employees
ADD phone VARCHAR(15),
ADD joining_date DATE;
```

---

# 3.5. Modify a Column

Suppose `employee_name` is currently:

```sql
VARCHAR(50)
```

We want:

```sql
VARCHAR(100)
```

MySQL:

```sql
ALTER TABLE employees
MODIFY employee_name VARCHAR(100);
```

---

# 3.6. Rename a Column

MySQL:

```sql
ALTER TABLE employees
RENAME COLUMN city TO location;
```

Now:

```text
city
```

becomes:

```text
location
```

---

# 3.7. Drop a Column

Suppose we don't need `phone`.

```sql
ALTER TABLE employees
DROP COLUMN phone;
```

The column is removed.

---

# 3.8. RENAME TABLE

Rename:

```sql
ALTER TABLE employees
RENAME TO staff;
```

Now the table name is:

```text
staff
```

Rename it back:

```sql
ALTER TABLE staff
RENAME TO employees;
```

---

# 3.9. DROP

`DROP` permanently removes a database object.

For example:

```sql
DROP TABLE employees;
```

The table and its structure are removed.

You can also drop a database:

```sql
DROP DATABASE company_db;
```

### Important

`DROP` is different from `DELETE`.

```text
DROP
→ Removes table/object

DELETE
→ Removes rows
```

---

# 3.10. TRUNCATE

`TRUNCATE` removes **all rows** from a table while keeping the table structure.

```sql
TRUNCATE TABLE employees;
```

Before:

```text
employees
100 rows
```

After:

```text
employees
0 rows
```

But:

```text
employee_id
employee_name
department
salary
city
```

still exist as columns.

### Difference

```text
DROP
→ Structure + data removed

TRUNCATE
→ Data removed
→ Structure remains

DELETE
→ Selected/all rows removed
→ Structure remains
```

---

# 3.11. DDL Summary

| Command    | Purpose          |
| ---------- | ---------------- |
| `CREATE`   | Create object    |
| `ALTER`    | Modify structure |
| `DROP`     | Remove object    |
| `TRUNCATE` | Remove all rows  |
| `RENAME`   | Rename object    |

---

# PART B — DML

# 4. DML — Data Manipulation Language

DML is used to **modify the data stored inside tables**.

Main commands:

```text
INSERT
UPDATE
DELETE
```

Think:

> **DML = Data inside the table**

---

# 4.1. INSERT

`INSERT` adds records to a table.

First recreate our table if necessary:

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(100),
    department VARCHAR(30),
    salary DECIMAL(10,2),
    city VARCHAR(30)
);
```

## Insert One Row

```sql
INSERT INTO employees
VALUES
(101, 'Rohit', 'IT', 55000, 'Mumbai');
```

---

# 4.2. Insert Multiple Rows

```sql
INSERT INTO employees
VALUES
(102, 'Amit', 'Sales', 45000, 'Pune'),
(103, 'Priya', 'HR', 50000, 'Mumbai'),
(104, 'Neha', 'IT', 65000, 'Delhi'),
(105, 'Rahul', 'Finance', 60000, 'Pune');
```

---

# 4.3. INSERT with Column Names

This is generally safer and clearer.

```sql
INSERT INTO employees
(employee_id, employee_name, department, salary, city)
VALUES
(106, 'Anjali', 'Marketing', 52000, 'Mumbai');
```

---

# 4.4. Complete Dataset

Let's create a larger dataset for exercises.

```sql
INSERT INTO employees
(employee_id, employee_name, department, salary, city)
VALUES
(101, 'Rohit', 'IT', 55000, 'Mumbai'),
(102, 'Amit', 'Sales', 45000, 'Pune'),
(103, 'Priya', 'HR', 50000, 'Mumbai'),
(104, 'Neha', 'IT', 65000, 'Delhi'),
(105, 'Rahul', 'Finance', 60000, 'Pune'),
(106, 'Anjali', 'Marketing', 52000, 'Mumbai'),
(107, 'Vikas', 'Sales', 48000, 'Delhi'),
(108, 'Sneha', 'HR', 58000, 'Pune'),
(109, 'Karan', 'IT', 72000, 'Mumbai'),
(110, 'Pooja', 'Finance', 68000, 'Delhi');
```

Check:

```sql
SELECT * FROM employees;
```

---

# 4.5. UPDATE

`UPDATE` modifies existing records.

## Update One Employee's Salary

```sql
UPDATE employees
SET salary = 60000
WHERE employee_id = 101;
```

Rohit's salary changes from:

```text
55000
```

to:

```text
60000
```

### Important

Always be careful with `WHERE`.

```sql
UPDATE employees
SET salary = 60000;
```

This updates **every employee**.

---

# 4.6. Update Multiple Columns

```sql
UPDATE employees
SET salary = 70000,
    city = 'Bangalore'
WHERE employee_id = 104;
```

---

# 4.7. Update Using a Condition

Give a ₹5,000 salary increase to IT employees:

```sql
UPDATE employees
SET salary = salary + 5000
WHERE department = 'IT';
```

This is an important real-world SQL operation.

---

# 4.8. DELETE

`DELETE` removes records.

Delete one employee:

```sql
DELETE FROM employees
WHERE employee_id = 102;
```

---

# 4.9. DELETE Multiple Rows

Delete all employees from Sales:

```sql
DELETE FROM employees
WHERE department = 'Sales';
```

---

# 4.10. DELETE All Rows

```sql
DELETE FROM employees;
```

This removes all records but keeps the table.

Compare:

```text
DELETE FROM employees;
```

with:

```sql
TRUNCATE TABLE employees;
```

Both remove all rows, but their transaction/logging/identity behavior can differ by DBMS. For MySQL specifically, `TRUNCATE` is treated differently from ordinary row-by-row `DELETE`.

---

# 4.11. DML Summary

| Command  | Purpose        |
| -------- | -------------- |
| `INSERT` | Add records    |
| `UPDATE` | Modify records |
| `DELETE` | Remove records |

---

# PART C — DQL

# 5. DQL — Data Query Language

DQL is used to **retrieve data from a database**.

Main command:

```sql
SELECT
```

Think:

> **DQL = Ask questions from the database**

---

# 5.1. SELECT All Columns

```sql
SELECT *
FROM employees;
```

`*` means all columns.

---

# 5.2. SELECT Specific Columns

```sql
SELECT employee_name, salary
FROM employees;
```

---

# 5.3. SELECT with Condition

```sql
SELECT *
FROM employees
WHERE salary > 60000;
```

---

# 5.4. SELECT with Multiple Conditions

```sql
SELECT *
FROM employees
WHERE department = 'IT'
AND salary > 60000;
```

---

# 5.5. DISTINCT

Find unique cities:

```sql
SELECT DISTINCT city
FROM employees;
```

Find unique departments:

```sql
SELECT DISTINCT department
FROM employees;
```

---

# 5.6. ORDER BY

Sort salary from highest to lowest:

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC;
```

Lowest to highest:

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary ASC;
```

---

# 5.7. LIMIT

Get the top 5 employees by salary:

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC
LIMIT 5;
```

---

# 5.8. DQL Summary

For now, remember:

```text
SELECT
FROM
WHERE
DISTINCT
ORDER BY
LIMIT
```

Later, DQL becomes much more powerful with:

```text
GROUP BY
HAVING
JOIN
Subquery
CTE
Window Functions
```

---

# PART D — DCL

# 6. DCL — Data Control Language

DCL manages **database permissions and access**.

Main commands:

```text
GRANT
REVOKE
```

DCL is mainly used by database administrators and database/security teams.

---

# 6.1. CREATE USER

Before granting permissions, a user can be created.

MySQL example:

```sql
CREATE USER 'analyst'@'localhost'
IDENTIFIED BY 'StrongPassword123!';
```

---

# 6.2. GRANT

`GRANT` gives privileges.

Give permission to read a database:

```sql
GRANT SELECT
ON company_db.*
TO 'analyst'@'localhost';
```

The analyst can now use `SELECT` on tables in `company_db`.

---

# 6.3. Grant Multiple Permissions

```sql
GRANT SELECT, INSERT, UPDATE
ON company_db.*
TO 'analyst'@'localhost';
```

The user receives:

```text
SELECT
INSERT
UPDATE
```

---

# 6.4. Grant All Privileges

```sql
GRANT ALL PRIVILEGES
ON company_db.*
TO 'analyst'@'localhost';
```

Use this carefully in real systems. Users should generally receive only the permissions required for their job.

---

# 6.5. SHOW GRANTS

Check permissions:

```sql
SHOW GRANTS FOR 'analyst'@'localhost';
```

---

# 6.6. REVOKE

Remove a permission.

```sql
REVOKE INSERT
ON company_db.*
FROM 'analyst'@'localhost';
```

Now the user no longer has the `INSERT` privilege granted by that statement.

---

# 6.7. DCL Summary

| Command  | Purpose           |
| -------- | ----------------- |
| `GRANT`  | Give permission   |
| `REVOKE` | Remove permission |

### Simple example

```text
GRANT
Teacher gives permission

REVOKE
Teacher takes permission back
```

---

# PART E — TCL

# 7. TCL — Transaction Control Language

TCL manages **transactions**.

Main commands:

```text
COMMIT
ROLLBACK
SAVEPOINT
```

Think:

> **TCL = Control changes made during a transaction**

---

# 7.1. What is a Transaction?

A transaction is a group of SQL operations treated as one logical unit.

Example:

Suppose ₹5,000 is transferred:

```text
Account A
↓
- ₹5,000

Account B
↓
+ ₹5,000
```

Both operations should succeed together.

---

# 7.2. START TRANSACTION

```sql
START TRANSACTION;
```

Then perform operations:

```sql
UPDATE employees
SET salary = salary + 5000
WHERE employee_id = 101;
```

At this point, depending on the transaction/autocommit settings and DBMS, the change may not yet be permanently committed.

---

# 7.3. COMMIT

`COMMIT` permanently commits the current transaction.

```sql
COMMIT;
```

Conceptually:

```text
START TRANSACTION
      ↓
UPDATE
      ↓
COMMIT
      ↓
Changes saved
```

---

# 7.4. ROLLBACK

`ROLLBACK` cancels uncommitted changes.

Example:

```sql
START TRANSACTION;

UPDATE employees
SET salary = salary + 10000
WHERE employee_id = 101;

ROLLBACK;
```

The salary change is undone if it was still part of the active transaction.

---

# 7.5. SAVEPOINT

A savepoint creates a point inside a transaction.

```sql
START TRANSACTION;

UPDATE employees
SET salary = salary + 5000
WHERE employee_id = 101;

SAVEPOINT salary_update;

UPDATE employees
SET salary = salary + 10000
WHERE employee_id = 102;
```

Now:

```sql
ROLLBACK TO SAVEPOINT salary_update;
```

The second update is rolled back, while the first update remains pending in the transaction.

Finally:

```sql
COMMIT;
```

---

# 7.7. TCL Flow

```text
START TRANSACTION
       ↓
   SQL Changes
       ↓
   SAVEPOINT
       ↓
 More SQL Changes
       ↓
ROLLBACK TO SAVEPOINT
       ↓
   COMMIT
```

Or:

```text
START TRANSACTION
       ↓
   SQL Changes
       ↓
    ROLLBACK
       ↓
 Changes undone
```

---

# 8. Complete Comparison

| Category | Full Form                    | Commands                                        | Main Job      |
| -------- | ---------------------------- | ----------------------------------------------- | ------------- |
| DDL      | Data Definition Language     | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` | Structure     |
| DML      | Data Manipulation Language   | `INSERT`, `UPDATE`, `DELETE`                    | Modify data   |
| DQL      | Data Query Language          | `SELECT`                                        | Retrieve data |
| DCL      | Data Control Language        | `GRANT`, `REVOKE`                               | Permissions   |
| TCL      | Transaction Control Language | `COMMIT`, `ROLLBACK`, `SAVEPOINT`               | Transactions  |

---

# 9. One Complete Example

Let's put everything together.

## Step 1 — Create Database

```sql
CREATE DATABASE company_db;

USE company_db;
```

## Step 2 — DDL: Create Table

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    city VARCHAR(50)
);
```

## Step 3 — DML: Insert Data

```sql
INSERT INTO employees
VALUES
(101, 'Rohit', 'IT', 55000, 'Mumbai'),
(102, 'Amit', 'Sales', 45000, 'Pune'),
(103, 'Priya', 'HR', 50000, 'Mumbai'),
(104, 'Neha', 'IT', 65000, 'Delhi'),
(105, 'Rahul', 'Finance', 60000, 'Pune');
```

## Step 4 — DQL: Read Data

```sql
SELECT *
FROM employees;
```

## Step 5 — DML: Update Data

```sql
UPDATE employees
SET salary = 60000
WHERE employee_id = 101;
```

## Step 6 — DQL: Verify

```sql
SELECT *
FROM employees
WHERE employee_id = 101;
```

## Step 7 — TCL: Transaction

```sql
START TRANSACTION;

UPDATE employees
SET salary = salary + 5000
WHERE department = 'IT';

ROLLBACK;
```

The salary increase is cancelled if the transaction remains uncommitted.

---

# 10. Solved Exercises

## Exercise 1 — Create a Products Table

### Question

Create a table named `products` with:

```text
product_id
product_name
category
price
quantity
```

### Solution

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    quantity INT
);
```

**Category:** DDL

---

# Exercise 2 — Insert Products

### Question

Insert these records:

| ID | Product  | Category    | Price | Quantity |
| -: | -------- | ----------- | ----: | -------: |
|  1 | Laptop   | Electronics | 55000 |       10 |
|  2 | Mouse    | Electronics |   800 |       50 |
|  3 | Keyboard | Electronics |  1500 |       30 |
|  4 | Chair    | Furniture   |  5000 |       20 |
|  5 | Desk     | Furniture   |  8000 |       15 |

### Solution

```sql
INSERT INTO products
VALUES
(1, 'Laptop', 'Electronics', 55000, 10),
(2, 'Mouse', 'Electronics', 800, 50),
(3, 'Keyboard', 'Electronics', 1500, 30),
(4, 'Chair', 'Furniture', 5000, 20),
(5, 'Desk', 'Furniture', 8000, 15);
```

**Category:** DML

---

# Exercise 3 — Display Products

### Question

Display all products.

### Solution

```sql
SELECT *
FROM products;
```

**Category:** DQL

---

# Exercise 4 — Find Expensive Products

### Question

Display products whose price is greater than `5000`.

### Solution

```sql
SELECT *
FROM products
WHERE price > 5000;
```

---

# Exercise 5 — Update Price

### Question

Increase the Laptop price to `60000`.

### Solution

```sql
UPDATE products
SET price = 60000
WHERE product_id = 1;
```

**Category:** DML

---

# Exercise 6 — Update Quantity

### Question

Increase the quantity of Mouse by 20.

### Solution

```sql
UPDATE products
SET quantity = quantity + 20
WHERE product_id = 2;
```

---

# Exercise 7 — Delete Product

### Question

Delete the Keyboard.

### Solution

```sql
DELETE FROM products
WHERE product_id = 3;
```

---

# Exercise 8 — Add Column

### Question

Add a column called `brand`.

### Solution

```sql
ALTER TABLE products
ADD brand VARCHAR(50);
```

**Category:** DDL

---

# Exercise 9 — Rename Column

### Question

Rename `brand` to `company`.

### Solution

```sql
ALTER TABLE products
RENAME COLUMN brand TO company;
```

---

# Exercise 10 — Transaction

### Question

Increase all product prices by 10%, but then cancel the operation.

### Solution

```sql
START TRANSACTION;

UPDATE products
SET price = price * 1.10;

ROLLBACK;
```

---

# Exercise 11 — Transaction with COMMIT

### Question

Increase Laptop quantity by 5 and permanently save the change.

### Solution

```sql
START TRANSACTION;

UPDATE products
SET quantity = quantity + 5
WHERE product_id = 1;

COMMIT;
```

---

# Exercise 12 — SAVEPOINT

### Question

Update two products but roll back only the second update.

### Solution

```sql
START TRANSACTION;

UPDATE products
SET price = price + 1000
WHERE product_id = 1;

SAVEPOINT first_update;

UPDATE products
SET price = price + 500
WHERE product_id = 2;

ROLLBACK TO SAVEPOINT first_update;

COMMIT;
```

The first update is committed.

The second update is rolled back.

---

# 11. Practice Questions

Try these **without looking at the solutions**.

## DDL Practice

### Q1

Create a database called:

```text
school_db
```

### Q2

Create a table called `students` with:

```text
student_id
student_name
course
fees
city
```

### Q3

Add an `email` column.

### Q4

Modify `student_name` from `VARCHAR(50)` to `VARCHAR(100)`.

### Q5

Rename `city` to `location`.

### Q6

Remove the `email` column.

### Q7

Rename the table from `students` to `student_details`.

---

# DML Practice

### Q8

Insert 5 students.

### Q9

Update the fees of one student.

### Q10

Increase fees of all students by 10%.

### Q11

Change the city of a particular student.

### Q12

Delete one student.

### Q13

Delete all students from Mumbai.

### Q14

Insert 3 more students using a single `INSERT` statement.

---

# DQL Practice

### Q15

Display all students.

### Q16

Display only:

```text
student_name
course
fees
```

### Q17

Display students whose fees are greater than `50000`.

### Q18

Display students from Mumbai.

### Q19

Display students from Mumbai or Pune.

### Q20

Display unique cities.

### Q21

Display students ordered by fees from highest to lowest.

### Q22

Display the top 3 students based on fees.

### Q23

Display students whose names start with `R`.

### Q24

Display students whose fees are between `30000` and `60000`.

---

# DCL Practice

### Q25

Create a user called:

```text
analyst
```

### Q26

Give the analyst `SELECT` permission on `school_db`.

### Q27

Give the analyst `SELECT` and `INSERT` permissions.

### Q28

Remove `INSERT` permission.

### Q29

Display the analyst's permissions.

---

# TCL Practice

### Q30

Start a transaction and increase one student's fees.

### Q31

Undo the update using `ROLLBACK`.

### Q32

Update a student's fees and permanently save using `COMMIT`.

### Q33

Perform two updates with a `SAVEPOINT`.

### Q34

Roll back only the second update using the savepoint.

---

# 54. Final Revision

Remember this one line:

```text
DDL → CREATE / ALTER / DROP / TRUNCATE
DML → INSERT / UPDATE / DELETE
DQL → SELECT
DCL → GRANT / REVOKE
TCL → COMMIT / ROLLBACK / SAVEPOINT
```

And the most important conceptual distinction:

```text
          SQL
           │
 ┌─────────┼──────────┐
 │         │          │
DDL       DML        DQL
 │         │          │
Structure  Data       Retrieve
 │         │          │
CREATE     INSERT     SELECT
ALTER      UPDATE
DROP       DELETE
TRUNCATE
           │
     ┌─────┴─────┐
     │           │
    DCL         TCL
     │           │
Permissions  Transactions
     │           │
GRANT        COMMIT
REVOKE       ROLLBACK
             SAVEPOINT
```

**Next logical chapter:** `SELECT → WHERE → Operators → LIKE → IN → BETWEEN → ORDER BY → LIMIT → DISTINCT`, followed by **GROUP BY + HAVING and then complete SQL JOINs**.
