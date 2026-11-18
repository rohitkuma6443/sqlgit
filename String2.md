# **SQL String Functions 


## **1. Create Database and Table**

```
CREATE DATABASE string_practice;

USE string_practice;

CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    middle_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    department VARCHAR(50),
    city VARCHAR(50),
    phone VARCHAR(20),
    emp_code VARCHAR(20),
    job_title VARCHAR(50)
);
```

## **2. Insert Sample Dataset**

```
INSERT INTO employees VALUES
(101,'Rohit','Kumar','Jha','ROHIT.JHA@GMAIL.COM','IT','Mumbai','9876543210','EMP-001','Data Analyst'),
(102,'Priya','Raj','Sharma','priya.sharma@yahoo.com','HR','Delhi','9876543211','EMP-002','HR Manager'),
(103,'Amit',NULL,'Verma','AMIT.VERMA@GMAIL.COM','Finance','Pune','9876543212','EMP-003','Accountant'),
(104,'Neha','Sunil','Patel','neha.patel@gmail.com','IT','Ahmedabad','9876543213','EMP-004','Software Developer'),
(105,'Rahul',NULL,'Kumar','rahul.kumar@yahoo.com','Sales','Mumbai','9876543214','EMP-005','Sales Executive'),
(106,'Sneha','Rajesh','Mehta','SNEHA.MEHTA@GMAIL.COM','Marketing','Nashik','9876543215','EMP-006','Digital Marketer'),
(107,'Vikas','Mohan','Singh','vikas.singh@gmail.com','IT','Delhi','9876543216','EMP-007','Python Developer'),
(108,'Anjali',NULL,'Gupta','anjali.gupta@yahoo.com','HR','Pune','9876543217','EMP-008','HR Executive'),
(109,'Karan','Deep','Malhotra','karan.m@gmail.com','Finance','Mumbai','9876543218','EMP-009','Senior Accountant'),
(110,'Pooja','Amit','Desai','pooja.desai@gmail.com','Sales','Surat','9876543219','EMP-010','Sales Manager');
```

Check your dataset:

```
SELECT * FROM employees;
```

## **3. Basic String Functions**

### **1. UPPER() – Convert to Uppercase**

```
SELECT first_name, UPPER(first_name) AS uppercase_name
FROM employees;
```

### **2. LOWER() – Convert to Lowercase**

```
SELECT email, LOWER(email) AS lowercase_email
FROM employees;
```

### **3. LENGTH() – Count Bytes**

```
SELECT first_name, LENGTH(first_name) AS name_length
FROM employees;
```

### **4. CHAR_LENGTH() – Count Characters**

```
SELECT first_name, CHAR_LENGTH(first_name) AS total_characters
FROM employees;
```

### **5. CONCAT() – Combine Strings**

```
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```

### **6. CONCAT_WS() – Combine Using Separator**

```
SELECT CONCAT_WS(' ', first_name, middle_name, last_name) AS full_name
FROM employees;
```

`CONCAT_WS()` skips NULL values after the separator.

### **7. REVERSE() – Reverse Text**

```
SELECT first_name, REVERSE(first_name) AS reverse_name
FROM employees;
```

### **8. REPEAT() – Repeat Text**

```
SELECT REPEAT(first_name, 2) AS repeated_name
FROM employees;
```

### **9. SPACE() – Generate Spaces**

```
SELECT CONCAT(first_name, SPACE(5), last_name) AS full_name
FROM employees;
```

### **10. ASCII() – Get ASCII Value**

Returns the ASCII value of the first character.

```
SELECT first_name, ASCII(first_name) AS ascii_value
FROM employees;
```

For Rohit, the result is 82 (ASCII of R).

### **11. CHAR() – Convert ASCII to Character**

```
SELECT CHAR(65, 66, 67);
```

Output: ABC

## **4. String Extraction Functions**

### **12. LEFT() – Extract from Left**

```
SELECT first_name, LEFT(first_name, 3)
FROM employees;
```

### **13. RIGHT() – Extract from Right**

```
SELECT first_name, RIGHT(first_name, 2)
FROM employees;
```

