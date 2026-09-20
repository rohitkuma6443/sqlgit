# SQL Data Types & Constraints — Practical Notes

## MySQL

---

# 1. What is a Data Type?

A **Data Type** tells MySQL what kind of data can be stored in a column.

Think about an Excel sheet:

| Student Name | Age |     Fees | Joining Date |
| ------------ | --: | -------: | ------------ |
| Amit         |  22 | 15000.50 | 2026-01-10   |

In SQL, we must tell MySQL what type of data each column will contain.

```sql
CREATE TABLE students (
    student_name VARCHAR(50),
    age INT,
    fees DECIMAL(10,2),
    joining_date DATE
);
```

### Practical Meaning

```text
student_name → Text
age          → Whole number
fees         → Decimal number
joining_date → Date
```

---

# 2. Create Practice Database

We will use one database for all examples.

```sql
CREATE DATABASE sql_training;

USE sql_training;
```

---

# 3. VARCHAR — Text Data

## What is VARCHAR?

`VARCHAR` is used to store text of variable length.

### Real-Life Examples

```text
Student Name
Employee Name
Email
City
Address
Product Name
```

### Example

```sql
CREATE TABLE students (
    student_name VARCHAR(50)
);
```

### Insert Data

```sql
INSERT INTO students
VALUES ('Rohit');
```

Another record:

```sql
INSERT INTO students
VALUES ('Amit Kumar');
```

### Check Data

```sql
SELECT * FROM students;
```

Result:

| student_name |
| ------------ |
| Rohit        |
| Amit Kumar   |

### Why VARCHAR(50)?

`50` specifies the maximum character length for the column.

```sql
student_name VARCHAR(50)
```

means the column can store up to 50 characters.

### Practical Example

```sql
CREATE TABLE customers (
    customer_name VARCHAR(100),
    city VARCHAR(50),
    email VARCHAR(100)
);
```

---

# 4. CHAR — Fixed Length Text

`CHAR` is used when the value has a fixed length.

### Practical Example

Suppose gender is stored as:

```text
M
F
```

We can use:

```sql
gender CHAR(1)
```

### Table

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    gender CHAR(1)
);
```

### Insert

```sql
INSERT INTO employees
VALUES
(1, 'Amit', 'M'),
(2, 'Priya', 'F');
```

### Result

| employee_id | employee_name | gender |
| ----------: | ------------- | ------ |
|           1 | Amit          | M      |
|           2 | Priya         | F      |

### CHAR vs VARCHAR

```text
CHAR
→ Fixed length

VARCHAR
→ Variable length
```

Use `CHAR` when the size is fixed.

Examples:

```text
Gender → M/F
Country Code → IN/US
State Code → MH/DL
```

---

# 5. INT — Whole Numbers

`INT` stores whole numbers.

### Practical Examples

```text
Age
Quantity
Employee ID
Student ID
Product ID
```

### Example

```sql
CREATE TABLE students (
    student_id INT,
    age INT,
    quantity INT
);
```

### Insert

```sql
INSERT INTO students
VALUES
(101, 22, 5),
(102, 25, 10);
```

### Result

| student_id | age | quantity |
| ---------: | --: | -------: |
|        101 |  22 |        5 |
|        102 |  25 |       10 |

### Important

Do not use `VARCHAR` for numbers if you need mathematical calculations.

Prefer:

```sql
salary INT
```

or:

```sql
salary DECIMAL(10,2)
```

instead of:

```sql
salary VARCHAR(20)
```

---

# 6. DECIMAL — Money

`DECIMAL` is very useful in real-world business databases.

Use it for:

```text
Salary
Product Price
Fees
Invoice Amount
Sales Amount
Tax
Discount
```

### Example

```sql
CREATE TABLE products (
    product_id INT,
    product_name VARCHAR(50),
    price DECIMAL(10,2)
);
```

### Insert

```sql
INSERT INTO products
VALUES
(1, 'Laptop', 55000.50),
(2, 'Mouse', 750.75),
(3, 'Keyboard', 1500.00);
```

### Result

| product_id | product_name |    price |
| ---------: | ------------ | -------: |
|          1 | Laptop       | 55000.50 |
|          2 | Mouse        |   750.75 |
|          3 | Keyboard     |  1500.00 |

### What does DECIMAL(10,2) mean?

```text
DECIMAL(10,2)

