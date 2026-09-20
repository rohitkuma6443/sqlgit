# SQL Querying — SELECT, WHERE, ORDER BY, DISTINCT

This chapter covers the four basic SQL querying commands/clauses:

1. `SELECT`
2. `WHERE`
3. `ORDER BY`
4. `DISTINCT`

We will use **MySQL** and one common dataset throughout the chapter.

---

# 1. What is Querying?

**Querying** means asking the database to retrieve specific information.

For example:

> Show me all employees.

```sql
SELECT *
FROM employees;
```

Or:

> Show employees whose salary is greater than ₹50,000.

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

The basic structure is:

```sql
SELECT columns
FROM table
WHERE condition
ORDER BY column;
```

---

# 2. Dataset

Create the database and table:

```sql
CREATE DATABASE company_db;

USE company_db;
```

Create the table:

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department VARCHAR(50),
    city VARCHAR(50),
    salary DECIMAL(10,2),
    experience INT
);
```

Insert data:

```sql
INSERT INTO employees
(employee_id, employee_name, department, city, salary, experience)
VALUES
(101, 'Rohit', 'IT', 'Mumbai', 55000, 5),
(102, 'Amit', 'Sales', 'Pune', 45000, 3),
(103, 'Priya', 'HR', 'Mumbai', 50000, 4),
(104, 'Neha', 'IT', 'Delhi', 65000, 7),
(105, 'Rahul', 'Finance', 'Pune', 60000, 6),
(106, 'Anjali', 'Marketing', 'Mumbai', 52000, 4),
(107, 'Vikas', 'Sales', 'Delhi', 48000, 3),
(108, 'Sneha', 'HR', 'Pune', 58000, 5),
(109, 'Karan', 'IT', 'Mumbai', 72000, 8),
(110, 'Pooja', 'Finance', 'Delhi', 68000, 7),
(111, 'Raj', 'Sales', 'Mumbai', 45000, 2),
(112, 'Neha', 'HR', 'Delhi', 50000, 4);
```

Our data looks like:

| employee_id | employee_name | department | city   | salary | experience |
| ----------: | ------------- | ---------- | ------ | -----: | ---------: |
|         101 | Rohit         | IT         | Mumbai |  55000 |          5 |
|         102 | Amit          | Sales      | Pune   |  45000 |          3 |
|         103 | Priya         | HR         | Mumbai |  50000 |          4 |
|         104 | Neha          | IT         | Delhi  |  65000 |          7 |
|         105 | Rahul         | Finance    | Pune   |  60000 |          6 |
|         106 | Anjali        | Marketing  | Mumbai |  52000 |          4 |
|         107 | Vikas         | Sales      | Delhi  |  48000 |          3 |
|         108 | Sneha         | HR         | Pune   |  58000 |          5 |
|         109 | Karan         | IT         | Mumbai |  72000 |          8 |
|         110 | Pooja         | Finance    | Delhi  |  68000 |          7 |
|         111 | Raj           | Sales      | Mumbai |  45000 |          2 |
|         112 | Neha          | HR         | Delhi  |  50000 |          4 |

---

# 3. SELECT

`SELECT` is used to retrieve data from a table.

Basic syntax:

```sql
SELECT column_name
FROM table_name;
```

---

## 3.1 SELECT All Columns

Use `*` to select all columns.

```sql
SELECT *
FROM employees;
```

### Meaning

```text
SELECT → What do you want?
*      → Everything
FROM   → From which table?
employees → Table name
```

---

# 4. SELECT Specific Columns

You don't always need all columns.

```sql
SELECT employee_name, salary
FROM employees;
```

Result:

| employee_name | salary |
| ------------- | -----: |
| Rohit         |  55000 |
| Amit          |  45000 |
| Priya         |  50000 |
| Neha          |  65000 |
| Rahul         |  60000 |
| ...           |    ... |

---

# 5. Selecting Multiple Columns

```sql
SELECT employee_id, employee_name, department, city
FROM employees;
```

The order of columns in the `SELECT` statement determines the order in the result.

```sql
SELECT city, employee_name, salary
FROM employees;
```

Result columns will be:

```text
city
employee_name
salary
```

---

# 6. SELECT with Calculation

SQL can perform calculations.

```sql
SELECT
    employee_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

