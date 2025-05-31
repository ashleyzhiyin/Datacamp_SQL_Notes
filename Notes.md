# Chapter 1: Introduction to SQL

# Chapter 2: Summary of Concepts

## 1. Data Filtering & Conditions
These are used to **select specific rows** based on criteria.

- **WHERE Clause** – Filter rows with conditions 
	- Filter rows with conditions
	- Filters records that meet specific conditions.
```sql
SELECT * FROM employees
WHERE department = 'Sales';
```

- **Comparison Operators** – =, >, <, >=, <=, != (<>)
	- Used to compare values.
```
SELECT * FROM orders
WHERE status = 'Shipped' AND total > 500;
```

- **Logical Operators** – AND, OR, NOT  
	- Combine multiple conditions.
```sql
SELECT * FROM orders
WHERE status = 'Shipped' AND total > 500;
```

- **IN Operator** – Match against a list of values  
	-  Check if a value is in a list.
```sql
SELECT * FROM customers
WHERE country IN ('USA', 'Canada', 'UK');
```

- **BETWEEN Operator** – Match values in a range  
	- Select values within a range.
```sql
SELECT * FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-03-31';
```

- **IS NULL Operator** – Check for missing (NULL) values 
	- Finds rows with NULL values.
```sql
SELECT * FROM users
WHERE phone_number IS NULL;
```
---
## 2. Pattern Matching & Uniqueness
Tools for **working with text patterns** and avoiding duplicates.

- **DISTINCT** – Return unique rows only  
	-  Removes duplicate results.
```sql
SELECT DISTINCT city 
FROM customers;
```

- **LIKE** – Simple wildcard text pattern matching  
	- Uses `%` (any characters) and `_` (single character).
```sql
SELECT * 
FROM employees 
WHERE name LIKE 'J%n';
```

- **REGEXP** – Advanced pattern matching using regular expressions  
	- More flexible than `LIKE`, supports complex patterns.
``` sql
SELECT * 
FROM users 
WHERE email REGEXP '^[a-z0-9._%+-]+@gmail\\.com$;
```

---
## 3. Aggregation & Grouping
Used to **summarize and categorize data**.

- **Aggregate Functions** – COUNT(), SUM(), AVG(), MIN(), MAX()  
	- Perform calculations on groups of rows.
 ```sql
SELECT COUNT(*) AS total_customers FROM customers; 
SELECT AVG(salary) FROM employees;
```
    
- **GROUP BY & HAVING** – Group data and filter aggregated results  
	- `GROUP BY` creates categories, `HAVING` filters them.
```sql
SELECT department, COUNT(*) AS num_employees 
FROM employees 
GROUP BY department 
HAVING COUNT(*) > 10;
```

---
## 4. Sorting & Limiting Results
Control the **order and number** of rows returned.

- **ORDER BY** – Sort results (ASC/DESC)  
	-  Sorts by one or more columns.
``` sql
SELECT * FROM products 
ORDER BY price DESC;
```

- **LIMIT** – Limit how many rows are returned  
	- Restricts the number of rows in the result.
``` sql
SELECT * FROM products
ORDER BY price DESC
LIMIT 5;
```
---
## 5. ➕ Arithmetic & Calculations
Used to **calculate values in queries**.

