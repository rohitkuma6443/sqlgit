# SQL JOIN — Complete Practical Notes

## 1. Create Database

```sql
CREATE DATABASE join_practice;
```

Select the database:

```sql
USE join_practice;
```

---

# 2. Create Tables

We will use **3 tables**:

```text
departments
     ↓
employees
     ↓
projects
```

### Table 1: `departments`

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50) NOT NULL,
    location VARCHAR(50)
);
```

### Insert Data

```sql
INSERT INTO departments
(department_id, department_name, location)
VALUES
(10, 'Sales', 'Mumbai'),
(20, 'IT', 'Pune'),
(30, 'HR', 'Delhi'),
(40, 'Finance', 'Nashik'),
(50, 'Marketing', 'Bangalore');
```

Check the table:

```sql
SELECT * FROM departments;
```

### Output

| department_id | department_name | location  |
| ------------: | --------------- | --------- |
|            10 | Sales           | Mumbai    |
|            20 | IT              | Pune      |
|            30 | HR              | Delhi     |
|            40 | Finance         | Nashik    |
|            50 | Marketing       | Bangalore |

---

# 3. Create Employees Table

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50) NOT NULL,
    department_id INT,
    salary DECIMAL(10,2),
    manager_id INT
);
```

### Insert Data

```sql
INSERT INTO employees
(employee_id, employee_name, department_id, salary, manager_id)
VALUES
(1, 'Amit', 10, 45000, NULL),
(2, 'Ravi', 20, 55000, 1),
(3, 'Neha', 10, 48000, 1),
(4, 'Priya', NULL, 40000, 2),
(5, 'Rahul', 30, 60000, 2),
(6, 'Sneha', 20, 52000, 2),
(7, 'Vikas', 40, 58000, 1),
(8, 'Anita', NULL, 42000, 3);
```

Check:

```sql
SELECT * FROM employees;
```

### Output

| employee_id | employee_name | department_id | salary | manager_id |
| ----------: | ------------- | ------------: | -----: | ---------: |
|           1 | Amit          |            10 |  45000 |       NULL |
|           2 | Ravi          |            20 |  55000 |          1 |
|           3 | Neha          |            10 |  48000 |          1 |
|           4 | Priya         |          NULL |  40000 |          2 |
|           5 | Rahul         |            30 |  60000 |          2 |
|           6 | Sneha         |            20 |  52000 |          2 |
|           7 | Vikas         |            40 |  58000 |          1 |
|           8 | Anita         |          NULL |  42000 |          3 |

Notice:

* Priya has no department.
* Anita has no department.
* Marketing department has no employee.

These records are intentionally added for practicing `LEFT JOIN`.

---

# 4. Create Projects Table

```sql
CREATE TABLE projects (
    project_id INT PRIMARY KEY,
    project_name VARCHAR(100),
    employee_id INT,
    budget DECIMAL(12,2)
);
```

### Insert Data

```sql
INSERT INTO projects
(project_id, project_name, employee_id, budget)
VALUES
(101, 'Website Development', 2, 150000),
(102, 'Sales Dashboard', 1, 100000),
(103, 'HR Management System', 5, 200000),
(104, 'Data Migration', 6, 175000),
(105, 'Marketing Campaign', NULL, 120000);
```

Check:

```sql
SELECT * FROM projects;
```

### Output

| project_id | project_name         | employee_id | budget |
| ---------: | -------------------- | ----------: | -----: |
|        101 | Website Development  |           2 | 150000 |
|        102 | Sales Dashboard      |           1 | 100000 |
|        103 | HR Management System |           5 | 200000 |
|        104 | Data Migration       |           6 | 175000 |
|        105 | Marketing Campaign   |        NULL | 120000 |

---

# 5. Relationship Between Tables

```text
departments
----------------
department_id PK
department_name
location
       ↑
       |
       |
employees
----------------
employee_id PK
employee_name
department_id FK
salary
manager_id
       ↑
       |
       |
projects
----------------
project_id PK
project_name
employee_id FK
budget
```

The important relationships are:

```text
departments.department_id
          =
employees.department_id
```

and

```text
employees.employee_id
          =
projects.employee_id
```

---

# 6. INNER JOIN

## Definition

Returns only records that exist in **both tables**.

### Question

Display employee name and department name.

### Query

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

### Result

| employee_name | department_name |
| ------------- | --------------- |
| Amit          | Sales           |
| Ravi          | IT              |
| Neha          | Sales           |
| Rahul         | HR              |
| Sneha         | IT              |
| Vikas         | Finance         |

Priya and Anita are not displayed because they don't have a department.

Marketing is not displayed because it doesn't have employees.

---

# 7. LEFT JOIN

## Definition

Returns:

```text
All rows from LEFT table
+
Matching rows from RIGHT table
```

### Question

Display all employees with their department.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

### Result

