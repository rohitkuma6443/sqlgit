## 1. What are Aggregate Functions?

Aggregate functions perform calculations on **multiple rows** and return a summarized result.

For example, from an employee table we can find:

* Total number of employees
* Total salary
* Average salary
* Highest salary
* Lowest salary

### Main Aggregate Functions

| Function  | Purpose             |
| --------- | ------------------- |
| `COUNT()` | Counts records      |
| `SUM()`   | Calculates total    |
| `AVG()`   | Calculates average  |
| `MIN()`   | Finds minimum value |
| `MAX()`   | Finds maximum value |

### Easy Way to Remember

```text
COUNT() → How many?
SUM()   → How much total?
AVG()   → What is the average?
MIN()   → What is the lowest?
MAX()   → What is the highest?
```

---

# 2. Practice Database

We will use the same database throughout this chapter.

## Create Database

```sql
CREATE DATABASE aggregate_training;

USE aggregate_training;
```

---

# 3. Create Employees Table

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    age INT,
    city VARCHAR(50),
    status VARCHAR(20),
    bonus DECIMAL(10,2)
);
```

---

# 4. Insert Dataset

```sql
INSERT INTO employees
(employee_id, employee_name, department, salary, age, city, status, bonus)
VALUES
(1, 'Amit', 'IT', 45000, 25, 'Mumbai', 'Active', 5000),
(2, 'Ravi', 'Sales', 55000, 30, 'Delhi', 'Active', 7000),
(3, 'Neha', 'IT', 60000, 28, 'Pune', 'Active', 8000),
(4, 'Priya', 'HR', 50000, 32, 'Mumbai', 'Inactive', NULL),
(5, 'Rahul', 'Finance', 70000, 35, 'Delhi', 'Active', 10000),
(6, 'Sneha', 'Sales', 48000, 27, 'Pune', 'Active', NULL),
(7, 'Vikas', 'Finance', 80000, 40, 'Mumbai', 'Active', 12000),
(8, 'Anita', 'HR', 52000, 29, 'Nashik', 'Active', 6000),
(9, 'Karan', 'IT', 42000, 24, 'Nashik', 'Active', NULL),
(10, 'Pooja', 'Sales', 58000, 31, 'Delhi', 'Inactive', 5000);
```

Check the data:

```sql
SELECT *
FROM employees;
```

### Employees Table

| ID | Employee | Department | Salary | Age | City   | Status   | Bonus |
| -: | -------- | ---------- | -----: | --: | ------ | -------- | ----: |
|  1 | Amit     | IT         |  45000 |  25 | Mumbai | Active   |  5000 |
|  2 | Ravi     | Sales      |  55000 |  30 | Delhi  | Active   |  7000 |
|  3 | Neha     | IT         |  60000 |  28 | Pune   | Active   |  8000 |
|  4 | Priya    | HR         |  50000 |  32 | Mumbai | Inactive |  NULL |
|  5 | Rahul    | Finance    |  70000 |  35 | Delhi  | Active   | 10000 |
|  6 | Sneha    | Sales      |  48000 |  27 | Pune   | Active   |  NULL |
|  7 | Vikas    | Finance    |  80000 |  40 | Mumbai | Active   | 12000 |
|  8 | Anita    | HR         |  52000 |  29 | Nashik | Active   |  6000 |
|  9 | Karan    | IT         |  42000 |  24 | Nashik | Active   |  NULL |
| 10 | Pooja    | Sales      |  58000 |  31 | Delhi  | Inactive |  5000 |

---

# 5. COUNT()

## What is COUNT()?

`COUNT()` is used to count records or values.

### Syntax

```sql
SELECT COUNT(column_name)
FROM table_name;
```

---

## Example 1: Count All Employees

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```

### Result

| total_employees |
| --------------: |
|              10 |

### Important

```text
COUNT(*) 
→ Counts rows

COUNT(column)
→ Counts non-NULL values
```

---

# 6. COUNT(column)