10 → Total digits
2  → Digits after decimal
```

Example:

```text
55000.50
```

---

# 7. FLOAT

`FLOAT` stores approximate decimal numbers.

### Practical Examples

```text
Temperature
Percentage
Measurements
Scientific calculations
```

Example:

```sql
CREATE TABLE measurements (
    temperature FLOAT,
    percentage FLOAT
);
```

Insert:

```sql
INSERT INTO measurements
VALUES
(36.5, 87.75);
```

---

# 8. DOUBLE

`DOUBLE` is another floating-point data type that provides more precision than `FLOAT`.

Example:

```sql
CREATE TABLE locations (
    latitude DOUBLE,
    longitude DOUBLE
);
```

Insert:

```sql
INSERT INTO locations
VALUES
(19.0760, 72.8777);
```

This can be useful for geographical coordinates.

---

# 9. FLOAT vs DOUBLE vs DECIMAL

| Type      | Practical Use                       |
| --------- | ----------------------------------- |
| `FLOAT`   | Approximate decimal values          |
| `DOUBLE`  | Higher-precision approximate values |
| `DECIMAL` | Exact decimal values                |

### Easy Rule

```text
Money
↓
DECIMAL

Scientific / measurement
↓
FLOAT / DOUBLE
```

---

# 10. DATE

`DATE` stores a date.

Format:

```text
YYYY-MM-DD
```

Example:

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    joining_date DATE
);
```

Insert:

```sql
INSERT INTO employees
VALUES
(1, 'Amit', '2026-01-10');
```

Result:

| employee_id | employee_name | joining_date |
| ----------: | ------------- | ------------ |
|           1 | Amit          | 2026-01-10   |

### Practical Uses

```text
Date of Birth
Joining Date
Order Date
Invoice Date
Payment Date
```

---

# 11. TIME

`TIME` stores time.

Example:

```sql
CREATE TABLE classes (
    class_name VARCHAR(50),
    class_time TIME
);
```

Insert:

```sql
INSERT INTO classes
VALUES
('SQL', '10:30:00');
```

Result:

| class_name | class_time |
| ---------- | ---------- |
| SQL        | 10:30:00   |

### Practical Uses

```text
Class Time
Office Start Time
Delivery Time
Appointment Time
```

---

# 12. DATETIME

`DATETIME` stores both date and time.

Example:

```sql
CREATE TABLE logins (
    user_id INT,
    login_time DATETIME
);
```

Insert:

```sql
INSERT INTO logins
VALUES
(101, '2026-08-10 10:30:00');
```

Result:

| user_id | login_time          |
| ------: | ------------------- |
|     101 | 2026-08-10 10:30:00 |

### Practical Use

For example, an e-commerce website may record:

```text
Order placed:
2026-08-10 14:35:20
```

---

# 13. TIMESTAMP

`TIMESTAMP` also stores date and time and is commonly used to track when records are created or changed.

Example:

```sql
CREATE TABLE users (
    user_id INT,
    created_at TIMESTAMP
);
```

---

# 14. ENUM

`ENUM` allows only one value from a predefined list.

### Practical Example

Employee status:

```text
Active
Inactive
```

Create table:

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    status ENUM('Active', 'Inactive')
);
```

Valid:

```sql
INSERT INTO employees
VALUES
(1, 'Amit', 'Active');
```

Invalid:

```sql
INSERT INTO employees
VALUES
(2, 'Rahul', 'Working');
```

`Working` is not present in the ENUM list.

### Practical Use

```text
Status
Gender
Order Status
Payment Status
```

Example:

```sql
order_status ENUM(
    'Pending',
    'Shipped',
    'Delivered',
    'Cancelled'
)
```

---

# 15. SET

`SET` allows multiple values from a predefined list.

Example:

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    skills SET('Excel', 'SQL', 'Python', 'Power BI')
);
```

