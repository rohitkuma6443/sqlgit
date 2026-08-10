
# 1. What is a SQL Clause?

A **clause** is a part of an SQL statement that helps us specify **how we want to retrieve, filter, group, sort, or limit data**.

For example:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 50000;
```

Here:

* `SELECT` → chooses the columns
* `FROM` → tells SQL where the data comes from
* `WHERE` → filters the records

In this chapter, we will learn:

| Clause     | Purpose                 |
| ---------- | ----------------------- |
| `FROM`     | Select the table        |
| `WHERE`    | Filter rows             |
| `DISTINCT` | Remove duplicate values |
| `ORDER BY` | Sort records            |
| `GROUP BY` | Create groups           |
| `HAVING`   | Filter groups           |
| `LIMIT`    | Restrict number of rows |
| `IN`       | Match multiple values   |
| `BETWEEN`  | Filter a range          |
| `LIKE`     | Search using a pattern  |
| `CASE`     | Apply conditions        |
| `AS`       | Create an alias         |

---

# 2. Create Database

We will use one database for all examples.

```sql
CREATE DATABASE sql_training;

USE sql_training;
```

---

# 3. Create Our Practice Table

We will use an `employees` table throughout this chapter.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    age INT,
    city VARCHAR(50),
    joining_date DATE,
    status VARCHAR(20)
);
```

---

# 4. Insert Practice Data

```sql
INSERT INTO employees
(employee_id, employee_name, department, salary, age, city, joining_date, status)
VALUES
(1, 'Amit', 'IT', 45000, 25, 'Mumbai', '2023-01-10', 'Active'),
(2, 'Ravi', 'Sales', 55000, 30, 'Delhi', '2022-05-15', 'Active'),
(3, 'Neha', 'IT', 60000, 28, 'Pune', '2021-08-20', 'Active'),
(4, 'Priya', 'HR', 50000, 32, 'Mumbai', '2020-03-12', 'Inactive'),
(5, 'Rahul', 'Finance', 70000, 35, 'Delhi', '2019-11-25', 'Active'),
(6, 'Sneha', 'Sales', 48000, 27, 'Pune', '2023-07-01', 'Active'),
(7, 'Vikas', 'Finance', 80000, 40, 'Mumbai', '2018-06-18', 'Active'),
(8, 'Anita', 'HR', 52000, 29, 'Nashik', '2022-09-10', 'Active'),
(9, 'Karan', 'IT', 42000, 24, 'Nashik', '2024-01-15', 'Active'),
(10, 'Pooja', 'Sales', 58000, 31, 'Delhi', '2021-12-05', 'Inactive');
```

Check the table:

```sql
SELECT * 
FROM employees;
```

### Dataset

| ID | Employee | Department | Salary | Age | City   | Status   |
| -: | -------- | ---------- | -----: | --: | ------ | -------- |
|  1 | Amit     | IT         |  45000 |  25 | Mumbai | Active   |
|  2 | Ravi     | Sales      |  55000 |  30 | Delhi  | Active   |
|  3 | Neha     | IT         |  60000 |  28 | Pune   | Active   |
|  4 | Priya    | HR         |  50000 |  32 | Mumbai | Inactive |
|  5 | Rahul    | Finance    |  70000 |  35 | Delhi  | Active   |
|  6 | Sneha    | Sales      |  48000 |  27 | Pune   | Active   |
|  7 | Vikas    | Finance    |  80000 |  40 | Mumbai | Active   |
|  8 | Anita    | HR         |  52000 |  29 | Nashik | Active   |
|  9 | Karan    | IT         |  42000 |  24 | Nashik | Active   |
| 10 | Pooja    | Sales      |  58000 |  31 | Delhi  | Inactive |

---

# 5. FROM Clause

## What is FROM?

`FROM` specifies the table from which we want to retrieve data.

### Syntax

```sql
SELECT column_name
FROM table_name;
```

### Example

Display all employees:

```sql
SELECT *
FROM employees;
```

Display only employee names:

```sql
SELECT employee_name
FROM employees;
```

Display employee names and salaries:

```sql
SELECT employee_name, salary
FROM employees;
```

### Remember

```text
FROM = From which table?
```

---

# 6. WHERE Clause

## What is WHERE?

`WHERE` is used to **filter rows according to a condition**.

### Syntax

```sql
SELECT columns
FROM table_name
WHERE condition;
```

---

## Example 1 — IT Employees

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

### Result