Suppose we want to count employees who have received a bonus.

```sql
SELECT COUNT(bonus) AS employees_with_bonus
FROM employees;
```

There are 7 employees with a bonus.

`NULL` values are not counted.

---

# 7. SUM()

## What is SUM()?

`SUM()` calculates the **total** of a numeric column.

### Syntax

```sql
SELECT SUM(column_name)
FROM table_name;
```

### Example

Find total salary:

```sql
SELECT
    SUM(salary) AS total_salary
FROM employees;
```

### Result

| total_salary |
| -----------: |
|       610000 |

---

## SUM() with WHERE

Find total salary of IT employees.

```sql
SELECT
    SUM(salary) AS total_it_salary
FROM employees
WHERE department = 'IT';
```

Calculation:

```text
45000 + 60000 + 42000
= 147000
```

---

# 8. AVG()

## What is AVG()?

`AVG()` calculates the average value of a numeric column.

### Formula

```text
Average = Total / Number of Values
```

### Example

```sql
SELECT
    AVG(salary) AS average_salary
FROM employees;
```

### Result

| average_salary |
| -------------: |
|          61000 |

---

## AVG() with WHERE

Find the average salary of IT employees.

```sql
SELECT
    AVG(salary) AS average_it_salary
FROM employees
WHERE department = 'IT';
```

Result:

```text
49000
```

---

# 9. MIN()

`MIN()` returns the smallest value.

### Example

Find the lowest salary:

```sql
SELECT
    MIN(salary) AS lowest_salary
FROM employees;
```

Result:

```text
42000
```

---

# 10. MAX()

`MAX()` returns the largest value.

### Example

Find the highest salary:

```sql
SELECT
    MAX(salary) AS highest_salary
FROM employees;
```

Result:

```text
80000
```

---

# 11. Using All Aggregate Functions Together

We can use multiple aggregate functions in one query.

```sql
SELECT
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary
FROM employees;
```

### Result

| Employees | Total Salary | Average | Minimum | Maximum |
| --------: | -----------: | ------: | ------: | ------: |
|        10 |       610000 |   61000 |   42000 |   80000 |

This type of query is very useful for **reports and dashboards**.

---

# 12. Aggregate Functions with WHERE

`WHERE` filters rows **before** the aggregate calculation.

### Example

Find the average salary of active employees.

```sql
SELECT
    AVG(salary) AS average_salary
FROM employees
WHERE status = 'Active';
```

The process is:

```text
All Employees
      ↓
WHERE status = 'Active'
      ↓
Filtered Employees
      ↓
AVG(salary)
      ↓
Result
```

---

# 13. GROUP BY

## What is GROUP BY?

`GROUP BY` is used to divide rows into groups based on one or more columns.

It is commonly used with aggregate functions.

Without `GROUP BY`:

```sql
SELECT AVG(salary)
FROM employees;
```

We get one overall average.

With `GROUP BY`:

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

We get an average for **each department**.

---

# 14. GROUP BY with COUNT()

Find the number of employees in each department.

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department;
```

### Result

| Department | Employees |
| ---------- | --------: |
| Finance    |         2 |
| HR         |         2 |
| IT         |         3 |
| Sales      |         3 |

### Meaning

```text
IT      → 3 employees
Sales   → 3 employees
HR      → 2 employees
Finance → 2 employees
```

---

# 15. GROUP BY with SUM()

Find total salary for each department.

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

### Result

| Department | Total Salary |
| ---------- | -----------: |
| Finance    |       150000 |
| HR         |       102000 |
| IT         |       147000 |
| Sales      |       161000 |

---

# 16. GROUP BY with AVG()

Find the average salary for each department.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

### Result

| Department | Average Salary |
| ---------- | -------------: |
| Finance    |          75000 |
| HR         |          51000 |
| IT         |          49000 |
| Sales      |       53666.67 |

---

# 17. GROUP BY with MIN() and MAX()

Find the lowest and highest salary in each department.

```sql
SELECT
    department,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary
FROM employees
GROUP BY department;
```

---

# 18. Complete Department Salary Report

We can combine everything:

```sql
SELECT
    department,
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary
FROM employees
GROUP BY department;
```

This is an excellent example of a real-world SQL report.

---

# 19. GROUP BY Multiple Columns

`GROUP BY` can use more than one column.

For example, suppose we want employee count by:

```text
City + Status
```

```sql
SELECT
    city,
    status,
    COUNT(*) AS total_employees
FROM employees
GROUP BY city, status;
```

Now SQL creates groups based on the combination of:

```text
Mumbai + Active
Mumbai + Inactive
Delhi + Active
Delhi + Inactive
...
```

---

# 20. GROUP BY with WHERE

We can filter rows before grouping.

### Example

Find the number of **active employees in each department**.

```sql
SELECT
    department,
    COUNT(*) AS active_employees
FROM employees
WHERE status = 'Active'
GROUP BY department;
```

### Execution Concept

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
COUNT()
 ↓
Result
```

---

# 21. HAVING Clause

## What is HAVING?

`HAVING` is used to filter **groups after aggregation**.

It is commonly used with `GROUP BY`.

### Syntax

```sql
SELECT
    column_name,
    aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

---

# 22. HAVING with COUNT()

Find departments having more than 2 employees.

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

### Result

| Department | Employees |
| ---------- | --------: |
| IT         |         3 |
| Sales      |         3 |

HR and Finance have only 2 employees, so they are removed.

---

# 23. HAVING with SUM()

Find departments where total salary is greater than `140000`.

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 140000;
```

---

# 24. HAVING with AVG()

Find departments where average salary is greater than `55000`.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 55000;
```

---

# 25. WHERE vs HAVING

This is one of the most important concepts.

| WHERE                                  | HAVING                                |
| -------------------------------------- | ------------------------------------- |
| Filters rows                           | Filters groups                        |
| Used before grouping                   | Used after grouping                   |
| Normally works with individual columns | Commonly works with aggregate results |
| Used before `GROUP BY`                 | Used after `GROUP BY`                 |

### WHERE Example

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
WHERE status = 'Active'
GROUP BY department;
```

First, inactive employees are removed.

Then the remaining employees are grouped.

---

### HAVING Example

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

First, departments are grouped.

Then departments with average salary ≤ 50000 are removed.

---

# 26. WHERE + GROUP BY + HAVING

We can use all three together.

### Question

Find active departments whose average salary is greater than `50000`.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
WHERE status = 'Active'
GROUP BY department
HAVING AVG(salary) > 50000;
```

### Logical Flow

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
Aggregate Function
 ↓
HAVING
 ↓
Result
```

---

# 27. NULL Values in Aggregate Functions

`NULL` means a value is missing or unknown.

Our `bonus` column contains `NULL`.

```text
Priya  → NULL
Sneha  → NULL
Karan  → NULL
```

Most aggregate functions ignore `NULL`.

For example:

```sql
SELECT AVG(bonus)
FROM employees;
```

The `NULL` bonus values are not treated as zero.

---

# 28. COUNT() and NULL

Consider:

```sql
SELECT COUNT(bonus)
FROM employees;
```

This counts only employees whose bonus is **not NULL**.

But:

```sql
SELECT COUNT(*)
FROM employees;
```

counts all employees.

### Remember

```text
COUNT(*)       → Counts all rows
COUNT(bonus)   → Counts non-NULL bonus values
```

---

# 29. COALESCE()

## What is COALESCE()?

`COALESCE()` returns the **first non-NULL value** from the supplied values.

### Syntax

```sql
COALESCE(value1, value2, value3, ...)
```

### Simple Example

```sql
SELECT
    COALESCE(NULL, 0);
```

Result:

```text
0
```

Because `NULL` is replaced by `0`.

---

# 30. COALESCE() with Employee Bonus

Suppose an employee does not have a bonus.

