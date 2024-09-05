
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

###  USING Clause
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

## 2. LEFT JOIN
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

## 3. RIGHT JOIN
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

## 4. FULL JOIN
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

## 5. CROSS JOIN
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

## 6. SELF JOIN
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

# SQL Set Operations: UNION, UNION ALL, INTERSECT, and EXCEPT

## 1. UNION
- **Description**: Combines the result sets of two or more queries and removes duplicate rows.
- **Usage**: When you want to merge the results from multiple queries while excluding duplicates.

### Example
Consider two tables, `Employees` and `Managers`:

| EmployeeID | FirstName | LastName | DepartmentID |
|------------|-----------|----------|--------------|
| 1          | John      | Doe      | 10           |
| 2          | Jane      | Smith    | 20           |
| 3          | Jim       | Brown    | 30           |
| 4          | Jake      | White    | 40           |

| ManagerID  | FirstName | LastName | DepartmentID |
|------------|-----------|----------|--------------|
| 1          | John      | Doe      | 10           |
| 2          | Jane      | Smith    | 20           |
| 5          | Susan     | Green    | 50           |
| 6          | Tom       | Black    | 60           |

### SQL Query
```sql
SELECT FirstName, LastName FROM Employees
UNION
SELECT FirstName, LastName FROM Managers;
```

*Result*: The output would be a list of unique `FirstName` and `LastName` values from both tables:

| FirstName | LastName |
|-----------|----------|
| John      | Doe      |
| Jane      | Smith    |
| Jim       | Brown    |
| Jake      | White    |
| Susan     | Green    |
| Tom       | Black    |


## 2. UNION ALL
- **Description**: Combines the result sets of two or more queries, including all duplicate rows.
- **Usage**: When you want to merge results from multiple queries and include all rows, even duplicates.

### SQL Query
```sql
SELECT FirstName, LastName FROM Employees
UNION ALL
SELECT FirstName, LastName FROM Managers;
```

*Result*: The output would be a list of all `FirstName` and `LastName` values, including duplicates:

| FirstName | LastName |
|-----------|----------|
| John      | Doe      |
| Jane      | Smith    |
| Jim       | Brown    |
| Jake      | White    |
| John      | Doe      |
| Jane      | Smith    |
| Susan     | Green    |
| Tom       | Black    |


## 3. INTERSECT
- **Description**: Returns only the rows that are common between the result sets of two queries.
- **Usage**: When you want to find common rows between multiple queries.

### SQL Query
```sql
SELECT FirstName, LastName FROM Employees
INTERSECT
SELECT FirstName, LastName FROM Managers;
```

*Result*: The output would be a list of `FirstName` and `LastName` values that appear in both tables:

| FirstName | LastName |
|-----------|----------|
| John      | Doe      |
| Jane      | Smith    |


## 4. EXCEPT
- **Description**: Returns rows from the first query that do not appear in the result set of the second query.
- **Usage**: When you want to find rows that exist in one query but not in the other.

### SQL Query
```sql
SELECT FirstName, LastName FROM Employees
EXCEPT
SELECT FirstName, LastName FROM Managers;
```

*Result*: The output would be a list of `FirstName` and `LastName` values from the `Employees` table that are not in the `Managers` table:

| FirstName | LastName |
|-----------|----------|
| Jim       | Brown    |
| Jake      | White    |


## Comparison: INTERSECT vs. INNER JOIN
- **INTERSECT**: Returns only the common values between two queries based on the selected columns.
- **INNER JOIN**: Joins two tables based on a specific condition, allowing you to return additional columns that are not part of the intersection.

### Example of INNER JOIN
```sql
SELECT Employees.FirstName, Employees.LastName
FROM Employees
INNER JOIN Managers ON Employees.FirstName = Managers.FirstName AND Employees.LastName = Managers.LastName;
```

*Result*: This would return similar results as `INTERSECT`, but could also include additional columns from either table:

| FirstName | LastName |
|-----------|----------|
| John      | Doe      |
| Jane      | Smith    |


# SQL Subqueries and Joins:

## 1. Subquerying with Semi-Joins and Anti-Joins
- **Semi-Joins**: Returns rows from the first table where one or more matches are found in the second table.
  
  **Example**: Find all employees who have managed projects.
  ```sql
  SELECT DISTINCT EmployeeID
  FROM Employees e
  WHERE EXISTS (
      SELECT 1 
      FROM Projects p 
      WHERE p.ManagerID = e.EmployeeID
  );
  ```

- **Anti-Joins**: Returns rows from the first table where no matches are found in the second table.

  **Example**: Find all employees who have not managed any projects.
  ```sql
  SELECT DISTINCT EmployeeID
  FROM Employees e
  WHERE NOT EXISTS (
      SELECT 1 
      FROM Projects p 
      WHERE p.ManagerID = e.EmployeeID
  );
  ```

## 2. Multiple WHERE Clauses
- SQL allows multiple conditions in the `WHERE` clause using logical operators like `AND`, `OR`, and `NOT`.
  
  **Example**: Find employees who work in the "Sales" department and have a salary greater than $50,000.
  ```sql
  SELECT *
  FROM Employees
  WHERE Department = 'Sales' 
  AND Salary > 50000;
  ```

## 3. Semi-Join
- A semi-join checks for the existence of a relationship between two tables, returning rows from the first table where a corresponding match is found in the second table.

  **Example**: List all departments that have employees.
  ```sql
  SELECT DepartmentName
  FROM Departments d
  WHERE EXISTS (
      SELECT 1 
      FROM Employees e 
      WHERE e.DepartmentID = d.DepartmentID
  );
  ```

## 4. Diagnosing Problems Using Anti-Joins
- Anti-joins are useful in diagnosing issues like finding records in one table that do not have corresponding records in another.

  **Example**: Find all orders that have not been shipped.
  ```sql
  SELECT OrderID
  FROM Orders o
  WHERE NOT EXISTS (
      SELECT 1 
      FROM Shipments s 
      WHERE s.OrderID = o.OrderID
  );
  ```

## 5. Subqueries Inside WHERE and SELECT
- Subqueries within the `WHERE` clause filter results based on another query’s output.

  **Example in WHERE**: List employees whose salary is greater than the average salary in their department.
  ```sql
  SELECT EmployeeID, FirstName, LastName
  FROM Employees e
  WHERE Salary > (
      SELECT AVG(Salary)
      FROM Employees 
      WHERE DepartmentID = e.DepartmentID
  );
  ```

- Subqueries in the `SELECT` clause allow calculating additional information related to each row.

  **Example in SELECT**: Show the total number of employees in each department.
  ```sql
  SELECT DepartmentName, 
         (SELECT COUNT(*) 
          FROM Employees e 
          WHERE e.DepartmentID = d.DepartmentID) AS EmployeeCount
  FROM Departments d;
  ```

## 6. Subquery Inside WHERE
- A subquery within the `WHERE` clause enables dynamic filtering based on another query's result.

  **Example**: Retrieve all employees who have the highest salary in their department.
  ```sql
  SELECT EmployeeID, FirstName, LastName
  FROM Employees e
  WHERE Salary = (
      SELECT MAX(Salary)
      FROM Employees 
      WHERE DepartmentID = e.DepartmentID
  );
  ```

## 7. Subquery Inside SELECT
- A subquery in the `SELECT` clause calculates additional metrics related to the main query.

  **Example**: Display each employee’s name along with the number of projects they have managed.
  ```sql
  SELECT FirstName, LastName, 
         (SELECT COUNT(*) 
          FROM Projects p 
          WHERE p.ManagerID = e.EmployeeID) AS ProjectCount
  FROM Employees e;
  ```

## 8. Subquery Inside FROM
- A subquery in the `FROM` clause acts like a temporary table that can be joined with other tables.

  **Example**: Find the average salary by department and compare it against the company’s average salary.
  ```sql
  SELECT DepartmentID, AVG(Salary) AS AvgDeptSalary
  FROM (SELECT DepartmentID, Salary 
        FROM Employees) AS DeptSalaries
  GROUP BY DepartmentID;
  ```

