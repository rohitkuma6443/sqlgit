# **SQL CTE (Common Table Expression) – Complete Chapter**

Database: MySQL 8.0+ | Level: Beginner to Advanced | Practical SQL Training

This chapter covers all 17 topics in your structure, using one common database: `cte_learning_db`.

# **Chapter 1: Introduction to CTE**

## **1.1 What is a CTE?**

CTE stands for **Common Table Expression**.

A CTE is a temporary named result set that is defined using the `WITH` keyword and used within a single SQL statement.

It helps us divide complex SQL queries into smaller, readable sections.

### **Example: Without CTE**

Find employees earning more than ₹50,000.

```
SELECT *
FROM employees
WHERE salary > 50000;
```

### **Example: With CTE**

```
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

Both queries produce the same result. The second query uses a CTE named `high_salary`.

## **1.2 Why do we use CTE?**

- To make complex queries easier to understand.
- To divide large queries into smaller parts.
- To reuse a named result within the same SQL statement.
- To simplify joins and aggregations.
- To support recursive queries.
- To improve query organization and maintainability.

## **1.3 General Syntax**

```
WITH cte_name AS (
    SELECT column1, column2
    FROM table_name
    WHERE condition
)
SELECT *
FROM cte_name;
```

**Important:** A CTE is not a permanent table. It is available only within the SQL statement in which it is defined.

# **Chapter 2: Dataset Creation**

We will use four tables:

1. Departments
2. Employees
3. Products
4. Sales

## **2.1 Database Creation**

```
CREATE DATABASE IF NOT EXISTS cte_learning_db;

USE cte_learning_db;
```

## **2.2 Departments Table**

```
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);

INSERT INTO departments VALUES
(1, 'IT'),
(2, 'HR'),
(3, 'Sales'),
(4, 'Finance'),
(5, 'Marketing');
```

| department_id | department_name |
| ------------- | --------------- |
| 1             | IT              |
| 2             | HR              |
| 3             | Sales           |
| 4             | Finance         |
| 5             | Marketing       |

## **2.3 Employees Table**

```
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT,
    salary DECIMAL(10,2),
    city VARCHAR(50),
    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);

INSERT INTO employees VALUES
(101, 'Rohit', 1, 55000, 'Mumbai'),
(102, 'Amit', 3, 35000, 'Pune'),
(103, 'Priya', 1, 65000, 'Mumbai'),
(104, 'Neha', 2, 42000, 'Delhi'),
(105, 'Rahul', 3, 48000, 'Pune'),
(106, 'Anjali', 2, 40000, 'Mumbai'),
(107, 'Vikas', 1, 75000, 'Delhi'),
(108, 'Sneha', 5, 52000, 'Mumbai'),
(109, 'Karan', 4, 70000, 'Pune'),
(110, 'Pooja', 4, 50000, 'Delhi'),
(111, 'Arjun', 3, 45000, 'Nashik'),
(112, 'Meera', 5, 58000, 'Pune'),
(113, 'Suresh', 1, 62000, 'Thane'),
(114, 'Kavita', 2, 44000, 'Mumbai'),
(115, 'Nikhil', 4, 68000, 'Delhi');
```

| employee_id | employee_name | department_id | salary | city   |
| ----------- | ------------- | ------------- | ------ | ------ |
| 101         | Rohit         | 1             | 55000  | Mumbai |
| 102         | Amit          | 3             | 35000  | Pune   |
| 103         | Priya         | 1             | 65000  | Mumbai |
| 104         | Neha          | 2             | 42000  | Delhi  |
| 105         | Rahul         | 3             | 48000  | Pune   |
| 106         | Anjali        | 2             | 40000  | Mumbai |
| 107         | Vikas         | 1             | 75000  | Delhi  |
| 108         | Sneha         | 5             | 52000  | Mumbai |
| 109         | Karan         | 4             | 70000  | Pune   |
| 110         | Pooja         | 4             | 50000  | Delhi  |
| 111         | Arjun         | 3             | 45000  | Nashik |
| 112         | Meera         | 5             | 58000  | Pune   |
| 113         | Suresh        | 1             | 62000  | Thane  |
| 114         | Kavita        | 2             | 44000  | Mumbai |
| 115         | Nikhil        | 4             | 68000  | Delhi  |

## **2.4 Products Table**

```
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    category VARCHAR(50),
    price DECIMAL(10,2)
);

INSERT INTO products VALUES
(201, 'Laptop', 'Electronics', 75000),
(202, 'Mouse', 'Electronics', 1500),
(203, 'Keyboard', 'Electronics', 3000),
(204, 'Monitor', 'Electronics', 25000),
(205, 'Office Chair', 'Furniture', 12000),
(206, 'Desk', 'Furniture', 18000),
(207, 'Printer', 'Electronics', 22000),
(208, 'Notebook', 'Stationery', 300),
(209, 'Pen', 'Stationery', 100),
(210, 'Table', 'Furniture', 15000),
(211, 'Headphones', 'Electronics', 5000),
(212, 'Bookshelf', 'Furniture', 9000);
```

| product_id | product_name | category    | price |
| ---------- | ------------ | ----------- | ----- |
| 201        | Laptop       | Electronics | 75000 |
| 202        | Mouse        | Electronics | 1500  |
| 203        | Keyboard     | Electronics | 3000  |
| 204        | Monitor      | Electronics | 25000 |
| 205        | Office Chair | Furniture   | 12000 |
| 206        | Desk         | Furniture   | 18000 |
| 207        | Printer      | Electronics | 22000 |
| 208        | Notebook     | Stationery  | 300   |
| 209        | Pen          | Stationery  | 100   |
| 210        | Table        | Furniture   | 15000 |
| 211        | Headphones   | Electronics | 5000  |
| 212        | Bookshelf    | Furniture   | 9000  |

## **2.5 Sales Table**

```
CREATE TABLE sales (
    sale_id INT PRIMARY KEY,
    employee_id INT,
    product_id INT,
    quantity INT,
    sales_amount DECIMAL(12,2),
    sale_date DATE,

    FOREIGN KEY (employee_id)
        REFERENCES employees(employee_id),

    FOREIGN KEY (product_id)
        REFERENCES products(product_id)
);

