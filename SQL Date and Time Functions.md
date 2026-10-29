# SQL Date and Time Functions — MySQL

Date and time functions are an important SQL topic for data analytics. They help you extract the year or month, calculate durations, filter records by date, format dates, and analyse sales over time.

We will use your existing `subquery_db` database and its `orders` table, which contains an `order_date` column.

## 1. Understand SQL date and time data types

| Data type   | Stores                                                              | Example               |
| ----------- | ------------------------------------------------------------------- | --------------------- |
| `DATE`      | Date only                                                           | `2026-10-29`          |
| `TIME`      | Time only                                                           | `14:30:00`            |
| `DATETIME`  | Date and time                                                       | `2026-10-29 14:30:00` |
| `TIMESTAMP` | Date and time, with time-zone-related conversion behaviour in MySQL | `2026-10-29 14:30:00` |
| `YEAR`      | Year                                                                | `2026`                |

Example using your existing table:

```
USE subquery_db;

SELECT order_id, order_date
FROM orders;
```

The `order_date` column uses the `DATE` data type, so it stores the date but not the time.

## 2. Important MySQL date and time functions

### Current date and time

```
SELECT
    CURDATE() AS current_date,
    CURTIME() AS current_time,
    NOW() AS current_datetime;
```

- `CURDATE()` returns the current date.
- `CURTIME()` returns the current time.
- `NOW()` returns the current date and time.

`CURRENT_DATE()`, `CURRENT_TIME()`, and `CURRENT_TIMESTAMP()` are also available.

### Extract year, month, and day

```
SELECT
    order_date,
    YEAR(order_date) AS order_year,
    MONTH(order_date) AS order_month,
    DAY(order_date) AS order_day
FROM orders;
```

For an order dated `2026-01-05`, the result is:

| order_date | order_year | order_month | order_day |
| ---------- | ---------- | ----------- | --------- |
| 2026-01-05 | 2026       | 1           | 5         |

### Get the month and day names

```
SELECT
    order_date,
    MONTHNAME(order_date) AS month_name,
    DAYNAME(order_date) AS day_name
FROM orders;
```

For example, `2026-01-05` falls on a Monday, so the month name is `January` and the day name is `Monday`.

## 3. Date and time functions reference

| Function          | Purpose                           | Example                                      |
| ----------------- | --------------------------------- | -------------------------------------------- |
| `YEAR(date)`      | Extracts year                     | `YEAR('2026-10-29')` → `2026`                |
| `MONTH(date)`     | Extracts month number             | `MONTH('2026-10-29')` → `10`                 |
| `DAY(date)`       | Extracts day of month             | `DAY('2026-10-29')` → `29`                   |
| `MONTHNAME(date)` | Returns month name                | `MONTHNAME('2026-10-29')` → `October`        |
| `DAYNAME(date)`   | Returns weekday name              | `DAYNAME('2026-10-29')` → `Thursday`         |
| `DAYOFWEEK(date)` | Sunday = 1 through Saturday = 7   | `DAYOFWEEK('2026-10-29')` → `5`              |
| `DAYOFYEAR(date)` | Day number within the year        | `DAYOFYEAR('2026-01-05')` → `5`              |
| `QUARTER(date)`   | Returns quarter 1–4               | `QUARTER('2026-10-29')` → `4`                |
| `LAST_DAY(date)`  | Returns the last day of the month | `LAST_DAY('2026-02-10')` → `2026-02-28`      |
| `DATE(datetime)`  | Extracts date portion             | `DATE('2026-10-29 14:30:00')` → `2026-10-29` |
| `TIME(datetime)`  | Extracts time portion             | `TIME('2026-10-29 14:30:00')` → `14:30:00`   |
| `HOUR(time)`      | Extracts hour                     | `HOUR('14:30:00')` → `14`                    |
| `MINUTE(time)`    | Extracts minute                   | `MINUTE('14:30:00')` → `30`                  |
| `SECOND(time)`    | Extracts second                   | `SECOND('14:30:45')` → `45`                  |

Try the functions together:

```
SELECT
    YEAR(order_date) AS order_year,
    MONTHNAME(order_date) AS order_month,
    DAYNAME(order_date) AS weekday,
    QUARTER(order_date) AS order_quarter
FROM orders;
```

## 4. DATE_FORMAT() — change the display format

`DATE_FORMAT()` displays a date in the format you specify. It does not change the stored date.