## 9. Subqueries Inside FROM
- Combining multiple subqueries in the `FROM` clause for complex operations.

  **Example**: Combine average salary calculations with other metrics in the same query.
  ```sql
  SELECT d.DepartmentName, ds.AvgDeptSalary, COUNT(e.EmployeeID) AS EmployeeCount
  FROM Departments d
  JOIN (SELECT DepartmentID, AVG(Salary) AS AvgDeptSalary
        FROM Employees
        GROUP BY DepartmentID) ds ON d.DepartmentID = ds.DepartmentID
  JOIN Employees e ON e.DepartmentID = d.DepartmentID
  GROUP BY d.DepartmentName, ds.AvgDeptSalary;
  ```
# Examples
  ### 1. Subqueries with Semi-Joins and Anti-Joins

#### Semi-Joins

**Example:**

We have two tables: "Employees" and "Projects". We want to extract employees who managed projects.

**Employees Table:**

| EmployeeID | Name    | DepartmentID |
|------------|---------|--------------|
| 1          | Alice   | 10           |
| 2          | Bob     | 20           |
| 3          | Charlie | 10           |
| 4          | Dave    | 30           |

**Projects Table:**

| ProjectID | Name      | ManagerID |
|-----------|-----------|-----------|
| 101       | Project A | 1         |
| 102       | Project B | 3         |

**Query:**

```sql
SELECT DISTINCT EmployeeID, Name
FROM Employees e
WHERE EXISTS (
    SELECT 1 
    FROM Projects p 
    WHERE p.ManagerID = e.EmployeeID
);
```

**Result:**

| EmployeeID | Name    |
|------------|---------|
| 1          | Alice   |
| 3          | Charlie |

#### Anti-Joins

**Example:**

We want to extract employees who did not manage any projects.

**Query:**

```sql
SELECT DISTINCT EmployeeID, Name
FROM Employees e
WHERE NOT EXISTS (
    SELECT 1 
    FROM Projects p 
    WHERE p.ManagerID = e.EmployeeID
);
```

**Result:**

| EmployeeID | Name |
|------------|------|
| 2          | Bob  |
| 4          | Dave |

### 2. Using Multiple WHERE Conditions

**Example:**

We have an "Employees" table and want to extract employees who work in the "Sales" department and have a salary greater than 50,000.

**Employees Table:**

| EmployeeID | Name    | Department | Salary |
|------------|---------|------------|--------|
| 1          | Alice   | Sales      | 60000  |
| 2          | Bob     | HR         | 45000  |
| 3          | Charlie | Sales      | 55000  |
| 4          | Dave    | Sales      | 48000  |

**Query:**

```sql
SELECT *
FROM Employees
WHERE Department = 'Sales' 
AND Salary > 50000;
```

**Result:**

| EmployeeID | Name    | Department | Salary |
|------------|---------|------------|--------|
| 1          | Alice   | Sales      | 60000  |
| 3          | Charlie | Sales      | 55000  |

### 3. Semi-Join

**Example:**

We want to extract all departments that have employees.

**Departments Table:**

| DepartmentID | DepartmentName |
|--------------|----------------|
| 10           | Sales          |
| 20           | HR             |
| 30           | IT             |

**Employees Table:**

| EmployeeID | Name    | DepartmentID |
|------------|---------|--------------|
| 1          | Alice   | 10           |
| 2          | Bob     | 20           |
| 3          | Charlie | 10           |

**Query:**

```sql
SELECT DepartmentName
FROM Departments d
WHERE EXISTS (
    SELECT 1 
    FROM Employees e 
    WHERE e.DepartmentID = d.DepartmentID
);
```

**Result:**

| DepartmentName |
|----------------|
| Sales          |
| HR             |

### 4. Diagnosing Issues Using Anti-Joins

**Example:**

We have an "Orders" table and a "Shipments" table. We want to extract orders that have not been shipped yet.

**Orders Table:**

| OrderID | CustomerID | OrderDate  |
|---------|------------|------------|
| 1       | 101        | 2024-08-01 |
| 2       | 102        | 2024-08-02 |
| 3       | 103        | 2024-08-03 |

**Shipments Table:**

| ShipmentID | OrderID | ShipmentDate |
|------------|---------|--------------|
| 201        | 1       | 2024-08-05   |
| 202        | 2       | 2024-08-06   |

**Query:**

