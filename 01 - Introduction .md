## 1. Core Concepts

* **What is Data?**
Raw facts, figures, or details (e.g., a student's name, age, or test score). On its own, data has no specific meaning until it is processed into information.
* **What is a Database?**
An organized collection of structured data, stored electronically. It allows data to be easily accessed, managed, updated, and protected.

### DBMS vs. RDBMS

A DBMS (Database Management System) is the software used to manage databases. An RDBMS (Relational Database Management System) is an advanced type of DBMS.

| Feature | DBMS | RDBMS |
| --- | --- | --- |
| **Data Storage** | Stored as files (hierarchical or navigational). | Stored in **tables** (rows and columns). |
| **Relationships** | Does not support relationships between files. | Supports relationships using keys. |
| **Security** | Lower security, generally single-user. | High security, supports multiple users. |
| **Examples** | XML, Windows Registry. | MySQL, Oracle, PostgreSQL. |

---

## 2. SQL vs. MySQL

People often confuse these two, but they serve completely different purposes.

| Aspect | SQL (Structured Query Language) | MySQL |
| --- | --- | --- |
| **What is it?** | A **language** used to communicate with databases. | A **software** (RDBMS) that stores and manages data. |
| **Function** | Used to write queries (e.g., `SELECT`, `INSERT`, `UPDATE`). | Executes those SQL queries to manage its own databases. |
| **Updates** | Standardized language; rarely changes. | Software program; gets frequent versions and updates. |

---

## 3. Structure of an RDBMS

Relational databases organize data into grids, much like a spreadsheet.

* **Table:** A collection of related data held in a structured format (also called a "Relation").
* **Column (Field):** A vertical structure holding a specific *type* of data (e.g., "Email" or "Age").
* **Row (Record):** A horizontal structure representing one complete, individual entry in the table (e.g., all the data for one specific student).

---

## 4. Understanding Keys

Keys are essential rules placed on columns to ensure data is unique and to link different tables together.

* **Primary Key (PK):** A column (or set of columns) that uniquely identifies *every* row in a table. It cannot be null (empty) and must be unique.
* *Example:* `StudentID` or `PassportNumber`.


* **Candidate Key:** Any column that *could* act as a Primary Key because it contains unique values, but wasn't chosen as the main one.
* *Example:* If `StudentID` is the Primary Key, `EmailAddress` is a Candidate Key.


* **Foreign Key (FK):** A column in one table that links to the Primary Key of another table. It creates a relationship between the two tables (as seen in the diagram above).
* *Example:* A `CourseID` column in a *Students* table that links to the `CourseID` Primary Key in the *Courses* table.


* **Composite Key:** A Primary Key made up of **two or more columns** combined to guarantee uniqueness, used when a single column isn't enough.
* *Example:* A library checkout table might use `MemberID` + `BookID` together as the key.



---

## 5. Setup & First Steps

#### MySQL Installation with Examples and Official Documentation ![](https://www.google.com/s2/favicons?domain=https://dev.mysql.com\&sz=32)

## 1. Download MySQL

Download the software from the official MySQL website.

MySQL Community Edition

MySQL Server and database tools

Download MySQL Installer Official Windows Installation Guide

For learning SQL on Windows, install these two components:

* MySQL Server: Stores databases and processes SQL commands.

* MySQL Workbench: Provides a graphical interface for writing and executing SQL.

The official Windows installation documentation describes the available installation packages and configuration process.


## 2. Installation steps

![](https://camo.githubusercontent.com/043fd482143ebb80bd8836029c81fbc31391ef166e882b04f773e528fef7f987/68747470733a2f2f696d616765732e6f70656e61692e636f6d2f7374617469632d7273632d342f715a48694b74344e6b475f2d4643334841457434415754514579367941535971635a5232484d737579436a326e3975495061717a35624971524a4d453853432d544f794756313547327334685f543939704d34692d4f646f5252693566506d386d5131656d3551496b674c4f3652456e383146423741484d33472d7839342d496756734d5a636743324e4c36745135465237636f7951736e74655750486b44424b66734b4b3232332d686b3f707572706f73653d696e6c696e65)

Step 1 — Select setup type

Choose `Custom` to install the server and development tools, including Workbench where available.


Step 2 — Install components

Click `Execute` to download and install the selected products.

![How to install MySQL 8.0.22 on Windows 10 | by Pooja N K | Medium](https://images.openai.com/static-rsc-4/qZHiKt4NkG_-FC3HAEt4AWTQEy6yASYqcZR2HMsuyCj2n9uIPaqz5bIqRJME8SC-TOyGV15G2s4h_T99pM4i-OdoRRi5fPm8mQ1em5QIkgLO6REn81FB7AHM3G-x94-IgVsMZcgC2NL6tQ5FR7coyQsnteWPHkDBKfsKK223-hk?purpose=inline)

Step 3 — Configure the server

Select a development configuration, retain port `3306` unless it is already occupied, and create a strong root password.

Step 4 — Connect

Open Workbench, select your local MySQL connection, and enter the root password.

The exact screens may differ by installer version. The official manual covers Windows configuration and troubleshooting.


## 3. First MySQL practical example

Let's create a student database and perform basic SQL operations.

### Step 1: Create a database

SQL

```
CREATE DATABASE school_db;
```

Explanation: `CREATE DATABASE` creates a new database named `school_db`.

### Step 2: Select the database

SQL

```
USE school_db;
```

This tells MySQL to use `school_db` for the following operations.

### Step 3: Create a table

SQL

```
CREATE TABLE Students (
    Student_ID INT PRIMARY KEY,
    Student_Name VARCHAR(50),
    Course VARCHAR(30),
    Marks INT
);
```
### Step 4: Insert records

SQL

```
INSERT INTO Students
    (Student_ID, Student_Name, Course, Marks)
VALUES
    (1, 'Amit', 'Python', 85),
    (2, 'Priya', 'SQL', 92),
    (3, 'Rahul', 'Excel', 76),
    (4, 'Neha', 'Python', 88),
    (5, 'Vikas', 'SQL', 65);
```

### Step 5: Display all records

SQL

```
SELECT * FROM Students;
```
### Step 6: Practise queries

SQL

```
-- Students scoring more than 80
SELECT *
FROM Students
WHERE Marks > 80;

-- Sort students by marks
SELECT *
FROM Students
ORDER BY Marks DESC;

-- Display only Python students
SELECT *
FROM Students
WHERE Course = 'Python';

-- Calculate average marks
SELECT AVG(Marks) AS Average_Marks
FROM Students;

-- Count students in each course
SELECT Course, COUNT(*) AS Total_Students
FROM Students
GROUP BY Course;
```
