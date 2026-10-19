# SQL Chapter 7.1 — Subqueries

Beginner to Advanced | MySQL | Notes, Dataset, Solved Examples, Exercises and Interview Questions

## 1. What is a Subquery?

A subquery is a SQL query written inside another SQL query.

In simple language, a subquery helps us use the result of one query inside another query.

For example, suppose we want to find products whose prices are greater than the average product price.

SQL

```
SELECT product_name, price
FROM products
WHERE price > (
    SELECT AVG(price)
    FROM products
);
```

The inner query calculates the average price. The outer query displays products whose prices are greater than that average.

## 2. Types of Subqueries

|
Type

|

Description

|
| --- | --- |
|

Single-row subquery

|

Returns one row or value

|
|

Multiple-row subquery

|

Returns multiple rows

|
|

Scalar subquery

|

Returns exactly one value

|
|

Correlated subquery

|

Uses a value from the outer query

|
|

Subquery in `FROM`

|

Creates a temporary result that can be queried

|

## 3. Create the Database and Dataset

We will use a simple company sales database containing three tables:

* `employees` — employee details

* `products` — product details

* `orders` — sales and order details

Run the following SQL in MySQL Workbench or your MySQL editor.

### Step 1: Create the database

SQL

```
CREATE DATABASE IF NOT EXISTS subquery_db;

USE subquery_db;
```

### Step 2: Create the employees table

SQL

```
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    city VARCHAR(50)
);
```

### Step 3: Insert employee data

SQL

```
INSERT INTO employees VALUES
(1, 'Rohit', 'IT', 50000, 'Mumbai'),
(2, 'Amit', 'Sales', 35000, 'Pune'),
(3, 'Priya', 'IT', 60000, 'Mumbai'),
(4, 'Neha', 'HR', 40000, 'Delhi'),
(5, 'Rahul', 'Sales', 45000, 'Pune'),
(6, 'Anjali', 'HR', 38000, 'Mumbai'),
(7, 'Vikas', 'IT', 70000, 'Delhi'),
(8, 'Sneha', 'Sales', 55000, 'Mumbai'),
(9, 'Karan', 'Finance', 65000, 'Pune'),
(10, 'Pooja', 'Finance', 48000, 'Delhi');
```

### Step 4: Create the products table

SQL

```
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    category VARCHAR(50),
    price DECIMAL(10,2)
);
```

### Step 5: Insert product data

SQL

```
INSERT INTO products VALUES
(101, 'Laptop', 'Electronics', 75000),
(102, 'Mouse', 'Electronics', 1500),
(103, 'Keyboard', 'Electronics', 3000),
(104, 'Monitor', 'Electronics', 25000),
(105, 'Office Chair', 'Furniture', 12000),
(106, 'Desk', 'Furniture', 18000),
(107, 'Printer', 'Electronics', 22000),
(108, 'Notebook', 'Stationery', 300),
(109, 'Pen', 'Stationery', 100),
(110, 'Table', 'Furniture', 15000);
```

### Step 6: Create the orders table

SQL

```
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    employee_id INT,
    product_id INT,
    quantity INT,
    sales DECIMAL(10,2),
    order_date DATE,
    FOREIGN KEY (employee_id)
        REFERENCES employees(employee_id),
    FOREIGN KEY (product_id)
        REFERENCES products(product_id)
);
```

### Step 7: Insert order data

SQL

```
INSERT INTO orders VALUES
(1001, 1, 101, 1, 75000, '2026-01-05'),
(1002, 2, 102, 5, 7500, '2026-01-07'),
(1003, 3, 103, 3, 9000, '2026-01-10'),
(1004, 4, 104, 2, 50000, '2026-01-12'),
(1005, 5, 105, 2, 24000, '2026-01-15'),
(1006, 6, 106, 1, 18000, '2026-01-18'),
(1007, 7, 107, 2, 44000, '2026-01-20'),
(1008, 8, 108, 10, 3000, '2026-01-23'),
(1009, 1, 102, 3, 4500, '2026-01-25'),
(1010, 2, 101, 1, 75000, '2026-01-28'),
(1011, 3, 105, 1, 12000, '2026-02-02'),
(1012, 4, 106, 2, 36000, '2026-02-05'),
(1013, 5, 102, 4, 6000, '2026-02-08'),
(1014, 6, 104, 1, 25000, '2026-02-11'),
(1015, 7, 108, 5, 1500, '2026-02-15'),
(1016, 8, 110, 2, 30000, '2026-02-18'),
(1017, 9, 101, 1, 75000, '2026-02-20'),
(1018, 10, 107, 1, 22000, '2026-02-23'),
(1019, 1, 106, 2, 36000, '2026-02-25'),
(1020, 3, 104, 1, 25000, '2026-02-28');
```

