### 1. Normalization?
Normalization is the process of organizing columns and tables in a relational database to minimize data redundancy and improve data integrity.

*   **Why do it?** The main goals are to reduce redundant data, prevent data anomalies (like insertion, update, and deletion anomalies), and ensure data is logically stored.
*   **Types of Normal Forms:**
    *   **1NF (First Normal Form):** Ensures that table cells hold a single, atomic value and each record is unique (usually via a primary key).
    *   **2NF (Second Normal Form):** Must be in 1NF. All non-key attributes must be fully dependent on the entire primary key, eliminating partial dependencies.
    *   **3NF (Third Normal Form):** Must be in 2NF. All attributes must be dependent only on the primary key, not on other non-key attributes. This removes transitive dependencies.
*   **When is denormalization required?** Denormalization is the process of intentionally adding redundancy to a database to improve query performance. It is often used in data warehousing or reporting systems where read speed is more critical than write efficiency, and complex joins on highly normalized tables would be too slow.

---

### 2. Database Indexing?
A database index is a data structure (commonly a B-Tree) that improves the speed of data retrieval operations on a table.

*   **Why use it?** To quickly locate and access data in a database without having to scan every row in a table. This significantly speeds up `SELECT` queries, especially those with `WHERE`, `JOIN`, and `ORDER BY` clauses.
*   **On which column?** Indexes are best placed on columns that are frequently searched, such as primary keys, foreign keys, and columns often used in `WHERE` clauses.
*   **Disadvantages:**
    *   **Performance Overhead:** Indexes slow down data modification operations (`INSERT`, `UPDATE`, `DELETE`) because the index must also be updated.
    *   **Storage Space:** Indexes require additional disk space.
*   **Is it expensive?** Yes, in terms of storage and the performance cost on write operations. The trade-off between faster reads and slower writes must be considered.

---

### 3. Joins?
A `JOIN` clause is used in SQL to combine rows from two or more tables based on a related column between them.

*   **Types of Joins:**
    *   **`INNER JOIN`**: Returns records that have matching values in both tables.
    *   **`LEFT JOIN` (Outer Join)**: Returns all records from the left table and the matched records from the right table.
    *   **`RIGHT JOIN` (Outer Join)**: Returns all records from the right table and the matched records from the left table.
    *   **`SELF JOIN`**: A table is joined with itself, often used for hierarchical data or comparing rows within the same table.
    *   **`EQUI JOIN`**: A join using an equality operator (`=`). `INNER JOIN` is a type of equi join.
    *   **`NON-EQUI JOIN`**: A join using a comparison operator other than equality (e.g., `>`, `<`, `BETWEEN`).
*   **Difference between Join and Subquery:**
    *   A **`JOIN`** combines columns from multiple tables into a new result set. They are often more readable and can be better optimized by the database.
    *   A **`Subquery`** is a query nested inside another query. It can return a single value, a list of values, or a table of values. Sometimes they are less efficient, but they are necessary for logic that cannot be expressed with a join.

---

### 4. 2nd Max Salary Query? (Variations: 3rd, 4th, Nth)
The most robust and modern way to find the Nth highest value is using a window function like `DENSE_RANK()`.

```sql
-- Query to find the Nth highest salary (e.g., N=2 for the 2nd highest)
WITH RankedSalaries AS (
    SELECT
        Salary,
        DENSE_RANK() OVER (ORDER BY Salary DESC) as salary_rank
    FROM Employees
)
SELECT Salary
FROM RankedSalaries
WHERE salary_rank = 2; -- Change 2 to N for the Nth highest
```
`DENSE_RANK` is used because it handles ties without skipping rank numbers.

---

### 5. ER Diagram / Schema Design?
This involves designing the database structure for a given scenario (e.g., a University). The process is as follows:
1.  **Identify Entities:** Find the main objects (nouns) like `Student`, `Course`, `Professor`, `Department`.
2.  **Identify Attributes:** Define the properties for each entity, like `StudentID`, `StudentName`, `CourseID`, `CourseName`. Identify primary keys.
3.  **Identify Relationships:** Determine how entities relate (e.g., a `Professor` *teaches* a `Course`). Cardinalities are defined (one-to-one, one-to-many, many-to-many).
4.  **Resolve Many-to-Many Relationships:** Create a junction table (or bridge entity). For example, to link `Students` and `Courses`, an `Enrollment` table is created with `StudentID` and `CourseID` as foreign keys.
5.  **Normalize:** Apply normalization rules (1NF, 2NF, 3NF) to refine the schema.

---

### 6. SQL vs NoSQL Difference?
*   **SQL (Relational):**
    *   **Data Model:** Structured data in tables with rows and columns.
    *   **Schema:** Predefined and fixed (schema-on-write).
    *   **Scalability:** Scales vertically (increasing the power of a single server).
    *   **Consistency:** Guarantees ACID properties (Atomicity, Consistency, Isolation, Durability).
    *   **Use Cases:** Financial systems, transactional applications, and any system where data integrity is paramount.
*   **NoSQL (Non-relational):**
    *   **Data Model:** Can be document, key-value, column-family, or graph.
    *   **Schema:** Dynamic and flexible (schema-on-read).
    *   **Scalability:** Scales horizontally (distributing data across multiple servers).
    *   **Consistency:** Follows the BASE model (Basically Available, Soft State, Eventual Consistency).
    *   **Use Cases:** Big data, social media feeds, IoT applications, and systems requiring high availability and scalability with unstructured data.

---

