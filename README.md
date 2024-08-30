
# Introduction to SQL

SQL (Structured Query Language) is the standard language for managing and manipulating relational databases. It is widely used for querying, updating, and managing data in databases such as PostgreSQL, MySQL, SQL Server, and others.

## Key SQL Clauses and Commands

### SELECT
Used to specify the columns that you want to retrieve from a database table.

### FROM
Specifies the table from which to retrieve the data.

### AS
Used to assign an alias to a column or table for simplicity or readability.

### DISTINCT
Used to return only distinct (different) values in the specified column.

### Example:
```sql
SELECT DISTINCT name
FROM employees;
```
This query returns unique employee names from the employees table.

## SELECT & FROM

The `SELECT` and `FROM` clauses are used to display data without storing the results. If you want to store the results in a new table, you can use `CREATE VIEW`:

### Example:
```sql
CREATE VIEW view_name AS
SELECT column_name
FROM table_name;
```
This query creates a view (a virtual table) based on the specified columns from the original table.

# Intermediate SQL Concepts

## COUNT()
The `COUNT()` function is used to count the number of rows or records that contain values in a specified column.

### Example:
```sql
SELECT COUNT(column_name) AS alias_name
FROM table_name;
```
This counts the number of rows in the specified column.

## COUNT WITH DISTINCT
To count the number of distinct (unique) values in a column:

### Example:
```sql
SELECT COUNT(DISTINCT column_name)
FROM table_name;
```

## LIMIT
Limits the number of rows returned by a query:

### Example:
```sql
SELECT column_name
FROM table_name
LIMIT 10;
```

## WHERE Condition
Used to filter data based on specified conditions:

### Example:
```sql
SELECT column_name
FROM table_name
WHERE condition;
```

### Important: 
The order of execution is as follows:
1. FROM
2. WHERE
3. SELECT
4. LIMIT

## AND, OR, BETWEEN
Used to add multiple conditions in a query:

### Example:
```sql
SELECT column_name
FROM table_name
WHERE condition1 AND condition2;
```

## FILTERING

### LIKE & NOT LIKE
Used to filter text based on patterns:

- `%` matches zero, one, or many characters.
- `_` matches a single character.

### Example:
```sql
SELECT column_name
FROM table_name
WHERE column_name LIKE 'A%'; -- Finds values that start with 'A'
```

### NULL
Represents empty values. Used in filtering:

### Example:
```sql
SELECT column_name
FROM table_name
WHERE column_name IS NULL;
```

## Aggregate Functions

### Numerical Only
- `AVG()` - calculates the average of a set of values.
- `SUM()` - sums up a set of values.

### Non-Numerical or Numerical
- `MIN()` - finds the minimum value.
- `MAX()` - finds the maximum value.
- `COUNT()` - counts the number of values.

### Example:
```sql
SELECT AVG(salary)
FROM employees;
```

### ROUND()
Used to round numbers to a specified number of decimal places.

### Example:
```sql
SELECT ROUND(123.56655, 2);
-- Result: 123.57
```

### Example with Negative Rounding:
```sql
SELECT ROUND(123456, -3);
-- Result: 123000
```

## Sorting Data

### ORDER BY
Sorts the results by the specified column in ascending (ASC) or descending (DESC) order.

### Example:
```sql
SELECT column_name
FROM table_name
ORDER BY column_name ASC;
```

# GROUPING DATA

## GROUP BY
Used to group data by a specific column. For example, grouping people by the certification they have obtained.

### Example:
```sql
SELECT certificat, COUNT(name) AS numOfName
FROM table_name
GROUP BY certificat;
```

**Result:**
| certificat | numOfName |
|------------|-----------|
| cnna       | 3         |
| sql        | 2         |
| git        | 1         |

# HAVING Clause

The `HAVING` clause is used to filter grouped data after aggregation. It is similar to `WHERE` but used after `GROUP BY`.

### Example:
```sql
SELECT certificat, COUNT(name) AS numOfName
FROM table_name
GROUP BY certificat
HAVING COUNT(name) > 1;
```

**Result:**
| certificat | numOfName |
|------------|-----------|
| cnna       | 3         |
| sql        | 2         |



# SQL Joins:

## 1. INNER JOIN
**Description:** Selects only the matching rows from both tables based on the specified condition.

**Example:**
```sql
SELECT customers.customer_id, orders.order_id
FROM customers
INNER JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Resulting Table:**
| customer_id | order_id |
|-------------|----------|
| 1           | 101      |
| 2           | 102      |

## 2. USING Clause
**Description:** When joining on two identical column names, we can use the `USING` command followed by the shared column name.

**Example:**
```sql
SELECT customers.customer_id, orders.order_id
FROM customers
INNER JOIN orders USING (customer_id);
```

**Resulting Table:**
| customer_id | order_id |
|-------------|----------|
| 1           | 101      |
| 2           | 102      |

## 3. LEFT JOIN
**Description:** Returns all rows from the left table and the matching rows from the right table. The result is `NULL` from the right side if there is no match.

**Example:**
```sql
SELECT customers.customer_id, orders.order_id
FROM customers
LEFT JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Resulting Table:**
| customer_id | order_id |
|-------------|----------|
| 1           | 101      |
| 2           | 102      |
| 3           | NULL     |

## 4. RIGHT JOIN
**Description:** Returns all rows from the right table and the matching rows from the left table. The result is `NULL` from the left side if there is no match.

**Example:**
```sql
SELECT customers.customer_id, orders.order_id
FROM customers
RIGHT JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Resulting Table:**
| customer_id | order_id |
|-------------|----------|
| 1           | 101      |
| 2           | 102      |
| NULL        | 103      |

## 5. FULL JOIN
**Description:** Returns all rows from both tables. It returns `NULL` on the side where there is no match.

**Example:**
```sql
SELECT customers.customer_id, orders.order_id
FROM customers
FULL JOIN orders
ON customers.customer_id = orders.customer_id;
```

**Resulting Table:**
| customer_id | order_id |
|-------------|----------|
| 1           | 101      |
| 2           | 102      |
| 3           | NULL     |
| NULL        | 103      |

## 6. CROSS JOIN
**Description:** Returns the Cartesian product of the two tables. All possible combinations of rows from both tables are included.

**Example:**
```sql
SELECT customers.customer_id, orders.order_id
FROM customers
CROSS JOIN orders;
```

**Resulting Table:**
| customer_id | order_id |
|-------------|----------|
| 1           | 101      |
| 1           | 102      |
| 1           | 103      |
| 2           | 101      |
| 2           | 102      |
| 2           | 103      |

## 7. SELF JOIN
**Description:** A join where a table is joined with itself to combine rows with other rows from the same table.

**Example:**
```sql
SELECT A.employee_id, B.manager_id
FROM employees A
INNER JOIN employees B
ON A.manager_id = B.employee_id;
```

**Resulting Table:**
| employee_id | manager_id |
|-------------|------------|
| 2           | 1          |
| 3           | 1          |
| 4           | 2          |

-----------------------