Check the data:

SQL

```
SELECT * FROM employees;
SELECT * FROM products;
SELECT * FROM orders;
```

## 4. Single-Value Subqueries

A single-value subquery returns one value, such as an average, maximum, minimum, or salary.

### Example 1: Find employees earning more than the average salary

SQL

```
SELECT employee_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Explanation:

1. The inner query calculates the average salary.

2. The outer query finds employees earning more than the average.

### Example 2: Find the highest-paid employee

SQL

```
SELECT employee_name, department, salary
FROM employees
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

### Example 3: Find the lowest-paid employee

SQL

```
SELECT employee_name, department, salary
FROM employees
WHERE salary = (
    SELECT MIN(salary)
    FROM employees
);
```

### Example 4: Find products costing more than the average product price

SQL

```
SELECT product_name, price
FROM products
WHERE price > (
    SELECT AVG(price)
    FROM products
);
```

### Example 5: Find employees earning more than Rohit

SQL

```
SELECT employee_name, salary
FROM employees
WHERE salary > (
    SELECT salary
    FROM employees
    WHERE employee_name = 'Rohit'
);
```

## 5. Subqueries with `IN` and `NOT IN`

Use `IN` when the inner query returns multiple values.

### Example 6: Find employees who have made sales

SQL

```
SELECT employee_id, employee_name
FROM employees
WHERE employee_id IN (
    SELECT employee_id
    FROM orders
);
```

### Example 7: Find employees who have not made any sales

SQL

```
SELECT employee_id, employee_name
FROM employees
WHERE employee_id NOT IN (
    SELECT employee_id
    FROM orders
);
```

In this dataset, employees who have no matching order will be returned.

### Example 8: Find products that have been sold

SQL

```
SELECT product_id, product_name
FROM products
WHERE product_id IN (
    SELECT product_id
    FROM orders
);
```

### Example 9: Find products that have never been sold

SQL

```
SELECT product_id, product_name
FROM products
WHERE product_id NOT IN (
    SELECT product_id
    FROM orders
);
```

Important: `NOT IN` can behave unexpectedly if the subquery contains `NULL`. In such cases, `NOT EXISTS` is often safer.

## 6. Subqueries with `GROUP BY` and `HAVING`

Subqueries can use aggregate functions to filter groups.

### Example 10: Find employees whose total sales exceed ₹50,000

SQL

```
SELECT employee_id, employee_name
FROM employees
WHERE employee_id IN (
    SELECT employee_id
    FROM orders
    GROUP BY employee_id
    HAVING SUM(sales) > 50000
);
```

### Example 11: Find employees who have placed more than one order

SQL

```
SELECT employee_id, employee_name
FROM employees
WHERE employee_id IN (
    SELECT employee_id
    FROM orders
    GROUP BY employee_id
    HAVING COUNT(*) > 1
);
```

### Example 12: Find products sold in more than one order

SQL

```
SELECT product_id, product_name
FROM products
WHERE product_id IN (
    SELECT product_id
    FROM orders
    GROUP BY product_id
    HAVING COUNT(*) > 1
);
```

## 7. Subqueries in the `SELECT` Clause

A subquery can be used to calculate a value for display.

### Example 13: Display each employee with the average salary

SQL

```
SELECT
    employee_name,
    salary,
    (SELECT AVG(salary) FROM employees) AS average_salary
FROM employees;
```

### Example 14: Display each employee with their total sales

SQL

```
SELECT
    e.employee_name,
    (
        SELECT COALESCE(SUM(o.sales), 0)
        FROM orders AS o
        WHERE o.employee_id = e.employee_id
    ) AS total_sales
FROM employees AS e;
```