INSERT INTO sales VALUES
(1001, 101, 201, 1, 75000, '2026-01-05'),
(1002, 102, 202, 5, 7500, '2026-01-07'),
(1003, 103, 203, 3, 9000, '2026-01-10'),
(1004, 104, 204, 2, 50000, '2026-01-12'),
(1005, 105, 205, 2, 24000, '2026-01-15'),
(1006, 106, 206, 1, 18000, '2026-01-18'),
(1007, 107, 207, 2, 44000, '2026-01-20'),
(1008, 108, 208, 10, 3000, '2026-01-23'),
(1009, 101, 202, 3, 4500, '2026-01-25'),
(1010, 102, 201, 1, 75000, '2026-01-28'),
(1011, 103, 205, 1, 12000, '2026-02-02'),
(1012, 104, 206, 2, 36000, '2026-02-05'),
(1013, 105, 202, 4, 6000, '2026-02-08'),
(1014, 106, 204, 1, 25000, '2026-02-11'),
(1015, 107, 208, 5, 1500, '2026-02-15'),
(1016, 108, 210, 2, 30000, '2026-02-18'),
(1017, 109, 201, 1, 75000, '2026-02-20'),
(1018, 110, 207, 1, 22000, '2026-02-23'),
(1019, 101, 206, 2, 36000, '2026-02-25'),
(1020, 103, 204, 1, 25000, '2026-02-28'),
(1021, 111, 211, 3, 15000, '2026-03-02'),
(1022, 112, 212, 2, 18000, '2026-03-04'),
(1023, 113, 201, 2, 150000, '2026-03-06'),
(1024, 114, 203, 4, 12000, '2026-03-08'),
(1025, 115, 210, 3, 45000, '2026-03-10'),
(1026, 101, 211, 2, 10000, '2026-03-12'),
(1027, 102, 205, 1, 12000, '2026-03-15'),
(1028, 103, 207, 2, 44000, '2026-03-18'),
(1029, 104, 208, 20, 6000, '2026-03-20'),
(1030, 105, 209, 50, 5000, '2026-03-22'),
(1031, 106, 201, 1, 75000, '2026-04-02'),
(1032, 107, 204, 2, 50000, '2026-04-05'),
(1033, 108, 203, 3, 9000, '2026-04-08'),
(1034, 109, 206, 1, 18000, '2026-04-10'),
(1035, 110, 212, 2, 18000, '2026-04-12'),
(1036, 111, 202, 10, 15000, '2026-04-15'),
(1037, 112, 205, 2, 24000, '2026-04-18'),
(1038, 113, 207, 3, 66000, '2026-04-20'),
(1039, 114, 211, 4, 20000, '2026-04-22'),
(1040, 115, 208, 30, 9000, '2026-04-25');
```

## **2.6 Verify Dataset**

```
SELECT 'Departments' AS table_name, COUNT(*) AS total_records
FROM departments

UNION ALL

SELECT 'Employees', COUNT(*) FROM employees

UNION ALL

SELECT 'Products', COUNT(*) FROM products

UNION ALL

