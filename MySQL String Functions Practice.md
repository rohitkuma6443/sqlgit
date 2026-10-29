# MySQL String Functions: Practice Questions with Solutions

This practice set uses the `string_practice` database and `customer` table. It includes easy questions, combined string functions, and practical SQL queries with solutions.

## 1. Database and table setup

Run this code in MySQL Workbench.

```
CREATE DATABASE string_practice;
USE string_practice;

CREATE TABLE customer (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    city VARCHAR(50)
);

INSERT INTO customer VALUES
(1, 'rohit', 'jha', 'rohit.jha@gmail.com', 'Mumbai'),
(2, 'PRIYA', 'sharma', 'priya.sharma@yahoo.com', 'Delhi'),
(3, 'Amit', 'kumar', 'amit.kumar@gmail.com', 'Pune'),
(4, 'neha', 'patel', 'neha.patel@outlook.com', 'Mumbai'),
(5, 'RAHUL', 'verma', 'rahul.verma@gmail.com', 'Nashik'),
(6, 'sneha', 'singh', 'sneha.singh@yahoo.com', 'Delhi'),
(7, 'Arjun', 'mehta', 'arjun.mehta@gmail.com', 'Pune'),
(8, 'pooja', 'desai', 'pooja.desai@outlook.com', 'Mumbai'),
(9, 'Karan', 'shah', 'karan.shah@gmail.com', 'Delhi'),
(10, 'meera', 'rao', 'meera.rao@yahoo.com', 'Pune');
```

## 2. Practice questions with solutions

### Basic level

Q1. Display the full name of every customer.

```
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM customer;
```

Explanation: `CONCAT()` joins the first name, a space, and the last name.

Q2. Display all first names in uppercase.

```
SELECT UPPER(first_name) AS first_name
FROM customer;
```

Explanation: `UPPER()` converts lowercase letters into uppercase.

Q3. Display all email addresses in lowercase.

```
SELECT LOWER(email) AS email
FROM customer;
```

Explanation: `LOWER()` converts text into lowercase.

Q4. Display the first three letters of each first name.

```
SELECT first_name, LEFT(first_name, 3) AS short_name
FROM customer;
```

Explanation: `LEFT()` returns three characters from the beginning of the name.

Q5. Display the last three letters of each last name.

```
SELECT last_name, RIGHT(last_name, 3) AS last_three_letters
FROM customer;
```

Explanation: `RIGHT()` returns three characters from the end.

### Intermediate level: Combine string functions

Q6. Display full names in uppercase.

```
SELECT UPPER(CONCAT(first_name, ' ', last_name)) AS full_name
FROM customer;
```

Explanation:

1. `CONCAT()` joins the names.
2. `UPPER()` converts the complete name to uppercase.

Q7. Generate initials for each customer.

```
SELECT
    first_name,
    last_name,
    CONCAT(
        UPPER(LEFT(first_name, 1)),
        UPPER(LEFT(last_name, 1))
    ) AS initials
FROM customer;
```

Explanation: `LEFT()` extracts the first character from each name, `UPPER()` capitalizes it, and `CONCAT()` combines the initials.

Example: Rohit Jha becomes `RJ`.

Q8. Display the username from each email address.

```
SELECT
    email,
    SUBSTRING_INDEX(email, '@', 1) AS username
FROM customer;
```

Explanation: `SUBSTRING_INDEX()` extracts the text before the first `@` symbol.

Q9. Display the email domain for each customer.

```
SELECT
    email,
    SUBSTRING_INDEX(email, '@', -1) AS domain
FROM customer;
```

Explanation: A count of `-1` extracts the text after the last `@` symbol.

Q10. Display each customer's full name and its character count.

```
SELECT
    CONCAT(first_name, ' ', last_name) AS full_name,
    CHAR_LENGTH(CONCAT(first_name, ' ', last_name)) AS name_length
FROM customer;
```

Explanation: `CHAR_LENGTH()` counts the characters, including the space between the names.