Example:

| employee_name | salary | annual_salary |
| ------------- | -----: | ------------: |
| Rohit         |  55000 |        660000 |
| Amit          |  45000 |        540000 |
| Priya         |  50000 |        600000 |

### Important

`AS` creates an **alias**.

```sql
salary * 12 AS annual_salary
```

Here:

```text
salary * 12 → Expression
annual_salary → Alias
```

---

# 7. SELECT with Arithmetic

SQL supports:

```text
+ Addition
- Subtraction
* Multiplication
/ Division
% Modulo
```

Example:

```sql
SELECT
    employee_name,
    salary,
    salary + 5000 AS increased_salary
FROM employees;
```

---

# 8. WHERE

`WHERE` is used to **filter rows**.

Syntax:

```sql
SELECT columns
FROM table
WHERE condition;
```

Example:

```sql
SELECT *
FROM employees
WHERE salary > 60000;
```

Result:

| employee_name | salary |
| ------------- | -----: |
| Neha          |  65000 |
| Karan         |  72000 |
| Pooja         |  68000 |

---

# 9. WHERE with Text

Find employees from Mumbai:

```sql
SELECT *
FROM employees
WHERE city = 'Mumbai';
```

### Important

Text values are normally written inside quotes:

```sql
'Mumbai'
'IT'
'Rohit'
```

Numbers don't require quotes:

```sql
50000
5
100
```

---

# 10. Comparison Operators

`WHERE` commonly uses comparison operators.

| Operator | Meaning                 |
| -------- | ----------------------- |
| `=`      | Equal                   |
| `<>`     | Not equal               |
| `!=`     | Not equal in many DBMSs |
| `>`      | Greater than            |
| `<`      | Less than               |
| `>=`     | Greater than or equal   |
| `<=`     | Less than or equal      |

---

# 11. WHERE with `=`

Find IT employees:

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

---

# 12. WHERE with `>`

Employees earning more than ₹60,000:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 60000;
```

---

# 13. WHERE with `<`

Employees earning less than ₹50,000:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary < 50000;
```

---

# 14. WHERE with `>=`

Employees earning ₹60,000 or more:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary >= 60000;
```

---

# 15. WHERE with `<=`

Employees earning ₹50,000 or less:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary <= 50000;
```

---

# 16. WHERE with `<>`

Find employees who are not from IT:

```sql
SELECT *
FROM employees
WHERE department <> 'IT';
```

---

# 17. WHERE with AND

`AND` means **all conditions must be true**.

Find IT employees earning more than ₹60,000:

```sql
SELECT *
FROM employees
WHERE department = 'IT'
AND salary > 60000;
```

Result:

| employee_name | department | salary |
| ------------- | ---------- | -----: |
| Neha          | IT         |  65000 |
| Karan         | IT         |  72000 |

---

# 18. WHERE with OR

`OR` means **at least one condition must be true**.

Find employees from Mumbai or Pune:

```sql
SELECT *
FROM employees
WHERE city = 'Mumbai'
OR city = 'Pune';
```

---

# 19. AND vs OR

### AND

```sql
WHERE city = 'Mumbai'
AND salary > 50000;
```

Means:

```text
Mumbai
AND
salary > 50000
```

Both conditions must be true.

### OR

```sql
WHERE city = 'Mumbai'
OR salary > 50000;
```

Only one condition needs to be true.

---

# 20. Parentheses with WHERE

When combining `AND` and `OR`, use parentheses to make the intended logic clear.

Example:

```sql
SELECT *
FROM employees
WHERE department = 'IT'
AND (city = 'Mumbai' OR city = 'Delhi');
```

This means:

```text
IT employees
AND
(Mumbai OR Delhi)
```

---

# 21. ORDER BY

`ORDER BY` is used to **sort the result**.

Syntax:

```sql
SELECT columns
FROM table
ORDER BY column;
```

Default sorting is generally ascending.

---

# 22. ORDER BY ASC

Sort salary from lowest to highest:

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary ASC;
```

Result starts:

| employee_name | salary |
| ------------- | -----: |
| Amit          |  45000 |
| Raj           |  45000 |
| Vikas         |  48000 |
| Priya         |  50000 |
| Neha          |  50000 |

---

# 23. ORDER BY DESC

Sort salary from highest to lowest:

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC;
```

Result starts:

| employee_name | salary |
| ------------- | -----: |
| Karan         |  72000 |
| Pooja         |  68000 |
| Neha          |  65000 |
| Rahul         |  60000 |
| Sneha         |  58000 |

---

# 24. ORDER BY Text

Sort employee names alphabetically:

```sql
SELECT employee_name
FROM employees
ORDER BY employee_name ASC;
```

Reverse alphabetical order:

```sql
SELECT employee_name
FROM employees
ORDER BY employee_name DESC;
```

---

# 25. ORDER BY Multiple Columns

You can sort by more than one column.

```sql
SELECT employee_name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
```

Meaning:

```text
1. Sort department A → Z
2. Within each department,
   sort salary highest → lowest
```

For example, IT employees would appear:

| employee_name | department | salary |
| ------------- | ---------- | -----: |
| Karan         | IT         |  72000 |
| Neha          | IT         |  65000 |
| Rohit         | IT         |  55000 |

---

# 26. ORDER BY Using Column Position

You can sometimes write:

```sql
SELECT employee_name, salary
FROM employees
ORDER BY 2 DESC;
```

Here:

```text
1 → employee_name
2 → salary
```

Therefore salary is sorted descending.

### Recommendation

For teaching and production SQL, prefer:

```sql
ORDER BY salary DESC;
```

because it is clearer and less fragile if the `SELECT` list changes.

---

# 27. DISTINCT

`DISTINCT` removes duplicate rows from the result.

Syntax:

```sql
SELECT DISTINCT column
FROM table;
```

---

# 28. DISTINCT Example

Our data contains:

```text
Mumbai
Pune
Mumbai
Delhi
Pune
Mumbai
Delhi
Pune
Mumbai
Delhi
Mumbai
Delhi
```

Get unique cities:

```sql
SELECT DISTINCT city
FROM employees;
```

Result:

| city   |
| ------ |
| Mumbai |
| Pune   |
| Delhi  |

---

# 29. DISTINCT Department

```sql
SELECT DISTINCT department
FROM employees;
```

Result:

| department |
| ---------- |
| IT         |
| Sales      |
| HR         |
| Finance    |
| Marketing  |

---

# 30. DISTINCT with Multiple Columns

This is important.

```sql
SELECT DISTINCT city, department
FROM employees;
```

Here SQL removes duplicate **combinations** of:

```text
city + department
```

For example:

| city   | department |
| ------ | ---------- |
| Mumbai | IT         |
| Mumbai | HR         |
| Mumbai | Marketing  |
| Pune   | Sales      |
| Pune   | Finance    |
| Delhi  | IT         |
| Delhi  | Sales      |
| Delhi  | HR         |
| Delhi  | Finance    |

`DISTINCT` does **not** mean unique city and unique department separately.

It means:

> Return unique combinations of the selected columns.

---

# 31. DISTINCT with Calculations

You can also use:

```sql
SELECT DISTINCT salary * 12 AS annual_salary
FROM employees;
```

This returns unique calculated annual salaries.

---

# 32. Combining SELECT + WHERE + ORDER BY

Now combine what we have learned.

### Question

Find employees earning more than ₹50,000 and sort them from highest to lowest salary.

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

This is a very important basic SQL pattern.

---

# 33. Combining DISTINCT + WHERE

### Question

Find unique cities where IT employees work.

```sql
SELECT DISTINCT city
FROM employees
WHERE department = 'IT';
```

Result:

```text
Mumbai
Delhi
```

---

# 34. Combining DISTINCT + ORDER BY

Find unique cities and sort alphabetically:

```sql
SELECT DISTINCT city
FROM employees
ORDER BY city ASC;
```

---

# 35. Complete Query

Let's write a more realistic query:

### Question

Find employees from Mumbai whose salary is at least ₹50,000 and display them from highest salary to lowest.

```sql
SELECT
    employee_name,
    department,
    salary
FROM employees
WHERE city = 'Mumbai'
AND salary >= 50000
ORDER BY salary DESC;
```

---

# 36. SQL Query Structure

For this chapter, remember:

```sql
SELECT columns
FROM table
WHERE condition
ORDER BY column ASC/DESC;
```

Example:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary >= 50000
ORDER BY salary DESC;
```

---

# 37. Logical Query Processing Order

Although we **write**:

```sql
SELECT
FROM
WHERE
ORDER BY
```

SQL conceptually processes the query roughly as:

```text
FROM
  ↓
WHERE
  ↓
SELECT
  ↓
ORDER BY
```

For example:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

Conceptually:

```text
1. FROM employees
       ↓
2. Filter salary > 50000
       ↓
3. Select employee_name, salary
       ↓
4. Sort salary DESC
```

This becomes especially important when you learn `GROUP BY`, `HAVING`, aliases, and window functions.

---

# 38. Common Mistakes

## Mistake 1 — Forgetting quotes around text

Wrong:

```sql
SELECT *
FROM employees
WHERE city = Mumbai;
```

Correct:

```sql
SELECT *
FROM employees
WHERE city = 'Mumbai';
```

---

## Mistake 2 — Using `=` with NULL

Wrong:

```sql
WHERE city = NULL;
```

Correct:

```sql
WHERE city IS NULL;
```

We will cover `NULL` separately in detail.

---

## Mistake 3 — Forgetting WHERE in UPDATE

Dangerous:

```sql
UPDATE employees
SET salary = salary + 5000;
```

This updates **every employee**.

If you want only IT employees:

```sql
UPDATE employees
SET salary = salary + 5000
WHERE department = 'IT';
```

---

## Mistake 4 — Forgetting WHERE in DELETE

Dangerous:

```sql
DELETE FROM employees;
```

This deletes all rows.

Specific:

```sql
DELETE FROM employees
WHERE employee_id = 101;
```

---

## Mistake 5 — Confusing DISTINCT with GROUP BY

`DISTINCT`:

```sql
SELECT DISTINCT city
FROM employees;
```

is primarily used to remove duplicate result rows.

`GROUP BY` is used to create groups, especially when performing aggregate calculations:

```sql
SELECT city, AVG(salary)
FROM employees
GROUP BY city;
```

We will cover `GROUP BY` separately.

---

# 39. Solved Exercises

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

Display only employee name and department.

### Solution

```sql
SELECT employee_name, department
FROM employees;
```

---

## Exercise 3

### Question

Display employees whose salary is greater than ₹60,000.

### Solution

```sql
SELECT *
FROM employees
WHERE salary > 60000;
```

---

## Exercise 4

### Question

Display employees from Mumbai.

### Solution

```sql
SELECT *
FROM employees
WHERE city = 'Mumbai';
```

---

## Exercise 5

### Question

Display IT employees.

### Solution

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

---

## Exercise 6

### Question

Display employees with at least 5 years of experience.

### Solution

```sql
SELECT employee_name, experience
FROM employees
WHERE experience >= 5;
```

---

## Exercise 7

### Question

Display employees whose salary is between ₹50,000 and ₹65,000.

### Solution

```sql
SELECT employee_name, salary
FROM employees
WHERE salary >= 50000
AND salary <= 65000;
```

---

## Exercise 8

### Question

Display employees from Mumbai who earn more than ₹50,000.

### Solution

```sql
SELECT *
FROM employees
WHERE city = 'Mumbai'
AND salary > 50000;
```

---

## Exercise 9

### Question

Display employees from Mumbai or Delhi.

### Solution

```sql
SELECT *
FROM employees
WHERE city = 'Mumbai'
OR city = 'Delhi';
```

---

## Exercise 10

### Question

Sort all employees by salary from highest to lowest.

### Solution

```sql
SELECT employee_name, salary
FROM employees
ORDER BY salary DESC;
```

---

## Exercise 11

### Question

Sort employees by experience from lowest to highest.

### Solution

```sql
SELECT employee_name, experience
FROM employees
ORDER BY experience ASC;
```

---

## Exercise 12

### Question

Display unique cities.

### Solution

```sql
SELECT DISTINCT city
FROM employees;
```

---

## Exercise 13

### Question

Display unique departments.

### Solution

```sql
SELECT DISTINCT department
FROM employees;
```

---

## Exercise 14

### Question

Find unique cities where IT employees work.

### Solution

```sql
SELECT DISTINCT city
FROM employees
WHERE department = 'IT';
```

---

## Exercise 15

### Question

Display employees earning more than ₹50,000, highest salary first.

### Solution

```sql
SELECT employee_name, department, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