SELECT 'Sales', COUNT(*) FROM sales;
```

Expected output:

| table_name  | total_records |
| ----------- | ------------- |
| Departments | 5             |
| Employees   | 15            |
| Products    | 12            |
| Sales       | 40            |

# **Chapter 3: Basic CTE**

## **3.1 Simple CTE**

**Question:** Display all employees using a CTE.

```
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM employee_data;
```

**Explanation:**

1. `WITH` creates a CTE.
2. `employee_data` is its name.
3. The inner query retrieves employees.
4. The outer query displays the CTE result.

## **3.2 Select Specific Columns**

**Question:** Display employee name and salary.

```
WITH employee_data AS (
    SELECT employee_name, salary
    FROM employees
)
SELECT *
FROM employee_data;
```

Expected output:

| employee_name | salary |
| ------------- | ------ |
| Rohit         | 55000  |
| Amit          | 35000  |
| Priya         | 65000  |
| Neha          | 42000  |
| Rahul         | 48000  |
| Anjali        | 40000  |
| Vikas         | 75000  |
| Sneha         | 52000  |
| Karan         | 70000  |
| Pooja         | 50000  |
| Arjun         | 45000  |
| Meera         | 58000  |
| Suresh        | 62000  |
| Kavita        | 44000  |
| Nikhil        | 68000  |

## **3.3 Rename CTE Columns**

```
WITH employee_data (emp_name, emp_salary) AS (
    SELECT employee_name, salary
    FROM employees
)
SELECT *
FROM employee_data;
```

## **3.4 CTE with ORDER BY**

```
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM employee_data
ORDER BY salary DESC;
```

## **3.5 CTE with LIMIT**

```
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM employee_data
ORDER BY salary DESC
LIMIT 5;
```

Expected output:

| employee_name | salary |
| ------------- | ------ |
| Vikas         | 75000  |
| Karan         | 70000  |
| Nikhil        | 68000  |
| Priya         | 65000  |
| Suresh        | 62000  |

# **Chapter 4: CTE with WHERE**

The `WHERE` clause filters records based on a condition.

## **Example 1: Employees earning more than ₹50,000**

```
WITH high_salary AS (
    SELECT *
    FROM employees
)
SELECT *
FROM high_salary
WHERE salary > 50000;
```

Expected employees: Rohit, Priya, Vikas, Sneha, Karan, Meera, Suresh and Nikhil.

## **Example 2: Employees from Mumbai**

```
WITH mumbai_employees AS (
    SELECT *
    FROM employees
)
SELECT *
FROM mumbai_employees
WHERE city = 'Mumbai';
```

Expected output:

| employee_name | city   |
| ------------- | ------ |
| Rohit         | Mumbai |
| Priya         | Mumbai |
| Anjali        | Mumbai |
| Sneha         | Mumbai |
| Kavita        | Mumbai |

## **Example 3: Multiple conditions**

**Question:** Find employees from Mumbai earning more than ₹45,000.

```
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM employee_data
WHERE city = 'Mumbai'
AND salary > 45000;
```

Expected output:

| employee_name | salary | city   |
| ------------- | ------ | ------ |
| Rohit         | 55000  | Mumbai |
| Priya         | 65000  | Mumbai |
| Sneha         | 52000  | Mumbai |

## **Example 4: IN operator**

```
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM employee_data
WHERE city IN ('Mumbai', 'Pune');
```

## **Example 5: BETWEEN operator**

```
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM employee_data
WHERE salary BETWEEN 40000 AND 60000;
```

# **Chapter 5: CTE with GROUP BY**

`GROUP BY` groups records and performs aggregate calculations.

Common aggregate functions:

- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()

## **Example 1: Department-wise employee count**

```
WITH department_count AS (
    SELECT department_id,
           COUNT(*) AS total_employees
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_count;
```

Expected output:

| department_id | total_employees |
| ------------- | --------------- |
| 1             | 4               |
| 2             | 3               |
| 3             | 3               |
| 4             | 3               |
| 5             | 2               |

## **Example 2: Department-wise average salary**

```
WITH department_salary AS (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_salary;
```

Expected output:

| department_id | avg_salary |
| ------------- | ---------- |
| 1             | 64250      |
| 2             | 42000      |
| 3             | 42666.67   |
| 4             | 62666.67   |
| 5             | 55000      |

## **Example 3: Department-wise total salary**

```
WITH department_salary AS (
    SELECT department_id,
           SUM(salary) AS total_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_salary;
```

## **Example 4: Product category-wise average price**

```
WITH category_price AS (
    SELECT category,
           AVG(price) AS average_price
    FROM products
    GROUP BY category
)
SELECT *
FROM category_price;
```

Expected output:

| category    | average_price |
| ----------- | ------------- |
| Electronics | 21916.67      |
| Furniture   | 13500         |
| Stationery  | 200           |

## **Example 5: Employee-wise total sales**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
)
SELECT *
FROM employee_sales
ORDER BY total_sales DESC;
```

# **Chapter 6: CTE with HAVING**

`HAVING` filters grouped or aggregated results.

**Difference:**

- `WHERE` filters individual rows before grouping.
- `HAVING` filters groups after aggregation.

## **Example 1: Departments with more than two employees**

```
WITH department_count AS (
    SELECT department_id,
           COUNT(*) AS total_employees
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_count
WHERE total_employees > 2;
```

Here, the outer `WHERE` filters the already aggregated CTE result.

## **Example 2: Using HAVING inside CTE**

```
WITH department_count AS (
    SELECT department_id,
           COUNT(*) AS total_employees
    FROM employees
    GROUP BY department_id
    HAVING COUNT(*) > 2
)
SELECT *
FROM department_count;
```

## **Example 3: Departments with average salary above ₹50,000**

```
WITH department_salary AS (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary) > 50000
)
SELECT *
FROM department_salary;
```

Expected output:

| department_id | avg_salary |
| ------------- | ---------- |
| 1             | 64250      |
| 4             | 62666.67   |
| 5             | 55000      |

## **Example 4: Employees with total sales above ₹50,000**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
    HAVING SUM(sales_amount) > 50000
)
SELECT *
FROM employee_sales;
```

# **Chapter 7: CTE with JOIN**

A CTE can be joined with other tables just like a normal table.

## **Example 1: Employee and department names**

```
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT e.employee_name,
       d.department_name,
       e.salary
FROM employee_data e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

Expected output:

| employee_name | department_name | salary |
| ------------- | --------------- | ------ |
| Rohit         | IT              | 55000  |
| Amit          | Sales           | 35000  |
| Priya         | IT              | 65000  |
| Neha          | HR              | 42000  |
| Rahul         | Sales           | 48000  |
| Anjali        | HR              | 40000  |
| Vikas         | IT              | 75000  |
| Sneha         | Marketing       | 52000  |
| Karan         | Finance         | 70000  |
| Pooja         | Finance         | 50000  |
| Arjun         | Sales           | 45000  |
| Meera         | Marketing       | 58000  |
| Suresh        | IT              | 62000  |
| Kavita        | HR              | 44000  |
| Nikhil        | Finance         | 68000  |

## **Example 2: Employee sales report**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
)
SELECT e.employee_name,
       es.total_sales
FROM employee_sales es
INNER JOIN employees e
ON es.employee_id = e.employee_id
ORDER BY es.total_sales DESC;
```

## **Example 3: Product sales report**

```
WITH product_sales AS (
    SELECT product_id,
           SUM(sales_amount) AS total_sales,
           SUM(quantity) AS total_quantity
    FROM sales
    GROUP BY product_id
)
SELECT p.product_name,
       p.category,
       ps.total_quantity,
       ps.total_sales
FROM product_sales ps
INNER JOIN products p
ON ps.product_id = p.product_id;
```

## **Example 4: Complete sales report**

```
WITH sales_data AS (
    SELECT *
    FROM sales
)
SELECT s.sale_id,
       e.employee_name,
       p.product_name,
       s.quantity,
       s.sales_amount,
       s.sale_date
FROM sales_data s
INNER JOIN employees e
ON s.employee_id = e.employee_id
INNER JOIN products p
ON s.product_id = p.product_id;
```

# **Chapter 8: Multiple CTEs**

We can create multiple CTEs in one SQL statement.

Separate each CTE with a comma.

## **Syntax**

```
WITH cte1 AS (
    SELECT ...
),
cte2 AS (
    SELECT ...
),
cte3 AS (
    SELECT ...
)
SELECT *
FROM cte1;
```

## **Example 1: Multiple CTEs for employee analysis**

```
WITH employee_count AS (
    SELECT department_id,
           COUNT(*) AS total_employees
    FROM employees
    GROUP BY department_id
),

salary_analysis AS (
    SELECT department_id,
           AVG(salary) AS avg_salary,
           MAX(salary) AS highest_salary
    FROM employees
    GROUP BY department_id
)

SELECT ec.department_id,
       ec.total_employees,
       sa.avg_salary,
       sa.highest_salary
FROM employee_count ec
JOIN salary_analysis sa
ON ec.department_id = sa.department_id;
```

## **Example 2: Employee and sales analysis**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
),

employee_details AS (
    SELECT employee_id,
           employee_name,
           department_id
    FROM employees
)

SELECT ed.employee_name,
       ed.department_id,
       es.total_sales
FROM employee_details ed
JOIN employee_sales es
ON ed.employee_id = es.employee_id;
```

## **Example 3: Three CTEs**

```
WITH sales_summary AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
),

employee_details AS (
    SELECT employee_id,
           employee_name,
           department_id
    FROM employees
),

department_details AS (
    SELECT department_id,
           department_name
    FROM departments
)

SELECT ed.employee_name,
       dd.department_name,
       ss.total_sales
FROM sales_summary ss
JOIN employee_details ed
ON ss.employee_id = ed.employee_id
JOIN department_details dd
ON ed.department_id = dd.department_id;
```

# **Chapter 9: CTE with Subqueries**

CTEs and subqueries can be used together.

## **Example 1: Employees earning above average salary**

```
WITH employee_data AS (
    SELECT employee_name,
           salary
    FROM employees
)
SELECT *
FROM employee_data
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Average salary = ₹53,266.67.

Expected employees: Rohit, Priya, Vikas, Karan, Meera, Suresh and Nikhil.

## **Example 2: Products priced above average**

```
WITH product_data AS (
    SELECT *
    FROM products
)
SELECT *
FROM product_data
WHERE price > (
    SELECT AVG(price)
    FROM products
);
```

## **Example 3: Employees with the highest salary in their department**

```
WITH employee_data AS (
    SELECT employee_id,
           employee_name,
           department_id,
           salary
    FROM employees
)
SELECT *
FROM employee_data e
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
    WHERE department_id = e.department_id
);
```

Expected output:

| employee_name | department_id | salary |
| ------------- | ------------- | ------ |
| Vikas         | 1             | 75000  |
| Kavita        | 2             | 44000  |
| Rahul         | 3             | 48000  |
| Karan         | 4             | 70000  |
| Meera         | 5             | 58000  |

## **Example 4: Products with no sales**

```
WITH product_data AS (
    SELECT *
    FROM products
)
SELECT *
FROM product_data p
WHERE NOT EXISTS (
    SELECT 1
    FROM sales s
    WHERE s.product_id = p.product_id
);
```

Expected output:

| product_id | product_name |
| ---------- | ------------ |
| —          | No records   |

All 12 products have at least one sale in this dataset.

# **Chapter 10: CTE with CASE**

The `CASE` expression performs conditional calculations and classification.

## **Example 1: Salary classification**

```
WITH employee_data AS (
    SELECT employee_name,
           salary
    FROM employees
)
SELECT employee_name,
       salary,
       CASE
           WHEN salary >= 65000 THEN 'High Salary'
           WHEN salary >= 45000 THEN 'Medium Salary'
           ELSE 'Low Salary'
       END AS salary_category