Instead of showing:

```text
NULL
```

we want:

```text
0
```

Use:

```sql
SELECT
    employee_name,
    COALESCE(bonus, 0) AS bonus
FROM employees;
```

### Result

| Employee | Bonus |
| -------- | ----: |
| Amit     |  5000 |
| Ravi     |  7000 |
| Neha     |  8000 |
| Priya    |     0 |
| Rahul    | 10000 |
| Sneha    |     0 |
| Vikas    | 12000 |
| Anita    |  6000 |
| Karan    |     0 |
| Pooja    |  5000 |

---

# 31. COALESCE() with Multiple Values

Example:

```sql
SELECT
    COALESCE(NULL, NULL, 100, 200);
```

Result:

```text
100
```

Because `100` is the first non-NULL value.

### Remember

```text
COALESCE()
↓
Find the first non-NULL value
```

---

# 32. COALESCE() in Practical Reporting

Suppose a report contains:

```sql
SELECT
    department,
    SUM(bonus) AS total_bonus
FROM employees
GROUP BY department;
```

If a group has no bonus values, the result may be `NULL`.

We can display `0` instead:

```sql
SELECT
    department,
    COALESCE(SUM(bonus), 0) AS total_bonus
FROM employees
GROUP BY department;
```

This is very useful in reports.

---

# 33. IFNULL()

`IFNULL()` is another MySQL function used to replace a `NULL` value.

### Syntax

```sql
IFNULL(value, replacement_value)
```

### Example

```sql
SELECT
    employee_name,
    IFNULL(bonus, 0) AS bonus
FROM employees;
```

### COALESCE vs IFNULL

| `COALESCE()`              | `IFNULL()`       |
| ------------------------- | ---------------- |
| SQL standard              | MySQL-specific   |
| Can check multiple values | Checks one value |
| `COALESCE(a,b,c)`         | `IFNULL(a,b)`    |

For MySQL, both can be useful.

---

# 34. ROUND()

`ROUND()` is useful when aggregate results contain decimal values.

For example:

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

The result may contain:

```text
53666.6667
```

We can round it:

```sql
SELECT
    department,
    ROUND(AVG(salary), 2) AS average_salary
FROM employees
GROUP BY department;
```

Result:

```text
53666.67
```

---

# 35. Basic JOIN with Aggregate Functions

JOIN is not the main focus of this chapter.

We only need a simple example to understand how aggregate reports can use data from another table.

## Create Departments Table

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);
```

Insert data:

```sql
INSERT INTO departments
(department_id, department_name)
VALUES
(1, 'IT'),
(2, 'Sales'),
(3, 'HR'),
(4, 'Finance');
```

For a simple JOIN example, assume the employee table uses a `department_id` relationship.

The important idea is:

```text
employees
    ↓
department_id
    ↓
departments
```

Then we can create a department-level report:

```sql
SELECT
    d.department_name,
    COUNT(e.employee_id) AS total_employees,
    SUM(e.salary) AS total_salary
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.department_id
GROUP BY d.department_name;
```

### Important

The main concept here is not JOIN.

The important pattern is:

```text
JOIN
 ↓
GROUP BY
 ↓
COUNT / SUM / AVG
```

Detailed JOIN concepts should be studied in the separate **SQL JOIN chapter**.

---

# 36. Practical Business Examples

## Example 1 — Employee Count by Department

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

---

## Example 2 — Salary Cost by Department

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

---

## Example 3 — Average Salary by Department

```sql
SELECT
    department,
    ROUND(AVG(salary), 2) AS average_salary
FROM employees
GROUP BY department;
```

---

## Example 4 — Departments with More Than 2 Employees

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

---

## Example 5 — Departments with Salary Cost Above 140000

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 140000;
```

---

## Example 6 — Active Employees Only

```sql
SELECT
    department,
    COUNT(*) AS active_employees
FROM employees
WHERE status = 'Active'
GROUP BY department;
```

---

