# SQL — CTE (Common Table Expression)

MySQL | Beginner to Advanced | Notes, Dataset, Solved Examples, Practice Questions and Interview Questions

## 1. What is a CTE?

CTE stands for Common Table Expression.

A CTE is a temporary named result set that you define using the `WITH` keyword and use inside a single SQL statement.

In simple language, a CTE lets us write a query in small, readable steps instead of putting all the logic into one complicated query.

### Example

Suppose we want to find employees whose salaries are greater than ₹50,000.

Using a normal query:

```
SELECT employee_name, salary
FROM employees
WHERE salary > 50000;
```

Using a CTE:

```
WITH high_salary AS (
    SELECT employee_name, salary
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

Here:

- `WITH` creates the CTE.
- `high_salary` is the CTE name.
- The query inside parentheses creates the temporary result.
- The final `SELECT` reads from that result.

A CTE is not a permanent table. It is available only to the SQL statement in which it is defined.

## 2. CTE Syntax

```
WITH cte_name AS (
    SELECT column1, column2
    FROM table_name
    WHERE condition
)
SELECT *
FROM cte_name;
```

You can think of the CTE as a named step in your query.

## 3. Create the Dataset

We will use the `subquery_db` database and the `employees`, `products`, and `orders` tables from the previous chapter.

If you have already created the database and tables, you do not need to recreate them.

```
USE subquery_db;

