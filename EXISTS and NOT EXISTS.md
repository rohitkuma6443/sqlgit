# Chapter 9: EXISTS and NOT EXISTS in SQL

In this chapter, we will learn `EXISTS` and `NOT EXISTS` in MySQL using your existing `subquery_db` database, with simple explanations, datasets, solved examples, exercises, practice questions, and interview questions.

## 1. What is EXISTS?

`EXISTS` checks whether a subquery returns at least one row.

- If the subquery returns one or more rows, `EXISTS` is `TRUE`.
- If the subquery returns no rows, `EXISTS` is `FALSE`.

Syntax:

```
SELECT columns
FROM table_name
WHERE EXISTS (
    SELECT 1
    FROM another_table
    WHERE condition
);
```

The `SELECT 1` is a common convention. With `EXISTS`, SQL checks whether a matching row exists; it does not need to return the row's actual column values.

## 2. What is NOT EXISTS?

`NOT EXISTS` checks whether the subquery returns no matching rows.

- If a matching row exists, `NOT EXISTS` is `FALSE`.
- If no matching row exists, `NOT EXISTS` is `TRUE`.

It is commonly used to find records without related records, such as employees who have never placed an order.

## 3. Dataset

We will use the tables already created in your database:

- `employees` — employee details
- `orders` — employee orders
- `products` — product details

Select the database first:

```
USE subquery_db;
```

Check the data:

```
SELECT * FROM employees;
SELECT * FROM orders;
SELECT * FROM products;
```

The relationship between the tables is:

employees

employee_id (Primary Key)

One employee can have multiple orders

orders

employee_id (Foreign Key)

## 4. Solved SQL examples

### Example 1: Find employees who have made at least one order

```
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.employee_id = e.employee_id
);
```

Explanation:

1. SQL takes an employee from `employees`.
2. The subquery searches `orders` for that employee's ID.
3. If a matching order exists, the employee is included in the result.

In your current dataset, all 10 employees have made at least one order, so all 10 will be returned.

### Example 2: Find employees who have never made an order

```
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.employee_id = e.employee_id
);
```

Explanation: The query returns employees for whom no matching order exists.

In your current dataset, the result is empty, because every employee has at least one order.

### Example 3: Find products that have never been sold

```
SELECT
    p.product_id,
    p.product_name
FROM products p
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.product_id = p.product_id
);
```

Expected result:

| product_id | product_name |
| ---------- | ------------ |
| 109        | Pen          |

Explanation: SQL checks whether each product's ID appears in the `orders` table. The Pen has no matching order.

### Example 4: Find products that have been sold at least once

```
SELECT
    p.product_id,
    p.product_name
FROM products p
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.product_id = p.product_id
);
```

This returns all products that have at least one matching order. The Pen is excluded.

### Example 5: Find employees who have sold an Electronics product

```
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p
        ON o.product_id = p.product_id
    WHERE o.employee_id = e.employee_id
      AND p.category = 'Electronics'
);
```

Explanation: The subquery checks whether each employee has at least one order for a product in the Electronics category.

An employee is returned even if they sold several Electronics products; `EXISTS` only needs one match.

### Example 6: Find employees who have never sold an Electronics product

```
SELECT
    e.employee_id,
    e.employee_name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p
        ON o.product_id = p.product_id
    WHERE o.employee_id = e.employee_id
      AND p.category = 'Electronics'
);
```

Explanation: This returns employees with no Electronics orders. An employee may still have sold Furniture or Stationery products.

### Example 7: Find employees who work in the IT department and have placed an order

```
SELECT
    e.employee_name,
    e.department
FROM employees e
WHERE e.department = 'IT'
  AND EXISTS (
      SELECT 1
      FROM orders o
      WHERE o.employee_id = e.employee_id
  );
```

This combines a normal filter (`department = 'IT'`) with an existence check.

Expected employees: Rohit, Priya, and Vikas.

### Example 8: Find employees who have placed an order for a Laptop

```
SELECT
    e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p
        ON o.product_id = p.product_id
    WHERE o.employee_id = e.employee_id
      AND p.product_name = 'Laptop'
);
```

The query returns employees who have at least one Laptop order.

### Example 9: Find employees whose orders include a product priced above ₹20,000

