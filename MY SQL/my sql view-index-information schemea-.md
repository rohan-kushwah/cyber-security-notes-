 MySQL VIEWs

  

A **VIEW** is a virtual table based on the result of an SQL statement. It does not store data itself but presents data from one or more tables.

  

---

### ✅ Advantages of VIEWs

*   Simplifies **complex queries** and data access.

*   Hides specific **columns** or data for security.

*   Adds an **abstraction layer** to your database.

*   Does **not consume physical space** (no data stored).

*   Provides **access restrictions**: may restrict `INSERT`, `UPDATE`, `DELETE`.

  

---

### ❌ Disadvantages of VIEWs

*   May **decrease performance** on large datasets.

*   **Dependent** on underlying tables.

*   If a **base table is dropped**, the view becomes invalid.

*   Views **load at runtime**, which may be slower.

*   Large views may **consume more memory** due to joins and processing.

  

---

### 📌 Creating a VIEW

To create a new view:

```sql

CREATE VIEW view_name AS

SELECT columns

FROM table1

JOIN table2 ON table1.col = table2.col;

```

  

#### Example:

```sql

CREATE VIEW user_info AS

SELECT u.id, u.name, u.age, u.gender, u.email,

       course.course_name, city.city_name

FROM users u

JOIN course ON u.course_name = course.course_id

JOIN city ON u.city_name = city.city_id;

```

  

---

### 🔄 Modify a VIEW

To modify an existing view without dropping it:

```sql

ALTER VIEW view_name AS

SELECT ...

```

  

#### Example:

```sql

ALTER VIEW user_info AS

SELECT u.id, u.name, u.email,

       course.course_name, city.city_name

FROM users u

JOIN course ON u.course_name = course.course_id

JOIN city ON u.city_name = city.city_id;

```

---

### ▶️ Create or Replace a VIEW

To create a view or replace it if it already exists:

```sql

CREATE OR REPLACE VIEW view_name

AS

SELECT columns

FROM table_name

INNER JOIN table2_name

ON table_name.column1 = table2_name.column1

```

#### Example:

```sql

CREATE OR REPLACE VIEW user_info AS

SELECT u.id, u.name, u.age, u.gender, u.email,

       course.course_name, city.city_name

FROM users u

JOIN course ON u.course_name = course.course_id

JOIN city ON u.city_name = city.city_id;

```

---

### ✏️ Rename a VIEW

To rename an existing view, use the `RENAME TABLE` syntax:

```sql

RENAME TABLE old_view_name TO new_view_name;

```

#### Example:

```sql

RENAME TABLE user_info TO userinfo;

```

---

### ❌ Drop a VIEW

To permanently delete a view:

```sql

DROP VIEW view_name;

```

#### Example:

```sql

DROP VIEW userinfo;

```

---

### 📋 Related Commands

To list all tables and views in the current database:

```sql

SHOW TABLES;

```

To list only the views:

```sql

SHOW FULL TABLES WHERE Table_type = 'VIEW';

```

To see the SQL statement that created a specific view:

```sql

SHOW CREATE VIEW view_name;

```

  

---

# MySQL INDEX

  

An **index** in MySQL is used to quickly look up rows based on the values in one or more columns, improving query performance.

  

---

### ✅ Why Use Indexes?

*   Speeds up `SELECT` queries.

*   Enhances performance for `WHERE`, `JOIN`, `ORDER BY`, and `GROUP BY`.

*   Automatically used for **Primary Keys** and **Unique Keys**.

  

---

### Syntax

#### Create Index

```sql

CREATE INDEX index_name ON table_name(column1, column2);

```

#### Drop Index

```sql

DROP INDEX index_name ON table_name;

```

  

---

### 📌 Example with `student` Table

#### ✅ Step 1: Create Table

```sql

CREATE TABLE student(

  id INT(10) NOT NULL AUTO_INCREMENT,

  name VARCHAR(255) NOT NULL,

  age INT NOT NULL,

  gender VARCHAR(1) NOT NULL,

  email VARCHAR(255) NOT NULL UNIQUE,

  percentage INT(3) NOT NULL,

  PRIMARY KEY(id)

);

```

#### ✅ Step 2: Insert Records

```sql

INSERT INTO student(name, age, gender, email, percentage) VALUES

('Amit', 18, 'M', 'amit@armour.com', 35),

('Ankit', 20, 'M', 'ankit@armour.com', 95),

('Rahul', 23, 'M', 'rahul@armour.com', 32),

('Pankaj', 24, 'M', 'ankaj@armour.com', 90),

('Vishal', 23, 'M', 'vishal@armour.com', 91),

('Ram', 21, 'M', 'ram@armour.com', 15),

('Veer', 20, 'M', 'veer@armour.com', 40),

('Pooja', 21, 'F', 'pooja@armour.com', 145);

```

  

```sql

show tables;

```

  

```sql

select * from student;

```

  

---

### 🔎 View Indexes

```sql

SHOW INDEX FROM student;

```

You will see:

*   `PRIMARY` index on `id`

*   `email` index (automatically created due to `UNIQUE` constraint)

  

---

### ➕ Create a New Index

```sql

CREATE INDEX students_name ON student(name);

```

#### Check:

```sql

SHOW INDEX FROM student;

```

Now you'll see an index named `students_name` on the `name` column.

---

### ❌ Drop Index

```sql

DROP INDEX students_name ON student;

```

#### Confirm:

```sql

SHOW INDEX FROM student;

```

Index will no longer be listed.

  

---