Insert:

```sql
INSERT INTO employees
VALUES
(1, 'Amit', 'Excel,SQL,Power BI');
```

Result:

| employee_id | employee_name | skills             |
| ----------: | ------------- | ------------------ |
|           1 | Amit          | Excel,SQL,Power BI |

### ENUM vs SET

```text
ENUM
→ One value

SET
→ Multiple values
```

---

# 16. BLOB

`BLOB` is used for binary data.

Example:

```sql
CREATE TABLE documents (
    document_id INT,
    document_file BLOB
);
```

Possible use:

```text
Images
Files
Binary data
```

In modern applications, files are often stored outside the database and their file path/URL is stored in the database instead.

---

# 17. Data Type Practical Summary

| Data Type   | Real-World Example        |
| ----------- | ------------------------- |
| `VARCHAR`   | Name, Email, City         |
| `CHAR`      | Gender code, Country code |
| `TEXT`      | Description               |
| `INT`       | Age, Quantity, ID         |
| `DECIMAL`   | Salary, Price, Fees       |
| `FLOAT`     | Percentage, Temperature   |
| `DOUBLE`    | Latitude, Longitude       |
| `DATE`      | Joining Date              |
| `TIME`      | Class Time                |
| `DATETIME`  | Login Date + Time         |
| `TIMESTAMP` | Record creation time      |
| `ENUM`      | Active/Inactive           |
| `SET`       | Multiple Skills           |
| `BLOB`      | Binary/File Data          |

---

# Part 2 — SQL Constraints

# 18. What is a Constraint?

A **constraint is a rule that controls what data can be stored in a table.**

Think about this:

Without constraints:

```text
Age = -50
Email = duplicate
Student ID = duplicate
Salary = -10000
```

With constraints, we can prevent invalid data.

---

# 19. NOT NULL

## Meaning

`NOT NULL` means:

> This column must have a value.

### Practical Example

A student must have a name.

```sql
CREATE TABLE students (
    student_id INT,
    student_name VARCHAR(50) NOT NULL
);
```

Valid:

```sql
INSERT INTO students
VALUES (1, 'Amit');
```

Invalid:

```sql
INSERT INTO students
VALUES (2, NULL);
```

### Real-Life Use

Use `NOT NULL` for compulsory information:

```text
Student Name
Employee Name
Product Name
Email
Phone Number
```

---

# 20. UNIQUE

## Meaning

`UNIQUE` prevents duplicate values.

### Practical Example

Every employee should have a different email.

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    email VARCHAR(100) UNIQUE
);
```

First:

```sql
INSERT INTO employees
VALUES
(1, 'Amit', 'amit@gmail.com');
```

Works.

Second:

```sql
INSERT INTO employees
VALUES
(2, 'Rahul', 'amit@gmail.com');
```

Error because the email already exists.

### Real-Life Use

```text
Email
Phone Number
PAN Number
Aadhaar-related identifier where appropriate
Username
```

---

# 21. PRIMARY KEY

## Meaning

A Primary Key uniquely identifies each record.

### Practical Example

Every student needs a unique Student ID.

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50)
);
```

Insert:

```sql
INSERT INTO students
VALUES
(101, 'Amit'),
(102, 'Priya'),
(103, 'Rahul');
```

Table:

| student_id | student_name |
| ---------: | ------------ |
|        101 | Amit         |
|        102 | Priya        |
|        103 | Rahul        |

Trying:

```sql
INSERT INTO students
VALUES
(101, 'Neha');
```

will fail because `101` already exists.

### Primary Key Rules

```text
PRIMARY KEY
↓
Unique
↓
Cannot be NULL
↓
Identifies each row
```