| employee_name | department |
| ------------- | ---------- |
| Amit          | IT         |
| Neha          | IT         |
| Karan         | IT         |

---

## Example 2 — Salary Greater Than 50000

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 50000;
```

---

## Example 3 — Active Employees

```sql
SELECT employee_name, status
FROM employees
WHERE status = 'Active';
```

---

## Example 4 — Employees from Mumbai

```sql
SELECT employee_name, city
FROM employees
WHERE city = 'Mumbai';
```

---

## WHERE with Comparison Operators

Common operators:

| Operator | Meaning               |
| -------- | --------------------- |
| `=`      | Equal                 |
| `>`      | Greater than          |
| `<`      | Less than             |
| `>=`     | Greater than or equal |
| `<=`     | Less than or equal    |
| `<>`     | Not equal             |

### Example

Find employees aged 30 or above:

```sql
SELECT employee_name, age
FROM employees
WHERE age >= 30;
```

---

# 7. DISTINCT Clause

## What is DISTINCT?

`DISTINCT` removes duplicate values from the result.

### Without DISTINCT

```sql
SELECT city
FROM employees;
```

Possible result:

```text
Mumbai
Delhi
Pune
Mumbai
Delhi
Pune
Mumbai
Nashik
Nashik
Delhi
```

There are duplicate cities.

### With DISTINCT

```sql
SELECT DISTINCT city
FROM employees;
```

### Result

| city   |
| ------ |
| Mumbai |
| Delhi  |
| Pune   |
| Nashik |

---

## Practical Example

Find unique departments:

```sql
SELECT DISTINCT department
FROM employees;
```

Result:

```text
IT
Sales
HR
Finance
```

### Remember

```text
DISTINCT = Remove duplicate results
```

---

# 8. ORDER BY Clause

## What is ORDER BY?

`ORDER BY` is used to **sort the result**.

There are two main directions:

```text
ASC  → Ascending
DESC → Descending
```

---

## Example 1 — Salary Ascending

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary ASC;
```

Lowest salary comes first.

---

## Example 2 — Salary Descending

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC;
```

Highest salary comes first.

### Result starts with:

| Employee | Salary |
| -------- | -----: |
| Vikas    |  80000 |
| Rahul    |  70000 |
| Neha     |  60000 |
| Pooja    |  58000 |

---

## Example 3 — Sort by Name

```sql
SELECT employee_name
FROM employees
ORDER BY employee_name ASC;
```

---

## Multiple Columns

We can sort using more than one column.

```sql
SELECT employee_name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
```

Meaning:

1. Sort department alphabetically.
2. Within each department, sort salary from high to low.

### Remember

```text
ORDER BY = Sort the result
```

---

# 9. GROUP BY Clause

## What is GROUP BY?

`GROUP BY` combines rows having the same value into groups.

It is commonly used with aggregate functions:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## Example 1 — Count Employees by Department

Question:

> How many employees are in each department?

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department;
```

### Result

| Department | Total Employees |
| ---------- | --------------: |
| Finance    |               2 |
| HR         |               2 |
| IT         |               3 |
| Sales      |               3 |

---

## Example 2 — Total Salary by Department

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

---

## Example 3 — Average Salary

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

---

## Example 4 — Highest Salary

```sql
SELECT
    department,
    MAX(salary) AS highest_salary
FROM employees
GROUP BY department;
```

---

## Example 5 — Lowest Salary

```sql
SELECT
    department,
    MIN(salary) AS lowest_salary
FROM employees
GROUP BY department;
```

### Remember

```text
GROUP BY = Create groups
```

---

# 10. HAVING Clause

## What is HAVING?

`HAVING` filters the results **after GROUP BY**.

### Important Difference

```text
WHERE  → Filters rows
HAVING → Filters groups
```

---

## Example

Question:

> Show departments having more than 2 employees.

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

### Result

| Department | Total Employees |
| ---------- | --------------: |
| IT         |               3 |
| Sales      |               3 |

---

## Another Example

Question:

> Show departments where average salary is greater than 55000.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 55000;
```

---

# 11. WHERE vs HAVING

This is one of the most important concepts.

## WHERE

Filters individual records.

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

## HAVING

Filters grouped results.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

### Easy Memory

```text
WHERE
↓
Filter Rows

GROUP BY
↓
Create Groups