```
SELECT
    e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p
        ON o.product_id = p.product_id
    WHERE o.employee_id = e.employee_id
      AND p.price > 20000
);
```

Explanation: The condition is true if the employee has ordered at least one product priced above ₹20,000.

### Example 10: Find products that have never been ordered in February 2026

```
SELECT
    p.product_id,
    p.product_name
FROM products p
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.product_id = p.product_id
      AND o.order_date >= '2026-02-01'
      AND o.order_date <  '2026-03-01'
);
```

This finds products without an order dated in February 2026. A product may have been ordered in January and still appear in this result.

## 5. EXISTS vs NOT EXISTS vs IN

| Feature                              | `EXISTS`                                      | `NOT EXISTS`                                  | `IN`                                                   |
| ------------------------------------ | --------------------------------------------- | --------------------------------------------- | ------------------------------------------------------ |
| Purpose                              | Checks whether a match exists                 | Checks whether no match exists                | Checks whether a value is in a list or subquery result |
| Typical use                          | Employees with orders                         | Employees without orders                      | Employees in selected departments                      |
| Stops needing actual returned values | Yes                                           | Yes                                           | Compares returned values                               |
| NULL considerations                  | Generally avoids `NOT IN`-style NULL pitfalls | Generally avoids `NOT IN`-style NULL pitfalls | `NULL` in the subquery can complicate `NOT IN` results |

Example using `IN`:

```
SELECT employee_name
FROM employees
WHERE employee_id IN (
    SELECT employee_id
    FROM orders
);
```

This finds employees with orders.

Equivalent existence-based query:

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.employee_id = e.employee_id
);
```

Both produce the same employee set for this dataset. `EXISTS` is particularly useful when you want to test for related rows without needing to retrieve values from those rows.

## 6. Solved exercises

### Exercise 1: Find employees with at least one order in January 2026.

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.employee_id = e.employee_id
      AND o.order_date >= '2026-01-01'
      AND o.order_date <  '2026-02-01'
);
```

### Exercise 2: Find employees who have never ordered a Furniture product.

```
SELECT e.employee_name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p
        ON o.product_id = p.product_id
    WHERE o.employee_id = e.employee_id
      AND p.category = 'Furniture'
);
```

### Exercise 3: Find products that have been ordered by Rohit.

```
SELECT p.product_id, p.product_name
FROM products p
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN employees e
        ON o.employee_id = e.employee_id
    WHERE o.product_id = p.product_id
      AND e.employee_name = 'Rohit'
);
```

### Exercise 4: Find products that have never been ordered by Rohit.

```
SELECT p.product_id, p.product_name
FROM products p
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    JOIN employees e
        ON o.employee_id = e.employee_id
    WHERE o.product_id = p.product_id
      AND e.employee_name = 'Rohit'
);
```

### Exercise 5: Find employees who have placed at least one order with a sales value above ₹50,000.

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.employee_id = e.employee_id
      AND o.sales > 50000
);
```

Notice that this checks the value of an individual order, not the employee's total sales.

## 7. Practice questions — solve these yourself

Use the existing `employees`, `products`, and `orders` tables.

1. Find employees who have placed at least one order.
2. Find employees who have never placed an order.
3. Find products that have never been sold.
4. Find products that have been sold at least once.
5. Find employees who have ordered Electronics products.
6. Find employees who have never ordered Electronics products.
7. Find employees who have placed an order in February 2026.
8. Find products that have never been ordered in January 2026.
9. Find employees who have ordered a Laptop.
10. Find employees who have placed at least one order with a sales value greater than ₹30,000.
11. Find employees who have never ordered a Furniture product.
12. Find products ordered by Rohit.
13. Find products never ordered by Rohit.
14. Find employees who have ordered products priced below ₹1,000.
15. Find employees who have at least one order with a quantity of 5 or more.

Hide practice solutions

1\. Employees with orders

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.employee_id = e.employee_id
);
```

2\. Employees without orders

```
SELECT e.employee_name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.employee_id = e.employee_id
);
```

3\. Products never sold

```
SELECT p.product_name
FROM products p
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.product_id = p.product_id
);
```

4\. Products sold at least once

```
SELECT p.product_name
FROM products p
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.product_id = p.product_id
);
```