### 7. Triggers vs Stored Procedures?
*   **Stored Procedure:** A set of SQL statements saved in the database that can be executed **explicitly** by calling its name. It can accept parameters and return values.
*   **Trigger:** A special type of stored procedure that is executed **implicitly** (automatically) when a specific event (`INSERT`, `UPDATE`, `DELETE`) occurs on a table.
*   **Benefits of Stored Procedures:**
    *   **Performance:** They are pre-compiled and reduce network traffic.
    *   **Security:** Grant permissions to execute the procedure without giving direct access to the tables.
    *   **Reusability:** Encapsulate complex business logic in one place.

---

### 8. Transactions in Databases?
A transaction is a sequence of database operations performed as a single logical unit of work. All operations within a transaction must succeed; if any operation fails, the entire transaction is rolled back, and the database is left unchanged. This is the "all-or-nothing" principle.

---

### 9. ACID Properties?
ACID is a set of properties that guarantee database transactions are processed reliably.
*   **Atomicity:** A transaction is an indivisible unit; either all of its operations complete successfully, or none of them do.
*   **Consistency:** A transaction brings the database from one valid state to another. Data must always satisfy all defined rules and constraints.
    *   **Real-world Example (Consistency):** In a bank transfer, you debit $100 from Account A and credit $100 to Account B. Consistency ensures that the total money across both accounts is the same before and after the transaction. If the system crashes after the debit but before the credit, the transaction is rolled back to maintain a consistent state.
*   **Isolation:** Concurrent transactions are executed in a way that their effects are isolated from each other. The result is the same as if the transactions were executed serially.
*   **Durability:** Once a transaction is committed, its changes are permanent and will survive system failures like a power outage or crash.

---

### 10. Types of Keys
*   **Super Key:** An attribute or set of attributes that uniquely identifies a row in a table.
*   **Candidate Key:** A minimal super key (a super key with no redundant attributes). A table can have multiple candidate keys.
*   **Primary Key:** The candidate key chosen by the database designer to uniquely identify each row in a table. It cannot contain NULL values.
*   **Foreign Key:** A key in one table that refers to the Primary Key of another table, used to establish a link between the two tables.
*   **Composite Key:** A primary key that consists of two or more columns that together uniquely identify a row.

---

### 11. HAVING vs WHERE Clause Difference?
*   **`WHERE`** filters rows **before** any aggregation (grouping) is performed. It operates on individual row data.
*   **`HAVING`** filters groups **after** aggregation has been performed. It operates on the results of aggregate functions like `COUNT()`, `SUM()`, `AVG()`.

---

### 12. Transitive Dependency?
A transitive dependency exists when a non-key attribute is dependent on another non-key attribute, rather than directly on the primary key. If A -> B and B -> C, then A -> C is a transitive dependency (where A is the primary key).
*   **How to remove?** To achieve 3rd Normal Form (3NF), you remove transitive dependencies by splitting the table into smaller tables. For example, if a `(StudentID, Major, Major_HOD)` table exists, you would split it into `(StudentID, Major)` and `(Major, Major_HOD)`.

---

### 13. How to Implement Many-to-Many Relationships?
A many-to-many relationship is implemented using a **junction table** (also called a **bridge entity** or associative table). This table sits between the two tables in the relationship and contains foreign keys that reference the primary keys of both tables. For example, to connect `Students` and `Courses`, an `Enrollment` table would be created with `StudentID_FK` and `CourseID_FK`.

---

### 14. CRUD Queries?
CRUD stands for the four basic database operations:
*   **Create:** `INSERT INTO table_name (...) VALUES (...);`
*   **Read:** `SELECT column1, column2 FROM table_name WHERE ...;`
*   **Update:** `UPDATE table_name SET column1 = value1 WHERE ...;`
*   **Delete:** `DELETE FROM table_name WHERE ...;`

---

### 15. Views?
A view is a virtual table based on the result-set of an SQL query. It acts as a stored query that can be accessed like a table.
*   **Why use them?**
    *   **Security:** To restrict access to specific columns or rows of a table.
    *   **Simplicity:** To hide the complexity of underlying queries (like complex joins) from users.
    *   **Consistency:** To provide a stable interface even if the underlying table structures change.

---

### 16. Query Optimization?
Query optimization is the process where the database system determines the most efficient way to execute a query.
*   **How to check if a query is slow?**
    *   Use the `EXPLAIN` (or `EXPLAIN ANALYZE`) command before your query. This shows the **execution plan**. Look for red flags like "Full Table Scan" on large tables, which indicates that an index might be missing or not being used effectively.
    *   Use database profiling tools to monitor execution time and resource consumption.

---

### 17. Stored Procedures?
This is covered in question #7. They are pre-compiled SQL code saved in the database for reuse.
*   **Syntax (Example in T-SQL):**
    ```sql
    CREATE PROCEDURE GetEmployeesByDepartment @DeptID INT
    AS
    BEGIN
        SELECT EmployeeName, JobTitle FROM Employees WHERE DepartmentID = @DeptID;
    END;
    ```
*   **Why use?** Security, reusability, and performance (reduced network traffic).
*   **Efficient?** Yes, because the execution plan is cached, and less data is sent over the network.

---

### 18. Clustered and Unclustered Indexing?
*   **Clustered Index:** Determines the **physical order** of data in a table. The data rows are stored in the order of the clustered index. Because of this, a table can only have **one** clustered index. Think of it like a dictionary where the words are physically sorted alphabetically.
*   **Unclustered Index:** Has a separate structure from the data rows, where the index contains pointers to the location of the actual data. A table can have **multiple** non-clustered indexes. Think of it like the index at the back of a book, which points to page numbers but doesn't change the order of the pages themselves.

---

### 19. Query to fetch names of the department where the greatest number of employees work (handle ties alphabetically)?
This can be solved efficiently using a Common Table Expression (CTE) and a window function.