### 🔐 Notes

*   Indexes take extra disk space.

*   They slightly slow down `INSERT`, `UPDATE`, `DELETE` operations due to maintenance.

*   Indexes do **not** guarantee uniqueness unless explicitly declared with `UNIQUE`.

  

---

---# Information_schema Metadata Queries in MySQL

  

The `information_schema` database stores metadata about all other databases maintained by the MySQL server.

  

---

## What Is a Schema?

  

A **schema** is a logical container or namespace for database objects such as:

*   Tables

*   Views

*   Indexes

*   Stored Procedures

*   Triggers

*   Functions

*   Events

*   Privileges

  

---

### 📝 Schema vs Database

  

In **MySQL**, these two terms mean the **same thing**.

  

```sql

CREATE DATABASE armour_db;

```

  

-- is the same as

```sql

CREATE SCHEMA armour_db;

```

  

Both create a new schema (database) named `armour_db`.

  

---

### 🔎 View All Schemas (Databases)

```sql

SHOW DATABASES;

```

---

### 📂 Select/Use a Schema

```sql

USE armour_db;

```

---

### 📊 List All Tables in a Schema

```sql

SHOW TABLES;

```

Or using `information_schema`:

```sql

SELECT TABLE_NAME

FROM information_schema.TABLES

WHERE TABLE_SCHEMA = 'armour_db';

```

---

### 📄 See Schema Details

From `information_schema.SCHEMATA`:

```sql

SELECT * FROM information_schema.SCHEMATA;

```

This shows all schemas along with collation, default character set, and other properties.

  

---

### 💡 Example: Check Tables & Columns in a Schema

List all tables in 'armour_db':

```sql

SELECT TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'armour_db';

```

List all columns in 'users' table:

```sql

SELECT COLUMN_NAME FROM information_schema.COLUMNS

WHERE TABLE_NAME = 'users' AND TABLE_SCHEMA = 'armour_db';

```

---

### 📚 Show All Databases

```sql

SHOW DATABASES;

```

---

### 📁 Use `information_schema` Database

```sql

USE information_schema;

```

---

### 📋 List All Tables in `information_schema`

```sql

SHOW TABLES;

```

You'll see tables like: `TABLES`, `COLUMNS`, `SCHEMATA`, `STATISTICS`, `KEY_COLUMN_USAGE`, etc.

  

### 📊 Explore Tables Metadata

  

To show all available information about tables:

```sql

SELECT * FROM TABLES;

```

  

To show only the schema and table name for all tables:

```sql

SELECT TABLE_SCHEMA, TABLE_NAME FROM TABLES;

```

  

To list all unique schemas (databases) from the `TABLES` metadata:

```sql

SELECT TABLE_SCHEMA FROM TABLES GROUP BY TABLE_SCHEMA;

```

  

To filter tables by a specific schema, for example, `armour_db`:

```sql

SELECT TABLE_NAME FROM TABLES WHERE TABLE_SCHEMA = 'armour_db';

```

To filter tables by the `mysql` schema:

```sql

SELECT TABLE_NAME FROM TABLES WHERE TABLE_SCHEMA = 'mysql';

```

---

### 🔎 Explore Columns Metadata

  

To show all available information about columns:

```sql

SELECT * FROM COLUMNS;

```

  

To list all unique schemas from the `COLUMNS` metadata:

```sql

SELECT TABLE_SCHEMA FROM COLUMNS GROUP BY TABLE_SCHEMA;

```

  

To list all tables within the `armour_db` schema:

```sql

SELECT TABLE_NAME FROM COLUMNS WHERE TABLE_SCHEMA = 'armour_db' GROUP BY TABLE_NAME;

```

  

To list all tables within the `mysql` schema:

```sql

SELECT TABLE_NAME FROM COLUMNS WHERE TABLE_SCHEMA = 'mysql' GROUP BY TABLE_NAME;

```

  

To list columns of a specific table within a specific schema:

```sql

SELECT COLUMN_NAME FROM COLUMNS

WHERE TABLE_NAME = 'users' AND TABLE_SCHEMA = 'armour_db';

```

  

Or just (if the schema is currently in use):

```sql

SELECT COLUMN_NAME FROM COLUMNS WHERE TABLE_NAME = 'users';

```

  

---

  

### 🔄 Redundant Yet Valid Queries

These return the same metadata using fully qualified table names:

  

To list all schemas:

  

```sql

SELECT TABLE_SCHEMA FROM information_schema.TABLES GROUP BY TABLE_SCHEMA;

```

  

To list tables from the 'armour_db' schema:

  

```sql

SELECT TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'armour_db';

```

  

To list columns from the 'users' table:

  

```sql

SELECT COLUMN_NAME FROM information_schema.COLUMNS WHERE TABLE_NAME = 'users';

```

  

---

  

##  Summary Table for Common Queries

  

| Task | Query |

| :--- | :--- |

| List databases | `SHOW DATABASES;` |

| List tables in a schema | `SELECT TABLE_NAME FROM TABLES WHERE TABLE_SCHEMA='armour_db';` |

| List columns in a table | `SELECT COLUMN_NAME FROM COLUMNS WHERE TABLE_NAME='users' AND TABLE_SCHEMA='armour_db';` |

| List all schemas from `TABLES` | `SELECT TABLE_SCHEMA FROM TABLES GROUP BY TABLE_SCHEMA;` |

| List all tables | `SELECT * FROM TABLES;` |

| List all columns | `SELECT * FROM COLUMNS;` |

  

---

-