### **14. SUBSTRING() – Extract Specific Characters**

Syntax:

```
SUBSTRING(string, start_position, length)
```

Example:

```
SELECT first_name,
       SUBSTRING(first_name, 2, 3) AS extracted_text
FROM employees;
```

### **15. MID() – Extract Middle Characters**

`MID()` is an alias of `SUBSTRING()`.

```
SELECT MID(first_name, 2, 3)
FROM employees;
```

### **16. SUBSTR() – Extract Substring**

```
SELECT SUBSTR(email, 1, 5)
FROM employees;
```

`SUBSTR()` is also an alias of `SUBSTRING()`.

### **17. SUBSTRING_INDEX() – Extract Text Before or After Delimiter**

This is especially useful for names and email addresses.

**Extract email username:**

```
SELECT email,
       SUBSTRING_INDEX(email, '@', 1) AS username
FROM employees;
```

**Extract domain:**

```
SELECT email,
       SUBSTRING_INDEX(email, '@', -1) AS domain
FROM employees;
```

**Extract first name from full name:**

```
SELECT SUBSTRING_INDEX('Rohit Kumar Jha', ' ', 1);
```

**Extract first two words:**

```
SELECT SUBSTRING_INDEX('Rohit Kumar Jha', ' ', 2);
```

## **5. String Cleaning Functions**

These functions are very important in Data Analytics because real-world datasets often contain unwanted spaces and inconsistent text.

### **18. TRIM() – Remove Both-Side Spaces**

```
SELECT TRIM('   Rohit Jha   ') AS clean_name;
```

### **19. LTRIM() – Remove Left Spaces**

```
SELECT LTRIM('   Rohit') AS clean_name;
```

### **20. RTRIM() – Remove Right Spaces**

```
SELECT RTRIM('Rohit   ') AS clean_name;
```

### **21. REPLACE() – Replace Text**

Replace Gmail with Outlook in email addresses:

```
SELECT email,
       REPLACE(email, 'gmail.com', 'outlook.com') AS new_email
FROM employees;
```

### **22. INSERT() – Replace a Portion of a String**

Syntax:

```
INSERT(string, position, length, new_string)
```

Example:

```
SELECT INSERT('Rohit Jha', 7, 3, 'Kumar');
```

Output: Rohit Kumar

### **23. REGEXP_REPLACE() – Replace Using Regular Expressions**

Available in MySQL 8.0+.

Remove all spaces from a string:

```
SELECT REGEXP_REPLACE('Rohit   Kumar   Jha', '[[:space:]]+', '');
```

Output: RohitKumarJha

Remove all digits:

```
SELECT REGEXP_REPLACE('EMP-12345', '[0-9]', '');
```

Output: EMP-

## **6. String Searching Functions**

### **24. LOCATE() – Find Position of Text**

```
SELECT LOCATE('gmail', email) AS position
FROM employees;
```

### **25. INSTR() – Find Position**

```
SELECT INSTR(email, '@') AS at_position
FROM employees;
```

### **26. POSITION() – Find Position**

```
SELECT POSITION('@' IN email) AS position
FROM employees;
```

### **27. FIND_IN_SET() – Search in Comma-Separated Values**

```
SELECT FIND_IN_SET('IT', 'HR,IT,Finance,Sales');
```

Output: 2

Returns 0 when the value is not found.

### **28. FIELD() – Find Value Position in a List**

```
SELECT FIELD('Finance', 'HR', 'IT', 'Finance', 'Sales');
```

Output: 3

### **29. SOUNDEX() – Find Similar-Sounding Words**

```
SELECT SOUNDEX('Smith'), SOUNDEX('Smyth');
```

Useful for basic phonetic matching, although it is not a reliable general-purpose fuzzy matching algorithm.

### **30. REGEXP_INSTR() – Find Position Using Pattern**

```
SELECT REGEXP_INSTR(email, '[0-9]');
```

Returns the position of the first digit matching the pattern, or 0 if none is found.

### **31. REGEXP_SUBSTR() – Extract Text Using Pattern**