---

# 40. Practice Questions

Try these yourself before checking any solution.

### Beginner

**Q1.** Display all columns from `employees`.

**Q2.** Display only:

```text
employee_name
city
salary
```

**Q3.** Display employees whose salary is exactly `50000`.

**Q4.** Display employees whose salary is less than `50000`.

**Q5.** Display employees with experience greater than `5`.

**Q6.** Display employees from `Delhi`.

**Q7.** Display employees who are not from `Mumbai`.

---

### Intermediate

**Q8.** Display IT employees earning more than `60000`.

**Q9.** Display employees from Mumbai with at least `4` years of experience.

**Q10.** Display employees from Mumbai or Delhi.

**Q11.** Display employees from Sales or HR.

**Q12.** Display employees whose salary is between `50000` and `65000`.

**Q13.** Display employees whose experience is between `3` and `6`.

**Q14.** Display all employees sorted by salary from highest to lowest.

**Q15.** Display all employees sorted by name alphabetically.

**Q16.** Display employees sorted first by department and then by salary descending.

---

### DISTINCT Practice

**Q17.** Display unique cities.

**Q18.** Display unique departments.

**Q19.** Display unique combinations of:

```text
city
department
```

**Q20.** Display unique cities where the department is `HR`.

---

### Advanced Basic Querying

**Q21.** Display employee name and annual salary.

```text
annual_salary = salary × 12
```

**Q22.** Display employees whose annual salary is greater than `700000`.

**Q23.** Display Mumbai employees earning more than `50000`, sorted by salary descending.

**Q24.** Display IT employees with at least `5` years of experience, sorted by experience descending.

**Q25.** Display unique departments for employees earning more than `55000`.

**Q26.** Display the employee name, salary and annual salary for all employees, sorted by annual salary from highest to lowest.

**Q27.** Display employees who are from Mumbai and have either IT or HR as their department.

**Q28.** Display employees who are from Mumbai or Pune and earn more than `50000`.

**Q29.** Display unique cities for employees whose salary is greater than `55000`.

**Q30.** Display all employees except those from the Sales department, sorted by salary descending.

---

# 41. Quick Revision

```text
SELECT
↓
Choose columns

WHERE
↓
Filter rows

ORDER BY
↓
Sort results

DISTINCT
↓
Remove duplicate result rows
```

### Basic pattern

```sql
SELECT column1, column2
FROM table_name
WHERE condition
ORDER BY column1 ASC;
```

### Example

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

### With DISTINCT

```sql
SELECT DISTINCT city
FROM employees;
```

### With calculation

```sql
SELECT
    employee_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

### The four concepts in one query

```sql
SELECT DISTINCT city
FROM employees
WHERE salary > 50000
ORDER BY city ASC;
```

This means:

```text
SELECT DISTINCT → return unique cities
FROM employees   → from employees table
WHERE salary > 50000 → filter rows
ORDER BY city ASC → sort cities A-Z
```

**Next chapter:** `SQL Operators — Comparison, Logical, Arithmetic, BETWEEN, IN, NOT IN, LIKE, IS NULL` with datasets and practice exercises.