- **Arithmetic Operations** – +, -, *, / in queries
	- Perform math directly in SQL.
    ```sql
    SELECT name, price, price * 0.9 AS discounted_price 
    FROM products;`
    ```

# Chapter 3: Joining Data in SQL
## Category 1: **Basic Joins**

Standard joins used to combine rows from two or more tables based on related columns.

---
### 1. Inner Join

**Description:**  
Returns only the rows where there is a match in both tables.

**Syntax:**

```sql
SELECT a.column, b.column 
FROM table_a AS a 
INNER JOIN table_b AS b   
ON a.id = b.id;
```

**When to Use:**

- You only need rows with matching values in both tables.
- Most commonly used join.

---

### 2. Left Join (Left Outer Join)

**Description:**  
Returns all rows from the left table, and matched rows from the right table. If there’s no match, NULL is returned for columns from the right table.

**Syntax:**
```sql
SELECT a.column, b.column 
FROM table_a AS a   -- left table = b
LEFT JOIN table_b AS b   -- right table = b
ON a.id = b.id;
```

**When to Use:**

- You want to keep all rows from the left table, even if there is no match.
- usually use left join rather than right join in SQL, because of reading habit. 
---
### 3. Right Join (Right Outer Join)

**Description:**  
Returns all rows from the right table and matched rows from the left. If there’s no match, NULL is returned for columns from the left table.

**Syntax:**
```sql
SELECT a.column, b.column 
FROM table_a AS a 
RIGHT JOIN table_b AS b   
ON a.id = b.id;
```

**When to Use:**
- Similar to left join, but when the right table is the primary focus.
---
### 4. Full Join (Full Outer Join)

**Description:**  
Returns all rows from both tables, matched where possible, and fills with NULL when there's no match.

**Syntax:**
```sql
SELECT a.column, b.column 
FROM table_a AS a 
FULL JOIN table_b AS b   
ON a.id = b.id;
```

**When to Use:**
- You need all data from both tables, including non-matching rows.
---
### 5. Cross Join

**Description:**  
Returns the Cartesian product of two tables — all combinations of rows.

**Syntax:**
```sql
SELECT * 
FROM table_a 
CROSS JOIN table_b;
```

**When to Use:**

- You need every combination (rare).
- Be careful: results can get large quickly.
### Difference Between CROSS JOIN and FULL JOIN

| Feature            | CROSS JOIN                                       | FULL JOIN                                                     |
| ------------------ | ------------------------------------------------ | ------------------------------------------------------------- |
| **Result Type**    | Cartesian product (all combinations)             | All matched and unmatched rows from both tables               |
| **Join Condition** | No condition required                            | Needs a condition (usually `ON` clause)                       |
| **Rows Returned**  | `rows_table1 * rows_table2` (all possible pairs) | All matched rows + unmatched from left and right (with NULLs) |
| **Use Case**       | Generate all combinations                        | Combine datasets completely, even unmatched data              |
| **Common in SQL?** | Less common; specific use cases                  | More common in reporting, data reconciliation                 |

---

## Category 2: **Join Variants and Special Techniques**

Advanced join patterns and syntax simplifications.

---
### 1. Self Join

**Description:**  
A table joins to itself, useful for comparing rows in the same table.

**Syntax:**
```sql
SELECT a.name AS employee, b.name AS manager 
FROM employees a 
JOIN employees b 
ON a.manager_id = b.id;
```

**When to Use:**
- For hierarchical data, like employees and managers.
- Comparing rows within one table.
---
### 2. Multiple Joins

**Description:**  
Joining more than two tables in one query.
**Syntax:**

```sql
SELECT o.id, c.name, p.title 
FROM orders o JOIN customers c 
ON o.customer_id = c.id 
JOIN products p ON o.product_id = p.id;
```

**When to Use:**
- You need to combine data from several related tables.
---
### 3. USING Clause

**Description:**  
Shorthand for `ON` when the join column has the same name in both tables.

**Syntax:**
```sql
SELECT * 
FROM table1 
JOIN table2 
USING (shared_column);
```

**When to Use:**
- The join column name is the same in both tables.
- You want cleaner syntax.
---
## Category 3: **Set Operations**

Combine or compare the results of multiple queries.

---
### 1. UNION (remove duplicates)

**Description:**  
Combines results of two SELECT statements and removes duplicates.

**Syntax:**
```sql
SELECT column 
FROM table1 UNION 
SELECT column 
FROM table2;
```
**When to Use:**
- You want to merge similar result sets and remove duplicates.
---
### 2. UNION ALL

**Description:**  
Same as UNION, but keeps duplicate rows.

**Syntax:**
```sql
SELECT column 
FROM table1 
UNION ALL 
SELECT column FROM table2;
```

**When to Use:**
- You want all results, including duplicates.
- Faster performance than UNION.
    

---
### 3. INTERSECT

**Description:**  
Returns only rows that appear in both queries.

**Syntax:**
```sql
SELECT column 
FROM table1 
INTERSECT 
SELECT column FROM table2;
```
**When to Use:**
- You want only the common records between two result sets.
    

---
### 4. EXCEPT

**Description:**  
Returns rows from the first query that are not present in the second.

**Syntax:**
```sql
SELECT column 
FROM table1 
EXCEPT 
SELECT column FROM table2;
```

**When to Use:**
- You want to find what's in one dataset but not the other.
---
## Category 4: **Semi Joins and Anti Joins**

---
### 1. Semi Join

**Description:**  
**only if a match exists** in another table, **without returning the joined data**. They are useful when you only care about existence, not the details.

**Syntax:**

```sql
SELECT * 
FROM employees e 
WHERE EXISTS 
(SELECT 1 
FROM departments d   
WHERE e.dept_id = d.id );
```
**When to Use:**
- You just want to check if a match exists, not retrieve the joined values.
---
### 2. Anti Join

**Description:**  
Returns rows from one table where **no match** exists in the other table.

**Syntax:**
```sql
SELECT * 
FROM employees e 
WHERE NOT EXISTS 
( SELECT 1 FROM departments d   
WHERE e.dept_id = d.id );
```

**When to Use:**
- You want to find data that doesn’t exist in a related table.

Comprehensive code: 
```sql
-- Find top nine countries with the most cities