```
SELECT REGEXP_SUBSTR('EMP-12345', '[0-9]+');
```

Output: 12345

### **32. REGEXP_LIKE() – Check Pattern Matching**

```
SELECT email
FROM employees
WHERE REGEXP_LIKE(email, '@gmail[.]com$');
```

Returns emails ending with gmail.com.

## **7. String Formatting Functions**

### **33. LPAD() – Add Characters on Left**

```
SELECT LPAD(emp_id, 6, '0') AS employee_code
FROM employees;
```

Example: 000101

### **34. RPAD() – Add Characters on Right**

```
SELECT RPAD(first_name, 10, '*') AS formatted_name
FROM employees;
```

### **35. ELT() – Return Value at Position**

```
SELECT ELT(2, 'HR', 'IT', 'Finance', 'Sales');
```

Output: IT

### **36. MAKE_SET() – Create a String from Selected Bits**

```
SELECT MAKE_SET(5, 'HR', 'IT', 'Finance', 'Sales');
```

Output: HR,Finance

### **37. EXPORT_SET() – Convert Bits into a Separated String**

```
SELECT EXPORT_SET(5, 'YES', 'NO', ',', 4);
```

Output: YES,NO,YES,NO

### **38. QUOTE() – Add Quotes and Escape Special Characters**

```
SELECT QUOTE('Rohit Jha');
```

Output: 'Rohit Jha'

### **39. HEX() – Convert Text to Hexadecimal**

```
SELECT HEX('ABC');
```

Output: 414243

### **40. UNHEX() – Convert Hexadecimal to Text**

```
SELECT UNHEX('414243');
```

Output: ABC

## **8. Important Real-World Data Cleaning Queries**

These examples combine multiple functions.

**Example 1: Generate full name**

```
SELECT
    emp_id,
    CONCAT_WS(' ', first_name, middle_name, last_name) AS full_name
FROM employees;
```

**Example 2: Extract email username and domain**

```
SELECT
    email,
    SUBSTRING_INDEX(email, '@', 1) AS username,
    SUBSTRING_INDEX(email, '@', -1) AS domain
FROM employees;
```

**Example 3: Generate employee ID with leading zeros**

```
SELECT
    emp_id,
    CONCAT('EMP-', LPAD(emp_id, 5, '0')) AS new_emp_code
FROM employees;
```

**Example 4: Create employee initials**

```
SELECT
    CONCAT(
        LEFT(first_name, 1),
        IF(middle_name IS NOT NULL,
           LEFT(middle_name, 1), ''),
        LEFT(last_name, 1)
    ) AS initials
FROM employees;
```

**Example 5: Display first name with department**

```
SELECT
    CONCAT(
        UPPER(first_name),
        ' - ',
        UPPER(department)
    ) AS employee_details
FROM employees;
```

**Example 6: Extract email provider**

```
SELECT
    email,
    SUBSTRING_INDEX(
        SUBSTRING_INDEX(email, '@', -1),
        '.',
        1
    ) AS email_provider
FROM employees;
```

**Example 7: Mask phone numbers**

```
SELECT
    CONCAT('XXXXXX', RIGHT(phone, 4)) AS masked_phone
FROM employees;
```

**Example 8: Display department in fixed width**

```
SELECT RPAD(department, 15, '-') AS formatted_department
FROM employees;
```

## **9. Complete Function Revision Table**