---

# 22. AUTO_INCREMENT

## Meaning

`AUTO_INCREMENT` automatically generates a number.

### Practical Example

Instead of manually entering:

```text
1
2
3
4
5
```

MySQL generates the ID.

```sql
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    student_name VARCHAR(50) NOT NULL
);
```

Insert:

```sql
INSERT INTO students(student_name)
VALUES
('Amit'),
('Priya'),
('Rahul');
```

Result:

| student_id | student_name |
| ---------: | ------------ |
|          1 | Amit         |
|          2 | Priya        |
|          3 | Rahul        |

### Very Common Pattern

```sql
id INT AUTO_INCREMENT PRIMARY KEY
```

---

# 23. DEFAULT

## Meaning

`DEFAULT` provides a value automatically when no value is supplied.

### Practical Example

Every new employee should be `Active` by default.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    status VARCHAR(20) DEFAULT 'Active'
);
```

Insert:

```sql
INSERT INTO employees
(employee_id, employee_name)
VALUES
(1, 'Amit');
```

We did not provide status.

MySQL automatically stores:

```text
Active
```

Result:

| employee_id | employee_name | status |
| ----------: | ------------- | ------ |
|           1 | Amit          | Active |

---

# 24. CHECK

## Meaning

`CHECK` ensures that a value satisfies a condition.

### Practical Example — Age

A training institute may allow only students aged 18 or above.

```sql
CREATE TABLE students (
    student_id INT,
    student_name VARCHAR(50),
    age INT CHECK (age >= 18)
);
```

Valid:

```sql
INSERT INTO students
VALUES
(1, 'Amit', 22);
```

Invalid:

```sql
INSERT INTO students
VALUES
(2, 'Rahul', 15);
```

Because:

```text
15 >= 18
↓
FALSE
```

---

## Another Practical Example — Salary

Salary should not be negative.

```sql
CREATE TABLE employees (
    employee_id INT,
    employee_name VARCHAR(50),
    salary DECIMAL(10,2)
        CHECK (salary >= 0)
);
```

Valid:

```text
50000
25000
100000
```

Invalid:

```text
-5000
```

---

# 25. FOREIGN KEY

## Meaning

A Foreign Key connects two related tables.

Let's take a real example.

We have:

```text
Departments
Employees
```

An employee belongs to a department.

---

## Step 1 — Create Department Table

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50) NOT NULL
);
```

Insert data:

```sql
INSERT INTO departments
VALUES
(1, 'IT'),
(2, 'HR'),
(3, 'Sales');
```

Table:

| department_id | department_name |
| ------------: | --------------- |
|             1 | IT              |
|             2 | HR              |
|             3 | Sales           |

---

## Step 2 — Create Employee Table

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50) NOT NULL,
    department_id INT,

    FOREIGN KEY (department_id)
    REFERENCES departments(department_id)
);
```

---

## Step 3 — Insert Employee

```sql
INSERT INTO employees
VALUES
(101, 'Amit', 1),
(102, 'Priya', 2),
(103, 'Rahul', 3);
```

This works because:

```text
1 → exists in departments
2 → exists in departments
3 → exists in departments
```

---

## What happens if we use 10?

```sql
INSERT INTO employees
VALUES
(104, 'Neha', 10);
```

This will fail because:

```text
department_id = 10
```

does not exist in the parent table.

### Relationship

```text
departments
----------------
department_id  ← Primary Key
       ↑
       |
       |
employees
----------------
department_id  ← Foreign Key
```

### Remember

> **Primary Key identifies the record. Foreign Key creates the relationship.**

---

# 26. INDEX

An index is used mainly to improve search/query performance.

Suppose we frequently search employees by email.

```sql
CREATE INDEX idx_employee_email
ON employees(email);
```

Now MySQL has an index that can help with searches on `email`.

### Unique Index

```sql
CREATE UNIQUE INDEX idx_email
ON employees(email);
```

A unique index also enforces uniqueness.

### Important

```text
INDEX
→ Mainly performance