```
SELECT
    order_date,
    DATE_FORMAT(order_date, '%d-%m-%Y') AS formatted_date
FROM orders;
```

For `2026-01-05`, the formatted result is `05-01-2026`.

Common format specifiers:

| Specifier | Meaning                    | Example   |
| --------- | -------------------------- | --------- |
| `%Y`      | Four-digit year            | `2026`    |
| `%y`      | Two-digit year             | `26`      |
| `%m`      | Two-digit month            | `01`      |
| `%c`      | Month without leading zero | `1`       |
| `%M`      | Full month name            | `January` |
| `%b`      | Abbreviated month name     | `Jan`     |
| `%d`      | Two-digit day              | `05`      |
| `%e`      | Day without leading zero   | `5`       |
| `%W`      | Full weekday name          | `Monday`  |
| `%a`      | Abbreviated weekday        | `Mon`     |
| `%H`      | Hour, 00–23                | `14`      |
| `%h`      | Hour, 01–12                | `02`      |
| `%i`      | Minutes                    | `30`      |
| `%s`      | Seconds                    | `00`      |
| `%p`      | AM or PM                   | `PM`      |

Example:

```
SELECT DATE_FORMAT(
    '2026-10-29 14:30:45',
    '%d %M %Y, %h:%i %p'
) AS formatted_datetime;
```

Output: `29 October 2026, 02:30 PM`

## 5. STR_TO_DATE() — convert text into a date

Use `STR_TO_DATE()` when date values arrive as text in a known format.

```
SELECT STR_TO_DATE('29-10-2026', '%d-%m-%Y') AS converted_date;
```

Output: `2026-10-29`

Another example:

```
SELECT STR_TO_DATE('05/01/2026', '%d/%m/%Y') AS converted_date;
```

Output: `2026-01-05`

Remember: `DATE_FORMAT()` converts a date into formatted text; `STR_TO_DATE()` parses text into a date or datetime value.

## 6. DATEDIFF() — calculate the number of days between dates

`DATEDIFF()` returns the difference in days between two dates. It ignores the time portion if datetime values are supplied.

Syntax:

```
DATEDIFF(end_date, start_date);
```

Example:

```
SELECT DATEDIFF('2026-02-01', '2026-01-01') AS days_difference;
```

Output: `31`

Use it with your orders:

```
SELECT
    order_id,
    order_date,
    DATEDIFF('2026-03-01', order_date) AS days_until_march
FROM orders;
```

This calculates the number of days between each order date and 1 March 2026.

## 7. TIMESTAMPDIFF() — calculate years, months, days, or hours

`TIMESTAMPDIFF()` calculates the difference in a specified unit.

Syntax:

```
TIMESTAMPDIFF(unit, start_datetime, end_datetime);
```

Example — difference in months:

```
SELECT TIMESTAMPDIFF(
    MONTH,
    '2026-01-01',
    '2026-04-15'
) AS months_difference;
```

Output: `3`

Other examples:

```
SELECT TIMESTAMPDIFF(
    YEAR, '2020-01-01', '2026-01-01'
) AS years_difference;

SELECT TIMESTAMPDIFF(
    DAY, '2026-01-01', '2026-01-10'
) AS days_difference;

SELECT TIMESTAMPDIFF(
    HOUR, '2026-01-01 09:00:00', '2026-01-01 17:00:00'
) AS hours_difference;
```

Outputs: `6`, `9`, and `8`, respectively.

Difference: Use `DATEDIFF()` for a simple difference in days. Use `TIMESTAMPDIFF()` when you need a specific unit such as years, months, hours, or minutes.

## 8. DATE_ADD() and DATE_SUB() — add or subtract time

### Add days

```
SELECT DATE_ADD('2026-01-05', INTERVAL 7 DAY) AS new_date;
```

Output: `2026-01-12`

### Subtract days

```
SELECT DATE_SUB('2026-01-05', INTERVAL 3 DAY) AS new_date;
```

Output: `2026-01-02`

### Add months

```
SELECT DATE_ADD('2026-01-31', INTERVAL 1 MONTH) AS new_date;
```

Output: `2026-02-28`

MySQL adjusts to the last valid day when the target month has fewer days.

### Other supported intervals