```sql
WITH DepartmentCounts AS (
    SELECT
        d.DepartmentName,
        COUNT(e.EmployeeID) AS NumberOfEmployees,
        RANK() OVER (ORDER BY COUNT(e.EmployeeID) DESC) as dept_rank
    FROM Departments d
    JOIN Employees e ON d.DepartmentID = e.DepartmentID
    GROUP BY d.DepartmentName
)
SELECT DepartmentName
FROM DepartmentCounts
WHERE dept_rank = 1
ORDER BY DepartmentName ASC;
```
This query first counts employees in each department, then ranks the departments by count (descending). `RANK()` correctly handles ties. Finally, it selects all departments with a rank of 1 and orders them alphabetically.

Of course. Here are the answers to the second list of database questions.

---

### 20. Query to find department name with no employees?
This can be done using a `LEFT JOIN` and checking for `NULL` values, or by using a subquery with `NOT IN`. The `LEFT JOIN` approach is often more performant.

```sql
-- Using LEFT JOIN (recommended)
SELECT d.DepartmentName
FROM Departments d
LEFT JOIN Employees e ON d.DepartmentID = e.DepartmentID
WHERE e.EmployeeID IS NULL;

-- Using NOT IN
SELECT DepartmentName
FROM Departments
WHERE DepartmentID NOT IN (SELECT DISTINCT DepartmentID FROM Employees WHERE DepartmentID IS NOT NULL);
```

---

### 21. Drop vs Truncate vs Delete?
| Feature | `DELETE` | `TRUNCATE` | `DROP` |
| :--- | :--- | :--- | :--- |
| **Type** | DML (Data Manipulation Language) | DDL (Data Definition Language) | DDL (Data Definition Language) |
| **Scope** | Removes some or all rows from a table. | Removes **all** rows from a table. | Removes the **entire table** (structure and data). |
| **`WHERE` Clause** | Can be used to specify which rows to remove. | Cannot be used. | Cannot be used. |
| **Logging** | Logs each row deletion. Can be slow for large tables. | Minimal logging (logs page deallocations). Very fast. | Minimal logging. |
| **Rollback** | Can be rolled back. | Cannot be rolled back (generally). | Cannot be rolled back. |
| **Triggers** | Will fire `DELETE` triggers for each row. | Does not fire triggers. | Does not fire triggers. |
| **Identity** | Does not reset identity columns. | Resets identity columns to their seed value. | N/A (table is gone). |

---

### 22. Indexes and its drawbacks?
*   **Indexes:** Data structures that improve the speed of data retrieval on a database table at the cost of slower writes and increased storage space.
*   **Drawbacks:**
    1.  **Slower Write Operations:** `INSERT`, `UPDATE`, and `DELETE` statements become slower because the indexes must also be updated.
    2.  **Storage Overhead:** Indexes take up disk space, which can be significant for large tables or tables with many indexes.
    3.  **Maintenance:** Indexes need to be maintained by the database system, which adds overhead.

---

### 23. SQL Injection? (How to prevent?)
*   **SQL Injection:** A web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It typically involves inserting malicious SQL statements into an entry field for execution.
*   **How to Prevent:**
    1.  **Prepared Statements (Parameterized Queries):** This is the most effective method. It separates the SQL code from the user-provided data, ensuring the data is treated as a literal value and not as executable code.
    2.  **Input Validation:** Sanitize and validate all user input to ensure it matches the expected type, length, and format.
    3.  **Principle of Least Privilege:** The application's database account should only have the minimum permissions necessary to function, limiting the damage an attacker can do.

---

### 24. Partial Dependency?
A partial dependency occurs in a table with a composite primary key when a non-key attribute is functionally dependent on only a part of the primary key, not the whole key. This violates the Second Normal Form (2NF).
*   **Example:** Consider a table `(StudentID, CourseID, StudentName)`. The primary key is `(StudentID, CourseID)`. `StudentName` depends only on `StudentID`, which is just a part of the primary key. This is a partial dependency.
*   **Solution:** Decompose the table into `(StudentID, StudentName)` and `(StudentID, CourseID)`.

---

### 25. Query on FYP ERD?
This question is context-dependent and refers to a specific Final Year Project (FYP) Entity-Relationship Diagram. A typical complex query might involve joining multiple tables, using aggregate functions, and filtering based on several conditions. For example: "Find the top 3 users who have placed the most orders in the last month, along with their total spending."

---

### 26. Query to fetch names of employees who don’t have any salary in the salary table?
Assuming `Employees` and `Salaries` are two different tables.

```sql
SELECT e.EmployeeName
FROM Employees e
LEFT JOIN Salaries s ON e.EmployeeID = s.EmployeeID
WHERE s.SalaryAmount IS NULL;
```

---

### 27. Query to find manager name with greatest number of employees (handle ties alphabetically)?
This requires a self-join on the employees table to link employees to their managers.

```sql
WITH ManagerCounts AS (
    SELECT
        m.EmployeeName AS ManagerName,
        COUNT(e.EmployeeID) AS NumberOfReports,
        RANK() OVER (ORDER BY COUNT(e.EmployeeID) DESC) as manager_rank
    FROM Employees e
    JOIN Employees m ON e.ManagerID = m.EmployeeID
    GROUP BY m.EmployeeName
)
SELECT ManagerName
FROM ManagerCounts
WHERE manager_rank = 1
ORDER BY ManagerName ASC;
```

---

### 28. Benefit of indexing?
(Covered in previous list) The primary benefit of indexing is significantly faster data retrieval (query speed), especially for `SELECT` statements with `WHERE`, `JOIN`, and `ORDER BY` clauses.

---

### 29. Surrogate key?
A surrogate key is a system-generated, unique identifier for a row in a table. It has no business meaning and exists purely to act as the primary key. An auto-incrementing integer (`IDENTITY` or `AUTO_INCREMENT`) is a common example. It is used in contrast to a natural key, which is a key that has a real-world, business meaning (e.g., Social Security Number, Vehicle VIN).