SELECT * FROM employees;
SELECT * FROM products;
SELECT * FROM orders;
```

The employee dataset includes these columns:

| employee_id | employee_name | department | salary | city   |
| ----------- | ------------- | ---------- | ------ | ------ |
| 1           | Rohit         | IT         | 50000  | Mumbai |
| 2           | Amit          | Sales      | 35000  | Pune   |
| 3           | Priya         | IT         | 60000  | Mumbai |
| 4           | Neha          | HR         | 40000  | Delhi  |
| 5           | Rahul         | Sales      | 45000  | Pune   |
| 6           | Anjali        | HR         | 38000  | Mumbai |
| 7           | Vikas         | IT         | 70000  | Delhi  |
| 8           | Sneha         | Sales      | 55000  | Mumbai |
| 9           | Karan         | Finance    | 65000  | Pune   |
| 10          | Pooja         | Finance    | 48000  | Delhi  |

The `orders` table includes `order_id`, `employee_id`, `product_id`, `quantity`, `sales`, and `order_date`.

## 4. Basic CTE Examples

### Example 1: Find employees earning more than ₹50,000

```
WITH high_salary AS (
    SELECT employee_name, department, salary
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

Explanation:

1. The CTE selects employees earning more than ₹50,000.
2. The main query displays the CTE result.

Expected employees: Priya, Vikas, Sneha, and Karan.

### Example 2: Find employees from the IT department

```
WITH it_employees AS (
    SELECT employee_name, salary, city
    FROM employees
    WHERE department = 'IT'
)
SELECT *
FROM it_employees;
```

### Example 3: Find employees earning more than the average salary

```
WITH average_salary AS (
    SELECT AVG(salary) AS avg_salary
    FROM employees
)
SELECT
    e.employee_name,
    e.salary
FROM employees AS e
CROSS JOIN average_salary AS a
WHERE e.salary > a.avg_salary;
```

The CTE calculates the average once in the query structure, and the outer query compares each employee's salary against it.

### Example 4: Find products priced above ₹20,000

```
WITH expensive_products AS (
    SELECT product_name, category, price
    FROM products
    WHERE price > 20000
)
SELECT *
FROM expensive_products
ORDER BY price DESC;
```

## 5. CTE with Aggregate Functions

CTEs are particularly useful when a query needs to calculate totals first and filter those totals afterward.

### Example 5: Calculate total sales per employee

```
WITH employee_sales AS (
    SELECT
        employee_id,
        SUM(sales) AS total_sales
    FROM orders
    GROUP BY employee_id
)
SELECT *
FROM employee_sales
ORDER BY total_sales DESC;
```

The CTE groups orders by employee and calculates each employee's total sales.

### Example 6: Find employees whose total sales exceed ₹50,000

```
WITH employee_sales AS (
    SELECT
        employee_id,
        SUM(sales) AS total_sales
    FROM orders
    GROUP BY employee_id
)
SELECT
    e.employee_name,
    es.total_sales
FROM employee_sales AS es
JOIN employees AS e
    ON es.employee_id = e.employee_id
WHERE es.total_sales > 50000
ORDER BY es.total_sales DESC;
```

Why use a CTE here? It separates the calculation of total sales from the filtering and display logic.

### Example 7: Calculate the number of orders per employee

```
WITH order_count AS (
    SELECT
        employee_id,
        COUNT(*) AS total_orders
    FROM orders
    GROUP BY employee_id
)
SELECT
    e.employee_name,
    COALESCE(oc.total_orders, 0) AS total_orders
FROM employees AS e
LEFT JOIN order_count AS oc
    ON e.employee_id = oc.employee_id
ORDER BY total_orders DESC;
```

`LEFT JOIN` keeps employees who have no orders. `COALESCE()` displays zero instead of `NULL` for those employees.

## 6. CTE with `GROUP BY` and `HAVING`

### Example 8: Find departments with an average salary greater than ₹45,000

```
WITH department_salary AS (
    SELECT
        department,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_salary
WHERE avg_salary > 45000
ORDER BY avg_salary DESC;
```

### Example 9: Find products ordered more than once

```
WITH product_orders AS (
    SELECT
        product_id,
        COUNT(*) AS total_orders
    FROM orders
    GROUP BY product_id
)
SELECT
    p.product_name,
    po.total_orders
FROM product_orders AS po
JOIN products AS p
    ON po.product_id = p.product_id
WHERE po.total_orders > 1;
```

This counts order records, not the total quantity sold.

## 7. Using Multiple CTEs

You can define multiple CTEs in the same `WITH` clause, separating them with commas.

### Example 10: Calculate employee sales and compare them with average sales

```
WITH employee_sales AS (
    SELECT
        employee_id,
        SUM(sales) AS total_sales
    FROM orders
    GROUP BY employee_id
),
average_sales AS (
    SELECT AVG(total_sales) AS avg_sales
    FROM employee_sales
)
SELECT
    e.employee_name,
    es.total_sales
FROM employee_sales AS es
JOIN employees AS e
    ON es.employee_id = e.employee_id
CROSS JOIN average_sales AS av
WHERE es.total_sales > av.avg_sales
ORDER BY es.total_sales DESC;
```

How it works:

1. `employee_sales` calculates total sales for each employee.
2. `average_sales` calculates the average of those employee totals.
3. The final query displays employees whose totals exceed that average.

Notice that this is the average of employee totals, not the average of individual order values.

## 8. CTE vs Subquery

| Feature           | CTE                                                    | Subquery                                         |
| ----------------- | ------------------------------------------------------ | ------------------------------------------------ |
| Definition        | Uses `WITH`                                            | Written inside another query                     |
| Readability       | Often clearer for multi-step logic                     | Can be concise for simple logic                  |
| Reuse             | Can be referenced multiple times in the same statement | Often repeated if the same calculation is needed |
| Permanent storage | No                                                     | No                                               |
| Scope             | One SQL statement                                      | The statement containing it                      |

## 10. Solved Exercises

### Exercise 1: Find employees earning more than ₹40,000

```
WITH employee_data AS (
    SELECT employee_name, salary
    FROM employees
)
SELECT *
FROM employee_data
WHERE salary > 40000;
```

### Exercise 2: Find the highest salary in each department

```
WITH department_max AS (
    SELECT
        department,
        MAX(salary) AS highest_salary
    FROM employees
    GROUP BY department
)
SELECT
    e.employee_name,
    e.department,
    e.salary
FROM employees AS e
JOIN department_max AS dm
    ON e.department = dm.department
   AND e.salary = dm.highest_salary;
```

### Exercise 3: Find the top five orders by sales

```
WITH order_data AS (
    SELECT order_id, employee_id, sales
    FROM orders
)
SELECT *
FROM order_data
ORDER BY sales DESC
LIMIT 5;
```

### Exercise 4: Find products with total quantity sold greater than 5

```
WITH product_quantity AS (
    SELECT
        product_id,
        SUM(quantity) AS total_quantity
    FROM orders
    GROUP BY product_id
)
SELECT
    p.product_name,
    pq.total_quantity
FROM product_quantity AS pq
JOIN products AS p
    ON pq.product_id = p.product_id
WHERE pq.total_quantity > 5
ORDER BY pq.total_quantity DESC;
```

### Exercise 5: Find each employee's percentage contribution to total sales

```
WITH employee_sales AS (
    SELECT
        employee_id,
        SUM(sales) AS total_sales
    FROM orders
    GROUP BY employee_id
),
overall_sales AS (
    SELECT SUM(sales) AS grand_total
    FROM orders
)
SELECT
    e.employee_name,
    es.total_sales,
    ROUND(
        es.total_sales * 100.0 / os.grand_total,
        2
    ) AS sales_percentage
FROM employee_sales AS es
JOIN employees AS e
    ON es.employee_id = e.employee_id
CROSS JOIN overall_sales AS os
ORDER BY sales_percentage DESC;
```

This calculates each employee's share of all sales recorded in `orders`.

## 11. Practice Questions

Use the same `subquery_db` dataset. Try writing the SQL queries independently.

### Beginner Level

1. Create a CTE to display all employees from Mumbai.
2. Create a CTE to display employees earning more than ₹45,000.
3. Create a CTE to display all products in the Furniture category.
4. Create a CTE to display orders with sales greater than ₹25,000.
5. Create a CTE to display employees sorted by salary from highest to lowest.
6. Use a CTE to calculate the average salary.
7. Use a CTE to find the most expensive product.
8. Use a CTE to find the cheapest product.

### Intermediate Level

9. Calculate total sales per employee.
10. Find employees whose total sales exceed ₹50,000.
11. Calculate the number of orders per employee, including employees with zero orders.
12. Calculate total quantity sold for each product.
13. Find products whose total quantity sold exceeds 5.
14. Calculate average salary per department.
15. Find departments whose average salary exceeds ₹45,000.
16. Find the five highest-value orders.
17. Find the highest-paid employee in each department.
18. Calculate each employee's percentage of total sales.
19. Calculate total sales by product category.
20. Find products whose total sales value exceeds ₹30,000.

### Advanced Level

21. Use two CTEs to calculate employee sales and average employee sales, then find employees above the average.
22. Use a CTE to rank employees by salary using `RANK()`.
23. Find the top three employees by total sales using a CTE and a window function.
24. Use a recursive CTE to generate numbers from 1 to 20.
25. Use a recursive CTE to generate the first 10 multiples of 5.

## 12. SQL CTE Interview Questions and Answers

### Q1. What is a CTE?

A CTE is a named temporary result set defined using the `WITH` keyword and used within a single SQL statement.

### Q2. What is the full form of CTE?

Common Table Expression.

### Q3. What is the syntax of a CTE?

```
WITH cte_name AS (
    SELECT column_name
    FROM table_name
)
SELECT *
FROM cte_name;
```

### Q4. Is a CTE a permanent table?

No. A regular CTE exists only for the SQL statement in which it is defined.

### Q5. Can we create multiple CTEs in one query?

Yes. Define them in the same `WITH` clause and separate their definitions with commas.

### Q6. Can a CTE be used with JOIN?

Yes. You can join a CTE to a table or another CTE.

### Q7. What is a recursive CTE?

A recursive CTE refers to itself and is useful for sequences and hierarchical data.

### Q8. What is the difference between `WITH` and `WITH RECURSIVE` in MySQL?

`WITH` defines ordinary CTEs. `WITH RECURSIVE` permits a CTE to refer to itself recursively.

### Q9. Can a CTE be referenced more than once?

Yes, a CTE can be referenced multiple times within the same SQL statement.

### Q10. Is a CTE always faster than a subquery?

No. Performance depends on the query structure, database optimizer, indexes, and execution plan.

### Q11. What is the difference between a CTE and a view?

A CTE is defined for one statement. A view is a named database object whose definition is stored in the database for use across statements.

### Q12. What are the main advantages of CTEs?

- Improve readability.
- Break complex queries into steps.
- Help organize calculations.
- Support recursive queries.
- Allow a named result to be referenced more than once in the same statement.

## 13. Quick Revision

| Keyword          | Purpose                                            |
| ---------------- | -------------------------------------------------- |
| `WITH`           | Defines a CTE                                      |
| `AS`             | Introduces the CTE query                           |
| `WITH RECURSIVE` | Defines a recursive CTE                            |
| `UNION ALL`      | Combines rows from the initial and recursive parts |
| `JOIN`           | Combines the CTE result with another table or CTE  |