SELECT countries.name AS country, COUNT(cities.name) AS cities_num

FROM countries

LEFT JOIN cities ON countries.code = cities.country_code -- specifize table

GROUP BY countries. name

-- Order by count of cities as cities_num

ORDER BY cities_num DESC, country ASC

-- Limit the results

LIMIT 9
```

---
## Category 5: Subqueries

Subqueries help break down complex logic into smaller, reusable parts. Subqueries are commonly used in `WHERE`, `FROM`, or `SELECT` clauses.

---
### 1. GROUP BY

**Description:**  
You’re using aggregate functions. You want to summarize data — like totals, averages, or counts — **per group**.

**Example:**
```sql
-- Select code, and language count as lang_num
SELECT code, count(name) AS lang_num
FROM languages
GROUP BY code
```

**Explain:**
group code in languages, and how many are in each.
### 2. Subqueries: WHERE IN 

**Description:**  
A subquery is used inside the `WHERE` clause to filter rows based on another query’s result.

**Use Case:**  
Filter rows that meet conditions defined by another query.

**Example:**
```sql
SELECT name 
FROM employees 
WHERE department_id IN 
(SELECT id   
FROM departments   
WHERE region = 'Europe' );
```

---
### 3. Subqueries

**Description:**  
A subquery in the `FROM` clause acts as a temporary table or derived table.

**Syntax:**
```sql
SELECT ...
FROM (
  SELECT ...
  FROM some_table
  WHERE ...
  GROUP BY ...
) AS subquery_alias
```

**Complex Example 1:**
(1) Main Table: economies
You're querying the `economies` table to get:
- `code` (country code),
- `inflation_rate`, and
- `unemployement_rate`
    
But **only for records where**:
- `year = 2015`
- and the country `code` is in a **specific list**.

(2)  Subquery: Filtering by Government Form
This subquery looks at the `countries` table and:
- Filters for only those countries where the `gov_form` contain `'Republic'` or `'Monarchy'`
- Then it **returns their `code`s**
- So the subquery result is a **list of country codes** that have those two government forms.

(3) Putting It Together
 - `code IN (subquery)`

```sql
-- Select relevant fields

SELECT code, inflation_rate, unemployement_rate

FROM economies

WHERE year = 2015

AND code IN

-- Subquery returning country codes filtered on gov_form

(SELECT code FROM countries

WHERE gov_form = 'Republic' OR gov_form = 'Monarchy')

ORDER BY inflation_rate;
```
---
**Complex Example 2 :**
- From `cities`, select the city name, country code, proper population, and metro area population, as well as the field `city_perc`, which calculates the proper population as a percentage of metro area population for each city (using the formula provided).
- Filter city name with a subquery that selects `capital` cities (capital city names) from `countries` in `'Europe'` or continents with `'America'` at the end of their name.
- Exclude `NULL` values in `metroarea_pop`.
- Order by `city_perc` (descending) and return only the first 10 rows.

```sql
-- Select fields from cities

