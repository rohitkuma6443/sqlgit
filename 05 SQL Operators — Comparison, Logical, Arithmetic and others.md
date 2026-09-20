# SQL Operators
---

# 1. What is an SQL Operator?

An **SQL operator** is a symbol or keyword used to perform an operation on data.

Operators help us:

* Perform calculations
* Compare values
* Combine conditions
* Filter records
* Search for patterns
* Check for `NULL` values

### Simple Example

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

Here:

```text
>
```

is a **comparison operator**.

It checks whether the employee's salary is greater than `50000`.

---

# 2. Types of SQL Operators

SQL operators can be divided into the following groups:

| Type       | Operators                                         | Purpose              |
| ---------- | ------------------------------------------------- | -------------------- |
| Arithmetic | `+`, `-`, `*`, `/`, `%`                           | Perform calculations |
| Comparison | `=`, `>`, `<`, `>=`, `<=`, `<>`, `!=`             | Compare values       |
| Logical    | `AND`, `OR`, `NOT`                                | Combine conditions   |
| Special    | `IN`, `BETWEEN`, `LIKE`, `IS NULL`, `IS NOT NULL` | Special filtering    |

---

# 3. Practice Database

We will use one common dataset throughout this chapter.

## Create Database

```sql
CREATE DATABASE sql_operator_training;

USE sql_operator_training;
```

---

# 4. Create Employees Table

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    age INT,
    city VARCHAR(50),
    status VARCHAR(20)
);
```

---

# 5. Insert Dataset

```sql
INSERT INTO employees
(employee_id, employee_name, department, salary, age, city, status)
VALUES
(1, 'Amit', 'IT', 45000, 25, 'Mumbai', 'Active'),
(2, 'Ravi', 'Sales', 55000, 30, 'Delhi', 'Active'),
(3, 'Neha', 'IT', 60000, 28, 'Pune', 'Active'),
(4, 'Priya', 'HR', 50000, 32, 'Mumbai', 'Inactive'),
(5, 'Rahul', 'Finance', 70000, 35, 'Delhi', 'Active'),
(6, 'Sneha', 'Sales', 48000, 27, 'Pune', 'Active'),
(7, 'Vikas', 'Finance', 80000, 40, 'Mumbai', 'Active'),
(8, 'Anita', 'HR', 52000, 29, 'Nashik', 'Active'),
(9, 'Karan', 'IT', 42000, 24, 'Nashik', 'Active'),
(10, 'Pooja', 'Sales', 58000, 31, 'Delhi', 'Inactive');
```

Check the data:

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

# Part 1 — Arithmetic Operators

# 6. What are Arithmetic Operators?

Arithmetic operators are used to perform **mathematical calculations** on numeric values.

They are useful when we need to calculate things such as:

* Salary after bonus
* Annual salary
* Discount
* Tax
* Profit
* Quantity × Price

### Arithmetic Operators

| Operator | Meaning        |
| -------- | -------------- |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `*`      | Multiplication |
| `/`      | Division       |
| `%`      | Remainder      |

---

# 7. Addition `+`

The `+` operator adds two numeric values.

### Example

Suppose every employee receives a `5000` bonus.

```sql
SELECT
    employee_name,
    salary,
    salary + 5000 AS salary_after_bonus
FROM employees;
```

### Example Result

| Employee | Salary | Salary After Bonus |
| -------- | -----: | -----------------: |
| Amit     |  45000 |              50000 |
| Ravi     |  55000 |              60000 |
| Neha     |  60000 |              65000 |

### Explanation

For Amit:

```text
45000 + 5000 = 50000
```

The calculation is performed only in the result. The original salary is not changed.

### Remember

```text
+ → Addition
```

---

# 8. Subtraction `-`

The `-` operator subtracts one value from another.

### Example

Suppose we want to calculate salary after a deduction of `2000`.

```sql
SELECT
    employee_name,
    salary,
    salary - 2000 AS salary_after_deduction
FROM employees;
```

For Amit:

```text
45000 - 2000 = 43000
```

### Remember

```text
- → Subtraction
```

---

# 9. Multiplication `*`

The `*` operator is used for multiplication.

### Practical Example

Suppose `salary` represents monthly salary.

We can calculate annual salary:

```sql
SELECT
    employee_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

For Amit:

```text
45000 × 12 = 540000
```

### Remember

```text
* → Multiplication
```

---

# 10. Division `/`