## Example 7 — Active Departments with Average Salary Above 50000

```sql
SELECT
    department,
    ROUND(AVG(salary), 2) AS average_salary
FROM employees
WHERE status = 'Active'
GROUP BY department
HAVING AVG(salary) > 50000;
```

---

# 37. Exercises with Solutions

## Exercise 1

### Question

Find the total number of employees.

### Solution

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```

---

## Exercise 2

### Question

Find the total salary of all employees.

### Solution

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

---

## Exercise 3

### Question

Find the average salary.

### Solution

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

---

## Exercise 4

### Question

Find the highest and lowest salary.

### Solution

```sql
SELECT
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM employees;
```

---

## Exercise 5

### Question

Find the number of employees in each department.

### Solution

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department;
```

---

## Exercise 6

### Question

Find the total salary for each department.

### Solution

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

---

## Exercise 7

### Question

Find the average salary for each department.

### Solution

```sql
SELECT
    department,
    ROUND(AVG(salary), 2) AS average_salary
FROM employees
GROUP BY department;
```

---

## Exercise 8

### Question

Find departments having more than 2 employees.

### Solution

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

---

## Exercise 9

### Question

Find departments where the total salary is greater than `140000`.

### Solution

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 140000;
```

---

## Exercise 10

### Question

Find active employees in each department.

### Solution

```sql
SELECT
    department,
    COUNT(*) AS active_employees
FROM employees
WHERE status = 'Active'
GROUP BY department;
```

---

## Exercise 11

### Question

Find active departments whose average salary is greater than `50000`.

### Solution

```sql
SELECT
    department,
    ROUND(AVG(salary), 2) AS average_salary
FROM employees
WHERE status = 'Active'
GROUP BY department
HAVING AVG(salary) > 50000;
```

---

## Exercise 12

### Question

Display employee bonus. If bonus is `NULL`, display `0`.

### Solution

```sql
SELECT
    employee_name,
    COALESCE(bonus, 0) AS bonus
FROM employees;
```

---

## Exercise 13

### Question

Find the total bonus for each department and display `0` instead of `NULL`.

### Solution

```sql
SELECT
    department,
    COALESCE(SUM(bonus), 0) AS total_bonus
FROM employees
GROUP BY department;
```

---

## Exercise 14

### Question

Find the number of employees in each city.

### Solution

```sql
SELECT
    city,
    COUNT(*) AS total_employees
FROM employees
GROUP BY city;
```

---

## Exercise 15

### Question

Find the average salary for each city, rounded to 2 decimal places.

### Solution

```sql
SELECT
    city,
    ROUND(AVG(salary), 2) AS average_salary
FROM employees
GROUP BY city;
```

---

# 38. Interview Questions and Answers

## Q1. What is an aggregate function?

**Answer:**
An aggregate function performs calculations on multiple rows and returns a summarized result.

Examples:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## Q2. What is GROUP BY?

**Answer:**
`GROUP BY` combines rows having the same value into groups so that aggregate calculations can be performed for each group.

---

## Q3. Why is GROUP BY used with aggregate functions?

**Answer:**
It allows us to calculate aggregate values separately for each group.

Example:

```sql
SELECT
    department,
    AVG(salary)
FROM employees
GROUP BY department;
```

---

## Q4. What is HAVING?

**Answer:**
`HAVING` filters groups after aggregation.

Example:

```sql
HAVING AVG(salary) > 50000
```

---

## Q5. Difference between WHERE and HAVING?

**Answer:**

```text
WHERE
→ Filters rows

HAVING
→ Filters groups
```

Example:

```sql
WHERE status = 'Active'
```

filters individual rows.

```sql
HAVING COUNT(*) > 2
```

filters grouped results.

---

## Q6. Can WHERE be used with GROUP BY?

**Answer:**
Yes.

```sql
SELECT
    department,
    COUNT(*)