SELECT name, country_code, city_proper_pop, metroarea_pop,(city_proper_pop / metroarea_pop * 100) AS city_perc

FROM cities

-- Use subquery to filter city name

WHERE name IN

(SELECT capital

FROM countries

WHERE continent LIKE 'Europe' OR continent LIKE '%America')

-- Add filter condition such that metroarea_pop does not have null values

AND metroarea_pop IS NOT NULL

-- Sort and limit the result

ORDER BY city_perc DESC

LIMIT 10
```

# Chapter 4:  Data Manipulation in SQL
![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 

## 1. MATCH 

-  full-text searches on columns with a full-text index. 
-  when you need to search text columns for specific words or phrases efficiently. 

**Example**: 

To find articles that contain the word "database" in the title or content columns: 
```sql
SELECT * FROM articles 
WHERE MATCH (column) AGAINST ('database'); 
```
- A full-text index must be created on the columns you're searching against. 
    
![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 

## 2. CASE 

-  performing conditional logic directly 

**Example**:
- This query categorizes the salary as either "High" or "Low" depending on the value of the salary. 
```sql
SELECT name, salary, 
    CASE 
        WHEN salary > 50000 THEN 'High' 
        ELSE 'Low' 
    END AS salary_category 
FROM employees; 
```

**Syntax**：
```sql
SELECT  
    CASE 
        WHEN condition THEN result 
        ELSE default_result 
    END AS new_column 
FROM table_name; 
```
- You can also use CASE for more complex conditions: 
```
SELECT date,  
    CASE  
        WHEN home_goal > away_goal THEN 'Home win!' 
        WHEN home_goal < away_goal THEN 'Home loss :(' 
        ELSE 'Tie' 
    END AS outcome 
FROM matches_spain; 
```

![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 
**CASE with Aggregate Functions (e.g., COUNT, SUM, AVG)**： 

**COUNT：**
-  perform calculations based on certain conditions
**Example:**
- Counting matches in the 2012/2013 season: 
```sql
SELECT c.name AS country, 
    COUNT(CASE WHEN m.season = '2012/2013' THEN m.id ELSE NULL END) AS matches_2012_2013 
FROM country AS c 
LEFT JOIN match AS m 
ON c.id = m.country_id 
GROUP BY country; 
```
- This query counts the number of matches in the 2012/2013 season for each country. 

**Example with AVG:**
```sql
AVG(CASE WHEN condition_is_met THEN 1 
         WHEN condition_is_not_met THEN 0 END) AS average 
```
- This calculates the average based on specific conditions. 

![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 

**CASE with Percentages：**
**Example:**
- Calculate the percentage of home wins for a team: 
```sql
SELECT  
    team_name, 
    AVG(CASE WHEN home_goal > away_goal THEN 1 ELSE 0 END) * 100 AS pct_homewins 
FROM matches_spain 
GROUP BY team_name; 
```
- This gives the percentage of home wins by calculating the average where the condition is true. 

![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 
## 3. Subquery
- A subquery is a query that is nested inside another query.
- It's used to return data that will be used in the main query. 
- A subquery can be in the SELECT, FROM, or WHERE clause. 

``` sql
SELECT column 
FROM (SELECT column 
      FROM table) AS subquery; 
```

- The subquery is named subquery, and the outer query uses the results of the subquery to return the final results. 

Keyword: exclude = NOT IN 
```sql
SELECT  
    team_long_name, 
    team_short_name 
FROM team  
WHERE team_api_id NOT IN  
     (SELECT DISTINCT hometeam_id  FROM match);
```
-  use ’WHERE’ to judge subqueries in the FROM statement 

```sql
SELECT  
    season, 
    COUNT(id) AS matches,                     
    (SELECT COUNT(id)  
     FROM match) AS total_matches 
FROM match 
GROUP BY season; 
```

Explanation: 
- **matches** = Number of matches in the current season
- **total_matches** = Total number of matches across all seasons (same value on every row)
    
![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) **Syntax:** 
```sql
SELECT  
    col1, 
    col2, 
    col3 