FROM employee_data;
```

## **Example 2: Product price classification**

```
WITH product_data AS (
    SELECT *
    FROM products
)
SELECT product_name,
       price,
       CASE
           WHEN price >= 50000 THEN 'Expensive'
           WHEN price >= 10000 THEN 'Moderate'
           ELSE 'Affordable'
       END AS price_category
FROM product_data;
```

## **Example 3: Sales performance classification**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
)
SELECT e.employee_name,
       es.total_sales,
       CASE
           WHEN es.total_sales >= 100000 THEN 'Excellent'
           WHEN es.total_sales >= 50000 THEN 'Good'
           ELSE 'Needs Improvement'
       END AS performance
FROM employee_sales es
JOIN employees e
ON es.employee_id = e.employee_id;
```

## **Example 4: Department classification**

```
WITH department_data AS (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT department_id,
       avg_salary,
       CASE
           WHEN avg_salary >= 60000 THEN 'High Paying'
           WHEN avg_salary >= 45000 THEN 'Medium Paying'
           ELSE 'Low Paying'
       END AS department_category
FROM department_data;
```

# **Chapter 11: CTE with Window Functions**

Window functions perform calculations across related rows without combining them into a single grouped row.

Important functions:

| Function     | Purpose                               |
| ------------ | ------------------------------------- |
| ROW_NUMBER() | Assigns a unique sequential number    |
| RANK()       | Assigns rank, leaving gaps after ties |
| DENSE_RANK() | Assigns rank without gaps after ties  |

