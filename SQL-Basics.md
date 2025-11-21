 # SQL

 We have to learn the abstraction in SQL.

 ## Why?

 - Understanding web application
 
 > **Web application**: A web-based software that interacts dynamically with users and exchanges data in real-time between the user and server.
 >
 - Learning SQL Injection
 - Learning Mass Assignment
 - Learning IDOR

 ---

 # Databases

 ### We store data in databases for several key reasons:

 1. Manageable
 2. Efficiently store
 3. Easier to organize
 4. **Easier to access**
 5. Easier to analyze
 6. Retrieving large amounts of data

 ---

 ## SQL (**Structured** Query Language)

 ### What does SQL do?

 - used to manage relational databases
 - enables users to interact with databases

 ## NoSQL (Not Only SQL)

 ### What does NoSQL do?

 - do not follow the traditional relational data model
 - designed to handle unstructured data (store HTTP headers)

 ---

 ## Connection

 ### How do we interact with a database?

 - Query (SQL Query)
     
	 > We need a username and password to connect to the database
	 >
 
 ### root

 - The default database user is root
 - root has access to everything
     
	 > There is a difference between the root user in the operating system and the root user in a database.
	 >
	 - Other users have limited access

 ---

 # Structure

 ![Database.drawio.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/eb72149f-aebd-46a2-b9ce-50e06d3ad55b/fcf6996a-b7d4-4287-a8c8-120924c3423f/Database.drawio.png)

 ---

 # Relation

 - **Relationship between two tables:**

 ![Database.drawio2.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/eb72149f-7b7a-4d4f-a2b3-f193f8cbab14/Database.drawio2.png)

 ```sql
 FOREIGN KEY (user_id) REFERENCES Users(id)
 ```

 ### Explanation:

 - **`user_id`**: This is the column in the current table (often called a child table) that will reference the `id` column in the **`users`** table (the parent table).
 - **`REFERENCES users(id)`**: This specifies that the `user_id` in the current table must match a valid `id` in the **`users`** table.

 ---

 ### CREATE DB

 ```sql
 CREATE DATABASE test;
 USE test;
 ```

 ### CREATE TABLE

 ```sql
 CREATE TABLE users (
	 id INT PRIMARY KEY AUTO_INCREMENT,
	 username VARCHAR(50) NOT NULL,
	 email VARCHAR(100) UNIQUE NOT NULL,
	 age INT,
	 created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
 );

 SHOW TABLES;
 ```

 - **Primary Key**: A unique identifier for each record in a database table, ensuring that no two records have the same value for this key and that it cannot be NULL.
 - **Auto Increment**: A property that automatically generates a unique sequential number for a column each time a new record is inserted. (1, 2, 3, …)
 - **Timestamp**: A data type that records the date and time at which an event or action occurred. It is often used to track when a record was created or last updated.
	 - For example, `1694671800` translates to `2024-09-14 08:30:00` in UTC.

 ### INSERT

 ```sql
 INSERT INTO users (username, email, age) VALUES ('Rebian', 'Rebian@gmail.com', 18);
 ```

 ### SELECT

 ```sql
 SELECT username, email, age FROM users;
 SELECT * FROM users;
 ```

 ### WHERE

 ```sql
 SELECT * FROM users WHERE username = 'Rebian';
 SELECT * FROM users WHERE id = 1;
 SELECT * FROM users WHERE id = 1 OR id = 2;
 SELECT * FROM users WHERE id = 1 AND id = 2;

 SELECT * FROM users WHERE username = 'Rebian' AND password = 'password';   -- Login Query
 ```

 ### UPDATE

 ```sql
 UPDATE users SET age = 32 WHERE id = 1;
 ```

 ### DELETE

 ```sql
 DELETE FROM users WHERE id > 25;
 ```

 ### DESCRIBE

 ```sql
 DESCRIBE users;
 ```

 ---

 ## Tips:

 - When we install **mysql-server**, the **mysql-client** is installed simultaneously as a dependency.

 ![Database.Diagram.drawio3.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/eb72149f-26f3d76c-e83c-4232-aa23-17888556278f/Database.Diagram.drawio3.png)

 ```bash
 mysql -u root # mysql = client
 ```

 - In most cases, when you connect to MySQL as the **`root`** user from **`localhost`**, a password is not required.

 ---

 # UNION SELECT

 - to combine the results
 - results of multiple SELECT queries
 - number of columns should be equal
 - The **`UNION`** operator in SQL is used to combine the results of multiple **`SELECT`** queries, not for inserting data.

 ```sql
 SELECT username, email, age FROM users
 UNION
 SELECT user_id, email, age FROM usr;

 SELECT id, name, username FROM users
 UNION
 SELECT 1, 2, 3;

 SELECT id, name, username FROM users
 UNION
 SELECT 'navid', 'rebian', 'amir';
 ```

 # ORDER BY

 - is used to sort the result

 ```sql
 SELECT x, y, z FROM users ORDER BY y;
 ```

 ```sql
 SELECT * FROM users
 UNION
 SELECT 1, 2, 3, 1, 1, 1
 ORDER BY 2;  -- ORDER BY 2 = second column (index number)
 ```

 ---

 # INFORMATION SCHEMA

 - information schema is a database
 - automatically created when MySQL is installed
 - stores databases, tables, columns, and more (names)

 > The `information_schema` displays information about the structure of databases, tables, columns, and more, but the user's access level determines which parts of this information are visible to them.
 >

 ## important tables and columns?

 ### 1. SCHEMATA:

 - **Description:** (Databases available on the MySQL server)
	 - **SCHEMA_NAME**: (List of all database names)

 ### 2. TABLES:

 - **Description:** (Tables available on the MySQL server)
	 - **TABLE_SCHEMA**: (List of database names)
	 - **TABLE_NAME**: (List of table names)

 ### 3. COLUMNS:

 - **Description:** (Columns available on the MySQL server)
	 - **TABLE_SCHEMA**: (List of database names)
	 - **TABLE_NAME**: (List of table names)
	 - **COLUMN_NAME**: (List of columns for each table)

 ```sql
 select SCHEMA_NAME from schemata;

 select TABLE_NAME from tables;
 select TABLE_NAME from tables where TABLE_SCHEMA='mamaddb';

 select COLUMN_NAME from columns where TABLE_SCHEMA='mamaddb' and TABLE_NAME='users';
 ```

 ---

 # MYSQL

 - mysql is a database

 ## important tables and columns?

 ### 1. user:

 - **host**: (Specifies the hosts from which users can connect)
 - **user**
 - **authentication_string**: (Stores users' hashed passwords for authentication)

 ## PERMISSION

 1. database restriction
 2. table restriction
 3. column restriction

 ```sql
 SELECT host, user, authentication_string from mysql.user;
 ```

 ---

 # FUNCTIONS

 ## USER()

 - returns the current MySQL user’s name and host in the format 'user'@'host'

 ## COUNT()

 - counts the number of rows that match a specific condition

 ```sql
 select count(table_name) from tables where table_schema='mamaddb';
 select count(*) from tables where table_schema='mamaddb';
 ```

 ## VERSION()

 - to retrieve the version of the MySQL database server

 ```sql
 select version(), user();
 ```

 ## SUBSTRING(string, start_position, length)

 - extracts a substring from a given string

 ```sql
 select version(), substring(user(), 1, 5);
 ```

 ## IF(condition, value_if_true, value_if_false)

 - conditional branching within queries

 ```sql
 select if(1>1, 'Hast', 'NIST');
 ```

 ## SLEEP(seconds)

 - to introduce a delay or pause in the execution of a query

 ```sql
 select if(1>=1, sleep(10), 'NIST');
 ```

 ## CHAR(ascii_code)

 - used to convert an ASCII code into its corresponding character

 ## CONCAT(first_name, ':', last_name)

 - concatenates two or more strings together

 ```sql
 select CONCAT(name, ' <> ', username) from users;
 ```

 ## GROUP_CONCAT()

 - to concatenates values from multiple rows into a single string

 ```sql
 select GROUP_CONCAT(table_name) from tables;
 ```

 ## HEX(value)

 - used to convert a string or numeric value into its hexadecimal representation

 ---

 ## SELECT 0x1A3B

 - MySQL treats it as a hexadecimal literal

 ## LIKE

 - to perform pattern matching within string values (%, _)
 - The `LIKE` operator in databases (such as MySQL) is used for searching and comparing strings. This operator allows searching based on a pattern and is typically used for flexible searches.

 ```sql
 select email from users where email like '%exampl.com%';
 ```

 ## Escape character

 - An escape character in a database is used to prevent special characters (like `'` or `%`) from being interpreted, so they are treated as part of the data
 - Common escape characters are **`\\`** (backslash), **`''`** (double single quotes), and the character specified with `ESCAPE`

 ```sql
 SELECT '\\\\';
 SELECT "'";
 ```

 ## Comment in MySQL

 ```sql
 SELECT 1# anything
 ```

---

## SQL Injection — Security & Bug Bounty Guide (English)

This section provides a defensive, educational, and bug-bounty-oriented overview of SQL Injection (SQLi). It focuses on concepts, detection strategies, safe testing practices in controlled environments, remediation, and reporting. It intentionally avoids actionable exploitation instructions for production systems; instead it emphasizes how to safely validate and fix issues and how to communicate findings to program owners.

### Goals of this section

- Define SQL Injection and principal variants.
- Explain how vulnerabilities typically appear in code and queries (conceptually).
- Provide safe testing guidance and lab setup recommendations for researchers.
- Provide mitigation and secure-coding patterns (examples use parameterized queries).
- Offer a practical bug-bounty reporting template and triage checklist.

---

### What is SQL Injection (concise)

SQL Injection is a class of vulnerabilities that arise when untrusted input is included in database queries without proper handling, allowing an attacker to influence the structure or logic of the query. The impact ranges from data disclosure and integrity loss to full application compromise depending on context and privileges.

Key properties:

- Exploitation requires the application to construct SQL statements using input that can change query structure.
- The severity depends on the database user privileges, database features enabled (e.g., multiple statements), and surrounding application logic.

---

### High-level variants (conceptual)

- In-band SQLi: The application directly returns results or errors from the same channel the injection was delivered through. Example categories: error-based and union-like data exfiltration (described conceptually).
- Blind SQLi: The database does not return query content directly; an attacker infers results through observable side-effects (e.g., boolean responses, response timing). This is slower but still impactful.
- Out-of-band SQLi: Exfiltration happens via secondary channels the database can reach (e.g., DNS, HTTP callbacks) when available. This needs additional network capabilities.

Note: These categories describe behavioral patterns, not attack recipes.

---

### Typical vulnerable patterns (conceptual)

Vulnerabilities usually occur when application code constructs SQL statements by concatenating strings that include user-supplied data, and then sends the result directly to the database. The high-level vulnerable pattern looks like:

- Vulnerable concept: building a query string by concatenation and sending it to the DB without binding parameters.

Example (conceptual, not runnable):

- Query template: "SELECT * FROM users WHERE username = " + user_input

Why this is problematic: when `user_input` contains characters or tokens that change the logical structure of the statement, the execution path can differ from the developer's intent.

---

### How to think about detection (safe and ethical)

When assessing whether a parameter might be vulnerable, frame your actions around three principles:

1. Safety: only test on targets you are explicitly authorized to test. Use an isolated lab when possible.
2. Minimal impact: avoid destructive tests and resource-heavy queries against production systems.
3. Reproducibility: craft tests that can be repeated in a safe lab to demonstrate and validate fixes.

Detection approach (high-level):

- Observe input points that reach database-backed behavior (search terms, filters, login forms, URL parameters, headers, JSON/XML bodies).
- Look for differences in application responses when input is slightly altered (structure-changing input vs. benign input), using safe and small probes.
- Use error messages and application behavior to guide further investigation; avoid causing long-duration delays or heavy loads.

---

### Safe testing environment (recommended steps)

1. Local lab: run a deliberately vulnerable app (e.g., OWASP Juice Shop, Damn Vulnerable Web App) inside an isolated VM or container.
2. Snapshot and restore: take VM/container snapshots before active testing so you can revert to a known state.
3. Use non-production datasets: populate your lab DB with innocuous, representative data instead of real user data.
4. Authorized scope: when participating in a bug bounty, strictly follow program scope and rules of engagement.

Tools (defensive use and learning):

- Web application security training platforms (Juice Shop, DVWA, WebGoat).
- Local database servers and clients to reproduce issues and test fixes.

---

### Detection techniques (non-exploitative descriptions)

- Baseline behavior: capture and compare normal responses for representative valid inputs.
- Structural probes: send inputs that modify query shape in an isolated lab and observe any change in response structure, status codes, or execution time.
- Logic variation checks: check whether the server-side logic treats different but syntactically similar inputs in different ways (use safe permutations).
- Logging and DB monitoring: inspect application and DB logs (in your lab) for suspicious queries or errors that reveal concatenated SQL strings.

Note: In-scope bug-bounty testing should avoid overloading systems, and should never use destructive operations against live targets.

---

### Defensive code examples (safe patterns)

Use parameterized queries / prepared statements. These examples show secure binding patterns — they are safe and recommended for production.

1) PHP (PDO) — prepared statement pattern