| No. | Function          | Purpose                     |
| --- | ----------------- | --------------------------- |
| 1   | UPPER()           | Uppercase                   |
| 2   | LOWER()           | Lowercase                   |
| 3   | LENGTH()          | Count bytes                 |
| 4   | CHAR_LENGTH()     | Count characters            |
| 5   | CONCAT()          | Join strings                |
| 6   | CONCAT_WS()       | Join with separator         |
| 7   | REVERSE()         | Reverse string              |
| 8   | REPEAT()          | Repeat string               |
| 9   | SPACE()           | Generate spaces             |
| 10  | ASCII()           | Get ASCII value             |
| 11  | CHAR()            | ASCII to character          |
| 12  | LEFT()            | Extract left characters     |
| 13  | RIGHT()           | Extract right characters    |
| 14  | SUBSTRING()       | Extract substring           |
| 15  | MID()             | Extract middle characters   |
| 16  | SUBSTR()          | Extract substring           |
| 17  | SUBSTRING_INDEX() | Extract using delimiter     |
| 18  | TRIM()            | Remove both-side spaces     |
| 19  | LTRIM()           | Remove left spaces          |
| 20  | RTRIM()           | Remove right spaces         |
| 21  | REPLACE()         | Replace text                |
| 22  | INSERT()          | Insert/replace text         |
| 23  | REGEXP_REPLACE()  | Replace using regex         |
| 24  | LOCATE()          | Find text position          |
| 25  | INSTR()           | Find text position          |
| 26  | POSITION()        | Find text position          |
| 27  | FIND_IN_SET()     | Search comma-separated list |
| 28  | FIELD()           | Find value index            |
| 29  | SOUNDEX()         | Phonetic matching           |
| 30  | REGEXP_INSTR()    | Search using regex          |
| 31  | REGEXP_SUBSTR()   | Extract using regex         |
| 32  | REGEXP_LIKE()     | Check regex pattern         |
| 33  | LPAD()            | Add characters left         |
| 34  | RPAD()            | Add characters right        |
| 35  | ELT()             | Return value by position    |
| 36  | MAKE_SET()        | Select values using bits    |
| 37  | EXPORT_SET()      | Format bit values           |
| 38  | QUOTE()           | Quote and escape text       |
| 39  | HEX()             | Convert to hexadecimal      |
| 40  | UNHEX()           | Convert hexadecimal to text |

*Note: These examples use MySQL. Regex functions require MySQL 8.0 or compatible versions; some advanced functions may not be available in older releases.*

## **10. Practical SQL Exercises**

Try these 20 questions using the employees table.

Beginner – Level 1

1. Display all employee names in uppercase.
2. Display all email addresses in lowercase.
3. Display the first three characters of every employee's name.
4. Display the last three characters of every employee's name.
5. Display the total characters in each department.
6. Combine first name and last name.
7. Display the reverse of every employee's first name.

Intermediate – Level 2

8. Extract the username from every email address.
9. Extract the email domain from every email address.
10. Display the first two characters of every department.
11. Generate employee codes like EMP-00101.
12. Create initials using first, middle and last names.
13. Find the position of @ in every email.
14. Replace gmail.com with yahoo.com.
15. Display only the last four digits of phone numbers.

Advanced – Level 3

16. Extract the first name from a full name using SUBSTRING_INDEX().
17. Remove all spaces from a text string using REGEXP_REPLACE().
18. Display the employee's full name in uppercase with department.
19. Mask phone numbers, showing only the last four digits.
20. Extract the email provider and create a new column showing its name in uppercase.

## **11. Solutions to Selected Exercises**

**Q8. Extract username**

```
SELECT
    email,
    SUBSTRING_INDEX(email, '@', 1) AS username
FROM employees;
```

**Q9. Extract email domain**

```
SELECT
    email,
    SUBSTRING_INDEX(email, '@', -1) AS domain
FROM employees;
```

**Q11. Generate employee codes**

```
SELECT
    emp_id,
    CONCAT('EMP-', LPAD(emp_id, 5, '0')) AS new_code
FROM employees;
```

**Q12. Generate initials**

```
SELECT
    first_name,
    middle_name,
    last_name,
    CONCAT(
        LEFT(first_name, 1),
        IFNULL(LEFT(middle_name, 1), ''),
        LEFT(last_name, 1)
    ) AS initials
FROM employees;
```

**Q15. Mask phone numbers**

```
SELECT
    CONCAT('XXXXXX', RIGHT(phone, 4)) AS masked_phone
FROM employees;
```

**Q20. Extract and capitalize email provider**

```
SELECT
    email,
    UPPER(
        SUBSTRING_INDEX(
            SUBSTRING_INDEX(email, '@', -1),
            '.',
            1
        )
    ) AS provider
FROM employees;
```