```sql
SELECT OrderID
FROM Orders o
WHERE NOT EXISTS (
    SELECT 1 
    FROM Shipments s 
    WHERE s.OrderID = o.OrderID
);
```

**Result:**

| OrderID |
|---------|
| 3       |

### 5. Subqueries Inside WHERE and SELECT

#### Example Inside WHERE

**Example:**

We want to extract employees whose salary is higher than the average salary in their department.

**Employees Table:**

| EmployeeID | Name    | DepartmentID | Salary |
|------------|---------|--------------|--------|
| 1          | Alice   | 10           | 60000  |
| 2          | Bob     | 20           | 50000  |
| 3          | Charlie | 10           | 55000  |
| 4          | Dave    | 20           | 47000  |

**Query:**

```sql
SELECT EmployeeID, Name
FROM Employees e
WHERE Salary > (
    SELECT AVG(Salary)
    FROM Employees 
    WHERE DepartmentID = e.DepartmentID
);
```

**Result:**

| EmployeeID | Name   |
|------------|--------|
| 1          | Alice  |
| 2          | Bob    |

#### Example Inside SELECT

**Example:**

We want to display the number of employees in each department.

**Departments Table:**

| DepartmentID | DepartmentName |
|--------------|----------------|
| 10           | Sales          |
| 20           | HR             |

**Query:**

```sql
SELECT DepartmentName, 
       (SELECT COUNT(*) 
        FROM Employees e 
        WHERE e.DepartmentID = d.DepartmentID) AS EmployeeCount
FROM Departments d;
```

**Result:**

| DepartmentName | EmployeeCount |
|----------------|---------------|
| Sales          | 2             |
| HR             | 2             |

### 6. Subqueries Inside WHERE

**Example:**

We want to extract all employees who have the highest salary in their department.

**Employees Table:**

| EmployeeID | Name    | DepartmentID | Salary |
|------------|---------|--------------|--------|
| 1          | Alice   | 10           | 60000  |
| 2          | Bob     | 20           | 50000  |
| 3          | Charlie | 10           | 55000  |
| 4          | Dave    | 20           | 47000  |

**Query:**

```sql
SELECT EmployeeID, Name
FROM Employees e
WHERE Salary = (
    SELECT MAX(Salary)
    FROM Employees 
    WHERE DepartmentID = e.DepartmentID
);
```

**Result:**

| EmployeeID | Name   |
|------------|--------|
| 1          | Alice  |
| 2          | Bob    |

### 7. Subqueries Inside SELECT

**Example:**

We want to display each employee's name with the number of projects they managed.

**Employees Table:**

| EmployeeID | Name    | DepartmentID |
|------------|---------|--------------|
| 1          | Alice   | 10           |
| 2          | Bob     | 20           |
| 3          | Charlie | 10           |
| 4          | Dave    | 20           |

**Projects Table:**

| ProjectID | Name      | ManagerID |
|-----------|-----------|-----------|
| 101       | Project A | 1         |
| 102       | Project B | 3         |

**Query:**

```sql
SELECT Name, 
       (SELECT COUNT(*) 
        FROM Projects p 
        WHERE p.ManagerID = e.EmployeeID) AS ProjectCount
FROM Employees e;
```

**Result:**

| Name    | ProjectCount |
|---------|--------------|
| Alice   | 1            |
| Bob     | 0            |
| Charlie | 1            |
| Dave    | 0            |

### 8. Subqueries Inside FROM

**Example:**

We want to extract the average salary for each department.

**Employees Table:**

| EmployeeID | Name    | DepartmentID | Salary |
|------------|---------|--------------|--------|
| 1          | Alice   | 10           | 60000  |
| 2          | Bob     | 20           | 50000  |
| 3          | Charlie | 10           | 55000  |
| 4          | Dave    | 20           | 47000  |

**Query:**

```sql
SELECT DepartmentID, AVG(Salary) AS AvgDeptSalary
FROM (SELECT DepartmentID, Salary 
      FROM Employees) AS DeptSalaries
GROUP BY DepartmentID;
```

**Result:**

| DepartmentID | AvgDeptSalary |
|--------------|---------------|
| 10           | 57500         |
| 20           | 48500         |

### 9. Subqueries Inside FROM

**Example:**

We want to extract the department with the highest average salary.

