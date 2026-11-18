# **SQL String Functions**

## **1. Sample Dataset**

**Table: Students**

| student_id | first_name | last_name |
| ---------- | ---------- | --------- |
| 1          | Rohit      | Jha       |
| 2          | Priya      | Sharma    |
| 3          | Amit       | Verma     |
| 4          | Neha       | Patel     |
| 5          | Rahul      | Kumar     |

## **2. Important SQL String Functions**

### **1. UPPER()**

Converts text into uppercase.

```
SELECT UPPER(first_name)
FROM students;
```

Output: ROHIT, PRIYA, AMIT

### **2. LOWER()**

Converts text into lowercase.

```
SELECT LOWER(first_name)
FROM students;
```

### **3. LENGTH()**

Returns the length of a string in bytes.

```
SELECT first_name, LENGTH(first_name)
FROM students;
```

### **4. CHAR_LENGTH()**

Returns the number of characters.

```
SELECT CHAR_LENGTH(first_name)
FROM students;
```

### **5. CONCAT()**

Combines two or more strings.

```
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM students;
```

### **6. CONCAT_WS()**

Combines strings using a separator.

```
SELECT CONCAT_WS(' ', first_name, last_name) AS full_name
FROM students;
```

### **7. SUBSTRING()**

Extracts a portion of a string.

```
SELECT SUBSTRING(first_name, 1, 3)
FROM students;
```

Output: Roh, Pri, Ami, Neh, Rah

### **8. LEFT()**

Extracts characters from the left side.

```
SELECT LEFT(first_name, 2)
FROM students;
```

### **9. RIGHT()**

Extracts characters from the right side.

```
SELECT RIGHT(first_name, 2)
FROM students;
```

### **10. TRIM()**

Removes leading and trailing spaces.

```
SELECT TRIM('  Rohit Jha  ');
```

### **11. LTRIM()**

Removes spaces from the left side.

```
SELECT LTRIM('  Rohit');
```

### **12. RTRIM()**

Removes spaces from the right side.

```
SELECT RTRIM('Rohit  ');
```

### **13. REPLACE()**

Replaces a specific string with another string.

```
SELECT REPLACE('I Love SQL', 'SQL', 'Python');
```

Output: I Love Python

### **14. REVERSE()**

Reverses a string.

```
SELECT REVERSE('ROHIT');
```

Output: TIHOR

### **15. LOCATE()**

Finds the position of a substring.

```
SELECT LOCATE('h', 'Rohit');
```

Output: 3

### **16. INSTR()**

Finds the position of a substring inside a string.

```
SELECT INSTR('Rohit Jha', 'Jha');
```

Output: 7

### **17. LPAD()**

Adds characters to the left side until the specified length is reached.

```
SELECT LPAD('123', 5, '0');
```

Output: 00123

### **18. RPAD()**

Adds characters to the right side.

```
SELECT RPAD('SQL', 6, '*');
```

Output: SQL\*\*\*

### **19. REPEAT()**

Repeats a string a specified number of times.

```
SELECT REPEAT('SQL ', 3);
```

Output: SQL SQL SQL

### **20. SPACE()**

Returns a string containing the specified number of spaces.

```
SELECT CONCAT('Hello', SPACE(3), 'World');
```

## **3. Quick Revision Table**

| Function      | Purpose                 |
| ------------- | ----------------------- |
| UPPER()       | Convert to uppercase    |
| LOWER()       | Convert to lowercase    |
| LENGTH()      | Find byte length        |
| CHAR_LENGTH() | Count characters        |
| CONCAT()      | Join strings            |
| CONCAT_WS()   | Join with separator     |
| SUBSTRING()   | Extract characters      |
| LEFT()        | Extract from left       |
| RIGHT()       | Extract from right      |
| TRIM()        | Remove both-side spaces |
| LTRIM()       | Remove left spaces      |
| RTRIM()       | Remove right spaces     |
| REPLACE()     | Replace text            |
| REVERSE()     | Reverse text            |
| LOCATE()      | Find position           |
| INSTR()       | Find substring position |
| LPAD()        | Add characters on left  |
| RPAD()        | Add characters on right |
| REPEAT()      | Repeat text             |
| SPACE()       | Generate spaces         |

## **4. Practice Questions**

Try these SQL queries using the Students table.

1. Display all student names in uppercase.
2. Display all student names in lowercase.
3. Display the full name using CONCAT().
4. Display the first three characters of each first name.
5. Display the last two characters of each last name.
6. Find the length of every first name.
7. Display the first name in reverse order.
8. Replace the name 'Rohit' with 'Rahul'.
9. Display the first two characters of each first name in uppercase.
10. Display full names separated by a hyphen (-).
exercises for classroom practice.