The `/` operator divides one value by another.

### Example

```sql
SELECT
    employee_name,
    salary,
    salary / 2 AS half_salary
FROM employees;
```

For Amit:

```text
45000 / 2 = 22500
```

---

# 11. Modulus `%`

The `%` operator returns the **remainder** after division.

### Example

```sql
SELECT 10 % 3 AS remainder;
```

Result:

| remainder |
| --------: |
|         1 |

Because:

```text
10 ÷ 3 = 3 remainder 1
```

### Practical Example

Find employees whose `employee_id` is even.

```sql
SELECT
    employee_id,
    employee_name
FROM employees
WHERE employee_id % 2 = 0;
```

### Result

| ID | Employee |
| -: | -------- |
|  2 | Ravi     |
|  4 | Priya    |
|  6 | Sneha    |
|  8 | Anita    |
| 10 | Pooja    |

### Remember

```text
% → Remainder
```

---

# Part 2 — Comparison Operators

# 12. What are Comparison Operators?

Comparison operators are used to **compare two values**.

They are commonly used with the `WHERE` clause to filter records.

### Comparison Operators

| Operator | Meaning                  |
| -------- | ------------------------ |
| `=`      | Equal to                 |
| `>`      | Greater than             |
| `<`      | Less than                |
| `>=`     | Greater than or equal to |
| `<=`     | Less than or equal to    |
| `<>`     | Not equal to             |
| `!=`     | Not equal to             |

---

# 13. Equal To `=`

`=` checks whether two values are equal.

### Example

Find employees from the IT department.

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE department = 'IT';
```

### Result

| Employee | Department |
| -------- | ---------- |
| Amit     | IT         |
| Neha     | IT         |
| Karan    | IT         |

### Explanation

SQL checks each row:

```text
Is department equal to 'IT'?
```

If yes, the row is returned.

---

# 14. Greater Than `>`

`>` checks whether the first value is greater than the second value.

### Example

Find employees earning more than `50000`.

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary > 50000;
```

Important:

```text
salary > 50000
```

does **not** include exactly `50000`.

---

# 15. Less Than `<`

`<` checks whether the first value is smaller than the second value.

### Example

Find employees earning less than `50000`.

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary < 50000;
```

---

# 16. Greater Than or Equal `>=`

`>=` means the value can be **greater than or equal to** the specified value.

### Example

Find employees earning at least `50000`.

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary >= 50000;
```

This includes:

```text
50000
55000
60000
70000
...
```

---

# 17. Less Than or Equal `<=`

`<=` means the value can be **less than or equal to** the specified value.

### Example

Find employees aged 30 or younger.

```sql
SELECT
    employee_name,
    age
FROM employees
WHERE age <= 30;
```

This includes an employee whose age is exactly `30`.

---

# 18. Not Equal `<>`

`<>` means **not equal to**.

### Example

Find employees who are not in IT.

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE department <> 'IT';
```

---

# 19. Not Equal `!=`

MySQL also supports `!=`.

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE department != 'IT';
```

In MySQL:

```text
<> 
```

and

```text
!=
```

both mean:

> Not equal to.

---

# Part 3 — Logical Operators

# 20. What are Logical Operators?

Logical operators are used to **combine multiple conditions**.

The three basic logical operators are:

```text
AND
OR
NOT
```

They become especially useful when a query has more than one condition.

---

# 21. AND Operator

## What is AND?

`AND` is used when **all conditions must be true at the same time**.

### Syntax

```sql
SELECT columns
FROM table_name
WHERE condition1
AND condition2;
```

### Practical Example

Find employees who:

1. Work in IT
2. Earn more than `50000`

```sql
SELECT
    employee_name,
    department,
    salary
FROM employees
WHERE department = 'IT'
AND salary > 50000;
```

### Result

| Employee | Department | Salary |
| -------- | ---------- | -----: |
| Neha     | IT         |  60000 |

### How SQL checks it

For each employee:

```text
Is department = IT?
        AND
Is salary > 50000?
```

Both must be true.

### Remember

```text
AND → ALL conditions must be TRUE
```

---

# 22. OR Operator

## What is OR?

`OR` is used when **at least one condition must be true**.

### Example

Find employees from Mumbai or Delhi.

```sql
SELECT
    employee_name,
    city
FROM employees
WHERE city = 'Mumbai'
OR city = 'Delhi';
```

An employee is included if either condition is true.

