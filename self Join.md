# **SQL Self Join – 8 Practice Questions with Solutions**

Using the same `employees` table:

| EmployeeID | EmployeeName | Department | Salary | ManagerID |
| ---------- | ------------ | ---------- | ------ | --------- |
| 101        | Rohit        | IT         | 30000  | 103       |
| 102        | Priya        | HR         | 35000  | 103       |
| 103        | Amit         | IT         | 60000  | 105       |
| 104        | Neha         | HR         | 32000  | 105       |
| 105        | Suresh       | Management | 90000  | NULL      |
| 106        | Karan        | IT         | 28000  | 101       |
| 107        | Pooja        | HR         | 31000  | 101       |
```
CREATE DATABASE self_join_db;

USE self_join_db;

CREATE TABLE employees (
    EmployeeID INT PRIMARY KEY,
    EmployeeName VARCHAR(50),
    Department VARCHAR(50),
    Salary INT,
    ManagerID INT
);

INSERT INTO employees VALUES
(101, 'Rohit', 'IT', 30000, 103),
(102, 'Priya', 'HR', 35000, 103),
(103, 'Amit', 'IT', 60000, 105),
(104, 'Neha', 'HR', 32000, 105),
(105, 'Suresh', 'Management', 90000, NULL),
(106, 'Karan', 'IT', 28000, 101),
(107, 'Pooja', 'HR', 31000, 101);

```

## **Q1. Display employee name and manager name.**

**Solution:**

```
SELECT
    A.EmployeeName AS Employee,
    B.EmployeeName AS Manager
FROM employees A
INNER JOIN employees B
ON A.ManagerID = B.EmployeeID;
```

**Output:**

| Employee | Manager |
| -------- | ------- |
| Rohit    | Amit    |
| Priya    | Amit    |
| Amit     | Suresh  |
| Neha     | Suresh  |
| Karan    | Rohit   |
| Pooja    | Rohit   |

Explanation:

- A = Employee table.
- B = Manager table.
- `A.ManagerID = B.EmployeeID` matches employees with their managers.

## **Q2. Display employees who have a manager.**

**Solution:**

```
SELECT
    A.EmployeeName,
    B.EmployeeName AS Manager
FROM employees A
INNER JOIN employees B
ON A.ManagerID = B.EmployeeID;
```

Explanation: INNER JOIN automatically excludes employees without a matching manager.

## **Q3. Display all employees, including those without managers.**

**Solution:**

```
SELECT
    A.EmployeeName AS Employee,
    B.EmployeeName AS Manager
FROM employees A
LEFT JOIN employees B
ON A.ManagerID = B.EmployeeID;
```

**Output:**

| Employee | Manager |
| -------- | ------- |
| Rohit    | Amit    |
| Priya    | Amit    |
| Amit     | Suresh  |
| Neha     | Suresh  |
| Suresh   | NULL    |
| Karan    | Rohit   |
| Pooja    | Rohit   |

Explanation: LEFT JOIN keeps all employees, even when their ManagerID is NULL.

## **Q4. Find employees working in the same department.**

**Solution:**

```
SELECT
    A.EmployeeName AS Employee1,
    B.EmployeeName AS Employee2,
    A.Department
FROM employees A
INNER JOIN employees B
ON A.Department = B.Department
AND A.EmployeeID < B.EmployeeID;
```

**Output:**

| Employee1 | Employee2 | Department |
| --------- | --------- | ---------- |
| Rohit     | Amit      | IT         |
| Rohit     | Karan     | IT         |
| Amit      | Karan     | IT         |
| Priya     | Neha      | HR         |
| Priya     | Pooja     | HR         |
| Neha      | Pooja     | HR         |

Explanation:

`A.EmployeeID < B.EmployeeID` prevents duplicate combinations.

## **Q5. Find employees whose salary is greater than their manager's salary.**

**Solution:**

```
SELECT
    A.EmployeeName AS Employee,
    A.Salary AS EmployeeSalary,
    B.EmployeeName AS Manager,
    B.Salary AS ManagerSalary
FROM employees A
INNER JOIN employees B
ON A.ManagerID = B.EmployeeID
WHERE A.Salary > B.Salary;
```

**Output:** Empty result for our current dataset.

Explanation: No employee earns more than their manager.

## **Q6. Find employees reporting to Amit.**

**Solution:**

```
SELECT
    A.EmployeeName AS Employee,
    B.EmployeeName AS Manager
FROM employees A
INNER JOIN employees B
ON A.ManagerID = B.EmployeeID
WHERE B.EmployeeName = 'Amit';
```

**Output:**

| Employee | Manager |
| -------- | ------- |
| Rohit    | Amit    |
| Priya    | Amit    |

Explanation: The WHERE condition filters records where the manager's name is Amit.

## **Q7. Display employees earning less than their managers.**

**Solution:**

```
SELECT
    A.EmployeeName AS Employee,
    A.Salary AS EmployeeSalary,
    B.EmployeeName AS Manager,
    B.Salary AS ManagerSalary
FROM employees A
INNER JOIN employees B
ON A.ManagerID = B.EmployeeID
WHERE A.Salary < B.Salary;
```

**Output:**

| Employee | EmployeeSalary | Manager | ManagerSalary |
| -------- | -------------- | ------- | ------------- |
| Rohit    | 30000          | Amit    | 60000         |
| Priya    | 35000          | Amit    | 60000         |
| Amit     | 60000          | Suresh  | 90000         |
| Neha     | 32000          | Suresh  | 90000         |
| Karan    | 28000          | Rohit   | 30000         |
| Pooja    | 31000          | Rohit   | 30000         |

## **Q8. Display employee, manager and manager's manager.**

This is an important advanced Self Join example.

**Solution:**

```
SELECT
    A.EmployeeName AS Employee,
    B.EmployeeName AS Manager,
    C.EmployeeName AS SeniorManager
FROM employees A
LEFT JOIN employees B
ON A.ManagerID = B.EmployeeID
LEFT JOIN employees C
ON B.ManagerID = C.EmployeeID;
```

**Output:**

| Employee | Manager | SeniorManager |
| -------- | ------- | ------------- |
| Rohit    | Amit    | Suresh        |
| Priya    | Amit    | Suresh        |
| Amit     | Suresh  | NULL          |
| Neha     | Suresh  | NULL          |
| Suresh   | NULL    | NULL          |
| Karan    | Rohit   | Amit          |
| Pooja    | Rohit   | Amit          |

Explanation:

- A = Employee.
- B = Employee's manager.
- C = Manager's manager.

We are using the same table three times with three different aliases.

## **Important interview questions**

| Question                            | Answer                                                |
| ----------------------------------- | ----------------------------------------------------- |
| What is Self Join?                  | Joining a table with itself.                          |
| Is Self Join a separate JOIN type?  | No, it is a technique.                                |
| Why use aliases?                    | To distinguish multiple references to the same table. |
| Can we use LEFT JOIN in Self Join?  | Yes.                                                  |
| Can we use INNER JOIN in Self Join? | Yes.                                                  |
| Can we join a table three times?    | Yes.                                                  |
| What is the most common use?        | Employee-manager hierarchy.                           |