FROM table1 
WHERE col1 IN 
    (SELECT id 
     FROM table2 
     WHERE year = 1991); 
```


Explanation: 
- The subquery inside the IN is formatted to clearly show that it selects id from table2 where the year is 1991. 
## 4. Correlated subquery 
- Uses values from the outer query to generate a result 
- Re-run for every row generated in the final data set 
- Used for advanced joining, filtering, and evaluating data  

**Syntax:**
```sql
SELECT column1, column2
FROM outer_table AS o
WHERE column3 = (
    SELECT column4
    FROM inner_table AS i
    WHERE i.some_column = o.some_column
);
```

- outer table: main table used in the outer query
## 5.  CTE (Common Table Expression)
-  breaking down complex queries, improving readability
-  temporary result
```sql
WITH cte_name AS (
    SELECT column1, column2
    FROM table_name
    WHERE condition
)
SELECT *
FROM cte_name;
```

## 6. Different use cases 

###  **1. Joins**

**Use Case:** Combine data from multiple related tables.
**Question**: What is the total sales amount per employee?

```sql
SELECT e.name, SUM(s.amount) AS total_sales
FROM employees e
JOIN sales s ON e.id = s.employee_id
GROUP BY e.name;
```
---
###  **2. Correlated Subqueries**

**Use Case:** Subquery that refers to the outer query row-by-row.
**Question:** Who are the employees that made sales above the average for their department?

```sql
SELECT name
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department = e.department
);
```

---
### **3. Multi-Table Query**

**Use Case:** Find relationships across 2+ tables.
**Question:** To whom does each employee report?

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

---
### **4. Multiple/Nested Subqueries**

**Use Case:** You want layered filters or aggregations.
**Question:** Which employees made more sales than the company average?

```sql
SELECT name
FROM employees
WHERE id IN (
    SELECT employee_id
    FROM sales
    GROUP BY employee_id
    HAVING SUM(amount) > (
        SELECT AVG(total_sales)
        FROM (
            SELECT employee_id, SUM(amount) AS total_sales
            FROM sales
            GROUP BY employee_id
        ) avg_sales
    )
);
```
---
### **5. Common Table Expressions (CTEs)**

**Use Case:** Organize logic, especially for recursion or readable complex queries.
**Question:** What’s the average deal size closed by each sales rep this quarter?

```sql
WITH sales_this_quarter AS (
    SELECT employee_id, amount
    FROM sales
    WHERE sale_date >= '2025-01-01' AND sale_date <= '2025-03-31'
)
SELECT e.name, AVG(s.amount) AS avg_deal_size
FROM sales_this_quarter s
JOIN employees e ON s.employee_id = e.id
GROUP BY e.name;
```

---

###  **6. Performance Metrics Across Teams**

**Use Case:** Compare metrics across departments/teams.
**Question:** How did the marketing, sales, growth, and engineering teams perform?

```sql
SELECT department,
       COUNT(*) AS total_employees,
       SUM(CASE WHEN performance_rating >= 4 THEN 1 ELSE 0 END) AS high_performers,
       AVG(performance_score) AS avg_score
FROM employees
WHERE department IN ('Marketing', 'Sales', 'Growth', 'Engineering')
GROUP BY department;
```

---
# Chapter 5: Window Functions
### 1. Window Function

**differences：**
- Aggregate functions reduce rows;  多行变成 **一行**（例如：求和、平均、计数）
- window functions keep all rows.  **额外加一列信息**（比如排名、总和、平均等）
---
#### 1. RANK() OVER

```sql
SELECT 
  employee_name, 
  department, 
  salary,
  RANK() OVER (
    PARTITION BY department 
    ORDER BY salary DESC
  ) AS dept_rank 