### Remember

```text
OR → AT LEAST ONE condition must be TRUE
```

---

# 23. AND vs OR

### AND

```sql
WHERE department = 'IT'
AND salary > 50000;
```

Both conditions must be true.

### OR

```sql
WHERE department = 'IT'
OR salary > 50000;
```

At least one condition must be true.

### Easy Memory

```text
AND → All
OR  → Any
```

---

# 24. NOT Operator

## What is NOT?

`NOT` reverses a condition.

If a condition is true, `NOT` makes it false.

If a condition is false, `NOT` makes it true.

### Example

Find employees who are not in IT.

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE NOT department = 'IT';
```

This is logically similar to:

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE department <> 'IT';
```

### Remember

```text
NOT → Reverse the condition
```

---

# 25. Combining AND and OR

We can combine multiple logical operators.

### Example

Find employees who:

* Are in IT and earn more than `50000`
* OR work in Finance

```sql
SELECT
    employee_name,
    department,
    salary
FROM employees
WHERE
    (department = 'IT' AND salary > 50000)
    OR department = 'Finance';
```

### Why use parentheses?

Parentheses make the intended logic clear.

Think of it as:

```text
(IT AND salary > 50000)
          OR
       Finance
```

---

# Part 4 — Special Operators

# 26. IN Operator

## What is IN?

`IN` is used when we want to check whether a column matches **one of several specified values**.

Instead of writing multiple `OR` conditions:

```sql
WHERE city = 'Mumbai'
OR city = 'Delhi'
OR city = 'Pune';
```

we can write:

```sql
WHERE city IN ('Mumbai', 'Delhi', 'Pune');
```

### Practical Example

```sql
SELECT
    employee_name,
    city
FROM employees
WHERE city IN ('Mumbai', 'Delhi', 'Pune');
```

### Remember

```text
IN → Match any value from a list
```

---

# 27. NOT IN

`NOT IN` excludes the specified values.

### Example

Find employees who are not from Mumbai or Delhi.

```sql
SELECT
    employee_name,
    city
FROM employees
WHERE city NOT IN ('Mumbai', 'Delhi');
```

### Remember

```text
IN     → Include these values
NOT IN → Exclude these values
```

---

# 28. BETWEEN Operator

## What is BETWEEN?

`BETWEEN` checks whether a value falls within a specified range.

The boundary values are included.

### Example

Find employees whose salary is between `50000` and `70000`.

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary BETWEEN 50000 AND 70000;
```

This includes:

```text
50000
55000
60000
70000
```

### Equivalent Condition

The same condition can be written as:

```sql
WHERE salary >= 50000
AND salary <= 70000;
```

### Remember

```text
BETWEEN → Range
```

---

# 29. NOT BETWEEN

`NOT BETWEEN` returns values outside the specified range.

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary NOT BETWEEN 50000 AND 70000;
```

This finds salaries below `50000` or above `70000`.

---

# 30. LIKE Operator

## What is LIKE?

`LIKE` is used to search for a **specific text pattern**.

It is especially useful when we do not know the complete value.

For example, instead of asking:

```text
Is the employee name exactly "Amit"?
```

we can ask:

```text
Does the employee name start with "A"?
```

### Wildcards

| Wildcard | Meaning                 |
| -------- | ----------------------- |
| `%`      | Zero or more characters |
| `_`      | Exactly one character   |

---

# 31. LIKE — Starts With

Find employees whose names start with `A`.

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

### Explanation

```text
'A%'
```

means:

```text
A + anything after A
```

---

# 32. LIKE — Ends With

Find names ending with `a`.

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '%a';
```

Here:

```text
'%a'
```

means:

```text
anything before a + a
```

---

# 33. LIKE — Contains

Find names containing the letter `i`.

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '%i%';
```

Here:

```text
'%i%'
```

means:

```text
anything + i + anything
```

---

# 34. LIKE — One Character `_`

The `_` wildcard represents exactly **one character**.