**Query:**

```sql
SELECT DepartmentID
FROM (SELECT DepartmentID, AVG(Salary) AS AvgSalary
      FROM Employees
      GROUP BY DepartmentID) AS DeptAvgSalaries
ORDER BY AvgSalary DESC
LIMIT 1;
```

**Result:**

| DepartmentID |
|--------------|
| 10           |

---
# Data Manipulation in SQL

## 1. **CASE Statements**

The `CASE` statement in SQL allows you to perform conditional logic within your queries, similar to an "if-else" statement in programming languages. It is often used to create new calculated fields or to modify existing values based on certain conditions.

#### **Syntax and Usage**

```sql
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ELSE result3
END AS new_column_name
```

You can also use `CASE` statements inside aggregate functions like `AVG()`, `SUM()`, etc., to calculate conditional aggregates.

#### **Example Usage with a Table**

Suppose you have a `Sales` table:

| OrderID | Customer | Product  | Quantity | Price |
|---------|----------|----------|----------|-------|
| 1       | Alice    | Laptop   | 2        | 1000  |
| 2       | Bob      | Smartphone| 5       | 300   |
| 3       | Alice    | Tablet   | 1        | 400   |
| 4       | John     | Laptop   | 1        | 950   |

You want to create a new column that shows the "Order Type" based on the product:

```sql
SELECT 
  OrderID,
  Customer,
  Product,
  Quantity,
  Price,
  CASE 
    WHEN Product = 'Laptop' THEN 'High Value'
    WHEN Product = 'Smartphone' THEN 'Medium Value'
    ELSE 'Low Value'
  END AS OrderType
FROM Sales;
```

#### **Conditional Aggregation Example**

If you want to find the average price for only the "Laptop" orders:

```sql
SELECT 
  AVG(
    CASE 
      WHEN Product = 'Laptop' THEN Price 
      ELSE NULL 
    END
  ) AS AvgLaptopPrice
FROM Sales;
```

### 2. **Simple vs. Correlated Subqueries**

#### **Simple Subquery**

A simple subquery (or nested query) is a query within another query. The subquery is executed once, and its result is used by the outer query. A simple subquery does not reference columns from the outer query.

**Example:**

Find the customers who have placed an order with the maximum price:

```sql
SELECT Customer
FROM Sales
WHERE Price = (SELECT MAX(Price) FROM Sales);
```

#### **Correlated Subquery**

A correlated subquery is a subquery that references a column from the outer query. It is evaluated once for each row processed by the outer query.

**Example:**

Find all customers who have made more than one purchase:

```sql
SELECT DISTINCT Customer
FROM Sales s1
WHERE 1 < (
  SELECT COUNT(*)
  FROM Sales s2
  WHERE s2.Customer = s1.Customer
);
```

### 3. **Common Table Expressions (CTEs)**

A Common Table Expression (CTE) is a temporary result set that you can reference within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement. CTEs make queries more readable and easier to maintain, especially when you have complex queries or multiple layers of nested subqueries.

#### **Syntax**

```sql
WITH cte_name AS (
  SELECT ...
  FROM ...
)
SELECT ...
FROM cte_name;
```

#### **Example Usage with a Single CTE**

Suppose we want to find the total sales by each customer:

```sql
WITH CustomerSales AS (
  SELECT Customer, SUM(Quantity * Price) AS TotalSales
  FROM Sales
  GROUP BY Customer
)
SELECT * 
FROM CustomerSales;
```

#### **Multiple CTEs Example**

You can define multiple CTEs by separating them with commas:

```sql
WITH 
ProductSales AS (
  SELECT Product, SUM(Quantity) AS TotalQuantity
  FROM Sales
  GROUP BY Product
),
HighValueOrders AS (
  SELECT OrderID, Customer, Product
  FROM Sales
  WHERE Product = 'Laptop'
)
SELECT p.Product, p.TotalQuantity, h.Customer
FROM ProductSales p
JOIN HighValueOrders h ON p.Product = h.Product;
```

### **Use Cases**

1. **CASE Statements:**
   - Create derived columns based on conditional logic.
   - Conditional aggregation (e.g., counting or summing only rows that meet certain criteria).