HAVING
↓
Filter Groups
```

---

# 12. LIMIT Clause

## What is LIMIT?

`LIMIT` restricts the number of rows returned.

### Example

Display only 5 employees:

```sql
SELECT *
FROM employees
LIMIT 5;
```

---

## Practical Example

Find the top 3 highest-paid employees:

```sql
SELECT
    employee_name,
    salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

### Result

| Employee | Salary |
| -------- | -----: |
| Vikas    |  80000 |
| Rahul    |  70000 |
| Neha     |  60000 |

### Important Combination

```sql
ORDER BY salary DESC
LIMIT 3;
```

means:

> Sort salary from highest to lowest and take only the first 3 rows.

---

# 13. IN Clause

## What is IN?

`IN` is used when we want to compare a column with **multiple possible values**.

### Without IN

```sql
SELECT *
FROM employees
WHERE department = 'IT'
   OR department = 'HR'
   OR department = 'Sales';
```

### With IN

```sql
SELECT *
FROM employees
WHERE department IN ('IT', 'HR', 'Sales');
```

Both queries produce the same logical result.

---

## Practical Example

Find employees from Mumbai, Delhi, or Pune.

```sql
SELECT employee_name, city
FROM employees
WHERE city IN ('Mumbai', 'Delhi', 'Pune');
```

### NOT IN

Find employees who are not from Mumbai or Delhi.

```sql
SELECT employee_name, city
FROM employees
WHERE city NOT IN ('Mumbai', 'Delhi');
```

### Remember

```text
IN = Match any value from a list
```

---

# 14. BETWEEN Clause

## What is BETWEEN?

`BETWEEN` filters values within a range.

The boundary values are included.

### Syntax

```sql
WHERE column_name BETWEEN value1 AND value2;
```

---

## Example — Salary

Find employees earning between 50000 and 70000.

```sql
SELECT employee_name, salary
FROM employees
WHERE salary BETWEEN 50000 AND 70000;
```

This means:

```text
salary >= 50000
AND
salary <= 70000
```

---

## Example — Age

Find employees aged between 25 and 30.

```sql
SELECT employee_name, age
FROM employees
WHERE age BETWEEN 25 AND 30;
```

---

## NOT BETWEEN

```sql
SELECT employee_name, salary
FROM employees
WHERE salary NOT BETWEEN 50000 AND 70000;
```

### Remember

```text
BETWEEN = Range

BETWEEN is inclusive.
```

---

# 15. LIKE Clause

## What is LIKE?

`LIKE` is used to search for a pattern in text.

Two important wildcards:

| Wildcard | Meaning                 |
| -------- | ----------------------- |
| `%`      | Zero or more characters |
| `_`      | Exactly one character   |

---

# 16. LIKE — Starts With

Find employees whose name starts with `A`.

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE 'A%';
```

Result:

```text
Amit
Anita
```

### Pattern

```text
'A%'
```

means:

```text
A + anything
```

---

# 17. LIKE — Ends With

Find names ending with `a`.

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '%a';
```

---

# 18. LIKE — Contains

Find names containing `i`.

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '%i%';
```

### Pattern

```text
'%i%'
```

means:

```text
anything + i + anything
```

---

# 19. LIKE — One Character

`_` represents exactly one character.

Example:

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '_mit';
```

This looks for:

```text
1 character + mit
```

So:

```text
Amit
```

matches.

---

# 20. CASE Clause

## What is CASE?

`CASE` is used to apply conditional logic.

It is similar to:

```text
IF
ELSE IF
ELSE
```

---

## Practical Example — Salary Category

Question:

> Create a salary category.

Rules:

```text
70000 or more → High
50000 or more → Medium
Below 50000   → Low
```

Query:

```sql
SELECT
    employee_name,
    salary,
    CASE
        WHEN salary >= 70000 THEN 'High'
        WHEN salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

### Example Result

| Employee | Salary | Category |
| -------- | -----: | -------- |
| Amit     |  45000 | Low      |
| Ravi     |  55000 | Medium   |
| Neha     |  60000 | Medium   |
| Rahul    |  70000 | High     |
| Vikas    |  80000 | High     |

---

## Another Example — Age Category

```sql
SELECT
    employee_name,
    age,
    CASE
        WHEN age >= 35 THEN 'Senior'
        WHEN age >= 25 THEN 'Middle'
        ELSE 'Junior'
    END AS age_category
FROM employees;
```

### Remember

```text
CASE = Conditional logic
```

---

# 21. AS Clause

## What is AS?

`AS` creates an **alias**, or temporary name, for a column.

---

## Example

```sql
SELECT
    employee_name AS name,
    salary AS monthly_salary