### Practical level: String functions with WHERE and GROUP BY

Q11. Find customers whose first name starts with R, regardless of case.

```
SELECT *
FROM customer
WHERE UPPER(LEFT(first_name, 1)) = 'R';
```

Explanation: `LEFT()` extracts the first letter, and `UPPER()` standardizes its case before filtering.

Q12. Display lowercase full names of customers living in Delhi.

```
SELECT
    LOWER(CONCAT(first_name, ' ', last_name)) AS full_name,
    city
FROM customer
WHERE city = 'Delhi';
```

Explanation: `WHERE` filters Delhi customers; `CONCAT()` and `LOWER()` format their names.

Q13. Count customers by email provider.

```
SELECT
    SUBSTRING_INDEX(email, '@', -1) AS provider,
    COUNT(*) AS total_customers
FROM customer
GROUP BY SUBSTRING_INDEX(email, '@', -1);
```

Explanation: `SUBSTRING_INDEX()` extracts the provider, `COUNT()` counts the records, and `GROUP BY` creates one group per provider.

Expected result:

| provider    | total_customers |
| ----------- | --------------- |
| gmail.com   | 5               |
| yahoo.com   | 3               |
| outlook.com | 2               |

Q14. Display customer IDs as five-character strings padded with zeros.

```
SELECT
    customer_id,
    LPAD(customer_id, 5, '0') AS formatted_id
FROM customer;
```

Explanation: `LPAD()` adds zeros on the left until the output has five characters. For example, `1` becomes `00001`.

Q15. Replace `gmail.com` with `example.com` in the displayed email addresses.

```
SELECT
    email,
    REPLACE(email, 'gmail.com', 'example.com') AS modified_email
FROM customer;
```

Explanation: `REPLACE()` substitutes matching text. It does not update the original table.

## 3. Bonus questions with solutions

Q16. Display customer names in reverse order.

```
SELECT
    first_name,
    REVERSE(first_name) AS reversed_name
FROM customer;
```

Q17. Find the position of the `@` symbol in each email address.

```
SELECT
    email,
    LOCATE('@', email) AS at_position
FROM customer;
```

Q18. Create usernames by combining lowercase first names and cities.

```
SELECT
    CONCAT(LOWER(first_name), '_', LOWER(city)) AS username
FROM customer;
```

Example: `rohit_mumbai`.

Q19. Display names that contain the letter `a`, regardless of case.

```
SELECT *
FROM customer
WHERE LOCATE('a', LOWER(first_name)) > 0;
```

Explanation: `LOWER()` standardizes the case, and `LOCATE()` returns the position of `a`. A result greater than zero means the letter exists.

Q20. Display full names sorted from longest to shortest.

```
SELECT
    CONCAT(first_name, ' ', last_name) AS full_name,
    CHAR_LENGTH(CONCAT(first_name, ' ', last_name)) AS name_length
FROM customer
ORDER BY name_length DESC;
```

Explanation: `ORDER BY ... DESC` sorts the longest names first.

## 4. Test yourself

Try these five questions without looking at the solutions above.

1\. Which function extracts the text after the last @ symbol?

LEFT()

SUBSTRING_INDEX()

REVERSE()

LPAD()

2\. What is the output of UPPER(LEFT('rohit', 2))?

ro

RO

ROHIT

oh

3\. Which function combines first_name and last_name with a space between them?

CONCAT(first_name, ' ', last_name)

LEFT(first_name, last_name)

COUNT(first_name, last_name)

LOCATE(first_name, last_name)

4\. What does CHAR_LENGTH('SQL') return?

2

3

4

0

5\. Which clause groups customers by the domain extracted from their emails?

ORDER BY

WHERE

GROUP BY

LIMIT

Check answers

Next topic: Once you are comfortable with these 20 questions, practise SQL string functions with `CASE`, `WHERE`, `GROUP BY`, and `HAVING` on a larger customer dataset.