```
SELECT DATE_ADD('2026-01-05', INTERVAL 2 MONTH);
SELECT DATE_ADD('2026-01-05', INTERVAL 1 YEAR);
SELECT DATE_ADD('2026-01-05 10:00:00', INTERVAL 3 HOUR);
SELECT DATE_SUB('2026-01-05 10:00:00', INTERVAL 30 MINUTE);
```

## 9. EXTRACT() — extract a specific date part

`EXTRACT()` provides another way to retrieve part of a date or datetime value.

```
SELECT
    EXTRACT(YEAR FROM order_date) AS order_year,
    EXTRACT(MONTH FROM order_date) AS order_month,
    EXTRACT(DAY FROM order_date) AS order_day
FROM orders;
```

For the year, month, and day, this provides the same kind of result as `YEAR()`, `MONTH()`, and `DAY()`.

## 10. Filter orders by date

This is particularly useful for data analytics reports.

### Orders in January 2026

```
SELECT *
FROM orders
WHERE order_date >= '2026-01-01'
  AND order_date <  '2026-02-01';
```

### Orders in February 2026

```
SELECT *
FROM orders
WHERE order_date >= '2026-02-01'
  AND order_date <  '2026-03-01';
```

### Orders placed after 15 January 2026

```
SELECT *
FROM orders
WHERE order_date > '2026-01-15';
```

### Orders placed on a particular day

```
SELECT *
FROM orders
WHERE order_date = '2026-01-05';
```

Performance tip: For datetime columns, a range condition is usually preferable to wrapping the column in `YEAR()` or `MONTH()` because it can make better use of an index.

For example, instead of:

```
WHERE YEAR(order_date) = 2026
```

Prefer:

```
WHERE order_date >= '2026-01-01'
  AND order_date <  '2027-01-01'
```

## 11. Solved data analytics examples

### Example A: Calculate total sales by month

```
SELECT
    YEAR(order_date) AS sales_year,
    MONTH(order_date) AS sales_month,
    SUM(sales) AS total_sales
FROM orders
GROUP BY
    YEAR(order_date),
    MONTH(order_date)
ORDER BY
    sales_year,
    sales_month;
```

This produces one row per year and month, making it useful for monthly sales reporting.

### Example B: Display month names instead of numbers

```
SELECT
    DATE_FORMAT(order_date, '%Y-%m') AS sales_month,
    SUM(sales) AS total_sales
FROM orders
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
ORDER BY sales_month;
```

### Example C: Find the first and last order dates

```
SELECT
    MIN(order_date) AS first_order_date,
    MAX(order_date) AS last_order_date
FROM orders;
```

Expected results: `2026-01-05` and `2026-02-28`.

### Example D: Count orders per weekday

```
SELECT
    DAYNAME(order_date) AS weekday,
    COUNT(*) AS total_orders
FROM orders
GROUP BY DAYNAME(order_date)
ORDER BY total_orders DESC;
```

This groups orders by weekday name. If you want weekdays in calendar order instead of sales frequency, group by `DAYOFWEEK(order_date)` and sort by that number.

### Example E: Find orders placed in the last 30 days

```
SELECT *
FROM orders
WHERE order_date >= CURDATE() - INTERVAL 30 DAY
  AND order_date <  CURDATE() + INTERVAL 1 DAY;
```

This includes dates from the last 30 days through today. Because the sample data is from January and February 2026, it may return no rows when run in October 2026.

## 12. Practice exercises with solutions

Use your existing `orders` table to solve these questions.

### Exercise 1: Extract the year, month, and day from every order date.

```
SELECT
    order_id,
    YEAR(order_date) AS order_year,
    MONTH(order_date) AS order_month,
    DAY(order_date) AS order_day
FROM orders;
```

### Exercise 2: Find orders placed in January 2026.

```
SELECT *
FROM orders
WHERE order_date >= '2026-01-01'
  AND order_date < '2026-02-01';
```

### Exercise 3: Display order dates in `DD/MM/YYYY` format.

```
SELECT
    order_id,
    DATE_FORMAT(order_date, '%d/%m/%Y') AS formatted_date
FROM orders;
```

### Exercise 4: Calculate days between each order and 1 March 2026.

```
SELECT
    order_id,
    order_date,
    DATEDIFF('2026-03-01', order_date) AS days_difference
FROM orders;
```

### Exercise 5: Find orders placed in the first quarter of 2026.

```
SELECT *
FROM orders
WHERE order_date >= '2026-01-01'
  AND order_date < '2026-04-01';
```

### Exercise 6: Display the last day of each order's month.