---

### 30. Self join query?
A self-join is a query where a table is joined to itself. It's useful for querying hierarchical data or comparing rows within the same table. A classic example is finding employees and their managers from a single `Employees` table.

```sql
SELECT
    e.EmployeeName AS Employee,
    m.EmployeeName AS Manager
FROM Employees e
LEFT JOIN Employees m ON e.ManagerID = m.EmployeeID;
```

---

### 31. Homogeneous and heterogeneous databases?
This refers to a distributed database system:
*   **Homogeneous Database:** A distributed system where all sites use the same DBMS software (e.g., all sites run Oracle 19c). They are easier to manage and design.
*   **Heterogeneous Database:** A distributed system where different sites may use different DBMS software (e.g., one site uses Oracle, another uses SQL Server, and a third uses MySQL). These systems are more complex and require a gateway or middleware to translate between different schemas and query languages.

---

### 32. Database definition?
A database is an organized, structured collection of data stored electronically, designed for efficient access, management, and updating. It is controlled by a Database Management System (DBMS).

---

### 33. Find query for nth max salary from employee table?
(Covered in previous list) The best method uses a window function like `DENSE_RANK`.

```sql
-- To find the Nth highest salary (e.g., N=3)
WITH RankedSalaries AS (
    SELECT
        Salary,
        DENSE_RANK() OVER (ORDER BY Salary DESC) as salary_rank
    FROM Employees
)
SELECT Salary
FROM RankedSalaries
WHERE salary_rank = 3; -- Change 3 to the desired N
```

---

### 34. Find the count of employees each manager is handling?

```sql
SELECT
    m.EmployeeName AS ManagerName,
    COUNT(e.EmployeeID) AS NumberOfReports
FROM Employees e
JOIN Employees m ON e.ManagerID = m.EmployeeID
GROUP BY m.EmployeeName
ORDER BY NumberOfReports DESC;
```

---

### 35. How to apply constraints?
Constraints are rules applied to table columns to enforce data integrity. They can be applied when a table is created (`CREATE TABLE`) or added to an existing table (`ALTER TABLE`).
*   **`CREATE TABLE` Syntax:**
    ```sql
    CREATE TABLE Employees (
        EmployeeID INT PRIMARY KEY,
        Email VARCHAR(255) UNIQUE NOT NULL,
        Age INT CHECK (Age >= 18),
        DepartmentID INT REFERENCES Departments(DepartmentID)
    );
    ```
*   **`ALTER TABLE` Syntax:**
    ```sql
    ALTER TABLE Orders ADD CONSTRAINT FK_CustomerOrder FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID);
    ```

---

### 36. Query to find all employees having same manager_id?
This requires finding `manager_id`s that are associated with more than one employee.

```sql
SELECT *
FROM Employees
WHERE ManagerID IN (
    SELECT ManagerID
    FROM Employees
    GROUP BY ManagerID
    HAVING COUNT(*) > 1
);
```

---

### 37. GROUP BY clause? When to use?
The `GROUP BY` clause is used with aggregate functions (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) to group rows that have the same values in specified columns into summary rows.
*   **When to use:** Use it whenever you need to perform a calculation on a set of rows and present a single, summary result for each group. For example, "find the number of employees *per department*," or "calculate the average salary *for each job title*."

---

### 38. Roll back?
`ROLLBACK` is a command that undoes all the database changes made during the current transaction, restoring the database to the state it was in before the transaction began. It is the command used to enforce the "Atomicity" property of ACID.

---

### 39. Smallest unit of data in computer? (Bit)
The smallest unit of data in a computer is a **bit** (binary digit), which can hold a value of either 0 or 1.

---

### 40. Which command is used to remove a relation from an SQL? (DROP TABLE)
The command is `DROP TABLE table_name;`. A "relation" is the formal term for a table in the relational model.

---

### 41. Which normal form is considered adequate for RDBMS design? (3NF usually)
**Third Normal Form (3NF)** is generally considered the standard for a well-designed relational database, as it eliminates both partial and transitive dependencies, providing a good balance between data integrity and performance. BCNF is stricter, but sometimes 3NF is more practical.

---

### 42. Most expensive function in SQL?
This is subjective, but operations that are often "expensive" (in terms of CPU, I/O, and time) include:
*   **Full Table Scans:** Reading every row in a large table because of a poorly written query or missing index.
*   **Certain `JOIN` operations:** A `CROSS JOIN` (Cartesian product) on large tables is extremely expensive.
*   **`DISTINCT` on large datasets:** Requires sorting the entire result set to find unique values.
*   **Poorly optimized subqueries:** Especially correlated subqueries that execute for every row of the outer query.

---

### 43. Aggregate functions?
Aggregate functions perform a calculation on a set of values and return a single summary value. Common examples include:
*   `COUNT()`: Counts the number of rows.
*   `SUM()`: Calculates the sum of values.
*   `AVG()`: Calculates the average of values.
*   `MAX()`: Returns the maximum value.
*   `MIN()`: Returns the minimum value.

---

### 44. One SQL query (often FYP related)
This is another context-specific question. See answer #25.

---

### 45. Shared Resources (in DB context)?
In a database context, shared resources are components that multiple concurrent transactions or users need to access, such as:
*   **Data:** The tables, rows, and data blocks on disk.
*   **Indexes:** The index structures used for fast lookups.
*   **Memory Buffers:** The shared memory pool (buffer cache) where data blocks are held.
*   **CPU and I/O channels.**

---

### 46. Mutual Exclusion (in DB context)?
Mutual exclusion is the mechanism that ensures that if one transaction is accessing a shared resource (like updating a row), no other transaction can access it in a conflicting way at the same time. This is the core principle of concurrency control and is implemented using **locks**.