UNIQUE
→ Mainly prevents duplicate values
```

---

# 27. Practical Employee Table Using Multiple Constraints

This is how constraints are commonly combined in a real table.

```sql
CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,

    employee_name VARCHAR(50) NOT NULL,

    email VARCHAR(100) UNIQUE,

    age INT CHECK (age >= 18),

    salary DECIMAL(10,2)
        CHECK (salary >= 0),

    department_id INT,

    status VARCHAR(20) DEFAULT 'Active',

    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

### What does each column do?

| Column          | Data Type | Constraint          | Purpose                   |
| --------------- | --------- | ------------------- | ------------------------- |
| `employee_id`   | INT       | PK + AUTO_INCREMENT | Unique employee ID        |
| `employee_name` | VARCHAR   | NOT NULL            | Name required             |
| `email`         | VARCHAR   | UNIQUE              | No duplicate email        |
| `age`           | INT       | CHECK               | Age must be 18+           |
| `salary`        | DECIMAL   | CHECK               | Salary cannot be negative |
| `department_id` | INT       | FOREIGN KEY         | Connects department       |
| `status`        | VARCHAR   | DEFAULT             | Default = Active          |

---

# Part 3 — Practical Exercises

## Exercise 1 — Student Table

### Question

Create a `students` table with:

* `student_id`
* `student_name`
* `email`
* `age`
* `fees`

Requirements:

* Student ID should be Primary Key
* Name cannot be NULL
* Email should be unique
* Age must be 18 or above
* Fees should be decimal

### Solution

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 18),
    fees DECIMAL(10,2)
);
```

---

# Exercise 2 — Employee Table

### Question

Create an employee table where:

* ID automatically increases
* Name is compulsory
* Email cannot be duplicated
* Salary cannot be negative
* Status is `Active` by default

### Solution

```sql
CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    employee_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10,2) CHECK (salary >= 0),
    status VARCHAR(20) DEFAULT 'Active'
);
```

---

# Exercise 3 — Department Relationship

### Question

Create:

1. `departments`
2. `employees`

`employees.department_id` should reference `departments.department_id`.

### Solution

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50) NOT NULL
);
```

```sql
CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    employee_name VARCHAR(50) NOT NULL,
    department_id INT,

    FOREIGN KEY (department_id)
    REFERENCES departments(department_id)
);
```

---

# Exercise 4 — Insert Valid Data

### Question

Insert three departments.

### Solution

```sql
INSERT INTO departments
VALUES
(1, 'IT'),
(2, 'HR'),
(3, 'Sales');
```

Insert employees:

```sql
INSERT INTO employees
(employee_name, department_id)
VALUES
('Amit', 1),
('Priya', 2),
('Rahul', 3);
```

---

# Exercise 5 — Find the Error

### Question

What is wrong with this table?

```sql
CREATE TABLE students (
    student_id VARCHAR(20),
    student_name INT,
    fees VARCHAR(20)
);
```

### Solution

The data types are inappropriate.

Correct:

```sql
CREATE TABLE students (
    student_id INT,
    student_name VARCHAR(50),
    fees DECIMAL(10,2)
);
```

### Why?

```text
student_id
→ Number → INT

student_name
→ Text → VARCHAR

fees
→ Money/decimal → DECIMAL
```

---

# Exercise 6 — Constraint Practice

### Question

What constraint should be used for each requirement?

| Requirement                              | Constraint       |
| ---------------------------------------- | ---------------- |
| Name cannot be empty/NULL                | `NOT NULL`       |
| Email cannot repeat                      | `UNIQUE`         |
| Employee ID uniquely identifies employee | `PRIMARY KEY`    |
| Employee belongs to department           | `FOREIGN KEY`    |
| Age must be 18+                          | `CHECK`          |
| Status should automatically be Active    | `DEFAULT`        |
| ID should generate automatically         | `AUTO_INCREMENT` |