```php
$pdo = new PDO($dsn, $user, $pass, $options);
$stmt = $pdo->prepare('SELECT id, username FROM users WHERE email = :email');
$stmt->execute(['email' => $input_email]);
$rows = $stmt->fetchAll(PDO::FETCH_ASSOC);
```

2) Python (mysql-connector / pymysql) — parameter binding

```python
cnx = mysql.connector.connect(**config)
cursor = cnx.cursor(dictionary=True)
cursor.execute("SELECT id, username FROM users WHERE email=%s", (input_email,))
rows = cursor.fetchall()
```

3) Java (JDBC) — PreparedStatement

```java
PreparedStatement ps = conn.prepareStatement("SELECT id, username FROM users WHERE email = ?");
ps.setString(1, inputEmail);
ResultSet rs = ps.executeQuery();
```

4) Node.js (mysql2) — parameterized query

```js
const [rows] = await pool.execute('SELECT id, username FROM users WHERE email = ?', [inputEmail]);
```

Why this helps: these patterns separate code (SQL structure) from data (parameters), which prevents input from being interpreted as SQL control tokens.

---

### Additional hardening recommendations

- Principle of least privilege: the DB account used by the application should have the minimum privileges required (e.g., SELECT/INSERT/UPDATE on specific tables only).
- Disable features you don't use: turn off multiple-statement execution where feasible, and restrict file-system or network access from the DB user.
- Use an ORM with parameterization defaults, but review generated SQL and avoid raw query construction without binding.
- Validate inputs positively where appropriate (e.g., numeric fields should be validated as numbers), and canonicalize input encoding.
- Escape output only when rendering into different contexts (separation of concerns: escaping is context-specific and not a substitute for parameterized queries).
- Monitor and alert: instrument queries and responses to detect anomalies (e.g., sudden increase in slow queries or errors).