---

### 47. Binary Semaphore (in DB context)?
A binary semaphore is a simple locking mechanism (a type of mutex) that can be in one of two states: locked (0) or unlocked (1). A transaction must "acquire" the semaphore (wait until it is 1, then set it to 0) before accessing a resource and "release" it (set it back to 1) afterward. It enforces mutual exclusion on a resource.

---

### 48. Context Switching (in DB context)?
Context switching is the process of the operating system's scheduler saving the state of one running process (or thread) and restoring the state of another. In a database, this happens constantly as the DBMS server juggles requests from many different user connections. High rates of context switching can indicate CPU contention and be a performance bottleneck.

---

### 49. Can we store a binary tree in DB? How many tables required? How to regenerate?
Yes. The most common way is the **Adjacency List Model**.
*   **How many tables?** You only need **one table**.
    ```
    CREATE TABLE TreeNodes (
        NodeID INT PRIMARY KEY,
        NodeValue VARCHAR(255),
        ParentID INT REFERENCES TreeNodes(NodeID) -- Foreign key to itself
    );
    ```
    The root node would have a `NULL` `ParentID`.
*   **How to regenerate?** You can regenerate the tree structure by starting at the root (`WHERE ParentID IS NULL`) and recursively querying for children (`WHERE ParentID = current_node_id`) until all leaf nodes are found.

---

### 50. What is entity?
In the context of database design (ER diagrams), an entity is a real-world object, concept, or event about which data is stored. For example, `Student`, `Course`, and `Car` are all entities. An entity becomes a table in the database.

---

### 51. Draw ERD for a library system where students can issue books (handle multiple copies)?
*   **Entities:**
    1.  `Students` (StudentID [PK], StudentName, etc.)
    2.  `Books` (BookID [PK], Title, Author, ISBN) - Represents the book's abstract identity.
    3.  `BookCopies` (CopyID [PK], BookID [FK], Status [e.g., 'Available', 'Issued']) - Represents each physical copy.
    4.  `Issues` (IssueID [PK], CopyID [FK], StudentID [FK], IssueDate, DueDate, ReturnDate) - The transaction table.
*   **Relationships:**
    *   `Students` to `Issues`: One-to-Many (A student can have many issue records).
    *   `BookCopies` to `Issues`: One-to-Many (A specific copy can be issued many times over its life, but only once at a time).
    *   `Books` to `BookCopies`: One-to-Many (One book title can have many physical copies).

---

### 52. Write DB query: result of select * from A,B where A has 10 rows, B has 20? (Cartesian Product = 200 rows)
The query `SELECT * FROM A, B;` (without a `WHERE` clause to link them) produces a **Cartesian Product (or CROSS JOIN)**. The number of rows in the result is the number of rows in table A multiplied by the number of rows in table B.
*   **Result:** 10 rows * 20 rows = **200 rows**.

---

### 53. Query to get a list of employees working in a given department?

```sql
SELECT e.EmployeeName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE d.DepartmentName = 'Sales'; -- Or use DepartmentID
```

---

### 54. DB schema and keys related questions?
This is a general category. It refers to questions about identifying entities, attributes, primary keys, foreign keys, and designing the overall database structure for a given problem. See #5, #51 for examples.

---

### 55. How to remove many-to-many relationship?
(Covered in previous list) You resolve a many-to-many relationship by creating a **junction table** (or bridge entity) that sits between the two original tables. This new table will have foreign keys that reference the primary keys of the two tables it connects.

---

### 56. LIMIT clause - DB?
The `LIMIT` clause is used to constrain the number of rows returned by a query. It's often used for pagination (e.g., show results 11-20).
*   **Syntax (MySQL/PostgreSQL):** `LIMIT number` or `LIMIT offset, count`
    ```sql
    -- Get the top 5 highest paid employees
    SELECT EmployeeName, Salary FROM Employees ORDER BY Salary DESC LIMIT 5;
    ```
*Note: In SQL Server, the equivalent is `TOP`. In Oracle, it's `FETCH FIRST N ROWS ONLY`.*

---

### 57. Query vs stored procedure?
A **query** is a single SQL statement (`SELECT`, `INSERT`, etc.). A **stored procedure** is a set of one or more pre-compiled SQL statements saved under a name in the database, which can be executed on demand. Stored procedures can take parameters, contain logic, and improve performance, security, and reusability.

---

### 58. Left outer join query on tables?
A `LEFT OUTER JOIN` (or simply `LEFT JOIN`) returns all rows from the left table, and the matched rows from the right table. If there is no match, the columns from the right table will have `NULL` values.
```sql
SELECT
    c.CustomerName,
    o.OrderID
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID;
-- This will list all customers, even those who have never placed an order.
```

---

### 59. Design Schema for Food panda?
*   **Entities:** `Users`, `Restaurants`, `MenuItems`, `Orders`, `OrderItems`, `Drivers`.
*   **Relationships:**
    *   A `User` places many `Orders`.
    *   An `Order` is placed with one `Restaurant`.
    *   An `Order` is delivered by one `Driver`.
    *   An `Order` consists of many `OrderItems`.
    *   An `OrderItem` is a specific `MenuItem` from that `Restaurant`.
    *   A `Restaurant` has many `MenuItems`.

---

### 60. Design Schema for Car booking app?
*   **Entities:** `Users`, `Cars` (with details like model, license plate, location, status), `Bookings`.
*   **Relationships:**
    *   A `User` can make many `Bookings`.
    *   A `Car` can be part of many `Bookings` (over time).
    *   A `Booking` links one `User` to one `Car` for a specific time period (StartTime, EndTime).

---