## **Example 1: ROW_NUMBER()**

Assign a serial number to employees based on salary.

```
WITH employee_ranking AS (
    SELECT employee_name,
           salary,
           ROW_NUMBER() OVER (
               ORDER BY salary DESC
           ) AS row_num
    FROM employees
)
SELECT *
FROM employee_ranking;
```

Expected first five rows:

| employee_name | salary | row_num |
| ------------- | ------ | ------- |
| Vikas         | 75000  | 1       |
| Karan         | 70000  | 2       |
| Nikhil        | 68000  | 3       |
| Priya         | 65000  | 4       |
| Suresh        | 62000  | 5       |

## **Example 2: RANK()**

```
WITH employee_ranking AS (
    SELECT employee_name,
           salary,
           RANK() OVER (
               ORDER BY salary DESC
           ) AS salary_rank
    FROM employees
)
SELECT *
FROM employee_ranking;
```

## **Example 3: DENSE_RANK()**

```
WITH employee_ranking AS (
    SELECT employee_name,
           salary,
           DENSE_RANK() OVER (
               ORDER BY salary DESC
           ) AS salary_rank
    FROM employees
)
SELECT *
FROM employee_ranking;
```

## **Example 4: Department-wise ranking**

```
WITH department_ranking AS (
    SELECT employee_name,
           department_id,
           salary,
           RANK() OVER (
               PARTITION BY department_id
               ORDER BY salary DESC
           ) AS dept_rank
    FROM employees
)
SELECT *
FROM department_ranking;
```

## **Example 5: Top 2 employees from each department**

```
WITH department_ranking AS (
    SELECT employee_name,
           department_id,
           salary,
           ROW_NUMBER() OVER (
               PARTITION BY department_id
               ORDER BY salary DESC
           ) AS rn
    FROM employees
)
SELECT *
FROM department_ranking
WHERE rn <= 2;
```

## **Example 6: Second-highest salary**

```
WITH salary_ranking AS (
    SELECT employee_name,
           salary,
           DENSE_RANK() OVER (
               ORDER BY salary DESC
           ) AS salary_rank
    FROM employees
)
SELECT *
FROM salary_ranking
WHERE salary_rank = 2;
```

Expected output:

| employee_name | salary | salary_rank |
| ------------- | ------ | ----------- |
| Karan         | 70000  | 2           |

# **Chapter 12: Recursive CTE**

A recursive CTE is a CTE that refers to itself.

It is useful for:

- Generating number sequences.
- Creating date sequences.
- Handling hierarchical employee structures.
- Building category hierarchies.
- Working with parent-child relationships.

## **12.1 Basic Syntax**

```
WITH RECURSIVE cte_name AS (

    -- Anchor member
    SELECT initial_value

    UNION ALL

    -- Recursive member
    SELECT next_value
    FROM cte_name
    WHERE condition
)

SELECT *
FROM cte_name;
```

A recursive CTE contains:

1. Anchor member: the starting result.
2. Recursive member: generates the next result.
3. Termination condition: stops recursion.

## **Example 1: Generate numbers from 1 to 10**

```
WITH RECURSIVE numbers AS (
    SELECT 1 AS num

    UNION ALL

    SELECT num + 1
    FROM numbers
    WHERE num < 10
)
SELECT *
FROM numbers;
```

Expected output:

| num |
| --- |
| 1   |
| 2   |
| 3   |
| 4   |
| 5   |
| 6   |
| 7   |
| 8   |
| 9   |
| 10  |

## **Example 2: Generate even numbers**

```
WITH RECURSIVE even_numbers AS (
    SELECT 2 AS num

    UNION ALL

    SELECT num + 2
    FROM even_numbers
    WHERE num < 20
)
SELECT *
FROM even_numbers;
```

## **Example 3: Generate multiplication table of 5**

```
WITH RECURSIVE multiplication AS (
    SELECT 1 AS num

    UNION ALL

    SELECT num + 1
    FROM multiplication
    WHERE num < 10
)
SELECT num,
       num * 5 AS result
FROM multiplication;
```

## **Example 4: Generate dates**

```
WITH RECURSIVE date_series AS (
    SELECT DATE('2026-01-01') AS sale_date

    UNION ALL

    SELECT DATE_ADD(sale_date, INTERVAL 1 DAY)
    FROM date_series
    WHERE sale_date < '2026-01-10'
)
SELECT *
FROM date_series;
```

