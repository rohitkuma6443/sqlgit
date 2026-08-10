# SQL Notes

# Introduction to SQL

## What is SQL
SQL (Structured Query Language) is a standard programming language used to manage and manipulate **[relational databases](ca://s?q=Relational_databases_explained)**.  
It allows users to perform operations such as **[querying data](ca://s?q=SQL_querying_data)**, **[updating records](ca://s?q=Update_records_in_SQL)**, and **[managing schemas](ca://s?q=Database_schema_in_SQL)**.  
SQL is essential for ensuring data consistency, integrity, and accessibility across applications.

---

## History
- Developed in the 1970s by IBM as part of their **[System R project](ca://s?q=IBM_System_R_SQL_history)**.  
- Standardized by ANSI in 1986 and ISO in 1987.  
- Over time, SQL has evolved with extensions like **[T-SQL](ca://s?q=Transact_SQL_basics)** (Microsoft SQL Server), **[PL/SQL](ca://s?q=PL_SQL_basics)** (Oracle), and **[pgSQL](ca://s?q=PostgreSQL_SQL_basics)** (PostgreSQL).  
- Today, SQL remains the backbone of most relational database systems.

---

## Use Cases
- **[Data Analysis](ca://s?q=SQL_for_data_analysis)** – Extracting insights from large datasets.  
- **[Reporting](ca://s?q=SQL_reporting_examples)** – Generating summaries, dashboards, and business intelligence reports.  
- **[Application Development](ca://s?q=SQL_in_application_development)** – Powering backend systems for e-commerce, finance, healthcare, and more.  
- **[Database Administration](ca://s?q=SQL_for_database_administration)** – Managing users, permissions, and performance tuning.  

---


---

# 1. SQL Comments

Comments are used to explain SQL code. They are not executed.

### Single-Line Comment

```sql
-- This is a comment
```

Example:

```sql
-- Display all students
SELECT * FROM Student;
```

### Multi-Line Comment

```sql
/*
This is a
multi-line comment
*/
```

### Uses

* Documentation
* Debugging
* Explaining code

---

# 2. Database

A **database** is a collection of related data stored in an organized way.

Example:

```text
SchoolDB
│
├── Student
├── Teachers
├── Classes
├── Subjects
└── Exams
```

---

## 2.1 Create Database

### Syntax

```sql
CREATE DATABASE database_name;
```

### Example

```sql
CREATE DATABASE SchoolDB;
```

---

## 2.2 Select Database

### Syntax

```sql
USE database_name;
```

### Example

```sql
USE SchoolDB;
```

---

## 2.3 Delete Database

### Syntax

```sql
DROP DATABASE database_name;
```

### Example

```sql
DROP DATABASE SchoolDB;
```

> `DROP DATABASE` deletes the complete database.

---

# 3. Data Types

A **data type** defines the type of data that can be stored in a column.

| Data Type    | Purpose              | Example        |
| ------------ | -------------------- | -------------- |
| `INT`        | Whole numbers        | `100`          |
| `VARCHAR(n)` | Variable-length text | `'Rahul'`      |
| `CHAR(n)`    | Fixed-length text    | `'A'`          |
| `DATE`       | Date                 | `'2026-08-09'` |
| `TEXT`       | Long text            | Description    |

### Example

```sql
CREATE TABLE Student(
    StudentID INT,
    Name VARCHAR(100),
    Section CHAR(1),
    DOB DATE
);
```

---

# 4. Constraints

A **constraint** is a rule applied to a column.

Common constraints:

* `PRIMARY KEY`
* `NOT NULL`
* `UNIQUE`
* `FOREIGN KEY`

---

## 4.1 PRIMARY KEY

A primary key uniquely identifies each record.

```sql
StudentID INT PRIMARY KEY
```

### Properties

* Unique
* Cannot contain `NULL`
* Identifies each record

Example:

```text
StudentID
---------
1
2
3
4
```

---

## 4.2 NOT NULL

`NOT NULL` means the column cannot contain `NULL`.

```sql
Name VARCHAR(100) NOT NULL
```

---

## 4.3 UNIQUE

`UNIQUE` prevents duplicate values.

```sql
Email VARCHAR(100) UNIQUE
```

---

## 4.4 FOREIGN KEY

A foreign key connects two tables.

```sql
FOREIGN KEY (TeacherID)
REFERENCES Teachers(TeacherID)
```

---

# 5. SQL Commands

SQL commands can be divided into different categories.

| Category | Full Form                    | Examples             |
| -------- | ---------------------------- | -------------------- |
| DDL      | Data Definition Language     | `CREATE`, `DROP`     |
| DML      | Data Manipulation Language   | `INSERT`             |
| DQL      | Data Query Language          | `SELECT`             |
| DCL      | Data Control Language        | `GRANT`, `REVOKE`    |
| TCL      | Transaction Control Language | `COMMIT`, `ROLLBACK` |

### Easy Remember

```text
DDL → Structure
DML → Data
DQL → Query
DCL → Permission
TCL → Transaction
```

---

# 6. SQL Clauses

A clause adds conditions or additional information to a SQL statement.

Example:

```sql
SELECT *
FROM Student
WHERE Age > 15;
```

Here:

```sql
WHERE
```

is a clause.

---

# 7. CREATE TABLE

A table stores data in rows and columns.

### Syntax

```sql
CREATE TABLE table_name(
    column1 datatype,
    column2 datatype,
    column3 datatype
);
```

---

## Example: Student Table

```sql
CREATE TABLE Student(
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    std VARCHAR(10) NOT NULL,
    address VARCHAR(200),
    age INT
);
```

### Structure

| Column    | Data Type    | Constraint  |
| --------- | ------------ | ----------- |
| `id`      | INT          | PRIMARY KEY |
| `name`    | VARCHAR(100) | NOT NULL    |
| `std`     | VARCHAR(10)  | NOT NULL    |
| `address` | VARCHAR(200) | —           |
| `age`     | INT          | —           |

---

# 8. DESC

`DESC` displays the structure of a table.

### Syntax

```sql
DESC table_name;
```

### Example

```sql
DESC Student;
```

---

# 9. INSERT

`INSERT` is used to add records to a table.

---

## 9.1 Insert One Record

### Syntax

```sql
INSERT INTO table_name
(column1, column2, column3)
VALUES
(value1, value2, value3);
```

### Example

```sql
INSERT INTO Student
(id, name, std, address, age)
VALUES
(1, 'Vikram', '1st', 'Dombivli', 5);
```

---

## 9.2 Insert Without Column Names

```sql
INSERT INTO Student
VALUES
(2, 'Pranay', '1st', 'Dombivli', 5);
```

> Values must follow the table's column order.

---

## 9.3 Insert Multiple Records

```sql
INSERT INTO Student
VALUES
(3, 'Yusuf', '1st', 'Dombivli', 5),
(4, 'Shoeb', '1st', 'Dombivli', 5),
(5, 'Kajal', '1st', 'Dombivli', 5);
```

---

# 10. SELECT

`SELECT` is used to retrieve data.

---

## 10.1 Select All Columns

```sql
SELECT *
FROM Student;
```

---

## 10.2 Select Specific Columns

```sql
SELECT name, age
FROM Student;
```

---

## 10.3 SELECT with WHERE

```sql
SELECT *
FROM Student
WHERE age > 5;
```

### Basic Pattern

```text
SELECT → What data?
FROM   → From which table?
WHERE  → Which condition?
```

---

# 11. TRUNCATE

`TRUNCATE` removes all records from a table.

```sql
TRUNCATE TABLE Student;
```

### Result

```text
Table remains
     +
All rows removed
```

---

# 12. DROP TABLE

`DROP TABLE` removes the complete table.

```sql
DROP TABLE Student;
```

### Result

```text
Table structure → Removed
Table data      → Removed
```

---

# 13. TRUNCATE vs DROP

| TRUNCATE           | DROP                   |
| ------------------ | ---------------------- |
| Removes all rows   | Removes complete table |
| Structure remains  | Structure removed      |
| Table still exists | Table no longer exists |

---

# 14. Foreign Key

A foreign key creates a relationship between two tables.

### Parent Table

```sql
CREATE TABLE Teachers(
    TeacherID INT PRIMARY KEY,
    Name VARCHAR(100),
    Subject VARCHAR(50),
    Experience INT,
    Email VARCHAR(100)
);
```

### Child Table

```sql
CREATE TABLE Classes(
    ClassID INT PRIMARY KEY,
    ClassName VARCHAR(10),
    Section CHAR(1),
    TotalStudents INT,
    TeacherID INT,

    FOREIGN KEY (TeacherID)
    REFERENCES Teachers(TeacherID)
);
```

### Relationship

```text
Teachers
   │
   │ TeacherID
   ↓
Classes
```

---

# 15. School Database

The source material uses a School Database containing these tables:

```text
1. Student
2. Teachers
3. Classes
4. Subjects
5. Exams
6. Attendance
7. Grades
8. Library
9. ExtracurricularActivities
10. ParentDetails
```

---

# 16. Teachers Table

```sql
CREATE TABLE Teachers (
    TeacherID INT PRIMARY KEY,
    Name VARCHAR(100),
    Subject VARCHAR(50),
    Experience INT,
    Email VARCHAR(100)
);
```

### Sample Data

```sql
INSERT INTO Teachers
(TeacherID, Name, Subject, Experience, Email)
VALUES
(1, 'Mr. Rajesh Kumar', 'Mathematics', 10,
 'rajesh.kumar@example.com'),

(2, 'Ms. Anita Desai', 'Science', 8,
 'anita.desai@example.com');
```

---

# 17. Classes Table

```sql
CREATE TABLE Classes (
    ClassID INT PRIMARY KEY,
    ClassName VARCHAR(10),
    Section CHAR(1),
    TotalStudents INT,
    TeacherID INT,

    FOREIGN KEY (TeacherID)
    REFERENCES Teachers(TeacherID)
);
```

### Relationship

```text
Teachers → Classes
```

---

# 18. Subjects Table

```sql
CREATE TABLE Subjects (
    SubjectID INT PRIMARY KEY,
    SubjectName VARCHAR(50),
    Credits INT,
    ClassID INT,

    FOREIGN KEY (ClassID)
    REFERENCES Classes(ClassID)
);
```

### Relationship

```text
Classes → Subjects
```

---

# 19. Exams Table

```sql
CREATE TABLE Exams (
    ExamID INT PRIMARY KEY,
    ExamName VARCHAR(50),
    Date DATE,
    TotalMarks INT,
    ClassID INT,

    FOREIGN KEY (ClassID)
    REFERENCES Classes(ClassID)
);
```

### Example

```sql
INSERT INTO Exams
VALUES
(1, 'Mid Term Exam', '2023-10-15', 100, 1);
```

---

# 20. Attendance Table

```sql
CREATE TABLE Attendance (
    AttendanceID INT PRIMARY KEY,
    StudentID INT,
    ClassID INT,
    Date DATE,
    Status ENUM('Present', 'Absent'),

    FOREIGN KEY (StudentID)
    REFERENCES Students(StudentID),

    FOREIGN KEY (ClassID)
    REFERENCES Classes(ClassID)
);
```

### Status

```text
Present
Absent
```

---

# 21. Grades Table

```sql
CREATE TABLE Grades (
    GradeID INT PRIMARY KEY,
    StudentID INT,
    SubjectID INT,
    Marks INT,

    FOREIGN KEY (StudentID)
    REFERENCES Students(StudentID),

    FOREIGN KEY (SubjectID)
    REFERENCES Subjects(SubjectID)
);
```

### Relationship

```text
Students ───→ Grades ←─── Subjects
```

---

# 22. Library Table

```sql
CREATE TABLE Library (
    BookID INT PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    ISBN VARCHAR(20),
    AvailableCopies INT
);
```

### Example

```sql
INSERT INTO Library
VALUES
(
    1,
    'The Alchemist',
    'Paulo Coelho',
    '978-0061122415',
    5
);
```

---

# 23. Extracurricular Activities

```sql
CREATE TABLE ExtracurricularActivities (
    ActivityID INT PRIMARY KEY,
    ActivityName VARCHAR(100),
    Description TEXT,
    ClassID INT,

    FOREIGN KEY (ClassID)
    REFERENCES Classes(ClassID)
);
```

Examples:

```text
Basketball
Debate Club
Science Club
Drama Club
Art Club
```

---

# 24. Parent Details

```sql
CREATE TABLE ParentDetails (
    ParentID INT PRIMARY KEY,
    StudentID INT,
    ParentName VARCHAR(100),
    Relationship VARCHAR(50),
    ContactNumber VARCHAR(15),

    FOREIGN KEY (StudentID)
    REFERENCES Students(StudentID)
);
```

---

# 25. School Database Relationships

```text
                     Teachers
                         │
                         ↓
                      Classes
                    /    │    \
                   ↓     ↓     ↓
              Subjects  Exams  Activities
                  │
                  ↓
                Grades
               ↑     ↑
              /       \
         Students    Subjects
             │
             ├──→ Attendance
             │
             └──→ ParentDetails
```

### Important Relationships

| Table         | Foreign Key | References |
| ------------- | ----------- | ---------- |
| Classes       | TeacherID   | Teachers   |
| Subjects      | ClassID     | Classes    |
| Exams         | ClassID     | Classes    |
| Attendance    | StudentID   | Students   |
| Attendance    | ClassID     | Classes    |
| Grades        | StudentID   | Students   |
| Grades        | SubjectID   | Subjects   |
| Activities    | ClassID     | Classes    |
| ParentDetails | StudentID   | Students   |

---

# 26. Exercises

## Exercise 1 — Database

Create a database named:

```text
CollegeDB
```

---

## Exercise 2 — Database Selection

Select the `CollegeDB` database.

---

## Exercise 3 — Student Table

Create a `Student` table with:

* StudentID
* Name
* Class
* Age

Make `StudentID` the primary key.

---

## Exercise 4 — Insert Data

Insert the following records:

| StudentID | Name  | Class | Age |
| --------: | ----- | ----- | --: |
|         1 | Rahul | 10th  |  15 |
|         2 | Priya | 10th  |  15 |
|         3 | Amit  | 9th   |  14 |
|         4 | Neha  | 10th  |  15 |
|         5 | Rohit | 9th   |  14 |

---

## Exercise 5 — Display Data

Display all records from the `Student` table.

---

## Exercise 6 — Select Columns

Display only:

* Name
* Age

---

## Exercise 7 — WHERE

Display students whose age is greater than `14`.

---

## Exercise 8 — Teacher Table

Create a `Teachers` table with:

* TeacherID
* Name
* Subject
* Experience

Make `TeacherID` the primary key.

---

## Exercise 9 — Insert Teachers

Insert:

| TeacherID | Name   | Subject     | Experience |
| --------: | ------ | ----------- | ---------: |
|         1 | Rajesh | Mathematics |         10 |
|         2 | Anita  | Science     |          8 |
|         3 | Sanjay | English     |         12 |

---

## Exercise 10 — Classes Table

Create a `Classes` table with:

* ClassID
* ClassName
* TeacherID

Make `TeacherID` a foreign key referencing `Teachers`.

---

## Exercise 11 — Insert Classes

Insert:

| ClassID | ClassName | TeacherID |
| ------: | --------- | --------: |
|       1 | 10th A    |         1 |
|       2 | 9th B     |         2 |
|       3 | 11th C    |         3 |

---

## Exercise 12 — TRUNCATE

Remove all records from the `Student` table without deleting the table.

---

## Exercise 13 — DROP

Delete the `Student` table completely.

---

# 27. Exercise Solutions

## Solution 1

```sql
CREATE DATABASE CollegeDB;
```

---

## Solution 2

```sql
USE CollegeDB;
```

---

## Solution 3

```sql
CREATE TABLE Student(
    StudentID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Class VARCHAR(20),
    Age INT
);
```

---

## Solution 4

```sql
INSERT INTO Student
VALUES
(1, 'Rahul', '10th', 15),
(2, 'Priya', '10th', 15),
(3, 'Amit', '9th', 14),
(4, 'Neha', '10th', 15),
(5, 'Rohit', '9th', 14);
```

---

## Solution 5

```sql
SELECT *
FROM Student;
```

---

## Solution 6

```sql
SELECT Name, Age
FROM Student;
```

---

## Solution 7

```sql
SELECT *
FROM Student
WHERE Age > 14;
```

---

## Solution 8

```sql
CREATE TABLE Teachers(
    TeacherID INT PRIMARY KEY,
    Name VARCHAR(100),
    Subject VARCHAR(50),
    Experience INT
);
```

---

## Solution 9

```sql
INSERT INTO Teachers
VALUES
(1, 'Rajesh', 'Mathematics', 10),
(2, 'Anita', 'Science', 8),
(3, 'Sanjay', 'English', 12);
```

---

## Solution 10

```sql
CREATE TABLE Classes(
    ClassID INT PRIMARY KEY,
    ClassName VARCHAR(20),
    TeacherID INT,

    FOREIGN KEY (TeacherID)
    REFERENCES Teachers(TeacherID)
);
```

---

## Solution 11

```sql
INSERT INTO Classes
VALUES
(1, '10th A', 1),
(2, '9th B', 2),
(3, '11th C', 3);
```

---

## Solution 12

```sql
TRUNCATE TABLE Student;
```

---

## Solution 13

```sql
DROP TABLE Student;
```

---

# Quick Revision

```text
DATABASE
   ↓
TABLE
   ↓
COLUMNS
   ↓
DATA
   ↓
PRIMARY KEY
   ↓
FOREIGN KEY
   ↓
RELATIONSHIP
```

### Most Important Commands

```sql
CREATE DATABASE
USE
DROP DATABASE

CREATE TABLE
DESC

INSERT INTO
SELECT

TRUNCATE TABLE
DROP TABLE
```

### Most Important Concepts

```text
Data Type
Constraint
Primary Key
Foreign Key
Parent Table
Child Table
Relationship
```

---

# Important Note from the Source

The uploaded file creates a table named `Student` with column `id`, but later uses `Students(StudentID)` in the `Attendance`, `Grades`, and `ParentDetails` definitions. This naming is inconsistent in the source.

For a working project, use one consistent design, for example:

```text
Students
└── StudentID
```

throughout the database.