### 61. Patient, Doctor, Appointment schema + SQL queries?
*   **Schema:**
    *   `Doctors` (DoctorID [PK], DoctorName, Speciality)
    *   `Patients` (PatientID [PK], PatientName, DOB)
    *   `Appointments` (AppointmentID [PK], DoctorID [FK], PatientID [FK], AppointmentDateTime, Status)
*   **Example SQL Query:** "Find all appointments for Dr. Smith tomorrow."
    ```sql
    SELECT p.PatientName, a.AppointmentDateTime
    FROM Appointments a
    JOIN Doctors d ON a.DoctorID = d.DoctorID
    JOIN Patients p ON a.PatientID = p.PatientID
    WHERE d.DoctorName = 'Dr. Smith' AND DATE(a.AppointmentDateTime) = CURDATE() + INTERVAL 1 DAY;
    ```

---

### 62. Difference between IN and BETWEEN operator?
*   **`IN`:** Checks if a value matches any value in a provided list. It is a shorthand for multiple `OR` conditions.
    *   `WHERE Country IN ('USA', 'Canada', 'Mexico')`
*   **`BETWEEN`:** Checks if a value falls within a continuous range (inclusive). It is a shorthand for a pair of `AND` conditions.
    *   `WHERE Salary BETWEEN 50000 AND 70000` (equivalent to `Salary >= 50000 AND Salary <= 70000`)

---

### 63. Set salary of Max as Salary of Lucifer (Example query)?
This requires a subquery.

```sql
UPDATE Employees
SET Salary = (SELECT Salary FROM Employees WHERE EmployeeName = 'Lucifer')
WHERE EmployeeName = 'Max';
```

---

### 64. Find frequency of values of a column having frequency > 1?
This is a classic use of `GROUP BY` and `HAVING`. It's used to find duplicate values in a column.

```sql
SELECT
    ColumnName,
    COUNT(ColumnName) AS Frequency
FROM TableName
GROUP BY ColumnName
HAVING COUNT(ColumnName) > 1;
```

---

### 65. How to insert data into table?
Using the `INSERT INTO` statement.

```sql
-- Inserting a single row, specifying columns
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');

-- Inserting a single row, assuming values match column order
INSERT INTO Customers VALUES (1, 'John Doe', 'New York', 'USA');
```

---

### 66. Remove a row from table?
Using the `DELETE FROM` statement with a `WHERE` clause.

```sql
DELETE FROM Customers WHERE CustomerName = 'Alfreds Futterkiste';
```

---

### 67. Difference between DBMS and RDBMS?
*   **DBMS (Database Management System):** A generic term for software that manages databases. It doesn't imply any specific data model (it could be hierarchical, network, relational, etc.).
*   **RDBMS (Relational Database Management System):** A specific type of DBMS that is based on the relational model, where data is stored in tables (relations) and relationships between data are also stored in tables. All RDBMS are DBMS, but not all DBMS are RDBMS. Examples of RDBMS include MySQL, PostgreSQL, Oracle, and SQL Server.

---

### 68. What format data is stored in NoSQL? (JSON often mentioned)
NoSQL databases use a variety of data models, not a single format. Common ones include:
*   **Document:** Data is stored in documents, often in a JSON or BSON format (e.g., MongoDB).
*   **Key-Value:** Simple pairs of a key and a value (e.g., Redis, DynamoDB).
*   **Column-Family:** Data is stored in columns rather than rows, good for aggregating over specific attributes (e.g., Cassandra, HBase).
*   **Graph:** Data is stored as nodes and edges, ideal for relationship-heavy data (e.g., Neo4j).

---

### 69. Fill factor in indexing?
The fill factor is a setting that determines the percentage of space on each leaf-level page of an index that is filled with data when the index is created or rebuilt.
*   A fill factor of **100** means the pages are completely full. This is good for read-heavy tables.
*   A fill factor of **70** means pages are 70% full, leaving 30% empty space. This is good for write-heavy tables, as it reduces page splits and improves `INSERT` performance.

---

### 70. Foreign key duplicate possible? (Yes)
**Yes**, a foreign key column can contain duplicate values. For example, in an `Orders` table, the `CustomerID` (a foreign key) can appear multiple times, because one customer can place many orders. The only constraint is that any value in the foreign key column must exist in the primary key column of the referenced table (or be `NULL`, if allowed).

---

### 71. BCNF?
**Boyce-Codd Normal Form (BCNF)** is a stricter version of 3NF. A table is in BCNF if, for every non-trivial functional dependency `X → Y`, `X` must be a superkey. In simpler terms, it means that the determinant of every dependency must be a candidate key. BCNF handles certain rare anomalies that 3NF does not.

---

### 72. Referential integrity?
Referential integrity is a database constraint that ensures that relationships between tables remain consistent. When one table has a foreign key to another table, the concept of referential integrity states that you cannot add a record to the table with the foreign key unless the corresponding record exists in the referenced table. It also governs what happens when you try to delete or update the referenced record (e.g., `CASCADE`, `SET NULL`, `RESTRICT`).

---

### 73. Cascade?
`CASCADE` is an option used in referential integrity constraints.
*   **`ON DELETE CASCADE`:** If a record in the parent table (the one with the primary key) is deleted, then the corresponding records in the child table (the one with the foreign key) will automatically be deleted.
*   **`ON UPDATE CASCADE`:** If the primary key value in the parent table is updated, then the corresponding foreign key values in the child table will automatically be updated.
Of course. Here are the answers to the third list of database questions.

---

### 74. Map many-to-many relations without a junction table?
In a relational database (RDBMS), it is **not recommended** and generally not possible to properly implement a many-to-many relationship without a junction (or bridge) table. A junction table is the standard and correct way to resolve this relationship while maintaining normalization.

