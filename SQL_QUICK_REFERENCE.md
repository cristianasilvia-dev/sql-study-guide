# SQL Quick Reference Cheat Sheet

Quick syntax reference for common SQL operations. For detailed explanations, see the main study guide.

## Table of Contents
1. [Basic SELECT](#basic-select)
2. [Data Types](#data-types)
3. [Creating Tables](#creating-tables)
4. [Inserting Data](#inserting-data)
5. [Updating Data](#updating-data)
6. [Deleting Data](#deleting-data)
7. [SELECT Queries](#select-queries)
8. [Operators](#operators)
9. [Functions](#functions)
10. [Joins](#joins)
11. [Aggregation](#aggregation)
12. [Subqueries](#subqueries)
13. [Views](#views)
14. [Transactions](#transactions)
15. [Users & Security](#users--security)

---

## Basic SELECT

```sql
-- Simple SELECT
SELECT * FROM table_name;
SELECT column1, column2 FROM table_name;

-- With WHERE
SELECT * FROM table_name WHERE condition;

-- With ORDER BY
SELECT * FROM table_name ORDER BY column1 ASC, column2 DESC;

-- With LIMIT
SELECT * FROM table_name LIMIT 10;

-- DISTINCT values
SELECT DISTINCT column1 FROM table_name;
```

---

## Data Types

### Character Types
```sql
CHAR(n)           -- Fixed-length character string
VARCHAR(n)        -- Variable-length character string
TEXT              -- Large text (no length limit in most databases)
NVARCHAR(n)       -- Unicode characters
```

### Numeric Types
```sql
INT, INTEGER      -- Integer numbers
SMALLINT          -- Small integer
BIGINT            -- Large integer
DECIMAL(p,s)      -- Exact decimal (p=precision, s=scale)
NUMERIC(p,s)      -- Same as DECIMAL
FLOAT, DOUBLE     -- Floating-point numbers
REAL              -- Single precision float
```

### Date/Time Types
```sql
DATE              -- Date only (YYYY-MM-DD)
TIME              -- Time only (HH:MM:SS)
TIMESTAMP         -- Date and time
DATETIME          -- Date and time combined
INTERVAL          -- Time interval
```

### Other Types
```sql
BOOLEAN, BOOL     -- True/False values
BLOB              -- Binary large object
CLOB              -- Character large object
JSON              -- JSON data (MySQL 5.7+, PostgreSQL)
UUID              -- Universally unique identifier
```

---

## Creating Tables

```sql
-- Basic CREATE TABLE
CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10, 2),
    hire_date DATE DEFAULT CURRENT_DATE,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(dept_id)
);

-- Add constraints after creation
ALTER TABLE employees
ADD CONSTRAINT chk_salary CHECK (salary > 0);

-- Add column
ALTER TABLE employees ADD COLUMN phone VARCHAR(15);

-- Modify column
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12, 2);

-- Drop column
ALTER TABLE employees DROP COLUMN phone;

-- Drop table
DROP TABLE employees;
DROP TABLE IF EXISTS employees;
```

### Constraints

```sql
PRIMARY KEY       -- Uniquely identifies each record
FOREIGN KEY       -- References another table
UNIQUE            -- All values in column are unique
NOT NULL          -- Column must have a value
CHECK (condition) -- Column must meet condition
DEFAULT value     -- Default value for new records
```

---

## Inserting Data

```sql
-- Single row insert
INSERT INTO employees (first_name, last_name, email)
VALUES ('John', 'Doe', 'john@example.com');

-- Multiple rows insert
INSERT INTO employees (first_name, last_name, email)
VALUES 
    ('Jane', 'Smith', 'jane@example.com'),
    ('Bob', 'Johnson', 'bob@example.com');

-- Insert from another table
INSERT INTO employees_backup
SELECT * FROM employees WHERE status = 'Inactive';
```

---

## Updating Data

```sql
-- Update all rows
UPDATE employees SET salary = salary * 1.10;

-- Update with WHERE
UPDATE employees
SET salary = 60000
WHERE emp_id = 5;

-- Update multiple columns
UPDATE employees
SET salary = 65000, status = 'Senior'
WHERE department_id = 3;

-- Update with JOIN
UPDATE employees e
SET e.salary = e.salary * 1.05
WHERE e.department_id IN (
    SELECT dept_id FROM departments WHERE region = 'North'
);
```

---

## Deleting Data

```sql
-- Delete all rows (be careful!)
DELETE FROM employees;

-- Delete with WHERE
DELETE FROM employees WHERE emp_id = 5;

-- Delete with complex condition
DELETE FROM employees
WHERE hire_date < '2015-01-01' AND status = 'Inactive';

-- DELETE vs TRUNCATE
DELETE FROM employees;      -- Slower, triggers work, can rollback
TRUNCATE TABLE employees;   -- Faster, resets identity, logs minimally
```

---

## SELECT Queries

```sql
-- Basic structure
SELECT column1, column2
FROM table_name
WHERE condition
GROUP BY column1
HAVING group_condition
ORDER BY column1
LIMIT 10;

-- SELECT with alias
SELECT emp_id AS employee_id, 
       CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;

-- SELECT with calculated column
SELECT emp_id, salary,
       salary * 0.10 AS bonus,
       salary * 1.10 AS new_salary
FROM employees;

-- SELECT with CASE
SELECT emp_id, salary,
    CASE 
        WHEN salary > 100000 THEN 'Executive'
        WHEN salary > 50000 THEN 'Senior'
        ELSE 'Junior'
    END AS level
FROM employees;
```

---

## Operators

### Comparison Operators
```sql
= Equal
<> or != Not equal
< Less than
> Greater than
<= Less than or equal
>= Greater than or equal
```

### Logical Operators
```sql
AND     Both conditions must be true
OR      At least one condition must be true
NOT     Reverses the condition

-- Examples
WHERE salary > 50000 AND department = 'Sales'
WHERE status = 'Active' OR status = 'Pending'
WHERE NOT department = 'HR'
```

### Other Operators
```sql
IN (list)              -- Value in list
NOT IN (list)          -- Value not in list
BETWEEN value1 AND value2  -- Range
NOT BETWEEN            -- Not in range
LIKE pattern           -- Pattern matching
NOT LIKE               -- Not matching pattern
IS NULL               -- Missing value
IS NOT NULL           -- Has value
EXISTS (subquery)     -- Subquery returns rows
NOT EXISTS            -- Subquery returns no rows
```

### LIKE Patterns
```sql
%     -- Any sequence of characters
_     -- Single character

-- Examples
WHERE email LIKE '%@example.com'        -- Ends with
WHERE name LIKE 'John%'                 -- Starts with
WHERE phone LIKE '555-____'             -- Pattern matching
WHERE address LIKE '%Street%'           -- Contains
```

---

## Functions

### Character Functions
```sql
UPPER(string)           -- Convert to uppercase
LOWER(string)           -- Convert to lowercase
LENGTH(string)          -- String length
SUBSTRING(string, pos, len) -- Extract substring
TRIM(string)            -- Remove leading/trailing spaces
LTRIM(string)           -- Remove leading spaces
RTRIM(string)           -- Remove trailing spaces
REPLACE(string, from, to)   -- Replace text
CONCAT(str1, str2, ...)     -- Concatenate strings
CHAR_LENGTH(string)     -- Character count

-- Examples
SELECT UPPER(name) FROM employees;
SELECT SUBSTRING(email, 1, INSTR(email, '@')-1) AS username;
SELECT TRIM(BOTH ' ' FROM name) FROM employees;
```

### Numeric Functions
```sql
ABS(number)             -- Absolute value
ROUND(number, decimals) -- Round to decimals
CEIL(number)            -- Round up
FLOOR(number)           -- Round down
SQRT(number)            -- Square root
POWER(number, power)    -- Raise to power
MOD(number1, number2)   -- Remainder

-- Examples
SELECT ROUND(salary, 2) FROM employees;
SELECT ABS(-100) AS absolute;
```

### Date Functions
```sql
CURRENT_DATE            -- Today's date
CURRENT_TIME            -- Current time
NOW()                   -- Current date and time
DATE_ADD(date, INTERVAL value unit)  -- Add to date
DATE_SUB(date, INTERVAL value unit)  -- Subtract from date
DATEDIFF(date1, date2)  -- Days between dates
DATEPART(part, date)    -- Extract part of date
YEAR(date)              -- Year from date
MONTH(date)             -- Month from date
DAY(date)               -- Day from date
DAYNAME(date)           -- Day name (Monday, etc.)
MONTHNAME(date)         -- Month name (January, etc.)

-- Examples
SELECT DATE_ADD(hire_date, INTERVAL 1 YEAR) FROM employees;
SELECT DATEDIFF(CURRENT_DATE, hire_date) AS days_employed;
SELECT YEAR(order_date) FROM orders;
```

### Aggregate Functions
```sql
COUNT(*)                -- Count rows
COUNT(column)           -- Count non-null values
SUM(number)             -- Sum values
AVG(number)             -- Average value
MIN(value)              -- Minimum value
MAX(value)              -- Maximum value
STDDEV(number)          -- Standard deviation
VARIANCE(number)        -- Variance

-- Examples
SELECT COUNT(*) FROM employees;
SELECT AVG(salary) FROM employees WHERE department = 'Sales';
SELECT MAX(hire_date) FROM employees;
```

---

## Joins

```sql
-- INNER JOIN (matching rows only)
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- LEFT OUTER JOIN (all from left table)
SELECT e.name, d.dept_name
FROM employees e
LEFT OUTER JOIN departments d ON e.dept_id = d.dept_id;

-- RIGHT OUTER JOIN (all from right table)
SELECT e.name, d.dept_name
FROM employees e
RIGHT OUTER JOIN departments d ON e.dept_id = d.dept_id;

-- FULL OUTER JOIN (all from both tables)
SELECT e.name, d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.dept_id;

-- CROSS JOIN (Cartesian product)
SELECT e.name, d.dept_name
FROM employees e
CROSS JOIN departments d;

-- SELF JOIN
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.emp_id;

-- Multiple JOINs
SELECT e.name, d.dept_name, l.location
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id
INNER JOIN locations l ON d.location_id = l.location_id;
```

---

## Aggregation

```sql
-- GROUP BY basics
SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department;

-- GROUP BY with aggregate functions
SELECT department, 
       COUNT(*) AS emp_count,
       AVG(salary) AS avg_salary,
       MIN(salary) AS min_salary,
       MAX(salary) AS max_salary
FROM employees
GROUP BY department;

-- GROUP BY multiple columns
SELECT department, job_title, COUNT(*) AS count
FROM employees
GROUP BY department, job_title;

-- HAVING clause (filter on groups)
SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;

-- Complex HAVING
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000 AND COUNT(*) >= 3;

-- WITH ROLLUP (subtotals)
SELECT department, job_title, COUNT(*) as count
FROM employees
GROUP BY department, job_title WITH ROLLUP;
```

---

## Subqueries

```sql
-- Subquery in WHERE
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Subquery with IN
SELECT name FROM employees
WHERE department_id IN 
    (SELECT dept_id FROM departments WHERE region = 'North');

-- Subquery with EXISTS
SELECT e.name FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.employee_id = e.employee_id
);

-- Subquery in FROM (derived table)
SELECT dept.name, avg_sal.avg_salary
FROM departments dept
JOIN (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
) avg_sal ON dept.dept_id = avg_sal.department_id;

-- Correlated subquery
SELECT e1.name, e1.salary
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id
);
```

---

## Compound Queries

```sql
-- UNION (removes duplicates)
SELECT name FROM employees
UNION
SELECT name FROM contractors;

-- UNION ALL (keeps duplicates)
SELECT order_id FROM 2023_orders
UNION ALL
SELECT order_id FROM 2024_orders;

-- INTERSECT (common rows)
SELECT customer_id FROM orders
INTERSECT
SELECT customer_id FROM returns;

-- EXCEPT / MINUS (in first but not second)
SELECT product_id FROM inventory
EXCEPT
SELECT product_id FROM discontinued_items;
```

---

## Views

```sql
-- CREATE VIEW
CREATE VIEW employee_summary AS
SELECT emp_id, name, email, department
FROM employees
WHERE status = 'Active';

-- CREATE OR REPLACE VIEW
CREATE OR REPLACE VIEW employee_summary AS
SELECT emp_id, name, email, department
FROM employees
WHERE status = 'Active';

-- Use a view
SELECT * FROM employee_summary;
SELECT * FROM employee_summary WHERE department = 'Sales';

-- CREATE VIEW with JOIN
CREATE VIEW department_salaries AS
SELECT 
    d.name as department,
    COUNT(e.emp_id) as emp_count,
    AVG(e.salary) as avg_salary
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.name;

-- DROP VIEW
DROP VIEW employee_summary;
DROP VIEW IF EXISTS employee_summary;

-- CREATE SYNONYM (Oracle, SQL Server)
CREATE SYNONYM emp FOR employees;
SELECT * FROM emp;
```

---

## Transactions

```sql
-- Basic transaction
BEGIN TRANSACTION;
UPDATE employees SET salary = 60000 WHERE emp_id = 5;
INSERT INTO salary_history (emp_id, old_salary, new_salary) 
    VALUES (5, 50000, 60000);
COMMIT;

-- With ROLLBACK
BEGIN TRANSACTION;
DELETE FROM orders WHERE order_date < '2020-01-01';
-- If something goes wrong:
ROLLBACK;
-- If everything is ok:
COMMIT;

-- SAVEPOINT (rollback to specific point)
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
SAVEPOINT before_transfer;

UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
-- If transfer fails:
ROLLBACK TO before_transfer;

COMMIT;

-- Transaction isolation levels
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

---

## Users & Security

```sql
-- CREATE USER (MySQL)
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';

-- GRANT privileges
GRANT SELECT ON database.* TO 'username'@'localhost';
GRANT SELECT, INSERT, UPDATE ON database.table TO 'user'@'host';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;

-- REVOKE privileges
REVOKE SELECT ON database.table FROM 'user'@'host';

-- VIEW user privileges
SHOW GRANTS FOR 'username'@'localhost';

-- CREATE ROLE (MySQL 8.0+, Oracle, PostgreSQL)
CREATE ROLE analyst;
CREATE ROLE manager;

-- GRANT privileges to ROLE
GRANT SELECT ON sales.* TO analyst;
GRANT SELECT, INSERT, UPDATE ON sales.* TO manager;

-- ASSIGN role to user
GRANT analyst TO 'jane'@'localhost';

-- Change password
ALTER USER 'username'@'localhost' IDENTIFIED BY 'newpassword';

-- Drop user
DROP USER 'username'@'localhost';

-- ALTER user password (PostgreSQL)
ALTER USER username WITH PASSWORD 'newpassword';
```

---

## Common Table Expressions (CTE)

```sql
-- Basic CTE
WITH employee_summary AS (
    SELECT department, COUNT(*) as count, AVG(salary) as avg_sal
    FROM employees
    GROUP BY department
)
SELECT * FROM employee_summary WHERE count > 5;

-- Multiple CTEs
WITH
dept_summary AS (
    SELECT dept_id, COUNT(*) as emp_count
    FROM employees
    GROUP BY dept_id
),
high_salary AS (
    SELECT emp_id, salary
    FROM employees
    WHERE salary > 100000
)
SELECT d.dept_id, d.emp_count, COUNT(h.emp_id) as high_earners
FROM dept_summary d
LEFT JOIN high_salary h ON d.dept_id = (SELECT dept_id FROM employees WHERE emp_id = h.emp_id)
GROUP BY d.dept_id, d.emp_count;

-- Recursive CTE
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 10
)
SELECT * FROM numbers;
```

---

## Window Functions (Introduction)

```sql
-- ROW_NUMBER
SELECT 
    name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- PARTITION BY with aggregate
SELECT 
    name,
    department,
    salary,
    AVG(salary) OVER (PARTITION BY department) as dept_avg
FROM employees;

-- RANK vs DENSE_RANK
SELECT 
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) as dense_rank
FROM employees;

-- LAG and LEAD (previous/next row)
SELECT 
    order_id,
    order_date,
    amount,
    LAG(amount) OVER (ORDER BY order_date) as previous_amount,
    LEAD(amount) OVER (ORDER BY order_date) as next_amount
FROM orders;
```

---

## Quick Tips

✅ **Always use WHERE before GROUP BY** to filter rows before grouping  
✅ **Use INNER JOIN by default** unless you specifically need outer joins  
✅ **CAST values** when comparing different data types: `CAST(column AS INT)`  
✅ **Escape special characters** in LIKE: `LIKE '%\_%'` (escapes underscore)  
✅ **Use backticks** for reserved keywords: `` `order` ``  
✅ **Test with LIMIT** before running large queries: `LIMIT 10`  
✅ **Always backup before DELETE/UPDATE** on production  
✅ **Use transactions** for related changes  
✅ **Document your queries** with comments  
✅ **Review EXPLAIN plans** for optimization  

---

**Quick Reference Version:** 1.0  
**Last Updated:** 2026-05-14  

For detailed explanations, refer to **SQL_Study_Guide.md**