### Example

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '_mit';
```

Pattern:

```text
_ + mit
```

So:

```text
Amit
```

matches.

---

# 35. IS NULL Operator

## What is NULL?

`NULL` represents a missing, unknown, or unavailable value.

`NULL` is different from:

```text
0
''
'NULL'
```

For example, if an employee does not have a phone number, the phone column may contain `NULL`.

---

## Create a Table with NULL Values

```sql
CREATE TABLE employee_contacts (
    employee_id INT,
    employee_name VARCHAR(50),
    phone VARCHAR(20)
);
```

Insert data:

```sql
INSERT INTO employee_contacts
(employee_id, employee_name, phone)
VALUES
(1, 'Amit', '9876543210'),
(2, 'Ravi', NULL),
(3, 'Neha', '9123456780'),
(4, 'Priya', NULL);
```

---

# 36. IS NULL

To find records where a value is missing, use `IS NULL`.

### Example

```sql
SELECT *
FROM employee_contacts
WHERE phone IS NULL;
```

### Result

| ID | Employee | Phone |
| -: | -------- | ----- |
|  2 | Ravi     | NULL  |
|  4 | Priya    | NULL  |

---

# 37. IS NOT NULL

To find records where a value exists, use `IS NOT NULL`.

```sql
SELECT *
FROM employee_contacts
WHERE phone IS NOT NULL;
```

This returns employees whose phone number is available.

---

# 38. Important NULL Rule

Do **not** write:

```sql
WHERE phone = NULL;
```

Use:

```sql
WHERE phone IS NULL;
```

Similarly, do not write:

```sql
WHERE phone != NULL;
```

Use:

```sql
WHERE phone IS NOT NULL;
```

### Remember

```text
NULL
 ↓
IS NULL
IS NOT NULL
```

---

# 39. Operator Precedence

When several operators are used in the same condition, SQL follows an order of evaluation.

For the operators covered here, remember:

```text
()
 ↓
NOT
 ↓
AND
 ↓
OR
```

### Example

```sql
WHERE department = 'IT'
OR department = 'Sales'
AND salary > 50000;
```

`AND` is evaluated before `OR`.

To make the intended logic clear, use parentheses:

```sql
WHERE
    (department = 'IT' OR department = 'Sales')
    AND salary > 50000;
```

### Best Practice

When mixing `AND` and `OR`, **use parentheses**.

It makes the query easier to read and reduces logical mistakes.

---

# 40. Practical Examples

## Example 1 — Salary Greater Than 60000

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary > 60000;
```

---

## Example 2 — IT Employees with Salary Above 50000

```sql
SELECT
    employee_name,
    department,
    salary
FROM employees
WHERE department = 'IT'
AND salary > 50000;
```

---

## Example 3 — Mumbai or Delhi

```sql
SELECT
    employee_name,
    city
FROM employees
WHERE city IN ('Mumbai', 'Delhi');
```

---

## Example 4 — Salary Range

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary BETWEEN 50000 AND 70000;
```

---

## Example 5 — Name Starts with A

```sql
SELECT
    employee_name
FROM employees
WHERE employee_name LIKE 'A%';
```

---

## Example 6 — Active Employees Earning Above 50000

```sql
SELECT
    employee_name,
    salary,
    status
FROM employees
WHERE status = 'Active'
AND salary > 50000;
```

---

## Example 7 — IT or Finance

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE department IN ('IT', 'Finance');
```

---

## Example 8 — Even Employee IDs

```sql
SELECT
    employee_id,
    employee_name
FROM employees
WHERE employee_id % 2 = 0;
```

---

# 41. Exercises with Solutions

## Exercise 1

### Question

Find employees whose salary is greater than `55000`.

### Solution

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary > 55000;
```

---

## Exercise 2

### Question

Find employees whose salary is less than or equal to `50000`.

### Solution

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary <= 50000;
```

---

## Exercise 3

### Question

Find employees who work in the Sales department and earn more than `50000`.

### Solution

```sql
SELECT
    employee_name,
    department,
    salary
FROM employees
WHERE department = 'Sales'
AND salary > 50000;
```

---

## Exercise 4

### Question

Find employees who work in either IT or HR.

### Solution

```sql
SELECT
    employee_name,
    department
FROM employees
WHERE department = 'IT'
OR department = 'HR';
```

---

## Exercise 5

### Question

Find employees who are not from Mumbai.

### Solution

```sql
SELECT
    employee_name,
    city
FROM employees
WHERE city <> 'Mumbai';
```

---

## Exercise 6

### Question

Find employees from Mumbai, Pune, or Nashik.

### Solution

```sql
SELECT
    employee_name,
    city
FROM employees
WHERE city IN ('Mumbai', 'Pune', 'Nashik');
```

---

## Exercise 7

### Question

Find employees with salary between `45000` and `60000`.