`COALESCE()` changes a `NULL` result to `0`. We will study it in a separate topic.

## 8. Correlated Subqueries

A correlated subquery uses a value from the outer query.

### Example 15: Find employees earning more than their department's average salary

SQL

```
SELECT
    e1.employee_name,
    e1.department,
    e1.salary
FROM employees AS e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees AS e2
    WHERE e2.department = e1.department
);
```

How it works:

1. The outer query selects an employee.

2. The inner query calculates the average salary of that employee's department.

3. The outer query checks whether the employee earns more than that average.

### Example 16: Find the highest-paid employee in each department

SQL

```
SELECT
    e1.employee_name,
    e1.department,
    e1.salary
FROM employees AS e1
WHERE e1.salary = (
    SELECT MAX(e2.salary)
    FROM employees AS e2
    WHERE e2.department = e1.department
);
```

This can return more than one employee if multiple employees share the highest salary in a department.

## 9. Subquery in the `FROM` Clause

A subquery in the `FROM` clause creates a temporary result that the outer query can use. It must have an alias in MySQL.

### Example 17: Find employees whose total sales exceed ₹50,000

SQL

```
SELECT
    employee_id,
    total_sales
FROM (
    SELECT
        employee_id,
        SUM(sales) AS total_sales
    FROM orders
    GROUP BY employee_id
) AS employee_sales
WHERE total_sales > 50000;
```

The inner query calculates total sales per employee. The outer query filters those totals.

## 10. Solved Exercises

### Exercise 1: Find employees earning more than ₹50,000

SQL

```
SELECT employee_name, salary
FROM employees
WHERE salary > 50000;
```

### Exercise 2: Find employees earning more than the average salary

SQL

```
SELECT employee_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### Exercise 3: Find employees working in the same department as Rohit

SQL

```
SELECT employee_name, department
FROM employees
WHERE department = (
    SELECT department
    FROM employees
    WHERE employee_name = 'Rohit'
);
```

### Exercise 4: Find products more expensive than the Laptop's price

SQL

```
SELECT product_name, price
FROM products
WHERE price > (
    SELECT price
    FROM products
    WHERE product_name = 'Laptop'
);
```

This returns no rows with the current dataset because the Laptop is the most expensive product.

### Exercise 5: Find employees who have made sales greater than ₹30,000 in at least one order

SQL

```
SELECT employee_id, employee_name
FROM employees
WHERE employee_id IN (
    SELECT employee_id
    FROM orders
    WHERE sales > 30000
);
```

### Exercise 6: Find employees whose total sales are greater than ₹70,000

SQL

```
SELECT employee_id, employee_name
FROM employees
WHERE employee_id IN (
    SELECT employee_id
    FROM orders
    GROUP BY employee_id
    HAVING SUM(sales) > 70000
);
```

### Exercise 7: Find products whose price is greater than the average price in their category

SQL

```
SELECT
    p1.product_name,
    p1.category,
    p1.price
FROM products AS p1
WHERE p1.price > (
    SELECT AVG(p2.price)
    FROM products AS p2
    WHERE p2.category = p1.category
);
```

### Exercise 8: Find employees with no orders

SQL

```
SELECT employee_name
FROM employees
WHERE employee_id NOT IN (
    SELECT employee_id
    FROM orders
);
```

## 11. Practice Questions

Try solving these questions independently. Use the dataset created above.

### Beginner Level

1. Find employees earning more than the average salary.

2. Find the employee with the highest salary.

3. Find the employee with the lowest salary.

4. Find employees earning more than Rohit.

5. Find employees working in the same department as Priya.

6. Find products priced above the average product price.

7. Find products cheaper than the Mouse.

8. Find employees who have made at least one sale.

9. Find employees who have not made any sales.

10. Find products that have never been sold.

### Intermediate Level

11. Find employees whose total sales exceed ₹50,000.

12. Find employees who have made more than one order.

13. Find products that appear in more than one order.

14. Find employees who have at least one order with sales above ₹30,000.

15. Find employees whose total sales are greater than the average order value.

16. Display each employee with their total sales.

17. Display each employee with their number of orders.

18. Find products priced above the average price of their category.

19. Find employees earning more than their department's average salary.

20. Find the highest-paid employee in each department.

### Advanced Level

21. Find the second-highest distinct salary.

22. Find the second-highest distinct product price.

23. Find employees whose total sales exceed Rohit's total sales.

24. Find products whose total quantity sold is greater than the average quantity per order.

25. Find employees who have never sold an Electronics product.

26. Find the department with the highest average salary.

27. Find employees whose salary is greater than the average salary of all employees except themselves.

28. Find products whose total sales value exceeds the average total sales value per product.

29. Find employees whose total sales are in the top 3 employee totals.

30. Find employees who have made an order on every month represented in the dataset.

## 12. SQL Subquery Interview Questions with Answers

### Q1. What is a subquery in SQL?

A subquery is a query written inside another SQL query. Its result is used by the outer query.

### Q2. What is the difference between a subquery and a JOIN?

A subquery places one query inside another. A JOIN combines rows from related tables. Both can solve some of the same problems, but the best choice depends on the requirement and query structure.

### Q3. What is a single-row subquery?

A single-row subquery returns one row. For example:

SQL

```
SELECT employee_name
FROM employees
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