Trying to do so without a junction table leads to bad design patterns, such as:
*   **Comma-Separated Lists:** Storing a list of IDs in a single column (e.g., a `Courses` column in the `Students` table containing "101, 102, 105"). This violates the First Normal Form (1NF), makes querying extremely difficult and inefficient, and removes the ability to enforce referential integrity.
*   **Redundant Columns:** Creating multiple columns like `Course1`, `Course2`, `Course3`. This is inflexible, difficult to query, and leads to wasted space.

**Conclusion:** For a normalized, scalable, and maintainable RDBMS, a junction table is the only proper solution. Some NoSQL databases, like graph or document databases, can model these relationships more directly (e.g., using arrays of IDs or embedded documents), but this is not a feature of the relational model.

---

### 75. Sequence (Database object)?
A **sequence** is a user-defined, schema-bound database object that generates a sequence of unique numbers according to specified rules.

*   **Purpose:** Its primary use is to generate unique values for primary key columns, independent of any table. Unlike an `IDENTITY` or `AUTO_INCREMENT` property, a single sequence can be used by multiple tables.
*   **Features:** Sequences are highly configurable. You can define the start value, increment value, min/max values, and whether the sequence should cycle after reaching its limit.
*   **Example (PostgreSQL/Oracle):**
    ```sql
    -- Create a sequence
    CREATE SEQUENCE user_id_seq START WITH 1 INCREMENT BY 1;

    -- Use it in an INSERT statement
    INSERT INTO Users (UserID, UserName) VALUES (NEXTVAL('user_id_seq'), 'Alice');
    ```

---

### 76. Can we create an index on primary key?
When you declare a primary key on a table, the database system **automatically creates a unique index** on that column (or columns) to enforce the key's uniqueness constraint. You do not need to create one manually. In most database systems (like SQL Server), the primary key index is a **clustered index** by default, meaning it defines the physical sort order of the data in the table.

---

### 77. Find number of employees having salary > 5000?

```sql
SELECT COUNT(*)
FROM Employees
WHERE Salary > 5000;
```

---

### 78. Find number of employees having salary > 5000 and having same name?
This query finds names that are duplicated in the `Employees` table and then counts how many of those employees also have a salary greater than 5000.

```sql
SELECT
    EmployeeName,
    COUNT(*) AS NumberOfEmployees
FROM Employees
WHERE
    Salary > 5000
AND
    EmployeeName IN (
        SELECT EmployeeName
        FROM Employees
        GROUP BY EmployeeName
        HAVING COUNT(*) > 1
    )
GROUP BY EmployeeName;
```

---

### 79. Find most recent salary of employees (versioned data)?
Assuming a `SalaryHistory` table with `EmployeeID`, `Salary`, and `EffectiveDate`. The most efficient way to solve this is with a window function.

```sql
WITH RankedSalaries AS (
    SELECT
        EmployeeID,
        Salary,
        EffectiveDate,
        ROW_NUMBER() OVER(PARTITION BY EmployeeID ORDER BY EffectiveDate DESC) as rn
    FROM SalaryHistory
)
SELECT
    EmployeeID,
    Salary,
    EffectiveDate
FROM RankedSalaries
WHERE rn = 1;
```

---

### 80. Why use database instead of files?
Databases (managed by a DBMS) offer significant advantages over storing data in flat files:
*   **Data Integrity & Consistency:** Enforces rules (constraints) to ensure data is valid. ACID properties in transactions guarantee reliability.
*   **Concurrency Control:** Manages simultaneous access by multiple users, preventing conflicts and data corruption using locking mechanisms.
*   **Reduced Redundancy:** Normalization helps eliminate redundant data, saving space and preventing anomalies.
*   **Security:** Provides robust user authentication and authorization mechanisms to control data access.
*   **Data Independence:** The data structure can be changed without requiring changes to the applications that access it.
*   **Efficient Querying:** A sophisticated query optimizer allows for complex, ad-hoc data retrieval that is far more efficient than manually parsing files.
*   **Backup and Recovery:** DBMS provides tools for easy backup and recovery from failures.

---

### 81. Can we run SQL queries on NoSQL databases?
**Sometimes.** While most NoSQL databases have their own native, non-SQL query languages (e.g., MongoDB Query Language), many have recognized the prevalence of SQL and provide SQL-like query interfaces.
*   **Examples:**
    *   **Cassandra** has CQL (Cassandra Query Language), which is very similar to SQL.
    *   **Couchbase** has N1QL (pronounced "nickel"), an SQL-like language for JSON data.
    *   Query engines like **Apache Drill**, **Presto**, or **Trino** can run SQL queries against various data sources, including NoSQL databases like MongoDB or HBase.

So, while you can't run standard SQL directly on every NoSQL database, many offer a similar syntax or can be queried via an abstraction layer that uses SQL.

---

### 82. What are DML/DDL keywords?
SQL commands are divided into sub-languages. The two main ones are:
*   **DDL (Data Definition Language):** Used to define or modify the database schema (the structure of tables, indexes, etc.).
    *   **Keywords:** `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`.
*   **DML (Data Manipulation Language):** Used for adding, deleting, and modifying the data within the tables.
    *   **Keywords:** `SELECT`, `INSERT`, `UPDATE`, `DELETE`.

---

### 83. Query to find names of employees of a specific department?
(This is a repeat, providing the answer again for clarity)

```sql
SELECT e.EmployeeName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE d.DepartmentName = 'Human Resources';
```

---

### 84. Primary, foreign, composite keys difference with example?
*   **Primary Key:** A column (or set of columns) that **uniquely identifies** each row in a table. It cannot contain `NULL` values. A table can only have one primary key.
    *   **Example:** In a `Students` table, `StudentID` is the primary key.