---

# Part 4 — Interview Questions

## Q1. What is a Data Type?

**Answer:**
A data type defines the type of value that can be stored in a column.

---

## Q2. Which data type should be used for salary?

**Answer:**
Usually `DECIMAL`, because salary is a financial value where exact decimal representation is important.

---

## Q3. Difference between CHAR and VARCHAR?

**Answer:**

```text
CHAR
→ Fixed-length data

VARCHAR
→ Variable-length data
```

---

## Q4. Difference between FLOAT and DECIMAL?

**Answer:**

```text
FLOAT
→ Approximate decimal value

DECIMAL
→ Exact decimal value
```

For financial data, `DECIMAL` is generally preferred.

---

## Q5. What is a Constraint?

**Answer:**
A constraint is a rule applied to a table column to maintain data integrity.

---

## Q6. What is the difference between PRIMARY KEY and UNIQUE?

**Answer:**

`PRIMARY KEY` uniquely identifies each row and cannot be `NULL`.

`UNIQUE` prevents duplicate values in a column. MySQL allows `NULL` values in a UNIQUE column, subject to its normal NULL semantics.

---

## Q7. Can a table have more than one Primary Key?

**Answer:**
No. A table can have only one Primary Key constraint, but that key can contain multiple columns.

---

## Q8. What is a Foreign Key?

**Answer:**
A Foreign Key is a column that references a key in another table and maintains referential integrity.

---

## Q9. What is AUTO_INCREMENT?

**Answer:**
It automatically generates a numeric value, commonly used with an ID Primary Key.

---

## Q10. What is DEFAULT?

**Answer:**
`DEFAULT` provides a value automatically when the user does not provide one.

---

## Q11. What is CHECK?

**Answer:**
`CHECK` ensures that a value satisfies a specified condition.

Example:

```sql
age INT CHECK (age >= 18)
```

---

## Q12. Can we use multiple constraints on one column?

**Answer:**
Yes.

Example:

```sql
employee_id INT AUTO_INCREMENT PRIMARY KEY
```

Here the column has both:

```text
AUTO_INCREMENT
PRIMARY KEY
```

---

# Final Summary

## Data Types

```text
VARCHAR
→ Variable-length text

CHAR
→ Fixed-length text

INT
→ Whole numbers

DECIMAL
→ Exact decimal values

FLOAT / DOUBLE
→ Approximate decimal values

DATE
→ Date

TIME
→ Time

DATETIME
→ Date + Time

TIMESTAMP
→ Timestamp

ENUM
→ One value from a predefined list

SET
→ Multiple values from a predefined list

BLOB
→ Binary data
```

---

## Constraints

```text
NOT NULL
→ Value is compulsory

UNIQUE
→ Duplicate values are not allowed

PRIMARY KEY
→ Unique identifier for each row

FOREIGN KEY
→ Connects two tables

CHECK
→ Validates a condition

DEFAULT
→ Automatically provides a default value

AUTO_INCREMENT
→ Automatically generates numbers

INDEX
→ Helps improve query/search performance
```

---

# Quick Real-World Example

For an employee database:

```sql
CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,

    employee_name VARCHAR(50) NOT NULL,

    email VARCHAR(100) UNIQUE,

    age INT CHECK (age >= 18),

    salary DECIMAL(10,2)
        CHECK (salary >= 0),

    department_id INT,

    status VARCHAR(20) DEFAULT 'Active',

    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

Read it like this:

```text
employee_id
→ Number
→ Automatically generated
→ Unique
→ Primary Key

employee_name
→ Text
→ Required

email
→ Text
→ Cannot be duplicated

age
→ Number
→ Must be 18 or above

salary
→ Decimal
→ Cannot be negative

department_id
→ Number
→ Must match a department

status
→ Text
→ Active by default
```

> **Remember:**
> **Data Type = What kind of data can be stored?**
> **Constraint = What rules should that data follow?**