```
SELECT
    order_id,
    order_date,
    LAST_DAY(order_date) AS month_end
FROM orders;
```

### Exercise 7: Find orders made on a Monday.

```
SELECT *
FROM orders
WHERE DAYOFWEEK(order_date) = 2;
```

In MySQL, Sunday is `1`, Monday is `2`, and Saturday is `7`.

### Exercise 8: Calculate the date 15 days after every order.

```
SELECT
    order_id,
    order_date,
    DATE_ADD(order_date, INTERVAL 15 DAY) AS date_after_15_days
FROM orders;
```

### Exercise 9: Find the number of orders per month.

```
SELECT
    DATE_FORMAT(order_date, '%Y-%m') AS order_month,
    COUNT(*) AS total_orders
FROM orders
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
ORDER BY order_month;
```

### Exercise 10: Find the total sales for each quarter.

```
SELECT
    YEAR(order_date) AS sales_year,
    QUARTER(order_date) AS sales_quarter,
    SUM(sales) AS total_sales
FROM orders
GROUP BY
    YEAR(order_date),
    QUARTER(order_date)
ORDER BY
    sales_year,
    sales_quarter;
```

## 13. Additional practice questions

Try these without looking at the solutions above.

1. Find orders placed in February 2026.
2. Display order dates in `DD-Mon-YYYY` format, such as `05-Jan-2026`.
3. Find the weekday name for every order.
4. Calculate the difference in days between the first and last order dates.
5. Find the total sales for January 2026.
6. Find the total sales for February 2026.
7. Display the quarter number for every order.
8. Find all orders placed after 20 January 2026.
9. Calculate the date 3 months after each order.
10. Find the number of orders placed on each calendar date.
11. Find products ordered during January 2026 by joining `orders` and `products`.
12. Find employees whose first order was placed in January 2026.
13. Find the earliest order date for each employee.
14. Calculate the number of days between each employee's first and last orders.
15. Find the month with the highest total sales.

## 14. SQL interview questions

Q1. What is the difference between `DATE` and `DATETIME`?

`DATE` stores a calendar date. `DATETIME` stores both date and time.

Q2. What is the difference between `NOW()` and `CURDATE()`?

`NOW()` returns the current date and time. `CURDATE()` returns only the current date.

Q3. What does `DATEDIFF()` return?

It returns the difference between two dates in days.

Q4. What is the difference between `DATE_FORMAT()` and `STR_TO_DATE()`?

`DATE_FORMAT()` converts a date into formatted text. `STR_TO_DATE()` parses text into a date or datetime value.

Q5. How do you add 7 days to a date?

```
SELECT DATE_ADD('2026-01-05', INTERVAL 7 DAY);
```

Q6. How do you find the last day of a month?

Use `LAST_DAY(date)`.

Q7. How do you extract the month number?

Use `MONTH(date)` or `EXTRACT(MONTH FROM date)`.

Q8. How do you calculate a person's age in completed years from a date of birth?

```
SELECT TIMESTAMPDIFF(
    YEAR,
    '2000-05-15',
    CURDATE()
) AS age;
```

This calculates completed years as of the current date.

Q9. How do you filter records for a particular month?

Use a start-inclusive, next-month-exclusive date range, such as:

```
WHERE order_date >= '2026-01-01'
  AND order_date < '2026-02-01'
```

Q10. What is the difference between `DATEDIFF()` and `TIMESTAMPDIFF()`?

`DATEDIFF()` returns a difference in days. `TIMESTAMPDIFF()` lets you specify a unit such as years, months, days, hours, or minutes.

## 15. Chapter summary

| Function                          | Main use                       |
| --------------------------------- | ------------------------------ |
| `NOW()`, `CURDATE()`, `CURTIME()` | Current date and time          |
| `YEAR()`, `MONTH()`, `DAY()`      | Extract date parts             |
| `DAYNAME()`, `MONTHNAME()`        | Get weekday or month names     |
| `DATE_FORMAT()`                   | Format dates for display       |
| `STR_TO_DATE()`                   | Parse date text                |
| `DATEDIFF()`                      | Difference in days             |
| `TIMESTAMPDIFF()`                 | Difference in a specified unit |
| `DATE_ADD()`, `DATE_SUB()`        | Add or subtract intervals      |
| `LAST_DAY()`                      | Get the end of a month         |
| `QUARTER()`, `EXTRACT()`          | Extract date components        |