*   **Foreign Key:** A column (or set of columns) in one table that **references the primary key** of another table. It is used to create a link between the two tables and enforce referential integrity. It can contain duplicate values and `NULL` values (unless specified otherwise).
    *   **Example:** In an `Enrollments` table, `StudentID` is a foreign key that references the `StudentID` primary key in the `Students` table.
*   **Composite Key:** A primary key that consists of **two or more columns** that, combined, uniquely identify a row.
    *   **Example:** In an `Enrollments` table (the junction table between `Students` and `Courses`), the primary key could be the combination of `(StudentID, CourseID)`. Individually, `StudentID` or `CourseID` can be duplicated, but the pair is unique for each row.

---

### 85. Query to find customers with order count >= 5?

```sql
SELECT
    c.CustomerName,
    COUNT(o.OrderID) AS OrderCount
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID, c.CustomerName -- Group by primary key and any other selected non-aggregate columns
HAVING COUNT(o.OrderID) >= 5;
```

---

### 86. Query to find customers who didn’t place any orders yet (without sub-query and joins)?
This is a trick question, as the standard, most readable, and often most performant way is with a `LEFT JOIN`.
```sql
-- The standard and best way (using LEFT JOIN)
SELECT c.CustomerName
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
WHERE o.OrderID IS NULL;
```
If strictly forbidden from using joins or subqueries, a set operator like `EXCEPT` (in PostgreSQL/SQL Server) or `MINUS` (in Oracle) could be used, which technically fits the criteria:
```sql
-- Using EXCEPT (not a join or subquery)
SELECT CustomerID FROM Customers
EXCEPT
SELECT CustomerID FROM Orders;
```

---

### 87. A company has millions of employees, updating salary causes connection timeout. How to solve as DB analyst?
This is a common issue with large-scale updates. The single, massive `UPDATE` statement holds locks for too long, blocking other operations and eventually timing out.

**Solutions:**
1.  **Batching:** Do not update all millions of rows in one transaction. Break the operation into smaller batches (e.g., updating 10,000 rows at a time) inside a loop. This keeps transaction times short and releases locks frequently.
2.  **Indexing:** Ensure the column(s) used in the `WHERE` clause of the update are properly indexed to avoid a full table scan.
3.  **Off-Peak Processing:** Schedule the update to run during off-peak hours (e.g., overnight) when database load is low.
4.  **Use a Stored Procedure:** Encapsulate the batching logic in a stored procedure for better performance and manageability.
5.  **Check Transaction Isolation Level:** Ensure you are using an appropriate isolation level to minimize unnecessary locking.

---

### 88. How to handle multiple copies of a same book in library DB schema?
(This is a repeat of a previous question)
The correct way is to separate the abstract concept of a book from its physical copies.
1.  **`Books` Table:** Stores unique information about the title (`BookID` [PK], `Title`, `Author`, `ISBN`).
2.  **`BookCopies` Table:** Stores information about each physical copy (`CopyID` [PK], `BookID` [FK], `Status` [Available, Issued, Damaged]).

This one-to-many relationship (one `Book` has many `BookCopies`) accurately models the real world and allows you to track each physical copy individually.

---

### 89. At which column index should be applied?
Indexes should be applied strategically to improve query performance. Good candidates for indexing are:
*   **Primary Keys:** Automatically indexed to enforce uniqueness.
*   **Foreign Keys:** Crucial for improving `JOIN` performance.
*   **Columns in `WHERE` Clauses:** Columns that are frequently used to filter data.
*   **Columns in `ORDER BY` and `GROUP BY` Clauses:** Indexing these can help the database avoid costly sorting operations.

Avoid over-indexing, as each index slows down write operations (`INSERT`, `UPDATE`, `DELETE`) and consumes disk space.

---

### 90. Write a query to display product name and category name where product ID is 1 (requires join)?
Assuming a `Products` table and a `Categories` table linked by `CategoryID`.

```sql
SELECT
    p.ProductName,
    c.CategoryName
FROM Products p
JOIN Categories c ON p.CategoryID = c.CategoryID
WHERE p.ProductID = 1;
```

---

### 91. Design database of university (keys, relationships, use cases)?
This is a high-level schema design exercise.

*   **Entities & Keys:**
    *   **`Departments`**: `DepartmentID` (PK), `DepartmentName`.
    *   **`Professors`**: `ProfessorID` (PK), `ProfessorName`, `DepartmentID` (FK).
    *   **`Students`**: `StudentID` (PK), `StudentName`, `MajorDepartmentID` (FK).
    *   **`Courses`**: `CourseID` (PK), `CourseName`, `Credits`, `DepartmentID` (FK).
    *   **`Sections`**: `SectionID` (PK), `CourseID` (FK), `ProfessorID` (FK), `Semester`, `Year`, `TimeSlot`. (This table represents a specific offering of a course).
    *   **`Enrollments`**: `(StudentID, SectionID)` (Composite PK), `Grade`. (This is the junction table).

*   **Relationships:**
    *   `Departments` to `Professors`/`Students`/`Courses`: One-to-Many.
    *   `Professors` to `Sections`: One-to-Many (A professor can teach many sections).
    *   `Courses` to `Sections`: One-to-Many (A course can have many sections).
    *   `Students` and `Sections` have a **Many-to-Many** relationship, resolved by the `Enrollments` junction table.

*   **Use Cases (Sample Queries):**
    1.  **Student Registration:** `INSERT` a new row into the `Enrollments` table.
    2.  **View a Student's Transcript:** `SELECT` all courses and grades for a given `StudentID` from the `Enrollments` table, joining with `Sections` and `Courses`.
    3.  **Find all courses taught by a professor:** `SELECT` from `Courses` by joining through `Sections` for a given `ProfessorID`.
    4.  **List all students in a specific section:** `SELECT` from `Students` by joining through `Enrollments` for a given `SectionID`.