5\. Employees who ordered Electronics

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p ON p.product_id = o.product_id
    WHERE o.employee_id = e.employee_id
      AND p.category = 'Electronics'
);
```

6\. Employees who never ordered Electronics

```
SELECT e.employee_name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p ON p.product_id = o.product_id
    WHERE o.employee_id = e.employee_id
      AND p.category = 'Electronics'
);
```

7\. Employees with a February order

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.employee_id = e.employee_id
      AND o.order_date >= '2026-02-01'
      AND o.order_date < '2026-03-01'
);
```

8\. Products never ordered in January

```
SELECT p.product_name
FROM products p
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.product_id = p.product_id
      AND o.order_date >= '2026-01-01'
      AND o.order_date < '2026-02-01'
);
```

9\. Employees who ordered a Laptop

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p ON p.product_id = o.product_id
    WHERE o.employee_id = e.employee_id
      AND p.product_name = 'Laptop'
);
```

10\. Employees with an order above ₹30,000

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.employee_id = e.employee_id
      AND o.sales > 30000
);
```

11\. Employees who never ordered Furniture

```
SELECT e.employee_name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p ON p.product_id = o.product_id
    WHERE o.employee_id = e.employee_id
      AND p.category = 'Furniture'
);
```

12\. Products ordered by Rohit

```
SELECT p.product_name
FROM products p
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN employees e ON e.employee_id = o.employee_id
    WHERE o.product_id = p.product_id
      AND e.employee_name = 'Rohit'
);
```

13\. Products never ordered by Rohit

```
SELECT p.product_name
FROM products p
WHERE NOT EXISTS (
    SELECT 1
    FROM orders o
    JOIN employees e ON e.employee_id = o.employee_id
    WHERE o.product_id = p.product_id
      AND e.employee_name = 'Rohit'
);
```

14\. Employees who ordered products below ₹1,000

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM orders o
    JOIN products p ON p.product_id = o.product_id
    WHERE o.employee_id = e.employee_id
      AND p.price < 1000
);
```

15\. Employees with an order quantity of 5 or more

```
SELECT e.employee_name
FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.employee_id = e.employee_id
      AND o.quantity >= 5
);
```

## 8. SQL interview questions and answers

Q1. What is `EXISTS` in SQL?

It checks whether a subquery returns at least one row. It returns true if a matching row exists.

Q2. What is `NOT EXISTS`?

It returns true when the subquery returns no matching rows.

Q3. What does `SELECT 1` mean inside `EXISTS`?

It is a conventional way to write the subquery. The actual value `1` is not important; `EXISTS` checks whether rows are returned.

Q4. Can we use `EXISTS` with correlated subqueries?

Yes. A correlated subquery references a column from the outer query, such as `e.employee_id`.

Q5. What is the difference between `EXISTS` and `IN`?

`IN` checks whether a value matches a value in a list or subquery result. `EXISTS` checks whether the subquery returns any matching row.

Q6. What is the difference between `NOT EXISTS` and `NOT IN`?

`NOT EXISTS` checks for the absence of matching rows. `NOT IN` can behave unexpectedly if its subquery returns `NULL`, so `NOT EXISTS` is often safer for finding missing relationships.

Q7. Can `EXISTS` return duplicate rows?

`EXISTS` itself checks for a match, but the outer query can still return duplicate rows if its own source contains duplicates. With the `employees` table here, each employee ID is unique.

Q8. Can `EXISTS` be used with JOINs inside the subquery?

Yes. For example, you can join `orders` with `products` to check whether an employee ordered an Electronics product.

Q9. Does `EXISTS` always run faster than `IN`?

No. Performance depends on the database optimizer, indexes, data size, and query structure. Use `EXPLAIN` in MySQL to examine query execution plans.

Q10. Give a real-world use case for `NOT EXISTS`.

Finding customers who have never ordered, products that have never been sold, or students who have never submitted an assignment.

## 9. Chapter summary

| Concept                | Meaning                                                             |
| ---------------------- | ------------------------------------------------------------------- |
| `EXISTS`               | At least one matching row exists                                    |
| `NOT EXISTS`           | No matching row exists                                              |
| Correlated subquery    | Subquery refers to the outer query                                  |
| `SELECT 1`             | Common expression used inside an existence check                    |
| `NOT EXISTS` with JOIN | Checks the absence of a related record matching multiple conditions |
