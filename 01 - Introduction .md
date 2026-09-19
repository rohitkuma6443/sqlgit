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

### Installing MySQL & MySQL Workbench

To get started, you need two things:

1. **MySQL Server:** The actual database engine running in the background.
2. **MySQL Workbench:** The graphical user interface (GUI) that lets you visually interact with your databases, write SQL scripts, and design tables without exclusively using the command line.

### Creating a Database

Once your server is running and you open MySQL Workbench, creating a database is as simple as running a single SQL command.

```sql
-- This creates a brand new database
CREATE DATABASE student_records;

-- This tells the system to start using it
USE student_records;

```