FROM employees;
```

The original column names remain unchanged.

Only the query result displays:

| name | monthly_salary |
| ---- | -------------: |
| Amit |          45000 |
| Ravi |          55000 |

---

## AS with Aggregate Functions

Instead of displaying:

```text
COUNT(*)
```

we can write:

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

Result:

| department | employee_count |
| ---------- | -------------: |
| IT         |              3 |
| Sales      |              3 |
| HR         |              2 |
| Finance    |              2 |

### Important

`AS` does not permanently rename the column.

It only gives the result a temporary name.

---

# 22. Combining Clauses

In real SQL queries, clauses are normally used together.

### Example

Question:

> Find active employees, group them by department, show only departments having at least 2 active employees, and sort by employee count.

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
WHERE status = 'Active'
GROUP BY department
HAVING COUNT(*) >= 2
ORDER BY total_employees DESC;
```

Here we use:

```text
FROM
↓
WHERE
↓
GROUP BY
↓
HAVING
↓
ORDER BY
```

This is an important SQL pattern.

---

# 23. SQL Query Structure

A common query structure is:

```sql
SELECT column1, column2
FROM table_name
WHERE condition
GROUP BY column1
HAVING condition
ORDER BY column1
LIMIT number;
```

Not every query needs every clause.

For example:

### Simple Query

```sql
SELECT *
FROM employees;
```

### Filter

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

### Filter + Sort

```sql
SELECT *
FROM employees
WHERE department = 'IT'
ORDER BY salary DESC;
```

### Group + Filter + Sort

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department
HAVING COUNT(*) >= 2
ORDER BY total_employees DESC;
```

---

# 24. Logical Processing Order

For the clauses covered in this chapter, a simplified logical processing order is:

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
  ↓
LIMIT
```

### Example

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
WHERE status = 'Active'
GROUP BY department
HAVING COUNT(*) >= 2
ORDER BY total_employees DESC
LIMIT 3;
```

Think:

```text
1. FROM      → Take data from employees
2. WHERE     → Keep Active employees
3. GROUP BY  → Group by department
4. HAVING    → Keep groups with 2+ employees
5. SELECT    → Display department and count
6. ORDER BY  → Sort result
7. LIMIT     → Return maximum 3 rows
```

---

# 25. Exercises

## Exercise 1

### Question

Display all employees.

### Solution

```sql
SELECT *
FROM employees;
```

---

## Exercise 2

### Question

Display employee name and salary.

### Solution

```sql
SELECT employee_name, salary
FROM employees;
```

---

## Exercise 3

### Question

Find employees from the IT department.

### Solution

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

---

## Exercise 4

### Question

Find employees earning more than 60000.

### Solution

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 60000;
```

---

## Exercise 5

### Question

Display unique cities.

### Solution

```sql
SELECT DISTINCT city
FROM employees;
```

---

## Exercise 6

### Question

Display employees from highest salary to lowest salary.

### Solution

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC;
```

---

## Exercise 7

### Question

Find the 3 employees with the highest salary.

### Solution

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

---

## Exercise 8

### Question

Count employees in each department.

### Solution

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
GROUP BY department;
```

---

## Exercise 9

### Question

Find the total salary of each department.

### Solution

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

---

## Exercise 10

### Question

Find the average salary of each department.

### Solution

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

---

## Exercise 11

### Question

Show departments having more than 2 employees.

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

## Exercise 12

### Question

Find employees from IT, HR, and Sales.

### Solution

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE department IN ('IT', 'HR', 'Sales');
```

---

## Exercise 13

### Question

Find employees whose salary is between 50000 and 70000.

### Solution

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary BETWEEN 50000 AND 70000;
```

---

## Exercise 14

### Question

Find employees whose names start with `A`.

### Solution

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE 'A%';
```

---

## Exercise 15

### Question

Create a salary category:

* 70000+ = High
* 50000–69999 = Medium
* Below 50000 = Low

### Solution

```sql
SELECT
    employee_name,
    salary,
    CASE
        WHEN salary >= 70000 THEN 'High'
        WHEN salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

---

# 26. Interview Questions and Answers

## Q1. What is a SQL clause?

**Answer:**
A clause is a part of an SQL statement that helps filter, group, sort, or control the result.

---

## Q2. What is the purpose of FROM?

**Answer:**
`FROM` specifies the table from which data is retrieved.

---

## Q3. What is the purpose of WHERE?