2. **Subqueries:**
   - Simple Subqueries: Used to filter results or perform calculations where the results are not dependent on the outer query.
   - Correlated Subqueries: Useful when the subquery needs information from the outer query to execute correctly (e.g., row-by-row comparisons).

3. **CTEs:**
   - Breaking down complex queries into more manageable parts.
   - Improving query readability and maintainability.
   - Recursion (e.g., hierarchical or tree-like data).
     

-------------------------------------
### Window Functions in SQL

Window functions are a powerful feature in SQL that allow you to perform calculations across a set of table rows that are related to the current row. They are used extensively for analytics and reporting.

#### 1. **ROW_NUMBER()**

This function assigns a unique sequential integer to rows within a partition of a result set, starting at 1.

- **Usage**: Useful for generating row numbers or rankings.
  
- **Syntax**:
  ```sql
  ROW_NUMBER() OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  Given a table `sales`:

  | SaleID | Employee | Amount |
  |--------|----------|--------|
  | 1      | John     | 500    |
  | 2      | Jane     | 700    |
  | 3      | John     | 400    |
  | 4      | Jane     | 600    |

  We want to assign row numbers to each employee’s sales:

  ```sql
  SELECT SaleID, Employee, Amount, 
         ROW_NUMBER() OVER (PARTITION BY Employee ORDER BY Amount DESC) AS RowNum
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | RowNum |
  |--------|----------|--------|--------|
  | 2      | Jane     | 700    | 1      |
  | 4      | Jane     | 600    | 2      |
  | 1      | John     | 500    | 1      |
  | 3      | John     | 400    | 2      |

#### 2. **LAG()**

This function provides access to a row at a specified physical offset before the current row within a result set.

- **Usage**: Useful for comparing a value with a previous row’s value.

- **Syntax**:
  ```sql
  LAG(column, offset, default_value) OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  Continuing with the `sales` table:

  ```sql
  SELECT SaleID, Employee, Amount, 
         LAG(Amount, 1, 0) OVER (PARTITION BY Employee ORDER BY Amount DESC) AS PreviousSale
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | PreviousSale |
  |--------|----------|--------|--------------|
  | 2      | Jane     | 700    | 0            |
  | 4      | Jane     | 600    | 700          |
  | 1      | John     | 500    | 0            |
  | 3      | John     | 400    | 500          |

#### 3. **LEAD()**

This function provides access to a row at a specified physical offset after the current row within a result set.

- **Usage**: Useful for comparing a value with a following row’s value.

- **Syntax**:
  ```sql
  LEAD(column, offset, default_value) OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  Continuing with the `sales` table:

  ```sql
  SELECT SaleID, Employee, Amount, 
         LEAD(Amount, 1, 0) OVER (PARTITION BY Employee ORDER BY Amount DESC) AS NextSale
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | NextSale |
  |--------|----------|--------|----------|
  | 2      | Jane     | 700    | 600      |
  | 4      | Jane     | 600    | 0        |
  | 1      | John     | 500    | 400      |
  | 3      | John     | 400    | 0        |

#### 4. **FIRST_VALUE()**

This function returns the first value in an ordered set of values.

- **Usage**: Useful when you need to retrieve the earliest value in a set.

- **Syntax**:
  ```sql
  FIRST_VALUE(column) OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  ```sql
  SELECT SaleID, Employee, Amount, 
         FIRST_VALUE(Amount) OVER (PARTITION BY Employee ORDER BY Amount DESC) AS FirstSale
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | FirstSale |
  |--------|----------|--------|-----------|
  | 2      | Jane     | 700    | 700       |
  | 4      | Jane     | 600    | 700       |
  | 1      | John     | 500    | 500       |
  | 3      | John     | 400    | 500       |

#### 5. **LAST_VALUE()**

This function returns the last value in an ordered set of values.

- **Usage**: Useful for retrieving the most recent value in a set.