---

### Safe verification strategy for confirmed issues

If you find behavior consistent with an injection risk in your authorized scope:

1. Reproduce in a local lab with a minimal example using a representative app and DB.
2. Capture non-destructive evidence: request/response pairs, application logs (if available in-scope), and a sanitized explanation of how data flows to the DB.
3. Avoid running destructive or high-impact queries on live infrastructure.

Sanitized PoC guidance: provide a minimal test case that demonstrates the behavior on a local mirror or a test endpoint. Include the exact request you sent and the exact (sanitized) response received — do not share exploit payloads that enable data exfiltration on the live target.

---

### Bug bounty triage and report template (concise, safe)

Title: [SQL Injection Risk] — [Endpoint or parameter name]

Severity: [Low/Medium/High/Critical] — justify with privileges and data impact

Scope: [Program name / URL / endpoint path — confirm it's in-scope]

Summary:

- One-line description of the issue and why it matters (e.g., ability to influence DB query logic resulting in unauthorized data access under certain conditions).

Impact:

- Which data or functionality could be affected, and the degree of privilege the application uses to access the database.

Reproduction (safe and minimal):

- Environment: describe how you tested (local lab, container snapshot, or in-scope test endpoint).
- Steps to reproduce on a local mirror (sanitized): include exact request format and the type of response that demonstrates the issue. Avoid giving active exploit strings for production targets.

Evidence:

- Attach non-sensitive request/response pairs or screenshots showing behavior differences.

Mitigation recommendation:

- Use parameterized queries/prepared statements for the affected code paths.
- Reduce DB account privileges for the application user.
- Add input validation and consider a WAF for high-risk contexts.

Notes:

- Any special constraints or temporary workarounds.

---

### Severity guidance (how to reason about impact)

- Low: Issue exists but requires additional conditions (e.g., low-privilege DB user and no sensitive data accessible).
- Medium: Potential disclosure of non-sensitive user data or ability to alter non-critical rows.
- High: Direct access to sensitive data (password hashes, PII) or ability to escalate to administrative actions.
- Critical: Full database compromise or ability to cause arbitrary code execution via DB features, or exfiltration of highly sensitive data with a single request.

When in doubt, explain assumptions in your report (DB privileges, accessible tables) and provide reproducible steps in a local environment.

---

### Common developer mistakes that lead to SQLi

- Building SQL via string concatenation with untrusted input.
- Using dynamic SQL for control flow without rigorous parameter binding.
- Granting excessive DB privileges to the application user.
- Relying solely on input escaping rather than parameterization (escaping is context-sensitive).
- Ignoring error handling that leaks useful diagnostic information to users.

---

### Defensive monitoring and logging (practical ideas)

- Log query latencies and sudden spikes in error types; investigate unusual query shapes.
- Audit the queries executed by the application in a staging environment prior to production rollout.
- Enable slow query logging to identify suspicious or repeated heavy queries that could indicate in-progress analysis.

---

### Responsible disclosure notes for bounty hunters

- Follow the program's rules: do not access account data or exfiltrate real user data beyond what is needed to prove the issue and only in authorized test environments.
- Use private communication channels the program provides for reporting.
- Provide clear remediation suggestions and help the team reproduce the issue in a test environment if asked.

---

### Quick checklist for developers (short)

- Use parameterized queries everywhere.
- Validate inputs and enforce schemas at the application boundary.
- Restrict DB user privileges.
- Avoid verbose error output in production; log details internally.
- Test fixes in an isolated lab before deploying.

---

### Further reading (educational resources)

- OWASP SQL Injection page — conceptual overview and defensive guidance.
- OWASP Top Ten — context on injection risks in web apps.
- Secure coding guides for your application's language/framework.

---