**Answer:**
`WHERE` filters individual rows based on a condition.

---

## Q4. What is DISTINCT?

**Answer:**
`DISTINCT` removes duplicate values from the query result.

---

## Q5. What is ORDER BY?

**Answer:**
`ORDER BY` sorts the result in ascending or descending order.

---

## Q6. What is GROUP BY?

**Answer:**
`GROUP BY` groups rows having the same value and is commonly used with aggregate functions.

---

## Q7. What is HAVING?

**Answer:**
`HAVING` filters grouped results.

---

## Q8. Difference between WHERE and HAVING?

**Answer:**

| WHERE                                | HAVING                                  |
| ------------------------------------ | --------------------------------------- |
| Filters rows                         | Filters groups                          |
| Used before grouping                 | Used after grouping                     |
| Commonly used with normal conditions | Commonly used with aggregate conditions |

Example:

```sql
WHERE salary > 50000
```

versus:

```sql
HAVING AVG(salary) > 50000
```

---

## Q9. What is LIMIT?

**Answer:**
`LIMIT` restricts the number of rows returned by a MySQL query.

---

## Q10. What is IN?

**Answer:**
`IN` checks whether a value matches any value in a specified list.

---

## Q11. What is BETWEEN?

**Answer:**
`BETWEEN` filters values within a specified range, including the boundary values.

---

## Q12. What is LIKE?

**Answer:**
`LIKE` is used for pattern matching.

Common wildcards:

```text
% → Zero or more characters
_ → Exactly one character
```

---

## Q13. What is CASE?

**Answer:**
`CASE` is used to implement conditional logic in SQL.

---

## Q14. What is AS?

**Answer:**
`AS` provides a temporary alias for a column or table.

---

# 27. Important Differences

## WHERE vs HAVING

```text
WHERE
→ Filter rows

HAVING
→ Filter groups
```

---

## IN vs BETWEEN

```text
IN
→ List of values

BETWEEN
→ Range of values
```

Example:

```sql
WHERE city IN ('Mumbai', 'Delhi')
```

```sql
WHERE salary BETWEEN 50000 AND 70000
```

---

## DISTINCT vs GROUP BY

```text
DISTINCT
→ Removes duplicate results

GROUP BY
→ Creates groups, usually for aggregate calculations
```

Example:

```sql
SELECT DISTINCT department
FROM employees;
```

versus:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

---

## ORDER BY ASC vs DESC

```text
ASC
→ Low to high / A to Z

DESC
→ High to low / Z to A
```

---

# 28. Quick Revision

```text
FROM
→ Select the source table

WHERE
→ Filter rows

DISTINCT
→ Remove duplicates

ORDER BY
→ Sort records

GROUP BY
→ Create groups

HAVING
→ Filter groups

LIMIT
→ Limit result rows

IN
→ Match multiple values

BETWEEN
→ Match a range

LIKE
→ Search a pattern

CASE
→ Apply conditions

AS
→ Create an alias
```

---

# 29. Most Important SQL Pattern

Remember this structure:

```sql
SELECT columns
FROM table
WHERE condition
GROUP BY columns
HAVING condition
ORDER BY columns
LIMIT number;
```

You **do not need to use every clause in every query**.

For example:

```sql
SELECT *
FROM employees;
```

is perfectly valid.

And:

```sql
SELECT
    department,
    COUNT(*) AS total_employees
FROM employees
WHERE status = 'Active'
GROUP BY department
HAVING COUNT(*) >= 2
ORDER BY total_employees DESC
LIMIT 3;
```

is a more advanced combination of the same basic clauses.

---

# 30. Final Summary

### The basic purpose of each clause

```text
FROM      → Where is the data?
WHERE     → Which rows do I want?
DISTINCT  → Remove duplicates
GROUP BY  → How should I group the data?
HAVING    → Which groups do I want?
ORDER BY  → How should I sort the result?
LIMIT     → How many rows do I need?
IN        → Which values from this list?
BETWEEN   → Which values in this range?
LIKE      → Which values match this pattern?
CASE      → What category/condition should I create?
AS        → What name should I display?
```

### One-line memory trick

> **FROM → Filter → Group → Filter Group → Select → Sort → Limit**

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
 ↓
LIMIT
```

This chapter intentionally stays within the **basic SQL Clauses syllabus**. Topics such as **JOIN, UNION, EXISTS, Subqueries, CTE, Window Functions, and other advanced SQL concepts** should be taught separately.