## **Example 5: Employee hierarchy**

Suppose we create a separate employee reporting table:

```
CREATE TABLE employee_hierarchy (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    manager_id INT NULL
);

INSERT INTO employee_hierarchy VALUES
(1, 'CEO', NULL),
(2, 'IT Manager', 1),
(3, 'Sales Manager', 1),
(4, 'Developer', 2),
(5, 'Tester', 2),
(6, 'Sales Executive', 3);
```

Find the reporting hierarchy:

```
WITH RECURSIVE employee_tree AS (
    SELECT employee_id,
           employee_name,
           manager_id,
           1 AS level_no
    FROM employee_hierarchy
    WHERE manager_id IS NULL

    UNION ALL

    SELECT e.employee_id,
           e.employee_name,
           e.manager_id,
           et.level_no + 1
    FROM employee_hierarchy e
    INNER JOIN employee_tree et
    ON e.manager_id = et.employee_id
)
SELECT *
FROM employee_tree
ORDER BY level_no, employee_id;
```

Expected output:

| employee_id | employee_name   | manager_id | level_no |
| ----------- | --------------- | ---------- | -------- |
| 1           | CEO             | NULL       | 1        |
| 2           | IT Manager      | 1          | 2        |
| 3           | Sales Manager   | 1          | 2        |
| 4           | Developer       | 2          | 3        |
| 5           | Tester          | 2          | 3        |
| 6           | Sales Executive | 3          | 3        |

# **Chapter 13: Real-World Use Cases**

This section applies CTEs to common business reporting tasks.

## **Example 1: Total sales by employee**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
)
SELECT e.employee_name,
       es.total_sales
FROM employee_sales es
JOIN employees e
ON es.employee_id = e.employee_id
ORDER BY es.total_sales DESC;
```

## **Example 2: Monthly sales report**

```
WITH monthly_sales AS (
    SELECT YEAR(sale_date) AS sales_year,
           MONTH(sale_date) AS sales_month,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY YEAR(sale_date), MONTH(sale_date)
)
SELECT *
FROM monthly_sales
ORDER BY sales_year, sales_month;
```

Expected output:

| sales_year | sales_month | total_sales |
| ---------- | ----------- | ----------- |
| 2026       | 1           | 310000      |
| 2026       | 2           | 210500      |
| 2026       | 3           | 323000      |
| 2026       | 4           | 304000      |

## **Example 3: Department-wise sales**

```
WITH department_sales AS (
    SELECT e.department_id,
           SUM(s.sales_amount) AS total_sales
    FROM employees e
    JOIN sales s
    ON e.employee_id = s.employee_id
    GROUP BY e.department_id
)
SELECT d.department_name,
       ds.total_sales
FROM department_sales ds
JOIN departments d
ON ds.department_id = d.department_id
ORDER BY ds.total_sales DESC;
```

## **Example 4: Best-selling products**

```
WITH product_performance AS (
    SELECT product_id,
           SUM(quantity) AS total_quantity,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY product_id
)
SELECT p.product_name,
       pp.total_quantity,
       pp.total_sales
FROM product_performance pp
JOIN products p
ON pp.product_id = p.product_id
ORDER BY pp.total_sales DESC
LIMIT 5;
```

## **Example 5: Top-performing employee in each department**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
),

employee_ranking AS (
    SELECT e.employee_name,
           e.department_id,
           es.total_sales,
           RANK() OVER (
               PARTITION BY e.department_id
               ORDER BY es.total_sales DESC
           ) AS sales_rank
    FROM employee_sales es
    JOIN employees e
    ON es.employee_id = e.employee_id
)

SELECT *
FROM employee_ranking
WHERE sales_rank = 1;
```

## **Example 6: Sales contribution percentage**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
),

overall_sales AS (
    SELECT SUM(sales_amount) AS grand_total
    FROM sales
)

SELECT e.employee_name,
       es.total_sales,
       ROUND(
           es.total_sales / os.grand_total * 100, 2
       ) AS contribution_percentage
FROM employee_sales es
JOIN employees e
ON es.employee_id = e.employee_id
CROSS JOIN overall_sales os
ORDER BY contribution_percentage DESC;
```

## **Example 7: Products with sales above average product sales**

```
WITH product_sales AS (
    SELECT product_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY product_id
),

average_sales AS (
    SELECT AVG(total_sales) AS avg_product_sales
    FROM product_sales
)

SELECT p.product_name,
       ps.total_sales
FROM product_sales ps
JOIN products p
ON ps.product_id = p.product_id
CROSS JOIN average_sales av
WHERE ps.total_sales > av.avg_product_sales;
```

# **Chapter 14: CTE vs Subquery vs Temporary Table**

| Feature           | CTE                        | Subquery                       | Temporary Table                       |
| ----------------- | -------------------------- | ------------------------------ | ------------------------------------- |
| Syntax            | WITH                       | SELECT inside another query    | CREATE TEMPORARY TABLE                |
| Readability       | High                       | Can become complex             | High                                  |
| Reuse             | Within one statement       | Depends on query structure     | Across multiple statements in session |
| Storage           | Optimizer-dependent        | Optimizer-dependent            | Stored as a temporary table           |
| Recursive support | Yes                        | No direct recursive CTE syntax | No direct recursion                   |
| Lifetime          | One SQL statement          | Part of containing query       | Until dropped or session ends         |
| Index support     | No independent index       | No independent index           | Can create indexes                    |
| Best use          | Organizing complex queries | Small nested calculations      | Reusing intermediate results          |

## **14.1 CTE example**

```
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