### Q4. What is a multiple-row subquery?

A multiple-row subquery returns more than one row. It is commonly used with operators such as `IN`, `ANY`, and `ALL`.

### Q5. What is a scalar subquery?

A scalar subquery returns exactly one value: one row and one column. It can be used where a single value is expected.

### Q6. What is a correlated subquery?

A correlated subquery refers to a column from the outer query.

SQL

```
SELECT e1.employee_name, e1.salary
FROM employees AS e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees AS e2
    WHERE e2.department = e1.department
);
```

### Q7. What is the difference between `IN` and `EXISTS`?

* `IN` checks whether a value matches a value in a list or subquery result.

* `EXISTS` checks whether the subquery returns at least one row.

### Q8. Can a subquery be used in the `SELECT` clause?

Yes. A scalar subquery can be used in the `SELECT` clause to return a calculated value.

### Q9. Can a subquery be used in the `FROM` clause?

Yes. A subquery in `FROM` acts as a derived table and needs an alias in MySQL.

### Q10. What happens if a single-value comparison subquery returns multiple rows?

MySQL returns an error for a comparison such as `=`, `>`, or `<` when the subquery produces multiple rows. Use an appropriate operator such as `IN`, `ANY`, or `ALL`, depending on the requirement.

### Q11. What is the difference between `ANY` and `ALL`?

* `> ANY` means greater than at least one value returned by the subquery.

* `> ALL` means greater than every value returned by the subquery.

Example:

SQL

```
SELECT product_name, price
FROM products
WHERE price > ALL (
    SELECT price
    FROM products
    WHERE category = 'Stationery'
);
```

### Q12. Can subqueries be nested?

Yes. A query can contain a subquery that itself contains another subquery. However, deeply nested queries can become difficult to read and maintain.

### Q13. What is a derived table?

A derived table is a subquery written in the `FROM` clause. It creates a result set that the outer query can treat like a table.

### Q14. Is a subquery always slower than a JOIN?

No. Performance depends on the query, indexes, database engine, and execution plan. Compare alternatives using `EXPLAIN` and representative data.

### Q15. What is the difference between `NOT IN` and `NOT EXISTS`?

`NOT IN` checks that a value is not present in the returned set. If that set contains `NULL`, the result can become unknown and produce unexpected filtering. `NOT EXISTS` checks whether a matching row exists and is often preferable for anti-join logic.

## 13. Quick Revision

|
Concept

|

Purpose

|
| --- | --- |
|

Subquery

|

Query inside another query

|
|

Scalar subquery

|

Returns one value

|
|

`IN`

|

Matches any value in a result set

|
|

`NOT IN`

|

Excludes values in a result set; take care with `NULL`

|
|

Correlated subquery

|

Refers to the outer query

|
|

Derived table

|

Subquery in `FROM`

|
|

`ANY`

|

Comparison succeeds for at least one value

|
|

`ALL`

|

Comparison must succeed for every value

|

Next topic: SQL `CASE` — creating categories and conditional output using `WHEN`, `THEN`, `ELSE`, and `END`.