- **Syntax**:
  ```sql
  LAST_VALUE(column) OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  ```sql
  SELECT SaleID, Employee, Amount, 
         LAST_VALUE(Amount) OVER (PARTITION BY Employee ORDER BY Amount ASC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS LastSale
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | LastSale |
  |--------|----------|--------|----------|
  | 2      | Jane     | 700    | 600      |
  | 4      | Jane     | 600    | 600      |
  | 1      | John     | 500    | 400      |
  | 3      | John     | 400    | 400      |

#### 6. **RANK()**

This function assigns a rank to each row within a partition of a result set. The rank starts at 1 and continues based on the ordering, but it leaves gaps in rank when there are ties.

- **Usage**: Useful for ranking data with gaps for tied values.

- **Syntax**:
  ```sql
  RANK() OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  ```sql
  SELECT SaleID, Employee, Amount, 
         RANK() OVER (PARTITION BY Employee ORDER BY Amount DESC) AS Rank
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | Rank |
  |--------|----------|--------|------|
  | 2      | Jane     | 700    | 1    |
  | 4      | Jane     | 600    | 2    |
  | 1      | John     | 500    | 1    |
  | 3      | John     | 400    | 2    |

#### 7. **DENSE_RANK()**

Similar to `RANK()`, but without gaps in the rank values.

- **Usage**: Useful for ranking data without gaps for tied values.

- **Syntax**:
  ```sql
  DENSE_RANK() OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  ```sql
  SELECT SaleID, Employee, Amount, 
         DENSE_RANK() OVER (PARTITION BY Employee ORDER BY Amount DESC) AS DenseRank
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | DenseRank |
  |--------|----------|--------|-----------|
  | 2      | Jane     | 700    | 1         |
  | 4      | Jane     | 600    | 2         |
  | 1      | John     | 500    | 1         |
  | 3      | John     | 400    | 2         |

#### 8. **NTILE()**

This function distributes rows of an ordered partition into a specified number of roughly equal groups.

- **Usage**: Useful for dividing data into buckets (e.g., quartiles).

- **Syntax**:
  ```sql
  NTILE(num_buckets) OVER (PARTITION BY column ORDER BY column)
  ```

- **Example**:
  ```sql
  SELECT SaleID, Employee, Amount, 
         NTILE(2) OVER (PARTITION BY Employee ORDER BY Amount DESC) AS NTile
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | NTile |
  |--------|----------|--------|-------|
  | 2      | Jane     | 700    | 1     |
  | 4      | Jane     | 600    | 2     |
  | 1      | John     | 500    | 1     |
  | 3      | John     | 400    | 2     |

#### 9. **MAX() and SUM()**

These aggregate functions can also be used with window functions to calculate cumulative totals or maximums over a window of data.

- **Usage**: Useful for cumulative totals or tracking maximum values over a series of rows.

- **Example**:
  ```sql
  SELECT SaleID, Employee, Amount, 
         SUM(Amount) OVER (PARTITION BY Employee ORDER BY SaleID) AS CumulativeSum,
         MAX(Amount) OVER (PARTITION BY Employee ORDER BY SaleID) AS MaxAmount
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | CumulativeSum | MaxAmount |
  |--------|----------|--------|---------------|-----------|
  | 1      | John     | 500    | 500           | 500       |
  | 3      | John     | 400    | 900           | 500       |
  | 2      | Jane     | 700    | 700           | 700       |
  | 4      | Jane     | 600    | 1300          | 700       |

#### 10. **FRAMES: RANGE BETWEEN vs. ROWS BETWEEN**

**RANGE** and **ROWS** are used to define a frame of rows in the result set to which the window function is applied.

- **RANGE BETWEEN**: Defines a window based on the value range.
  
- **ROWS BETWEEN**: Defines a window based on the number of rows.

- **Example**:
  ```sql
  SELECT SaleID, Employee, Amount, 
         SUM(Amount) OVER (ORDER BY SaleID ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS RowSum,
         SUM(Amount) OVER (ORDER BY SaleID RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS RangeSum
  FROM sales;
  ```

  **Result**:

  | SaleID | Employee | Amount | RowSum | RangeSum |
  |--------|----------|--------|--------|----------|
  | 1      | John     | 500    | 500    | 500      |
  | 3      | John     | 400    | 900    | 900      |
  | 2      | Jane     | 700    | 700    | 700      |
  | 4      | Jane     | 600    | 1300   | 1300     |

In this example, `RowSum` considers only the previous row and the current row, while `RangeSum` considers all rows from the start to the current row.

