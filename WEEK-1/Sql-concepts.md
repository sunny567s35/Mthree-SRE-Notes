Here’s your SQL learning documentation formatted as a **Markdown (`.md`) file** with proper syntax for headings, tables, and links.

* * *

### **SQL Learning Documentation - 10-02-2025**

**What is SQL?**
----------------

Imagine a wardrobe where you store different types of clothes. SQL works similarly for databases—it helps you **store, organize, and find information efficiently**.

Your wardrobe has sections (**tables**) like:

*   **Shirts**
*   **Jeans**
*   **Jackets**

Each section has racks (**columns**) for organizing by:

*   **Color** (Red, Blue, Pink, Black)
*   **Size** (Small, Medium, Large)
*   **Type** (Casual, Formal)

If you need to find all pink shirts, instead of searching manually, you can just ask:  
📢 `"Show me all shirts where the color is pink!"`

SQL does the same with data—helping you **store, manage, and retrieve** information efficiently.

**Understanding Rows in SQL**
-----------------------------

In this analogy, a **row** represents a specific item of clothing in a section (**table**).

### **Example: Shirts Table (SQL Table)**

ID

Color

Size

Type

1

Red

Small

Casual

2

Blue

Medium

Formal

3

Pink

Large

Casual

Each row represents **one shirt** with properties (**color, size, type**).

### **SQL Query Example**

To find all pink shirts, use:

`SELECT * FROM Shirts WHERE Color = 'Pink';` 

👉 This retrieves the row(s) where the **color is Pink!**

* * *

**1\. What is a Database?**
---------------------------

A **database** is a structured collection of data that can be **easily accessed, managed, and updated**. It helps store large amounts of information efficiently.

**2\. What is DBMS?**
---------------------

A **Database Management System (DBMS)** is software that allows users to **store, retrieve, and manage** data in databases.

**3\. Types of DBMS**
---------------------

*   **Relational DBMS (RDBMS)** – Uses tables (e.g., MySQL, PostgreSQL).
*   **NoSQL DBMS** – Stores data in key-value pairs, documents, or graphs (e.g., MongoDB).

* * *

**4\. SQL Example (Finding Pink Shirts in 400 Shirts)**
-------------------------------------------------------

If you have a table named `supboard` with different shirt types, you can find all pink shirts using SQL:

`SELECT * FROM supboard WHERE color = 'pink';` 

This selects all rows where the **color column** is `'pink'`.

* * *

**5\. Table Structure Example (Creating a Supboard Table)**
-----------------------------------------------------------

We can create a table with different types of **shirts, jeans, and compartments as racks**.

`CREATE TABLE supboard (
    id INT PRIMARY KEY,
    type VARCHAR(50),
    color VARCHAR(20),
    rack_number INT
);` 

Each **row** represents a clothing item, and **columns** store attributes like type, color, and rack number.

* * *

**6\. Steps to Download MySQL Workbench**
-----------------------------------------

To install **MySQL Workbench**:

1.  Visit the [MySQL official website](https://www.geeksforgeeks.org/how-to-install-sql-workbench-for-mysql-on-windows/).
2.  Download and install MySQL Workbench.
3.  Connect to the database and start running SQL queries.

* * *

**7\. DDL (Data Definition Language) Commands**
-----------------------------------------------

These commands modify database structure.

*   **Create Table**
    
    `CREATE TABLE employees (id INT, name VARCHAR(50), salary INT);` 
    
*   **Alter Table (Add Column)**
    
    `ALTER TABLE employees ADD COLUMN department VARCHAR(50);` 
    
*   **Drop Table**
    
    `DROP TABLE employees;` 
    

* * *

**8\. DML (Data Manipulation Language) Commands**
-------------------------------------------------

These modify data inside tables.

*   **Insert Values**
    
    `INSERT INTO employees (id, name, salary) VALUES (1, 'John', 50000);` 
    
*   **Update Data**
    
    `UPDATE employees SET salary = 55000 WHERE id = 1;` 
    
*   **Delete Data**
    
    `DELETE FROM employees WHERE id = 1;` 
    

* * *

**9\. Difference Between TRUNCATE and DELETE**
----------------------------------------------

Feature

DELETE

TRUNCATE

Removes Specific Rows?

✅ Yes

❌ No (removes all)

Allows Rollback?

✅ Yes

❌ No

Performance

Slower

Faster

* * *

**10\. Cloning a Table (Deep Copy vs. Shallow Copy)**
-----------------------------------------------------

Feature

Deep Copy

Shallow Copy

Data Copied?

✅ Yes

❌ No

Structure Copied?

✅ Yes

✅ Yes

Changes Reflect in Original?

❌ No

✅ Yes

**Deep Copy Example:**

`CREATE TABLE new_table AS SELECT * FROM old_table;` 

**Shallow Copy Example:**

`CREATE TABLE new_table LIKE old_table;` 

* * *

**11\. DESC Command (Describe Table)**
--------------------------------------

`DESC employees;` 

* * *

**12\. Temporary Tables**
-------------------------

Temporary tables store data for a **session** and disappear after use:

`CREATE TEMPORARY TABLE temp_table AS SELECT * FROM employees;` 

* * *

**13\. Views (Short Explanation)**
----------------------------------

A **view** is a **virtual table** based on a query:

`CREATE VIEW high_salary AS
SELECT * FROM employees WHERE salary > 60000;` 

* * *

**14\. Difference Between CHAR and VARCHAR**
--------------------------------------------

Feature

CHAR

VARCHAR

Storage

Fixed-length

Variable-length

Speed

Faster

Slower

Padding

Adds spaces

No extra spaces

* * *

**15\. SQL Query Clauses**
--------------------------

*   **ORDER BY** – Sorts data in ascending (`ASC`) or descending (`DESC`) order.
    
    `SELECT * FROM employees ORDER BY salary DESC;` 
    
*   **HAVING** – Filters grouped results.
    
    `SELECT department, COUNT(*)
    FROM employees
    GROUP BY department
    HAVING COUNT(*) > 5;` 
    
*   **WHERE** – Filters rows before grouping.
    
    `SELECT * FROM employees WHERE salary > 50000;` 
    
*   **LIMIT** – Restricts the number of results.
    
    `SELECT * FROM employees LIMIT 5;` 
    
*   **Finding Unique Values**
    
    `SELECT DISTINCT department FROM employees;` 
    

* * *

**16\. Primary Key, UNIQUE, and NOT NULL Constraints**
------------------------------------------------------

### **Primary Key**

`CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    salary INT
);` 

### **UNIQUE Constraint**

`CREATE TABLE users (
    user_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);` 

### **NOT NULL Constraint**

`CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);` 

* * *

**[LeetCode Problems](https://leetcode.com/)**
----------------------------------------------

1.  [Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/)
2.  [Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/)
3.  [Big Countries](https://leetcode.com/problems/big-countries/description/)
4.  [Article Views I](https://leetcode.com/problems/article-views-i/description/)
5.  [Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/)

* * *

This document is now fully formatted as **Markdown (`.md`)**! 🚀

![Export to Google Doc](chrome-extension://iapioliapockkkikccgbiaalfhoieano/assets/create.svg)![Copy with formatting](chrome-extension://iapioliapockkkikccgbiaalfhoieano/assets/copy.svg)![Select for Multi-select](chrome-extension://iapioliapockkkikccgbiaalfhoieano/assets/multi-select.svg)