| employee_name | department_name |
| ------------- | --------------- |
| Amit          | Sales           |
| Ravi          | IT              |
| Neha          | Sales           |
| Priya         | NULL            |
| Rahul         | HR              |
| Sneha         | IT              |
| Vikas         | Finance         |
| Anita         | NULL            |

Priya and Anita are included because `employees` is the LEFT table.

---

# 8. RIGHT JOIN

### Question

Display all departments and their employees.

```sql
SELECT
    d.department_name,
    e.employee_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

### Result

| department_name | employee_name |
| --------------- | ------------- |
| Sales           | Amit          |
| Sales           | Neha          |
| IT              | Ravi          |
| IT              | Sneha         |
| HR              | Rahul         |
| Finance         | Vikas         |
| Marketing       | NULL          |

Marketing appears even though it has no employees.

---

# 9. FULL OUTER JOIN

MySQL does not directly support:

```sql
FULL OUTER JOIN
```

We can use:

```sql
LEFT JOIN
UNION
RIGHT JOIN
```

### Query

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id

UNION

SELECT
    e.employee_name,
    d.department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

This gives:

* employees with departments
* employees without departments
* departments without employees

---

# 10. Find Employees Without Department

This is a very important real-world JOIN query.

### Question

Find employees who do not belong to any department.

### Solution

```sql
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

### Output

| employee_id | employee_name |
| ----------: | ------------- |
|           4 | Priya         |
|           8 | Anita         |

### Pattern to remember

```sql
LEFT JOIN
WHERE right_table.id IS NULL
```

means:

> Find records in the left table that have no matching record in the right table.

---

# 11. Find Departments Without Employees

### Question

Find departments that have no employees.

```sql
SELECT
    d.department_id,
    d.department_name
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

### Output

| department_id | department_name |
| ------------: | --------------- |
|            50 | Marketing       |

---

# 12. JOIN Three Tables

Now we will join:

```text
employees
    ↓
departments
    ↓
projects
```

Actually, the relationships are:

```text
employees → departments
employees → projects
```

### Question

Display:

* Employee name
* Department name
* Project name

### Solution

```sql
SELECT
    e.employee_name,
    d.department_name,
    p.project_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
LEFT JOIN projects p
    ON e.employee_id = p.employee_id;
```

### Result

| employee_name | department_name | project_name         |
| ------------- | --------------- | -------------------- |
| Amit          | Sales           | Sales Dashboard      |
| Ravi          | IT              | Website Development  |
| Neha          | Sales           | NULL                 |
| Priya         | NULL            | NULL                 |
| Rahul         | HR              | HR Management System |
| Sneha         | IT              | Data Migration       |
| Vikas         | Finance         | NULL                 |
| Anita         | NULL            | NULL                 |

---

# 13. Employee + Department + Project Budget

### Question

Display employee name, department, project and budget.

```sql
SELECT
    e.employee_name,
    d.department_name,
    p.project_name,
    p.budget
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.department_id
LEFT JOIN projects p
    ON e.employee_id = p.employee_id;
```

---

# 14. JOIN with WHERE

### Question

Display employees working in the IT department.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```

### Output

| employee_name | department_name |
| ------------- | --------------- |
| Ravi          | IT              |
| Sneha         | IT              |

---

# 15. JOIN with Salary Condition

### Question

Find employees earning more than ₹50,000 with their department.

```sql
SELECT
    e.employee_name,
    e.salary,
    d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
WHERE e.salary > 50000;
```

---

# 16. JOIN with GROUP BY

### Question

Count employees in each department.

```sql
SELECT
    d.department_name,
    COUNT(e.employee_id) AS total_employees
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
GROUP BY d.department_name;
```

### Result

| department_name | total_employees |
| --------------- | --------------: |
| Sales           |               2 |
| IT              |               2 |
| HR              |               1 |
| Finance         |               1 |
| Marketing       |               0 |

### Why `LEFT JOIN`?

Because we also want:

```text
Marketing → 0 employees
```

With `INNER JOIN`, Marketing would disappear.

---

# 17. Total Salary by Department

```sql
SELECT
    d.department_name,
    SUM(e.salary) AS total_salary
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
GROUP BY d.department_name;
```

---

# 18. Average Salary by Department

```sql
SELECT
    d.department_name,
    AVG(e.salary) AS average_salary
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
GROUP BY d.department_name;
```

---

# 19. SELF JOIN

We already have `manager_id` in the employees table.

For example:

```text
Amit → Manager = NULL
Ravi → Manager = Amit
Neha → Manager = Amit
Priya → Manager = Ravi
```

### Question

Display employee name and manager name.

```sql
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```

### Result

| employee | manager |
| -------- | ------- |
| Amit     | NULL    |
| Ravi     | Amit    |
| Neha     | Amit    |
| Priya    | Ravi    |
| Rahul    | Ravi    |
| Sneha    | Ravi    |
| Vikas    | Amit    |
| Anita    | Neha    |