FROM employees;
```

employees table:

| employee_name | department | salary |
|---------------|------------|--------|
| Alice         | Sales      | 8000   |
| Bob           | Sales      | 7500   |
| Carol         | Sales      | 7000   |
| Dave          | HR         | 9000   |
| Eva           | HR         | 7000   |
| Frank         | IT         | 9500   |
| Grace         | IT         | 9500   |
| Henry         | IT         | 8500   |

Result:

| employee_name | department | salary | dept_rank |
|---------------|------------|--------|-----------|
| Alice         | Sales      | 8000   | 1         |
| Bob           | Sales      | 7500   | 2         |
| Carol         | Sales      | 7500   | 2         |
| Dave          | HR         | 9000   | 1         |
| Eva           | HR         | 7000   | 2         |
| Frank         | IT         | 9500   | 1         |
| Grace         | IT         | 9500   | 1         |
| Henry         | IT         | 8500   | 3         |

**Explanation:**
- `PARTITION BY department`: Resets ranking for each department.
- `ORDER BY salary DESC`: Ranks highest salaries first.
- `RANK()`: Assigns the same rank to ties, skips the next rank (e.g., 1, 1, 3).
    
---
#### 2. OVER()
- `OVER()` is the core syntax of **Window Functions** in SQL. 
- ROWS BETWEEN **UNBOUNDED PRECEDING AND CURRENT ROW** (Start at the first row and stop at current row)
- ROWS BETWEEN **CURRENT ROW  AND UNBOUNDED FOLLOWING**.
- It defines the **data range (window)** that each row should refer to when performing a calculation. 
- Unlike `GROUP BY`, which aggregates data into a single row per group, `OVER()` **retains all original rows**, giving each row its own calculated result.
#### 3. Sliding Window - OVER & PARTITION BY
- a **dynamic window frame** that "slides" over rows, usually based on time or sequence, to calculate aggregates like moving averages, cumulative sums, etc.

**Common Use Cases:**
- Moving averages (7-day, 30-day, etc.)
- Rolling sums/totals
- Cumulative metrics (e.g., revenue YTD)
- Running rankings or counts

**Example:**
```sql
SELECT 
  Athlete,
  Year,
  Points,
  ROW_NUMBER() OVER (PARTITION BY Year ORDER BY Points DESC) AS Rank
FROM 
  AthleteScores;
```

**Explanation:**
 - ROW_NUMBER() will **assign a number** to each row (i.e., each athlete for each year). 
- The PARTITION BY Year ensures that the row numbers are **calculated separately** for each year. 
- ORDER BY **Points** DESC makes sure that the highest-scoring athletes get the lowest row number (1st, 2nd, 3rd, etc.). 

### 2. Reigning champion 

#### 1. lag(): past
- To identify a reigning champion
- LAG(): LAG(Champion): This function gets the Champion value from the **previous row** (the previous year's champion). 

```sql
SELECT 
  Year, 
  Champion, 
  LAG(Champion) OVER (PARTITION BY Gender ORDER BY Year ASC) AS Last_Champion 
FROM Tennis_Gold 
ORDER BY Year ASC; 
```

- LAG(Champion): This gets the Champion value from the previous row. 
- PARTITION BY Gender: This ensures that the calculation is done separately for each gender (male/female). 
- ORDER BY Year ASC: Orders the data by year so that the LAG() function can fetch the previous year's champion. 
    

**How to Choose PARTITION BY and GROUP BY** 

- PARTITION BY:  when you want to divide the dataset into smaller groups. 
    
- GROUP BY: The most left column.  aggregate data across multiple rows. 
-  Typically used with functions like COUNT(), SUM(), AVG(), etc.
![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 
#### 2. Relative LAG(column, n) 

-  Returns the value of the specified column from the row n rows before the current row. 
- You want to access prior values in the dataset based on a specified offset. 

**Syntax**: 

`LAG(column, n) OVER (PARTITION BY column ORDER BY column)`

- column: The column whose value you want to retrieve. 
- n: The number of rows before the current row you want to look at. If you omit this value, it defaults to 1.     
**Example:**
```sql
SELECT  
    sales_date,  
    sales_amount, 
    LAG(sales_amount, 1) OVER (ORDER BY sales_date) AS previous_sales 