## **14.2 Subquery example**

```
SELECT *
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

## **14.3 Temporary table example**

```
CREATE TEMPORARY TABLE high_salary AS
SELECT *
FROM employees
WHERE salary > 50000;

SELECT *
FROM high_salary;
```

Remove the temporary table:

```
DROP TEMPORARY TABLE high_salary;
```

**Which should you use?**

- Use a CTE to organize a complex query.
- Use a subquery for a small nested calculation.
- Use a temporary table when you need to reuse intermediate data across multiple statements.

Performance is not automatically better with a CTE. The MySQL optimizer determines how a CTE is executed, including whether it can be merged or needs materialization.

# **Chapter 15: Solved Exercises**

These exercises combine multiple concepts.

## **Exercise 1: Find employees earning above average salary.**

**Solution:**

```
WITH salary_analysis AS (
    SELECT AVG(salary) AS avg_salary
    FROM employees
)
SELECT e.employee_name,
       e.salary
FROM employees e
CROSS JOIN salary_analysis sa
WHERE e.salary > sa.avg_salary;
```

Expected average salary: ₹53,266.67.

## **Exercise 2: Find the highest-paid employee in each department.**

```
WITH employee_ranking AS (
    SELECT employee_name,
           department_id,
           salary,
           ROW_NUMBER() OVER (
               PARTITION BY department_id
               ORDER BY salary DESC
           ) AS rn
    FROM employees
)
SELECT *
FROM employee_ranking
WHERE rn = 1;
```

## **Exercise 3: Find employees whose total sales exceed ₹50,000.**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
)
SELECT e.employee_name,
       es.total_sales
FROM employee_sales es
JOIN employees e
ON es.employee_id = e.employee_id
WHERE es.total_sales > 50000
ORDER BY es.total_sales DESC;
```

## **Exercise 4: Find products with total quantity sold greater than 10.**

```
WITH product_quantity AS (
    SELECT product_id,
           SUM(quantity) AS total_quantity
    FROM sales
    GROUP BY product_id
)
SELECT p.product_name,
       pq.total_quantity
FROM product_quantity pq
JOIN products p
ON pq.product_id = p.product_id
WHERE pq.total_quantity > 10;
```

## **Exercise 5: Find departments whose average salary exceeds ₹50,000.**

```
WITH department_analysis AS (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT d.department_name,
       da.avg_salary
FROM department_analysis da
JOIN departments d
ON da.department_id = d.department_id
WHERE da.avg_salary > 50000;
```

## **Exercise 6: Find the second-highest sales employee.**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
),

sales_ranking AS (
    SELECT employee_id,
           total_sales,
           DENSE_RANK() OVER (
               ORDER BY total_sales DESC
           ) AS sales_rank
    FROM employee_sales
)

SELECT e.employee_name,
       sr.total_sales
FROM sales_ranking sr
JOIN employees e
ON sr.employee_id = e.employee_id
WHERE sr.sales_rank = 2;
```

## **Exercise 7: Find monthly sales and rank the months.**

```
WITH monthly_sales AS (
    SELECT MONTH(sale_date) AS sales_month,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY MONTH(sale_date)
),

monthly_ranking AS (
    SELECT sales_month,
           total_sales,
           RANK() OVER (
               ORDER BY total_sales DESC
           ) AS sales_rank
    FROM monthly_sales
)

SELECT *
FROM monthly_ranking;
```

## **Exercise 8: Find each department's sales contribution.**

```
WITH department_sales AS (
    SELECT e.department_id,
           SUM(s.sales_amount) AS total_sales
    FROM employees e
    JOIN sales s
    ON e.employee_id = s.employee_id
    GROUP BY e.department_id
),

grand_sales AS (
    SELECT SUM(sales_amount) AS total_sales
    FROM sales
)

SELECT d.department_name,
       ds.total_sales,
       ROUND(
           ds.total_sales / gs.total_sales * 100, 2
       ) AS contribution
FROM department_sales ds
JOIN departments d
ON ds.department_id = d.department_id
CROSS JOIN grand_sales gs;
```

## **Exercise 9: Find employees who have sales in January and February.**

```
WITH employee_months AS (
    SELECT employee_id,
           COUNT(DISTINCT MONTH(sale_date)) AS total_months
    FROM sales
    WHERE MONTH(sale_date) IN (1, 2)
    GROUP BY employee_id
)
SELECT e.employee_name
FROM employee_months em
JOIN employees e
ON em.employee_id = e.employee_id
WHERE em.total_months = 2;
```

## **Exercise 10: Classify employees based on sales performance.**

```
WITH employee_sales AS (
    SELECT employee_id,
           SUM(sales_amount) AS total_sales
    FROM sales
    GROUP BY employee_id
)
SELECT e.employee_name,
       es.total_sales,
       CASE
           WHEN es.total_sales >= 100000 THEN 'Excellent'
           WHEN es.total_sales >= 50000 THEN 'Good'
           WHEN es.total_sales >= 30000 THEN 'Average'
           ELSE 'Low'
       END AS performance