This is called a **SELF JOIN** because:

```sql
employees e
```

is joined with:

```sql
employees m
```

---

# 20. CROSS JOIN

### Question

Generate every possible combination of employees and departments.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees e
CROSS JOIN departments d;
```

There are:

```text
8 employees × 5 departments
= 40 combinations
```

---

# 21. JOIN Exercises

## Exercise 1

### Question

Display employee name and department name.

### Solution

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

---

## Exercise 2

### Question

Display all employees, including employees without departments.

### Solution

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

---

## Exercise 3

### Question

Display all departments, including departments without employees.

### Solution

```sql
SELECT
    d.department_name,
    e.employee_name
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id;
```

---

## Exercise 4

### Question

Find employees who don't have a department.

### Solution

```sql
SELECT
    e.employee_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

**Answer:**

```text
Priya
Anita
```

---

## Exercise 5

### Question

Find departments that don't have employees.

### Solution

```sql
SELECT
    d.department_name
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

**Answer:**

```text
Marketing
```

---

## Exercise 6

### Question

Display employee name, department and location.

### Solution

```sql
SELECT
    e.employee_name,
    d.department_name,
    d.location
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

---

## Exercise 7

### Question

Find employees working in Sales.

### Solution

```sql
SELECT
    e.employee_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_name = 'Sales';
```

**Answer:**

```text
Amit
Neha
```

---

## Exercise 8

### Question

Find employees earning more than ₹55,000.

### Solution

```sql
SELECT
    e.employee_name,
    e.salary,
    d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
WHERE e.salary > 55000;
```

---

## Exercise 9

### Question

Count employees in every department.

### Solution

```sql
SELECT
    d.department_name,
    COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
GROUP BY d.department_name;
```

---

## Exercise 10

### Question

Find the department with the highest total salary.

### Solution

```sql
SELECT
    d.department_name,
    SUM(e.salary) AS total_salary
FROM departments d
JOIN employees e
ON d.department_id = e.department_id
GROUP BY d.department_name
ORDER BY total_salary DESC
LIMIT 1;
```

---

## Exercise 11

### Question

Display employees and their managers.

### Solution

```sql
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```

---

## Exercise 12

### Question

Display employee, department and project name.

### Solution

```sql
SELECT
    e.employee_name,
    d.department_name,
    p.project_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
LEFT JOIN projects p
ON e.employee_id = p.employee_id;
```

---

# 22. Interview Questions

### Q1. What is JOIN?

**Answer:** JOIN combines related data from two or more tables.

### Q2. What is INNER JOIN?

**Answer:** It returns only matching records from both tables.

### Q3. What is LEFT JOIN?

**Answer:** It returns all records from the left table and matching records from the right table.

### Q4. What is RIGHT JOIN?

**Answer:** It returns all records from the right table and matching records from the left table.

### Q5. Does MySQL support FULL OUTER JOIN?

**Answer:** No, MySQL does not directly support `FULL OUTER JOIN`. We can use `LEFT JOIN + RIGHT JOIN + UNION`.

### Q6. What is SELF JOIN?

**Answer:** Joining a table with itself. It is commonly used for employee-manager relationships.

### Q7. What is CROSS JOIN?

**Answer:** It produces every possible combination of rows from two tables.

### Q8. What is the difference between ON and WHERE?

**Answer:**

```text
ON     → Defines the JOIN condition
WHERE  → Filters the result
```

### Q9. Can we JOIN three or more tables?

**Answer:** Yes. SQL allows multiple JOINs in one query.

### Q10. How do you find unmatched records?

**Answer:**

```sql
LEFT JOIN
WHERE right_table.id IS NULL
```

Example:

```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

---

# 23. JOIN Quick Revision

| JOIN              | Meaning                   |
| ----------------- | ------------------------- |
| `INNER JOIN`      | Only matching records     |
| `LEFT JOIN`       | All LEFT + matching RIGHT |
| `RIGHT JOIN`      | All RIGHT + matching LEFT |
| `FULL OUTER JOIN` | Everything from both      |
| `CROSS JOIN`      | Every combination         |
| `SELF JOIN`       | Table joined with itself  |

### Most Important Patterns

```sql
-- Matching records
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

```sql
-- All employees
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

```sql
-- Employees without department
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

```sql
-- Departments without employees
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
WHERE e.employee_id IS NULL;
```

```sql
-- Employee + Manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```

```sql
-- Three tables
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
JOIN projects p
ON e.employee_id = p.employee_id;
```

## Final JOIN Concept

```text
                    SQL JOIN
                       |
        +--------------+--------------+
        |              |              |
      INNER          OUTER          SPECIAL
        |              |              |
        |        +-----+-----+      +---+---+
        |        |           |      |       |
     Matching   LEFT       RIGHT   CROSS   SELF
                  |
             Keep all LEFT
```