FROM sales_data; 
```
![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 

#### 3. LEAD(column, n) : future
- The value of the specified column from the row n rows after the current row.
-  Look at future values in the dataset 

**Syntax:** 
`LEAD(column, n) OVER (PARTITION BY column ORDER BY column) `

- column: The column whose value you want to retrieve. 
- n: The number of rows after the current row you want to look at. The default is 1. 

**Example:** 
```sql
SELECT  
    sales_date,  
    sales_amount, 
    LEAD(sales_amount, 1) OVER (ORDER BY sales_date) AS next_day_sales 
FROM sales_data; 
```

![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 
#### 4. FIRST_VALUE(column) 

- Returns the first value in the specified column based on the order of the result set or partition. 

**Syntax:** 
`FIRST_VALUE(column) OVER (PARTITION BY column ORDER BY column) `
    
**Example:**
```sql
SELECT  
    sales_date,  
    sales_amount, 
    FIRST_VALUE(sales_amount) OVER (ORDER BY sales_date) AS first_sales 
FROM sales_data; 
```

![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 
#### 4. LAST_VALUE(column) 

- Returns the last value of the specified column 

**Syntax:** 
`LAST_VALUE(column) OVER (PARTITION BY column ORDER BY column ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) `

- ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING: Ensures that the last value is always retrieved. 
    
**Example:**
```sql
SELECT  
    sales_date,  
    sales_amount, 
    LAST_VALUE(sales_amount) OVER (ORDER BY sales_date ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_sales 

FROM sales_data; 
```

### 3. The Ranking Functions 

#### 1. ROW_NUMBER() OVER () 

- This gives a unique number to each row, regardless of duplicates: 

|       |       |            |
| ----- | ----- | ---------- |
| Name  | Score | Row_Number |
| Alice | 100   | 1          |
| Bob   | 95    | 2          |
| Carol | 95    | 3          |
| David | 90    | 4          |

![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 
#### 2. RANK() OVER () 

- This gives the same rank to ties, and skips the next rank(s): 

|   |   |   |
|---|---|---|
|Name|Score|Rank|
|Alice|100|1|
|Bob|95|2|
|Carol|95|2|
|David|90|4|

![Shape](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAANSURBVBhXY2BgYGAAAAAFAAGKM+MAAAAAAElFTkSuQmCC) 
#### 3. DENSE_RANK() OVER () 

- Also gives the same rank to ties, but doesn't skip the next number: 

|   |   |   |
|---|---|---|
|Name|Score|Dense_Rank|
|Alice|100|1|
|Bob|95|2|
|Carol|95|2|
|David|90|3|
#### 4. ROWS BETWEEN: 

`ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING `
### 4. Paging 

#### 1. NTILE()—to split data into pages 

- Divide your entire dataset into n equal(ish) parts. 
- You don't know how many rows there are, but you know how many groups (or pages) you want 
- You want to display evenly-distributed groups 
- You want to build a “paged” display, but don’t want to worry about dynamic OFFSET/LIMIT （跳过/取 n 行）

**Syntax:**
`NTILE(15) OVER (ORDER BY Discipline) `

- This divides the result set into 15 roughly equal groups, assigning a page number (1–15) to each row. 

#### 2. MAX(Medals) OVER()AS


``` sql
MAX(Medals) OVER (ORDER BY Year ASC ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) 
```

Output Table 

|   |   |   |   |
|---|---|---|---|
|Year|Medals|Max_Medals|Max_Medals_Last|
|1996|36|36|36|
|2000|66|66|66|
|2004|47|66|66|
|2008|43|66|47|
|2012|47|66|47|

#### 3. Pivot   

- CREATE EXTENSION IF NOT EXISTS tablefunc; 
- Converts rows data of a table into column data. 

**Syntax:**
```sql 
SELECT * 
FROM CROSSTAB($$ 

    SELECT row_header, category, value 

    FROM your_table 

    ORDER BY row_header, category 

$$) AS result_table ( 

    row_header_data_type, 

    category1_data_type, 

    category2_data_type, 
    ... 
); 
```
**Example:**
```sql

$$) AS ct ( 
            Gender VARCHAR, 
            "2008" VARCHAR, 
            "2012" VARCHAR 
           ) 
ORDER BY Gender ASC; 
```
- Category type need to **align with your variable**(here is country)