### Solution

```sql
SELECT
    employee_name,
    salary
FROM employees
WHERE salary BETWEEN 45000 AND 60000;
```

---

## Exercise 8

### Question

Find employees whose names start with `R`.

### Solution

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE 'R%';
```

---

## Exercise 9

### Question

Find employees whose names contain the letter `a`.

### Solution

```sql
SELECT employee_name
FROM employees
WHERE employee_name LIKE '%a%';
```

---

## Exercise 10

### Question

Find employees whose employee ID is an even number.

### Solution

```sql
SELECT
    employee_id,
    employee_name
FROM employees
WHERE employee_id % 2 = 0;
```

---

## Exercise 11

### Question

Calculate annual salary for every employee.

### Solution

```sql
SELECT
    employee_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

---

## Exercise 12

### Question

Find active employees whose salary is between `50000` and `70000`.

### Solution

```sql
SELECT
    employee_name,
    salary,
    status
FROM employees
WHERE status = 'Active'
AND salary BETWEEN 50000 AND 70000;
```

---

## Exercise 13

### Question

Find employees who are from IT or Finance and earn more than `50000`.

### Solution

```sql
SELECT
    employee_name,
    department,
    salary
FROM employees
WHERE
    (department = 'IT' OR department = 'Finance')
    AND salary > 50000;
```

---

## Exercise 14

### Question

Find employees whose phone number is missing.

Use the `employee_contacts` table.

### Solution

```sql
SELECT *
FROM employee_contacts
WHERE phone IS NULL;
```

---

## Exercise 15

### Question

Find employees whose phone number is available.

### Solution

```sql
SELECT *
FROM employee_contacts
WHERE phone IS NOT NULL;
```

---

# 42. Interview Questions and Answers

## Q1. What is an SQL operator?

**Answer:**
An SQL operator is a symbol or keyword used to perform calculations, comparisons, logical operations, or special filtering in an SQL query.

---

## Q2. What are the main types of SQL operators?

**Answer:**

```text
1. Arithmetic
2. Comparison
3. Logical
4. Special
```

---

## Q3. What are arithmetic operators?

**Answer:**

```text
+
-
*
/
%
```

They are used to perform mathematical calculations.

---

## Q4. What are comparison operators?

**Answer:**

```text
=
>
<
>=
<=
<>
!=
```

They are used to compare values.

---

## Q5. What is the difference between `=` and `<>`?

**Answer:**

```text
=   → Equal to
<>  → Not equal to
```

Example:

```sql
WHERE department = 'IT';
```

returns IT employees.

```sql
WHERE department <> 'IT';
```

returns employees who are not in IT.

---

## Q6. What is the difference between AND and OR?

**Answer:**

`AND` requires **all conditions to be true**.

`OR` requires **at least one condition to be true**.

```text
AND → All
OR  → Any
```

---

## Q7. What does NOT do?

**Answer:**
`NOT` reverses the result of a condition.

Example:

```sql
WHERE NOT department = 'IT';
```

means employees whose department is not IT.

---

## Q8. What is the `%` operator?

**Answer:**
In an arithmetic expression, `%` returns the remainder after division.

```sql
SELECT 10 % 3;
```

Result:

```text
1
```

---

## Q9. What is the difference between `%` in arithmetic and LIKE?

**Answer:**

In arithmetic:

```sql
10 % 3
```

means remainder.

In `LIKE`:

```sql
employee_name LIKE 'A%'
```

means the name starts with `A` and can have zero or more characters after it.

---

## Q10. What is the IN operator?

**Answer:**
`IN` checks whether a value matches any value from a specified list.

```sql
WHERE city IN ('Mumbai', 'Delhi');
```

---

## Q11. What is BETWEEN?

**Answer:**
`BETWEEN` checks whether a value falls within a specified range. In MySQL, both boundary values are included.

```sql
WHERE salary BETWEEN 50000 AND 70000;
```

---

## Q12. What is LIKE?

**Answer:**
`LIKE` is used for pattern matching in text values.

The main wildcards are:

```text
% → Zero or more characters
_ → Exactly one character
```

---

## Q13. How do you check for NULL?

**Answer:**

Use:

```sql
IS NULL
```

or:

```sql
IS NOT NULL
```

Do not use:

```sql
= NULL
```

---

## Q14. What is the difference between IN and OR?

**Answer:**

These two queries are logically equivalent:

```sql
WHERE city = 'Mumbai'
OR city = 'Delhi';
```

and:

```sql
WHERE city IN ('Mumbai', 'Delhi');
```

`IN` is usually cleaner when checking several values.

---

## Q15. Is BETWEEN inclusive?

**Answer:**
Yes. `BETWEEN` includes both boundary values.

```sql
BETWEEN 50000 AND 70000
```

includes both:

```text
50000
70000
```

---

# 43. Important Differences

## AND vs OR

| AND                         | OR                                  |
| --------------------------- | ----------------------------------- |
| All conditions must be true | At least one condition must be true |
| More restrictive            | Less restrictive                    |

---

## IN vs OR

This:

```sql
WHERE city = 'Mumbai'
OR city = 'Delhi'
OR city = 'Pune';
```

can be written as:

```sql
WHERE city IN ('Mumbai', 'Delhi', 'Pune');
```

---

## BETWEEN vs AND

This:

```sql
WHERE salary BETWEEN 50000 AND 70000;
```

is equivalent to:

```sql
WHERE salary >= 50000
AND salary <= 70000;
```

---

## NULL Comparison

Wrong:

```sql
WHERE phone = NULL;
```

Correct:

```sql
WHERE phone IS NULL;
```

Wrong:

```sql
WHERE phone != NULL;
```

Correct:

```sql
WHERE phone IS NOT NULL;
```

---

# 44. Quick Revision

## Arithmetic

```text
+  → Addition
-  → Subtraction
*  → Multiplication
/  → Division
%  → Remainder
```

## Comparison

```text
=   → Equal
>   → Greater than
<   → Less than
>=  → Greater than or equal
<=  → Less than or equal
<>  → Not equal
!=  → Not equal
```

## Logical

```text
AND → All conditions
OR  → At least one condition
NOT → Reverse condition
```

## Special

```text
IN          → Match a list
NOT IN      → Exclude a list
BETWEEN     → Match a range
NOT BETWEEN → Exclude a range
LIKE        → Match a text pattern
IS NULL     → Check missing value
IS NOT NULL → Check available value
```

---

# 45. Final Summary

| Operator Type | Main Purpose                        | Examples                  |
| ------------- | ----------------------------------- | ------------------------- |
| Arithmetic    | Calculate values                    | `+ - * / %`               |
| Comparison    | Compare values                      | `= > < >= <= <>`          |
| Logical       | Combine conditions                  | `AND OR NOT`              |
| Special       | Advanced/basic filtering conditions | `IN BETWEEN LIKE IS NULL` |

### One-Line Memory Trick

> **Arithmetic = Calculate | Comparison = Compare | Logical = Combine | Special = Filter**

```text
Arithmetic
    ↓
Calculate

Comparison
    ↓
Compare

Logical
    ↓
Combine Conditions

Special
    ↓
Filter / Pattern / NULL
```

---

# 46. Most Important Queries to Remember

### Comparison

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

### AND

```sql
SELECT *
FROM employees
WHERE department = 'IT'
AND salary > 50000;
```

### OR

```sql
SELECT *
FROM employees
WHERE city = 'Mumbai'
OR city = 'Delhi';
```

### IN

```sql
SELECT *
FROM employees
WHERE city IN ('Mumbai', 'Delhi', 'Pune');
```

### BETWEEN

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 50000 AND 70000;
```

### LIKE

```sql
SELECT *
FROM employees
WHERE employee_name LIKE 'A%';
```

### NULL

```sql
SELECT *
FROM employee_contacts
WHERE phone IS NULL;
```

### Arithmetic

```sql
SELECT
    employee_name,
    salary * 12 AS annual_salary
FROM employees;
```

---

# 47. Chapter Summary

After completing this chapter, you should be able to:

* Use arithmetic operators for calculations.
* Compare values using comparison operators.
* Combine conditions using `AND`, `OR`, and `NOT`.
* Filter multiple values using `IN`.
* Filter ranges using `BETWEEN`.
* Search text patterns using `LIKE`.
* Check missing values using `IS NULL`.
* Check available values using `IS NOT NULL`.
* Use parentheses when combining complex conditions.
* Understand the difference between `AND`, `OR`, `IN`, and `BETWEEN`.
* Understand why `NULL` requires `IS NULL` instead of `= NULL`.

> **Core idea:** SQL operators are the building blocks used to **calculate, compare, combine, and filter data**.
