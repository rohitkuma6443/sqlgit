# MySQL Installation with Examples and Official Documentation

## 1. Download MySQL

Download the software from the official MySQL website.

MySQL Community Edition

MySQL Server and database tools

Download MySQL Installer Official Windows Installation Guide

For learning SQL on Windows, install these two components:

* MySQL Server: Stores databases and processes SQL commands.

* MySQL Workbench: Provides a graphical interface for writing and executing SQL.

The official Windows installation documentation describes the available installation packages and configuration process.

![](https://www.google.com/s2/favicons?domain=https://dev.mysql.com\&sz=32)

MySQL

+1

## 2. Installation steps

![MySQL: da instalação até a configuração | Alura Cursos Online](https://images.openai.com/static-rsc-4/eXTea72rEmqW-P5QCt6cZx8Un_4tproXCtEfEd_LnPISQEdp2oBM9Wxzc9MpsIACHB8d26rBvG8bt5Wk_SHs67vjFQ-XKqiPRztBg2pZH1p9UgE9Uuz88BAk1EFE1YyRwQAxim8QJ07AJt4x0nsRDdn1B26HTIaGtkoeJh2-_Fw?purpose=inline)

Step 1 — Select setup type

Choose `Custom` to install the server and development tools, including Workbench where available.



Step 2 — Install components

Click `Execute` to download and install the selected products.

![How to install MySQL 8.0.22 on Windows 10 | by Pooja N K | Medium](https://images.openai.com/static-rsc-4/qZHiKt4NkG_-FC3HAEt4AWTQEy6yASYqcZR2HMsuyCj2n9uIPaqz5bIqRJME8SC-TOyGV15G2s4h_T99pM4i-OdoRRi5fPm8mQ1em5QIkgLO6REn81FB7AHM3G-x94-IgVsMZcgC2NL6tQ5FR7coyQsnteWPHkDBKfsKK223-hk?purpose=inline)

Step 3 — Configure the server

Select a development configuration, retain port `3306` unless it is already occupied, and create a strong root password.

![MySQL Connector: "Allow User Variables" - what does it do? | Community](https://images.openai.com/static-rsc-4/DwMtGMPwTUjxrbqazrrCT5IjTt_5eeOq5sT_rQsYYxqqiD5ZA1-xaHHEkykruzhq8o5NqmS43I2CBDjGhi0pHIyBhG9NJ1IPpMvfDy4aNpFyEWnIBNcPLeWo-piv22qE7fYrLQ6Vk3p89xRqyGBy6t4n07a2uBFp--ZRSRYCi3U?purpose=inline)

Step 4 — Connect

Open Workbench, select your local MySQL connection, and enter the root password.

The exact screens may differ by installer version. The official manual covers Windows configuration and troubleshooting.

![](https://www.google.com/s2/favicons?domain=https://dev.mysql.com\&sz=32)

MySQL

+1


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

|
Column

|

Meaning

|
| --- | --- |
|

Student_ID

|

Unique student ID

|
|

Student_Name

|

Student's name

|
|

Course

|

Course name

|
|

Marks

|

Marks obtained

|

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

Expected output:

|
Student_ID

|

Student_Name

|

Course

|

Marks

|
| --- | --- | --- | --- |
|

1

|

Amit

|

Python

|

85

|
|

2

|

Priya

|

SQL

|

92

|
|

3

|

Rahul

|

Excel

|

76

|
|

4

|

Neha

|

Python

|

88

|
|

5

|

Vikas

|

SQL

|

65

|

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

## 4. Official MySQL documentation

Use these resources to learn installation, SQL syntax, and database management.

MySQL 8.4 Reference Manual

Complete technical documentation for MySQL.

Read Reference Manual

MySQL Getting Started Tutorial

Connecting, creating databases, creating tables, and retrieving data.

Start the SQL Tutorial

Windows Installation Documentation

Installation packages, server configuration, and troubleshooting.

Read Installation Guide

MySQL Workbench Manual

Learn the graphical SQL editor and database administration tools.

Read Workbench Manual