FROM employees
WHERE status = 'Active'
GROUP BY department;
```

---

## Q7. Can HAVING be used without GROUP BY?

**Answer:**
Yes, MySQL can use `HAVING` to filter an aggregate result even without an explicit `GROUP BY`.

For beginner-level SQL, however, the most common pattern is:

```text
GROUP BY → HAVING
```

---

## Q8. Does COUNT(*) count NULL values?

**Answer:**
`COUNT(*)` counts rows, regardless of whether individual columns contain `NULL`.

---

## Q9. Does COUNT(column) count NULL?

**Answer:**
No. `COUNT(column)` counts only non-NULL values.

---

## Q10. What does COALESCE() do?

**Answer:**
`COALESCE()` returns the first non-NULL value.

Example:

```sql
SELECT COALESCE(NULL, 0);
```

Result:

```text
0
```

---

## Q11. Why is COALESCE() useful in reports?

**Answer:**
It can replace missing values with a meaningful value such as `0`.

Example:

```sql
COALESCE(SUM(bonus), 0)
```

---

## Q12. What is IFNULL()?

**Answer:**
`IFNULL()` is a MySQL function that returns an alternative value when the first value is `NULL`.

```sql
IFNULL(bonus, 0)
```

---

## Q13. What is the difference between COALESCE() and IFNULL()?

**Answer:**

```text
COALESCE()
→ Can check multiple values
→ SQL standard

IFNULL()
→ Checks one value and one replacement
→ Commonly used in MySQL
```

---

## Q14. Why do we use ROUND() with AVG()?

**Answer:**
`AVG()` can return many decimal places. `ROUND()` makes the result easier to read.

```sql
ROUND(AVG(salary), 2)
```

---

## Q15. Can multiple aggregate functions be used in one query?

**Answer:**
Yes.

```sql
SELECT
    COUNT(*),
    SUM(salary),
    AVG(salary),
    MIN(salary),
    MAX(salary)
FROM employees;
```

---

# 39. Important SQL Query Order

When using these concepts together, remember the basic logical order:

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
```

For our current chapter, the most important part is:

```text
WHERE
   ↓
GROUP BY
   ↓
Aggregate Function
   ↓
HAVING
```

Example:

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
WHERE status = 'Active'
GROUP BY department
HAVING AVG(salary) > 50000;
```

---

# 40. Quick Revision

### Aggregate Functions

```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

### GROUP BY

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

### HAVING

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

### WHERE + GROUP BY

```sql
SELECT department, COUNT(*)
FROM employees
WHERE status = 'Active'
GROUP BY department;
```

### COALESCE

```sql
COALESCE(bonus, 0)
```

### ROUND

```sql
ROUND(AVG(salary), 2)
```

### IFNULL

```sql
IFNULL(bonus, 0)
```

---

# 41. Final Summary

| Topic        | Main Purpose               |
| ------------ | -------------------------- |
| `COUNT()`    | Count records              |
| `SUM()`      | Calculate total            |
| `AVG()`      | Calculate average          |
| `MIN()`      | Find minimum               |
| `MAX()`      | Find maximum               |
| `GROUP BY`   | Create groups              |
| `HAVING`     | Filter groups              |
| `WHERE`      | Filter rows                |
| `COALESCE()` | Replace `NULL`             |
| `IFNULL()`   | Replace `NULL` in MySQL    |
| `ROUND()`    | Round decimal results      |
| Basic `JOIN` | Combine tables for reports |

### The Most Important Pattern

```text
                SQL Aggregate Reporting

                       Data
                        ↓
                     WHERE
                        ↓
                    GROUP BY
                        ↓
              Aggregate Function
                        ↓
                     HAVING
                        ↓
                    Final Result
```

### Remember These 5 Questions

```text
COUNT() → How many?
SUM()   → What is the total?
AVG()   → What is the average?
MIN()   → What is the lowest?
MAX()   → What is the highest?
```

**Core skill:** Once you understand `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`, `GROUP BY`, `HAVING`, and `COALESCE()`, you can create a large number of practical SQL summary and reporting queries.