FROM employee_sales es
JOIN employees e
ON es.employee_id = e.employee_id;
```

# **Chapter 16: Practice Questions**

Try solving these questions independently using the `cte_learning_db` dataset.

## **Basic Level**

| No. | Question                                               |
| --- | ------------------------------------------------------ |
| 1   | Display all employees using a CTE.                     |
| 2   | Display employees earning more than ₹60,000.           |
| 3   | Display employees from Pune.                           |
| 4   | Display products costing less than ₹5,000.             |
| 5   | Display sales records from January 2026.               |
| 6   | Display employees earning between ₹40,000 and ₹60,000. |
| 7   | Find the total number of employees in each department. |
| 8   | Find the average product price by category.            |
| 9   | Find total sales by employee.                          |
| 10  | Find total quantity sold for each product.             |

## **Intermediate Level**

| No. | Question                                                                           |
| --- | ---------------------------------------------------------------------------------- |
| 11  | Find departments with average salary above ₹50,000.                                |
| 12  | Find employees whose total sales exceed ₹75,000.                                   |
| 13  | Find the top five products by total sales.                                         |
| 14  | Display employee names with their department names using CTE and JOIN.             |
| 15  | Find products with total quantity sold above 10.                                   |
| 16  | Find the highest-paid employee in each department.                                 |
| 17  | Find the second-highest salary using DENSE_RANK().                                 |
| 18  | Find employees who made sales in March 2026.                                       |
| 19  | Calculate monthly sales and monthly sales growth compared with the previous month. |
| 20  | Classify products as Expensive, Moderate or Affordable using CASE.                 |

## **Advanced Level**

| No. | Question                                                                                            |
| --- | --------------------------------------------------------------------------------------------------- |
| 21  | Find the top two sales employees in each department.                                                |
| 22  | Find the percentage contribution of each employee to total sales.                                   |
| 23  | Find departments whose total sales exceed ₹2,00,000.                                                |
| 24  | Find the product with the highest sales in each category.                                           |
| 25  | Find employees whose sales are greater than their department's average employee sales.              |
| 26  | Generate numbers from 1 to 100 using recursive CTE.                                                 |
| 27  | Generate the dates between 1 January and 31 January 2026.                                           |
| 28  | Find employees who made sales in all four months represented in the dataset.                        |
| 29  | Find the running total of sales by sale date.                                                       |
| 30  | Create a complete employee performance report using multiple CTEs, JOIN, CASE and window functions. |

# **Chapter 17: SQL CTE Interview Questions and Answers**

**Q1. What is a CTE?**

A CTE is a temporary named result set created using the `WITH` keyword. It can be used within a single SQL statement.

**Q2. What is the full form of CTE?**

Common Table Expression.

**Q3. Which keyword is used to create a CTE?**

`WITH`.

**Q4. Can we create multiple CTEs in one query?**

Yes. Separate CTE definitions using commas.

**Q5. Can we use JOIN with a CTE?**

Yes. A CTE can be joined with normal tables and other CTEs.

**Q6. Can we use GROUP BY inside a CTE?**

Yes. We can use `GROUP BY` and aggregate functions inside a CTE.

**Q7. What is a recursive CTE?**

A recursive CTE references itself and is used for sequences and hierarchical data.

**Q8. What is the difference between WHERE and HAVING in a CTE query?**

`WHERE` filters individual records, while `HAVING` filters grouped results.

**Q9. Can we use window functions inside a CTE?**

Yes. Functions such as `ROW_NUMBER()`, `RANK()` and `DENSE_RANK()` can be used inside a CTE.

**Q10. Is a CTE a permanent table?**

No. A normal CTE exists only for the duration of its SQL statement.

**Q11. Can we use a CTE inside another CTE?**

Yes. A later CTE can reference an earlier CTE defined in the same `WITH` clause.

**Q12. Can we use INSERT, UPDATE and DELETE with CTEs?**

MySQL supports CTEs with statements such as `SELECT`, `INSERT`, `UPDATE` and `DELETE`, subject to the applicable statement syntax and restrictions.

**Q13. What is the difference between CTE and temporary table?**

A CTE is scoped to one SQL statement. A temporary table can be reused across multiple statements during its session.

**Q14. What is the difference between RANK and DENSE_RANK?**

`RANK()` leaves gaps after tied ranks, whereas `DENSE_RANK()` does not.

**Q15. Does using a CTE always improve performance?**

No. A CTE primarily improves query readability and organization. Actual performance depends on the query and the database optimizer.

# **Quick Revision Sheet**

| Topic               | Important Syntax / Function         |
| ------------------- | ----------------------------------- |
| Basic CTE           | `WITH cte_name AS (...)`            |
| Filtering           | `WHERE`                             |
| Grouping            | `GROUP BY`                          |
| Aggregate filtering | `HAVING`                            |
| Joining             | `JOIN`                              |
| Multiple CTEs       | `WITH cte1 AS (...), cte2 AS (...)` |
| Conditional logic   | `CASE WHEN`                         |
| Subqueries          | Nested `SELECT`                     |
| Row numbering       | `ROW_NUMBER()`                      |
| Ranking             | `RANK()`                            |
| Dense ranking       | `DENSE_RANK()`                      |
| Recursive queries   | `WITH RECURSIVE`                    |
| Temporary storage   | `CREATE TEMPORARY TABLE`            |


   CTE concepts, differences, performance and interview questions

**Teaching note:** Use MySQL 8.0 or newer for this chapter. The dataset has 72 initial records across four tables; the optional `employee_hierarchy` table is created separately for the recursive hierarchy example.
