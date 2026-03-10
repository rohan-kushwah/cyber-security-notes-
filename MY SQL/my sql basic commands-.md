# Connecting to MySQL Server

Connect as `root` and prompt for password:

`mysql -u root -h 192.168.1.31 --ssl=FALSE -p`

Connect as `root` and provide password inline (not recommended):

`mysql -u root -h 192.168.1.31 --ssl=FALSE -pArmour@1234`

Connect as user `armour` with password:

`mysql -u armour -h 192.168.1.31 --ssl=FALSE -pArmour@1234`

---

# Basic Database Operations

Show all databases:

`SHOW DATABASES;`

Create a database:

`CREATE DATABASE db_name;`

Use (switch to) a database:

`USE db_name;`

Drop (remove) a database:

`DROP DATABASE db_name;`

Example — create and switch to a sample database:

`CREATE DATABASE armour_db; USE armour_db;`

---

# Creating and Managing Tables

Create a `users` table:

`CREATE TABLE users (   id INT,   name VARCHAR(50),   email VARCHAR(50),   password VARCHAR(50),   enable INT(1) );`

Create another table `users1` with similar structure:

`CREATE TABLE users1 (   id INT,   name VARCHAR(50),   email VARCHAR(50),   password VARCHAR(50),   enable INT(1) );`

Show all columns and metadata from `users`:

`SHOW COLUMNS FROM users;`

Display the structure of the `users` table:

`DESC users; -- or DESCRIBE users;`

Delete the `users1` table:

`DROP TABLE users1;`

Re-check tables in the current database:

`SHOW TABLES;`

---

# Inserting Data Into Tables

-- Insert first user (admin)
INSERT INTO users (id, name, email, password, enable)
VALUES (1, 'admin', 'admin@armour.com', '@rmour123', 1);

-- Insert another user
INSERT INTO users (id, name, email, password, enable)
VALUES (2, 'rahul', 'rahul@armour.com', '@rmour1232', 1);

-- Add a disabled user record
INSERT INTO users (id, name, email, password, enable)
VALUES (3, 'ankit', 'ankit@armour.com', 'pass123', 0);


### 🧩 **Inserting Multiple Rows**

You can insert **multiple rows** into a table in a **single query** using this syntax:

`INSERT INTO tablename (column1, column2, column3) VALUES (value1, value2, value3), (value4, value5, value6), (value7, value8, value9);`

---

### ✅ **Example: Inserting Multiple Users**

`INSERT INTO users (id, name, email, password, enable) VALUES (4, 'Pooja', 'pooja@armour.com', '123456', 1), (5, 'Krishna', 'krishna@armour.com', 'e49r03a', 1), (6, 'Vishal', 'vishal@armour.com', '12343221', 0);`

---

### 🔒 **MySQL Constraints**

| **Constraint**  | **Description**                                                     |
| --------------- | ------------------------------------------------------------------- |
| **NOT NULL**    | Ensures a column cannot contain `NULL` values.                      |
| **UNIQUE**      | Guarantees all values in a column are unique.                       |
| **PRIMARY KEY** | Uniquely identifies each record and automatically creates an index. |
| **FOREIGN KEY** | Establishes a relationship between tables using a column.           |
| **CHECK**       | Validates that values in a column meet a specific condition.        |
| **DEFAULT**     | Sets a default value for a column if no value is provided.          |
|                 |                                                                     |
### **Creating Tables With Constraints**

Create a `user2` table with multiple constraints like `NOT NULL`, `UNIQUE`, `CHECK`, and `DEFAULT`.

`CREATE TABLE user2 (     id INT(11) NOT NULL AUTO_INCREMENT,     name VARCHAR(255) NOT NULL,     age INT NOT NULL CHECK (age >= 16),     email VARCHAR(255) NOT NULL UNIQUE,     password VARCHAR(255) NOT NULL,     course_name VARCHAR(255) NOT NULL DEFAULT 'IT Security',     enable INT(1) NOT NULL,     PRIMARY KEY (id) );`

---

### **Alternative Syntax**

Alternative syntax for creating the same table in a single line:

`CREATE TABLE user (     id INT(11) NOT NULL AUTO_INCREMENT,     name VARCHAR(255) NOT NULL,     age INT NOT NULL CHECK (age > 16),     email VARCHAR(255) NOT NULL UNIQUE,     password VARCHAR(255) NOT NULL,     course_name VARCHAR(255) NOT NULL DEFAULT 'IT Security',     enable INT(1) NOT NULL,     PRIMARY KEY (id) );`

---

✅ **Explanation of Constraints:**

- **NOT NULL** → Prevents a column from having `NULL` values.
    
- **UNIQUE** → Ensures all values in a column are different.
    
- **CHECK** → Validates values based on a condition (e.g., `age >= 16`).
    
- **DEFAULT** → Sets a default value if none is provided.
    
- **PRIMARY KEY** → Uniquely identifies each record.
    

##  Inserting Data Into Tables With Constraints

-- 1. Insert a valid row into the user2 table
INSERT INTO user2 (id, name, age, email, password, course_name, enable)
VALUES (1, 'admin', 20, 'admin@armour.com', 'password123', 'Linux', 1);

-- 2. Insert a row while skipping the id column (auto-incremented)
INSERT INTO user2 (name, age, email, password, course_name, enable)
VALUES ('rahul', 18, 'rahul@armour.com', 'pass123', 'Linux', 1);

-- 3. Insert a row using the default value for course_name
INSERT INTO user2 (name, age, email, password, enable)
VALUES ('rahull', 18, 'rahull@armour.com', 'pass123', 1);

-- 4. Omit the age field (INVALID because age has a NOT NULL constraint)
-- This will cause an error
INSERT INTO user2 (name, email, password, enable)
VALUES ('rahull', 'rahull@armour.com', 'pass123', 1);

-- 5. Insert multiple users at once into user2
INSERT INTO user2 (name, age, email, password, enable)
VALUES
('u1', 18, 'u1@armour.com', 'pass123', 0),
('u2', 20, 'u2@armour.com', 'pass123', 1),
('u3', 21, 'u3@armour.com', 'pass333', 0),
('u4', 20, 'u4@armour.com', 'pass444', 1);


#  Read data sql select all commands --
### 🔹 1. Select all columns and all rows

`SELECT * FROM user2;`

✅ Shows every column and every record from the `user2` table.

---

### 🔹 2. Select specific columns

`SELECT id, name, email FROM user2;`

✅ Displays only the `id`, `name`, and `email` columns.

---

### 🔹 3. Select with a WHERE condition

`SELECT * FROM user2 WHERE age > 18;`

✅ Returns only users older than 18.
-- 1. Select record where id = 1
SELECT * FROM user2 WHERE id = 1;

-- 2. Select records where id is less than 5
SELECT * FROM user2 WHERE id < 5;

-- 3. Select records where id is greater than 5
SELECT * FROM user2 WHERE id > 5;

-- 4. Select records where id = 5
SELECT * FROM user2 WHERE id = 5;

-- 5. Select records where id <= 5
SELECT * FROM user2 WHERE id <= 5;

-- 6. Select records where id >= 5
SELECT * FROM user2 WHERE id >= 5;

-- 7. Select records where enable = 0 (disabled users)
SELECT * FROM user2 WHERE enable = 0;

-- 8. Select records where enable = 1 (active users)
SELECT * FROM user2 WHERE enable = 1;

-- 9. Select only id, name, and email for active users
SELECT id, name, email FROM user2 WHERE enable = 1;

-- 10. Select by specific name
SELECT id, name, email FROM user2 WHERE name = 'rahul';

-- 11. Select by specific email
SELECT id, name, email FROM user2 WHERE email = 'rahul@armour.com';

-- 12. Select users older than 20
SELECT * FROM user2 WHERE age > 20;

-- 13. Select users younger than 20
SELECT * FROM user2 WHERE age < 20;


---

### 🔹 4. Select with multiple conditions (AND / OR)

`SELECT * FROM user2 WHERE age >= 18 AND enable = 1;`

✅ Returns users aged 18 or older who are enabled (active).

## 🟩 PART 2: REGEX MATCHING IN SQL (10 EXAMPLES)

> In **MySQL**, the keyword for regex is `REGEXP` (or `RLIKE`).

Here are **10 useful examples** 👇

-- 1. Match names starting with 'r'
SELECT * FROM user2 WHERE name REGEXP '^r';

-- 2. Match names ending with 'l'
SELECT * FROM user2 WHERE name REGEXP 'l$';

-- 3. Match emails containing 'armour'
SELECT * FROM user2 WHERE email REGEXP 'armour';

-- 4. Match users whose name starts with 'a' or 'r'
SELECT * FROM user2 WHERE name REGEXP '^(a|r)';

-- 5. Match users whose name contains any number
SELECT * FROM user2 WHERE name REGEXP '[0-9]';

-- 6. Match emails ending with '.com'
SELECT * FROM user2 WHERE email REGEXP '\\.com$';

-- 7. Match users whose name has exactly 3 characters
SELECT * FROM user2 WHERE name REGEXP '^.{3}$';

-- 8. Match users whose name contains 'u' followed by any character and then 'l'
SELECT * FROM user2 WHERE name REGEXP 'u.l';

-- 9. Match users whose email does NOT match pattern (using NOT REGEXP)
SELECT * FROM user2 WHERE email NOT REGEXP '@armour\\.com$';

-- 10. Match users whose course_name is either 'Linux' or 'IT Security'
SELECT * FROM user2 WHERE course_name REGEXP '^(Linux|IT Security)$';

---

### 🧠 Quick REGEX Symbol Reference:

|Symbol|Meaning|Example|Matches|
|---|---|---|---|
|`^`|Starts with|`^r`|starts with “r”|
|`$`|Ends with|`l$`|ends with “l”|
|`.`|Any single character|`u.l`|“ual”, “u1l”, “u_l”|
|`[abc]`|Any listed character|`[ru]`|“r” or “u”|
|`[0-9]`|Any digit|`^[0-9]`|starts with a number|
|`(a|b)`|Either a or b|`(Linux|
|`+`|One or more|`u+`|“u”, “uu”, “uuu”|
|`*`|Zero or more|`a*`|empty, “a”, “aa”|
|`{n}`|Exactly n times|`^.{3}$`|exactly 3 characters|

---

### 🔹 5. Select with ORDER BY (sorting results)

`SELECT * FROM user2 ORDER BY name ASC;`

✅ Sorts results alphabetically by name (`ASC` = ascending, `DESC` = descending).\

-- 1. Sort by name ascending
SELECT * FROM user2 WHERE  BY name ASC;

-- 2. Sort by name descending
SELECT * FROM user2 ORDER BY name DESC;

-- 3. Sort by age ascending
SELECT * FROM user2 ORDER BY age ASC;

-- 4. Sort by age descending
SELECT * FROM user2 ORDER BY age DESC;

-- 5. Sort by course_name ascending
SELECT * FROM user2 ORDER BY course_name ASC;

-- 6. Sort by enable status then by name
SELECT * FROM user2 ORDER BY enable DESC, name ASC;

-- 7. Sort by email alphabetically
SELECT * FROM user2 ORDER BY email ASC;

-- 8. Sort by age (youngest first)
SELECT name, age FROM user2 ORDER BY age;

-- 9. Sort by password length (using LENGTH function)
SELECT name, password FROM user2 ORDER BY LENGTH(password) DESC;

-- 10. Sort by multiple columns: course_name then age
SELECT * FROM user2 ORDER BY course_name ASC, age DESC;

---

### 🔹 6. Select unique values (DISTINCT)

`SELECT DISTINCT course_name FROM user2;`

✅ Returns a list of unique course names only.


-- 1. Unique course names
SELECT DISTINCT course_name FROM user2;

-- 2. Unique enable values
SELECT DISTINCT enable FROM user2;

-- 3. Unique ages
SELECT DISTINCT  age FROM user2;
-- 4. Unique names
SELECT DISTINCT name FROM user2;

-- 5. Unique email domains
SELECT DISTINCT SUBSTRING_INDEX(email, '@', -1) AS domain FROM user2;

-- 6. Unique first letters of names
SELECT DISTINCT LEFT(name, 1) AS first_letter FROM user2;

-- 7. Unique password lengths
SELECT DISTINCT LENGTH(password) AS pass_length FROM user2;

-- 8. Unique combination of name and email
SELECT DISTINCT name, email FROM user2;

-- 9. Unique course and enable status
SELECT DISTINCT course_name, enable FROM user2;

-- 10. Unique age groups (divided by 10s)
SELECT DISTINCT FLOOR(age/10)*10 AS age_group FROM user2;

---

### 🔹 7. Select with LIKE (pattern matching)

`SELECT * FROM user2 WHERE email LIKE '%armour.com';`

✅ Finds users whose email ends with “armour.com”.


-- 1. Name starts with 'r'
SELECT * FROM user2 WHERE name LIKE 'r%';

-- 2. Name ends with 'l'
SELECT * FROM user2 WHERE name LIKE '%l';

-- 3. Name contains 'a'
SELECT * FROM user2 WHERE name LIKE '%a%';

-- 4. Email contains 'armour'
SELECT * FROM user2 WHERE email LIKE '%armour%';

-- 5. Email ends with '.com'
SELECT * FROM user2 WHERE email LIKE '%.com';

-- 6. Name has second letter 'a'
SELECT * FROM user2 WHERE name LIKE '_a%';

-- 7. Course name starts with 'IT'
SELECT * FROM user2 WHERE course_name LIKE 'IT%';

-- 8. Name starts with 'u' and ends with '1'
SELECT * FROM user2 WHERE name LIKE 'u%1';

-- 9. Email not like '@armour.com'
SELECT * FROM user2 WHERE email NOT LIKE '%@armour.com';

-- 10. Names with three characters only
SELECT * FROM user2 WHERE name LIKE '___';


---

### 🔹 8. Select with LIMIT (restrict number of rows)

`SELECT * FROM user2 LIMIT 3;`

✅ Displays only the first 3 records.

-- 1. Get first record
SELECT * FROM user2 LIMIT 1;

-- 2. Get first 3 records
SELECT * FROM user2 LIMIT 3;

-- 3. Get 5 records
SELECT * FROM user2 LIMIT 5;

-- 4. Skip first 2 and get next 3
SELECT * FROM user2 LIMIT 2, 3;

-- 5. Get first 2 enabled users
SELECT * FROM user2 WHERE enable = 1 LIMIT 2;

-- 6. Get oldest 2 users
SELECT * FROM user2 ORDER BY age DESC LIMIT 2;

-- 7. Get youngest user
SELECT * FROM user2 ORDER BY age ASC LIMIT 1;

-- 8. Get top 5 oldest users
SELECT * FROM user2 ORDER BY age DESC LIMIT 5;

-- 9. Get first 3 names only
SELECT name FROM user2 LIMIT 3;

-- 10. Get random 1 user (MySQL only)
SELECT * FROM user2 ORDER BY RAND() LIMIT 1;

---

### 🔹 9. Select with BETWEEN

`SELECT * FROM user2 WHERE age BETWEEN 18 AND 25;`

✅ Finds users aged between 18 and 25.
-- 1. Age between 18 and 25
SELECT * FROM user2 WHERE age BETWEEN 18 AND 25;

-- 2. ID between 1 and 5
SELECT * FROM user2 WHERE id BETWEEN 1 AND 5;

-- 3. Age not between 18 and 20
SELECT * FROM user2 WHERE age NOT BETWEEN 18 AND 20;

-- 4. ID between 2 and 8 (inclusive)
SELECT * FROM user2 WHERE id BETWEEN 2 AND 8;

-- 5. Age between 20 and 30 for enabled users
SELECT * FROM user2 WHERE enable = 1 AND age BETWEEN 20 AND 30;

-- 6. Filter by email ASCII order (a–m)
SELECT * FROM user2 WHERE email BETWEEN 'a' AND 'm';

-- 7. Password length between 6 and 10

-- 8. Age between 16 and 18 (teenagers)
SELECT * FROM user2 WHERE age BETWEEN 16 AND 18;

-- 9. Age between 25 and 40
SELECT * FROM user2 WHERE age BETWEEN 25 AND 40;

-- 10. ID between 10 and 20 (for pagination use)
SELECT * FROM user2 WHERE id BETWEEN 10 AND 20;


---

### 🔹 10. Select with IN (multiple matches)

`SELECT * FROM user2 WHERE name IN ('admin', 'rahul', 'u1');`

✅ Returns users whose names match any of the listed values.
-- 1. Names in a list
SELECT * FROM user2 WHERE name IN ('admin', 'rahul', 'u1');

-- 2. Age in (18, 20, 25)
SELECT * FROM user2 WHERE age IN (18, 20, 25);

-- 3. Course name in ('Linux', 'IT Security')
SELECT * FROM user2 WHERE course_name IN ('Linux', 'IT Security');

-- 4. ID in range 1–3
SELECT * FROM user2 WHERE id IN (1, 2, 3);

-- 5. Enabled users only (1)
SELECT * FROM user2 WHERE enable IN (1);

-- 6. Disabled users only (0)
SELECT * FROM user2 WHERE enable IN (0);

-- 7. Emails from specific users
SELECT * FROM user2 WHERE email IN ('admin@armour.com', 'rahul@armour.com');

-- 8. Names not in list
SELECT * FROM user2 WHERE name NOT IN ('u1', 'u2');

-- 9. Course_name in list of 3 values
SELECT * FROM user2 WHERE course_name IN ('Python', 'Linux', 'Networking');

-- 10. Age not in (17, 19)
SELECT * FROM user2 WHERE age NOT IN (17, 19);


---

### 🔹 11. Select with alias (renaming columns)

`SELECT name AS Username, email AS 'Email Address' FROM user2;`

✅ Renames columns in the output.

---

### 🔹 12. Select with aggregate functions

`SELECT COUNT(*) AS TotalUsers FROM user2; SELECT AVG(age) AS AverageAge FROM user2; SELECT MAX(age) AS OldestUser FROM user2; SELECT MIN(age) AS YoungestUser FROM user2;`

✅ Performs calculations on table data.

-- 1. Count total users
SELECT COUNT(*) AS total_users FROM user2;

-- 2. Average age of users
SELECT AVG(age) AS avg_age FROM user2;

-- 3. Maximum age
SELECT MAX(age) AS oldest FROM user2;

-- 4. Minimum age
SELECT MIN(age) AS youngest FROM user2;

-- 5. Count enabled users
SELECT COUNT(*) AS active_users FROM user2 WHERE enable = 1;

-- 6. Count users per course
SELECT course_name, COUNT(*) AS total FROM user2 GROUP BY course_name;

-- 7. Average age per course
SELECT course_name, AVG(age) AS avg_age FROM user2 GROUP BY course_name;

-- 8. Total users with same enable status
SELECT enable, COUNT(*) AS count FROM user2 GROUP BY enable;

-- 9. Find total and average age
SELECT COUNT(age) AS total, AVG(age) AS avg FROM user2;

-- 10. Combine multiple aggregates
SELECT COUNT(*) AS total_users, MAX(age) AS oldest, MIN(age) AS youngest, AVG(age) AS avg_age FROM user2;



---

---

# SQL UPDATE (Update Records)

---

Used to modify existing records in a table.

**Syntax:**

```sql

UPDATE table_name SET column1 = "value1", column2 = "value2" WHERE condition;

```

  

If the `WHERE` clause is omitted, **all rows** in the table will be updated.

```sql

use armour_db;

```

  

Displays all data before making any updates.

```sql

SELECT * FROM user2;

```

  

Sets the `course_name` to 'Linux' for **all records**.

```sql

UPDATE user2 SET course_name = 'Linux';

```

  

Updates the `email` of the user with `id = 1`.

```sql

UPDATE user2 SET email = 'admin123@gmail.com' WHERE id = 1;

```

  

Updates multiple columns of the user with `id = 1`.

```sql

UPDATE user2 SET enable = 1, age = 22, name = "Admin" WHERE id = 1;

```

  

Disables the user with `id = 1`.

```sql

UPDATE user2 SET enable = 0 WHERE id = 1;

```

  

Disables all users with `id` greater than 4.

```sql

UPDATE user2 SET enable = 0 WHERE id > 4;

```

  

Enables all users with `id` greater than 4.

```sql

UPDATE user2 SET enable = 1 WHERE id > 4;

```

  

Disables users whose `id` is 2 or 4.

```sql

UPDATE user2 SET enable = 0 WHERE id IN(2,4);

```

  

Disables all users whose `name` starts with "u".

```sql

UPDATE user2 SET enable = 0 WHERE name LIKE "u%";

```

  

Enables users whose `name` matches the regex pattern "ad".

```sql

UPDATE user2 SET enable = 1 WHERE name REGEXP "ad";

```

  

Enables **all users** by setting `enable = 1` for every row.

```sql

UPDATE user2 SET enable = 1;

```

## SQL DELETE (Delete Records)

---

Used to delete **existing rows** from a table.

**Syntax:**

```sql

DELETE FROM table_name WHERE condition;

```

  

If you **omit** the `WHERE` clause, **all rows** in the table will be deleted — use this with caution.

  

Displays all data before deletion.

```sql

SELECT * FROM user2;

```

  

Deletes **all rows** from the `user2` table.

```sql

DELETE FROM user2;

```

  

Deletes the record where `id` is 10.

```sql

DELETE FROM user2 WHERE id = 10;

```

  

Deletes multiple records where `id` is either 11 or 13.

```sql

DELETE FROM user2 WHERE id IN (11,13);

```

  

Deletes all records where `id` is greater than 12.

```sql

DELETE FROM user2 WHERE id > 12;

```
# COMMIT & ROLLBACK

---

### AUTOCOMMIT Mode

---

In MySQL, `autocommit` is **enabled by default**, meaning each `INSERT`, `UPDATE`, or `DELETE` operation is **immediately saved (committed)** and **cannot be rolled back** unless inside an explicit transaction.

  

### Commands Overview

---

Check the current autocommit status.

```sql

SELECT @@autocommit;

```

  

Disable autocommit mode (start manual transaction control).

```sql

SET autocommit = 0;

```

  

Enable autocommit mode (default behavior).

```sql

SET autocommit = 1;

```

  

---

### Example Transaction with COMMIT

---

All changes made during this session are **permanently saved**.

```sql

SET autocommit=0;

```

  

```sql

UPDATE user2 SET enable = 1 WHERE id = 22;

```

  

```sql

UPDATE user2 SET name = "USER1" WHERE id = 22;

```

  

```sql

UPDATE user2 SET email = "USER1@armour.com" WHERE id = 21;

```

  

```sql

INSERT INTO user2(id, name, age, email, password, course_name, enable) VALUES(29, 'admin', 20, 'admin1@armour.com', 'password123', 'Linux', 1);

```

  

```sql

COMMIT;

```

  

### Example Transaction with ROLLBACK

---

Any uncommitted changes are **discarded** after the rollback.

```sql

SET autocommit=0;

```

  

```sql

DELETE FROM user2;

```

  

Should show empty

  

```sql

SELECT * FROM user2;

```

  

```sql

ROLLBACK;

```

  

Data is restored

  

```sql

SELECT * FROM user2;

```

  

### Confirming and Resetting

---

```sql

SET autocommit=1;

```

  

```sql

SELECT @@autocommit;

```

  

### Behavior When Autocommit is Enabled

---

Rollback **won't undo changes** when autocommit is enabled — they are committed immediately.

```sql

UPDATE user2 SET name = "USER1" WHERE id = 22;

```

  

```sql

DELETE FROM user2 WHERE id = 21;

```

  

```sql

ROLLBACK;

```

  

```sql

SELECT * FROM user2;

```

  


# PRIMARY KEY & FOREIGN KEY Constraints

---

  

## SQL PRIMARY KEY Constraint

  

*   Uniquely identifies each record in a table.

*   Must contain **unique and NOT NULL** values.

*   Only one primary key per table (can be single or multiple columns).

*   Defined during `CREATE TABLE` or using `ALTER TABLE`.

  

### Syntax:

  

**While creating the table**

  

```sql

CREATE TABLE table_name (

    id INT,

    name VARCHAR(255),

    PRIMARY KEY (id)

);

```

  

**Using ALTER to add a primary key**

  

```sql

ALTER TABLE table_name

ADD PRIMARY KEY(id);

```

  

### Example:

  

```sql

CREATE TABLE user3 (

    id INT NOT NULL AUTO_INCREMENT UNIQUE,

    name VARCHAR(255) NOT NULL,

    age INT NOT NULL CHECK (age >= 16),

    gender VARCHAR(1) NOT NULL,

    email VARCHAR(255) NOT NULL UNIQUE,

    course_name VARCHAR(255) NOT NULL DEFAULT 'it security',

    PRIMARY KEY (id)

);

```

  

---

  

## SQL FOREIGN KEY Constraint

  

*   A foreign key in one table refers to the **primary key in another table**.

*   Used to enforce **referential integrity** between related tables.

*   The table with the foreign key is called the **child table**, the table it references is the **parent table**.

  

### Syntax:

  

**Inside CREATE TABLE**

  

```sql

CREATE TABLE child_table (

    id INT,

    ref_column INT,

    PRIMARY KEY (id),

    FOREIGN KEY (ref_column) REFERENCES parent_table(parent_column)

);

```

  

**Using ALTER TABLE**

  

```sql

ALTER TABLE child_table

ADD FOREIGN KEY (ref_column) REFERENCES parent_table(parent_column);

```

  

---

## Complete Example

  

### Create Parent Tables

  

```sql

CREATE TABLE course (

    course_id INT NOT NULL AUTO_INCREMENT,

    course_name VARCHAR(255),

    PRIMARY KEY (course_id)

);

```

  

```sql

CREATE TABLE city (

    city_id INT NOT NULL AUTO_INCREMENT,

    city_name VARCHAR(255),

    PRIMARY KEY (city_id)

);

```

  

### Insert Sample Data

  

```sql

INSERT INTO course (course_name)

VALUES

    ("Information Security Expert"),

    ("Ethical Hacking & Penetration Testing"),

    ("Computer Hacking & Forensic Expert"),

    ("Network Security Expert"),

    ("Website Security Expert"),

    ("Wireless Security Expert");

```

  

```sql

INSERT INTO city (city_name)

VALUES

    ('Indore'), ('Dewas'), ('Bhopal'), ('Mhow'),

    ('Ujjain'), ('Sawer'), ('Dhar'), ('Jhabua'),

    ('Mashewar');

```

---

## Create Child Table with Two Foreign Keys

  

```sql

CREATE TABLE users (

    id INT(10) NOT NULL AUTO_INCREMENT,

    name VARCHAR(255) NOT NULL,

    age INT NOT NULL,

    gender VARCHAR(1) NOT NULL,

    email VARCHAR(255) NOT NULL UNIQUE,

    course_name INT NOT NULL,

    city_name INT NOT NULL,

    PRIMARY KEY (id),

    FOREIGN KEY (course_name) REFERENCES course(course_id),

    FOREIGN KEY (city_name) REFERENCES city(city_id)

);

```

  

## Insert Data

  

```sql

INSERT INTO users(name, age, gender, email, course_name, city_name)

VALUES

    ('pankaj', 21, 'M', 'pankaj@armour.com', 2, 2),

    ('pooja', 20, 'F', 'pooja@armour.com', 3, 9),

    ('vishal', 23, 'M', 'vishal@armour.com', 5, 8),

    ('abhi', 25, 'M', 'abhi@armour.com', 1, 1),

    ('yashica', 25, 'F', 'yashica@armour.com', 3, 5),

    ('oishi', 22, 'F', 'oishi@armour.com', 5, 8),

    ('saira', 25, 'F', 'saira@armour.com', 1, 1),

    ('ridhi', 24, 'F', 'ridhi@armour.com', 2, 1),

    ('krishna', 21, 'M', 'krishna@armour.com', 1, 9),

    ('aman', 20, 'M', 'aman@armour.com', 3, 9);

```

  

## Notes

  

*   Trying to insert a value into `course_name` or `city_name` in `users` that doesn't exist in the parent table will **fail** due to referential integrity.

*   Use `SHOW CREATE TABLE users;` to inspect constraints.

  

---

✅ Displays the oldest user.

# example
CREATE TABLE student1 (
    id INT(10) NOT NULL AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    age INT NOT NULL,
    gender VARCHAR(1) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    percentage INT(3) NOT NULL,
    PRIMARY KEY (id)
);

### 🧩 **What is `ALTER TABLE`?**

The `ALTER TABLE` statement is used to **modify the structure** of an existing table in a MySQL database.

You can use it to:

- Add, delete, or modify columns
    
- Add or remove constraints (like `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`)
    
- Rename a column or the entire table
    
- Change data types of columns
    

---

### ⚙️ **Common Uses with Examples**

#### 1️⃣ **Add a New Column**

`ALTER TABLE student1 ADD COLUMN address VARCHAR(255);`

🟢 _Adds a new column named `address` to the `student1` table._

---

#### 2️⃣ **Modify an Existing Column**

`ALTER TABLE student1 MODIFY COLUMN percentage DECIMAL(5,2);`

🟢 _Changes the data type of `percentage` column to store decimal values (like 85.50)._

---

#### 3️⃣ **Rename a Column**

`ALTER TABLE student1 CHANGE COLUMN name full_name VARCHAR(255);`

🟢 _Renames the column `name` to `full_name` and keeps its data type as `VARCHAR(255)`._

---

#### 4️⃣ **Delete (Drop) a Column**

`ALTER TABLE student1 DROP COLUMN address;`

🟢 _Removes the `address` column from the table._

---

#### 5️⃣ **Rename the Table**

`ALTER TABLE student1 RENAME TO students;`

🟢 _Changes the table name from `student1` to `students`._

---

#### 6️⃣ **Add a Constraint**

`ALTER TABLE student1 ADD CONSTRAINT unique_email UNIQUE (email);`

🟢 _Ensures that no two students have the same email address._

---

### 💡 **Why is it Used?**

The `ALTER TABLE` command is used when you need to:

- **Update the database design** without deleting or recreating the table
    
- **Add new features** (like new columns or constraints)
    
- **Fix structure issues** after the table is created
    
- **Optimize data storage** by changing data types


 SQL JOINs in MySQL

***

SQL JOINs are used to combine rows from two or more tables based on a related column between them.

  

## 🔗 INNER JOIN

***

Returns **only matching** rows from both tables.

  

```sql

SELECT columns

FROM table1

INNER JOIN table2

ON table1.column = table2.column;

```

  

### Examples:

  

```sql

SELECT * FROM users

INNER JOIN course ON users.course_name = course.course_id;

```

  

```sql

SELECT u2.id, u2.name, u2.age, u2.gender, u2.email, co.course_name, ci.city_name

FROM users2 u2

INNER JOIN course co ON u2.course_name = co.course_id

INNER JOIN city ci ON u2.city_name = ci.city_id;

```

  

## LEFT JOIN

***

Returns **all rows from the left table**, and matched rows from the right. If no match, right table columns return `NULL`.

  

```sql

SELECT columns

FROM table1

LEFT JOIN table2

ON table1.column = table2.column;

```

  

### Examples:

  

```sql

SELECT users.id, users.name, users.age, users.gender, users.email, course.course_name

FROM users

LEFT JOIN course ON users.course_name = course.course_id;

```

  

```sql

SELECT u.id, u.name, u.age, u.gender, u.email, co.course_name, ci.city_name

FROM users u

LEFT JOIN city ci ON u.city_name = ci.city_id

LEFT JOIN course co ON u.course_name = co.course_id;

```

***

  

## RIGHT JOIN

***

Returns **all rows from the right table**, and matched rows from the left. If no match, left table columns return `NULL`.

  

```sql

SELECT columns

FROM table1

RIGHT JOIN table2

ON table1.column = table2.column;

```

  

### Examples:

  

```sql

SELECT * FROM users

RIGHT JOIN course ON users.course_name = course.course_id;

```

  

```sql

SELECT u.id, u.name, u.age, u.gender, u.email, co.course_name, ci.city_name

FROM users u

RIGHT JOIN city ci ON u.city_name = ci.city_id

LEFT JOIN course co ON u.course_name = co.course_id;

```

***

  

## ❌ CROSS JOIN

***

Returns the **Cartesian product** of two tables (every combination of rows).

  

```sql

SELECT columns

FROM table1

CROSS JOIN table2;

```

  

### Examples:

```sql

SELECT * FROM users2 CROSS JOIN city;

```

```sql

SELECT * FROM users2 CROSS JOIN course;

```

```sql

SELECT * FROM users2 CROSS JOIN city CROSS JOIN course;

```

*   A `FULL JOIN` is not supported in MySQL. Use `UNION` of `LEFT JOIN` and `RIGHT JOIN` with `NULL` handling if needed.

  

---

## 📝 Example Schema (Simplified)

***

  

| Table  | Key Column               | Notes                        |

| :----- | :----------------------- | :--------------------------- |

| users  | course\_name, city\_name | Foreign keys to course, city |

| course | course\_id               | Course names                 |

| city   | city\_id                 | City names                   |

  

---

## Difference between all

  

## INNER JOIN

***

  

```sql

SELECT * FROM users

INNER JOIN course ON users.course_name = course.course_id;

```

✅ **Returns:** Only users who are enrolled in a course **that exists** in the `course` table.

  

❌ **Excludes:** Any `users.course_name` values that don't have a match in `course.course_id`.

  

| users.id | name  | age | ... | course_name | ... | course_id | course_name (course) |

| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |

| 1    | Ankit | 26  | ... | 12          | ... | 12        | Ethical Hacking      |

  

## LEFT JOIN

***

  

```sql

SELECT * FROM users

LEFT JOIN course ON users.course_name = course.course_id;

```

✅ **Returns:** All users - even if their `course_name` **does not exist** in `course`.

  

💥 **If no match**, `course` columns will be `NULL`.

  

| users.id | name  | age | ... | course_name | ... | course_id | course_name (course) |

| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |

| 1    | Ankit | 26  | ... | 12          | ... | NULL      | NULL                 |

| 2    | Pooja | 20  | ... | 3           | ... | 3         | Python Programming   |

  

## RIGHT JOIN

***

  

```sql

SELECT * FROM users

RIGHT JOIN course ON users.course_name = course.course_id;

```

✅ **Returns:** All courses, even if **no users** are enrolled in them.

  

💥 **If no match**, `user` columns will be `NULL`.

  

| users.id | name | age | ... | course_name | ... | course_id | course_name (course) |

| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |

| NULL | NULL | NULL| ... | NULL        | ... | 4         | Web Development      |

| 1    | Ankit| 26  | ... | 12          | ... | 12        | Ethical Hacking      |

  

## CROSS JOIN

***

  

```sql

SELECT * FROM users CROSS JOIN course;

```

✅ **Returns:** The **Cartesian Product**. Every row from `users` is combined with every row from `course`, regardless of any matching values.

  

💥 **Warning:** This can generate a very large result set. Use with caution.

  

| users.id | name  | age | ... | course_id | course_name (course) |

| :--- | :--- | :--- | :--- | :--- | :--- |

| 1    | Ankit | 26  | ... | 12        | Ethical Hacking      |

| 1    | Ankit | 26  | ... | 3         | Python Programming   |

| 2    | Pooja | 20  | ... | 12        | Ethical Hacking      |

| 2    | Pooja | 20  | ... | 3         | Python Programming   |

# best example=
## 💡 **What is SQL JOIN?**

👉 A **JOIN** in SQL is used to **combine rows from two or more tables** based on a related column between them.  
It helps you fetch **related data** stored across multiple tables.

---

## 🧱 Example Setup

Let’s say we have two tables:

### **Table 1: student**

|student_id|name|city_id|
|---|---|---|
|1|Rohan|101|
|2|Priya|102|
|3|Aman|103|

### **Table 2: city**

|city_id|city_name|
|---|---|
|101|Mumbai|
|102|Pune|
|104|Delhi|

---

## 🧩 1. **INNER JOIN**

✅ Returns only the records that have **matching values** in both tables.

`SELECT student.name, city.city_name FROM student INNER JOIN city ON student.city_id = city.city_id;`

### 🧠 Result:

|name|city_name|
|---|---|
|Rohan|Mumbai|
|Priya|Pune|

(Only matching city IDs appear)

---

## 🧩 2. **LEFT JOIN (or LEFT OUTER JOIN)**

✅ Returns **all rows from the left table** (`student`) and **matched rows from the right table** (`city`).  
If no match → NULL is shown.

`SELECT student.name, city.city_name FROM student LEFT JOIN city ON student.city_id = city.city_id;`

### 🧠 Result:

|name|city_name|
|---|---|
|Rohan|Mumbai|
|Priya|Pune|
|Aman|NULL|

---

## 🧩 3. **RIGHT JOIN (or RIGHT OUTER JOIN)**

✅ Returns **all rows from the right table** (`city`) and **matched rows from the left table** (`student`).

`SELECT student.name, city.city_name FROM student RIGHT JOIN city ON student.city_id = city.city_id;`

### 🧠 Result:

|name|city_name|
|---|---|
|Rohan|Mumbai|
|Priya|Pune|
|NULL|Delhi|

---

## 🧩 4. **FULL JOIN (FULL OUTER JOIN)**

✅ Returns **all records when there’s a match in either left or right table.**  
👉 MySQL doesn’t support FULL JOIN directly — you can simulate it using `UNION`.

`SELECT student.name, city.city_name FROM student LEFT JOIN city ON student.city_id = city.city_id  UNION  SELECT student.name, city.city_name FROM student RIGHT JOIN city ON student.city_id = city.city_id;`

### 🧠 Result:

|name|city_name|
|---|---|
|Rohan|Mumbai|
|Priya|Pune|
|Aman|NULL|
|NULL|Delhi|

---

## 🧩 5. **CROSS JOIN**

✅ Returns **all possible combinations** of rows from both tables.

`SELECT student.name, city.city_name FROM student CROSS JOIN city;`

### 🧠 Result (3 students × 3 cities = 9 rows)

|name|city_name|
|---|---|
|Rohan|Mumbai|
|Rohan|Pune|
|Rohan|Delhi|
|Priya|Mumbai|
|Priya|Pune|
|Priya|Delhi|
|Aman|Mumbai|
|Aman|Pune|
|Aman|Delhi|

---

## 🧩 6. **SELF JOIN**

✅ A table joins with **itself**.  
Useful when you have hierarchical or relational data in one table.

### Example:

Table: `employee`

|emp_id|emp_name|manager_id|
|---|---|---|
|1|Rohan|NULL|
|2|Priya|1|
|3|Aman|1|

`SELECT e.emp_name AS Employee, m.emp_name AS Manager FROM employee e LEFT JOIN employee m ON e.manager_id = m.emp_id;`

### 🧠 Result:

|Employee|Manager|
|---|---|
|Rohan|NULL|
|Priya|Rohan|
|Aman|Rohan|

---

## 🧾 Summary Table

|Join Type|Returns|Example|
|---|---|---|
|**INNER JOIN**|Only matching rows|Common data in both|
|**LEFT JOIN**|All left + matched right|Keep all from left|
|**RIGHT JOIN**|All right + matched left|Keep all from right|
|**FULL JOIN**|All rows from both|Left + Right combined|
|**CROSS JOIN**|All combinations|Cartesian product|
|**SELF JOIN**|Join table with itself|Hierarchical data|

---

Would you like me to show all these JOINs using your **existing tables** (like `user2`, `course`, `city`) from your earlier messages?  
That way you’ll get **real working examples** from your own database.

##### You said: 
## Summary

***

  

| JOIN Type  | Returns Rows From...                     | Missing Matches From... |

| :--------- | :--------------------------------------- | :---------------------- |

| INNER JOIN | Matching records in both                 | None                    |

| LEFT JOIN  | All `users`, matched `course`            | Course data = NULL      |

| RIGHT JOIN | All `course`, matched `users`            | User data = NULL        |

| CROSS JOIN | All combinations of `users` and `course` | Not Applicable          |

  

---

---
# MySQL Group By Clause

***

The `GROUP BY` clause in MySQL is used to arrange identical data into groups. This is commonly used with aggregate functions such as `COUNT()`, `MAX()`, `MIN()`, `SUM()`, and `AVG()` to perform operations on each group of data.

  

## Syntax

***

```sql

SELECT column1, aggregate_function(column2)

FROM table_name

WHERE condition

GROUP BY column1;

```

***

  

## Using Armour_Db Database

***

Sets the working database to `armour_db`.

  

```sql

USE armour_db;

```

***

## Basic User Table Queries

***

### View All Users

Displays all records in the `users` table.

```sql

SELECT * FROM users;

```

  

### Filter Users With ID Less Than 16

Returns users with `id` less than 16.

```sql

SELECT * FROM users WHERE id < 16;

```

  

### Display Course Name Column

Returns the `course_name` values from all users.

```sql

SELECT course_name FROM users;

```

  

---

## Grouping By Course Name

***

### Group By Course Name

Groups the results by each unique `course_name`. Does not show count or other aggregates.

```sql

SELECT course_name FROM users GROUP BY course_name;

```

  

### Count Users Per Course

Shows each course and how many users are enrolled in it.

```sql

SELECT course_name, COUNT(course_name)

FROM users

GROUP BY course_name;

```

  

### Count Users Per Course With Condition

Counts users in each course, but only includes users with `id < 8`.

```sql

SELECT course_name, COUNT(course_name)

FROM users

WHERE id < 8

GROUP BY course_name;

```

  

---

## Join Users and Course Table With Grouping

***

### Inner Join To Get Course Names

Fetches proper course names using `INNER JOIN` with the `course` table.

```sql

SELECT course.course_name

FROM users

INNER JOIN course ON users.course_name = course.course_id;

```

  

### Group By Course Name After Join

Groups joined data by course name.

```sql

SELECT course.course_name

FROM users

INNER JOIN course ON users.course_name = course.course_id

GROUP BY course_name;

```

  

### Count Users Per Course With Course Names

Counts how many users are enrolled in each course using readable names.

```sql

SELECT course.course_name, COUNT(course.course_name)

FROM users

INNER JOIN course ON users.course_name = course.course_id

GROUP BY course_name;

```

  

---

## Grouping By Gender

***

### Group By Gender

Returns distinct gender values from the `users` table.

```sql

SELECT gender FROM users GROUP BY gender;

```

  

### Count Users Grouped By Gender

Counts users for each gender.

```sql

SELECT gender, COUNT(gender)

FROM users

GROUP BY gender;

```

  

### Count Female Users

Counts users who identify as female. Since only one group is returned, no `GROUP BY` is required.

```sql

SELECT gender, COUNT(gender)

FROM users

WHERE gender = 'F';

```

  

### Count Male Users

Counts users who identify as male.

```sql

SELECT gender, COUNT(gender)

FROM users

WHERE gender = 'M';

```

  

---

## Grouping By City Name

***

### List City Name IDs

Displays city ID references from the `users` table.

```sql

SELECT city_name FROM users;

```

  

### Group By City Name IDs

Returns each unique `city_name` value (as a foreign key).

```sql

SELECT city_name FROM users GROUP BY city_name;

```

  

### Count Users Per City ID

Counts users in each city based on `city_name` IDs.

```sql

SELECT city_name, COUNT(city_name)

FROM users

GROUP BY city_name;

```

  

### Count Male Users Per City ID

Counts male users in each city based on city ID.

```sql

SELECT city_name, COUNT(city_name)

FROM users

WHERE gender = 'M'

GROUP BY city_name;

```

  

---

## Join With City Table And Grouping

***

### Count Users Per City Name (All Genders)

Joins the `users` table with the `city` table and counts how many users are in each named city.

```sql

SELECT city.city_name, COUNT(city.city_name)

FROM users

INNER JOIN city ON users.city_name = city.city_id

GROUP BY city_name;

```

  

### Count Male Users Per City Name

Same as above but only counts users with gender `M`.

```sql

SELECT city.city_name, COUNT(city.city_name)

FROM users

INNER JOIN city ON users.city_name = city.city_id

WHERE gender = 'M'

GROUP BY city_name;

```

  

## Summary Table

***

  

| Task Description          | Example SQL Usage                 |

| :------------------------ | :-------------------------------- |

| Count per group           | `COUNT(column)`                   |

| Restrict before grouping  | Use `WHERE` Before `GROUP BY`     |

| Filter groups after count | Use `HAVING` (not shown above)    |

| Combine multiple tables   | Use `JOIN` followed by `GROUP BY` |

| Clarify results           | Use aliases like `AS total`       |

## Additional Notes

*   Use `HAVING` for filtering after aggregation. Example:

```sql

SELECT course_name, COUNT(*) AS total

FROM users

GROUP BY course_name

HAVING total > 3;

```

  

*   You can use `GROUP_CONCAT()` to merge values in a group:

```sql

SELECT city_name, GROUP_CONCAT(name SEPARATOR ', ')

FROM users

GROUP BY city_name;

```

  

___


HAVING Clause

---

  

The `HAVING` clause is used to filter grouped results based on aggregate functions like `COUNT()`, `SUM()`, etc.

Unlike `WHERE`, which filters before grouping, `HAVING` filters after grouping.

  
  

**Syntax:**

  

```sql

SELECT columns

FROM table_name

GROUP BY column_name(s)

HAVING condition;

```

  

---

  

View all records in the `users` table.

```sql

SELECT * FROM users;

```

  

Get all male users (use `WHERE`, not `HAVING`).

```sql

SELECT * FROM users WHERE gender = "M";

```

  

Get all female users.

```sql

SELECT * FROM users WHERE gender = "F";

```

  

List all distinct genders in the table.

```sql

SELECT gender FROM users GROUP BY gender;

```

  

Count how many users are in each gender.

```sql

SELECT gender, COUNT(gender) FROM users GROUP BY gender;

```

  

Show only male group after counting.

```sql

SELECT gender, COUNT(gender) FROM users GROUP BY gender HAVING gender = "M";

```

  

Show gender(s) with exactly 4 users.

```sql

SELECT gender, COUNT(gender) FROM users GROUP BY gender HAVING COUNT(gender) = 4;

```

  

❌ Invalid: Cannot use WHERE with aggregate function.

```sql

SELECT gender, COUNT(gender) FROM users GROUP BY gender WHERE COUNT(gender) = 4;

```

  

List all cities from users table (may include duplicates).

```sql

SELECT city_name FROM users;

```

  

Count users per city using JOIN with city table.

```sql

SELECT city.city_name, COUNT(city.city_name)

FROM users

INNER JOIN city ON users.city_name = city.city_id

GROUP BY city_name;

```

  

Show cities with exactly 2 users.

```sql

SELECT city.city_name, COUNT(city.city_name)

FROM users

INNER JOIN city ON users.city_name = city.city_id

GROUP BY city_name

HAVING COUNT(city.city_name) = 2;

```

  

Show cities with fewer than 2 users.

```sql

SELECT city.city_name, COUNT(city.city_name)

FROM users

INNER JOIN city ON users.city_name = city.city_id

GROUP BY city_name

HAVING COUNT(city.city_name) < 2;

```

  

Show cities with more than 1 user.

```sql

SELECT city.city_name, COUNT(city.city_name)

FROM users

INNER JOIN city ON users.city_name = city.city_id

GROUP BY city_name

HAVING COUNT(city.city_name) > 1;

```
#    SubQuery

---

  

A SubQuery (also called an inner query or nested query) is a query within another SQL query.

It is used to return data that will be used by the main query as a condition.

  
  

**Syntax:**

```sql

SELECT columns

FROM table1

WHERE column = (SELECT columns FROM table2 WHERE condition);

```

  

View all records in the `users` table.

```sql

SELECT * FROM users;

```

  

Show users with course_name = 1 (likely course ID).

```sql

SELECT * FROM users WHERE course_name = 1;

```

  

View all courses in the `course` table.

```sql

SELECT * FROM course;

```

  

Find users where the course name is "Information Security Expert".

```sql

SELECT * FROM users WHERE course_name = "Information Security Expert";

```

  

❌ This assumes `course_name` in `users` is a string. But if it's a foreign key (ID), use a subquery.

  

Get course ID of "Information Security Expert".

```sql

SELECT course_id FROM course WHERE course_name = "Information Security Expert";

```

  

Find users enrolled in "Information Security Expert" using a subquery.

```sql

SELECT * FROM users

WHERE course_name = (

    SELECT course_id FROM course

    WHERE course_name = "Information Security Expert"

);

```

  

Same for "Website Security Expert"

```sql

SELECT * FROM users

WHERE course_name = (

    SELECT course_id FROM course

    WHERE course_name = "Website Security Expert"

);

```

  

---

## SubQuery with EXISTS

  

The `EXISTS` operator returns `TRUE` if the subquery returns any rows.

It is used for checking the existence of data.

  

**Syntax:**

```sql

SELECT columns

FROM table1

WHERE EXISTS (SELECT columns FROM table2 WHERE condition);

```

  

Check if a course exists before selecting users.

```sql

SELECT * FROM users

WHERE EXISTS (

    SELECT course_id FROM course

    WHERE course_name = "Website Security Expert"

);

```

  

Check existence of course name with partial match (no match expected).

```sql

SELECT * FROM users

WHERE EXISTS (

    SELECT course_id FROM course

    WHERE course_name = "Website Security"

);

```

  

## SubQuery with NOT EXISTS

The `NOT EXISTS` operator returns `TRUE` if the subquery returns no rows.

It is used to filter records that are not associated with values in another table.

  

**Syntax:**

```sql

SELECT columns

FROM table1

WHERE NOT EXISTS (SELECT columns FROM table2 WHERE condition);

```

  

Select users only if the course "Website Security Expert" does not exist.

```sql

SELECT * FROM users

WHERE NOT EXISTS (

    SELECT course_id FROM course

    WHERE course_name = "Information Security Expert"

);

```

  

Check for course name that doesn't exist - query returns all users if true.

```sql

SELECT * FROM users

WHERE NOT EXISTS (

    SELECT course_id FROM course

    WHERE course_name = "Website Security"

);
```
# UNION AND UNION ALL
```
#   UNION and UNION ALL


  

Both `UNION` and `UNION ALL` are used to combine the result sets of two or more `SELECT` statements.

  

## ✅ UNION

  

*   Combines result sets and **removes duplicates**.

*   Each `SELECT` must:

    *   Return the **same number of columns**.

    *   Have **similar data types** in the corresponding columns.

    *   Use the **same column order**.

  

## ✅ UNION ALL

  

*   Combines result sets **including duplicates**.

*   Rules are the same as `UNION`, but no deduplication happens.

  

---

## 📝 Example Queries

  

Show all rows in `users` table.

```sql

SELECT * FROM users;

```

  

Show all rows in `city` table.

```sql

SELECT * FROM city;

```

  

❌ Invalid: Different column counts.

```sql

SELECT * FROM users

UNION

```

  

❌ Invalid: Left side has 2 columns, right side has all.

```sql

SELECT id, name FROM users

UNION

SELECT * FROM city;

```

  

✅ Correct: Match columns and types.

```sql

SELECT id, name FROM users

UNION

SELECT city_id, city_name FROM city;

```

  

✅ Combine names from users and city, remove duplicates.

```sql

SELECT name FROM users

UNION

SELECT city_name FROM city;

```

  

✅ Same as above, but keep duplicates.

```sql

SELECT name FROM users

UNION ALL

SELECT city_name FROM city;

```

  

✅ Combine IDs from both tables, no duplicates.

```sql

SELECT id FROM users

UNION

SELECT city_id FROM city;

```

  

```sql

SELECT course_id FROM course UNION SELECT city_id FROM city;

```

  

✅ Keep all IDs, even duplicates.

```sql

SELECT id FROM users

UNION ALL

SELECT city_id FROM city;

```

  

```sql

SELECT course_id FROM course UNION ALL SELECT city_id FROM city;

```

  

✅ Filtered union with ALL results.

```sql

SELECT id FROM users WHERE id < 5

UNION ALL

SELECT city_id FROM city;

```

  

✅ Users with id < 5 combined with all city records.

```sql

SELECT id, name FROM users WHERE id < 5

UNION

SELECT city_id, city_name FROM city;

```

  

✅ Only user with id = 5 and all cities.

```sql

SELECT id, name FROM users WHERE id = 5

UNION

SELECT city_id, city_name FROM city;

```

  

✅ Union based on matching ID = 5 in both tables.

```sql

SELECT id, name FROM users WHERE id = 5

UNION

SELECT city_id, city_name FROM city WHERE city_id = 5;

```

  

✅ Combine filtered and all users (removes duplicates).

```sql

SELECT * FROM users

WHERE course_name = (

    SELECT course_id FROM course

    WHERE course_name = "Information Security Expert"

)

UNION

SELECT * FROM users;

```

  

❌ Invalid unless column counts match:

```sql

SELECT id, name FROM users

WHERE course_name = (

    SELECT course_id FROM course

    WHERE course_name = "Information Security Expert"

)

UNION

SELECT * FROM city;

```

  

✅ Complex JOIN with UNION

```sql

SELECT u2.id, u2.name, u2.age, u2.gender, u2.email, course.course_name, city.city_name

FROM users2 u2

INNER JOIN course ON u2.course_name = course.course_id

INNER JOIN city ON u2.city_name = city.city_id

UNION

SELECT * FROM users;

```

  

❗ Must ensure both SELECTs return same column count and compatible data types.

  

---

---  

# The `IF()` function in MySQL returns a value based on a condition:

  

***

### Syntax:

```sql

SELECT column1, column2,

IF(condition, true_result, false_result) AS alias_name

FROM table_name;

```

*   If the **condition** is true -> returns the **true_result**

*   If the **condition** is false -> returns the **false_result**

***

### Example: Table Setup

  

Create a student table:

  

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

  

Insert sample data into student :

  

```sql

INSERT INTO student(name, age, gender, email, percentage) VALUES

('Amit', 18, 'M', 'amit@armour.com', 35),

('Ankit', 20, 'M', 'ankit@armour.com', 95),

('Rahul', 23, 'M', 'rahul@armour.com', 32),

('Pankaj', 24, 'M', 'ankaj@armour.com', 90),

```

  

### Examples Using IF()

*   Check all users:

    ```sql

    SELECT * FROM users;

    ```

*   Display "T" if city_name >= 5, else "F":

    ```sql

    SELECT id, name, IF(city_name >= 5, "T", "F") AS CITY FROM users;

    ```

*   Display "T" if city_name = 1, else "F":

    ```sql

    SELECT id, name, IF(city_name = 1, "T", "F") AS CITY FROM users;

    ```

*   Display "T" if gender is "M", else "F":

    ```sql

    SELECT id, name, IF(gender = "M", "T", "F") AS Gender FROM users;

    ```

### IF() Example with student Table

*   View all student records:

```sql

SELECT * FROM student;

```

*   Show pass/fail result based on percentage:

```sql

SELECT *, IF(percentage >= 33, "Pass", "Fail") AS Result FROM student;

```

*   Select specific columns and result:

```sql

SELECT id, name, percentage, IF(percentage >= 33, "Pass", "Fail") AS Result FROM student;

```

  

---

---
# CASE Clause

  

The `CASE` clause in SQL is used to perform conditional logic inside queries—similar to `if-else` or `switch-case` statements in programming.

  

### Syntax:

  

```sql

SELECT column1, column2,

  CASE

    WHEN condition1 THEN result1

    WHEN condition2 THEN result2

    ...

    ELSE default_result

  END AS alias_name

FROM table_name;

```

  

* You can use it in `SELECT`, `UPDATE`, `ORDER BY`, etc.

* It is more flexible than `IF()` and supports multiple conditions.

* View all students

  

```sql

SELECT * FROM student;

```

  

## Grade Evaluation with CASE

  

* Assign grades based on percentage:

  

```sql

SELECT id, name, email, percentage,

  CASE

    WHEN percentage >= 80 AND percentage <= 100 THEN "A+"

    WHEN percentage >= 60 AND percentage < 80 THEN "A"

    WHEN percentage >= 45 AND percentage < 60 THEN "B"

    WHEN percentage >= 33 AND percentage < 45 THEN "C"

    WHEN percentage < 33 THEN "Fail"

  END AS Grade

FROM student;

```

  

* Same query in single-line format:

  

```sql

SELECT id, name, email, percentage,

  CASE

    WHEN percentage >= 80 AND percentage <= 100 THEN "A+"

    WHEN percentage >= 60 AND percentage < 80 THEN "A"

    WHEN percentage >= 45 AND percentage < 60 THEN "B"

    WHEN percentage >= 33 AND percentage < 45 THEN "C"

    WHEN percentage < 33 THEN "Fail"

  END AS Grade

FROM student;

```

  

* Add an ELSE fallback in case of out-of-bound values:

  

```sql

SELECT id, name, email, percentage,

  CASE

    WHEN percentage >= 80 AND percentage <= 100 THEN "A+"

    WHEN percentage >= 60 AND percentage < 80 THEN "A"

    WHEN percentage >= 45 AND percentage < 60 THEN "B"

    WHEN percentage >= 33 AND percentage < 45 THEN "C"

    WHEN percentage < 33 THEN "Fail"

    ELSE "NOT Correct"

  END AS Grade

FROM student;

```

  

## Update Values with CASE

  

* Update percentage conditionally using CASE:

  

```sql

UPDATE student

SET percentage = (

  CASE id

    WHEN 4 THEN 80

    WHEN 7 THEN 45

  END

)

WHERE id IN (4, 7);

```

  

* View updated table:

```sql

SELECT * FROM student;

```

  

---

---# Arithmetic Operators in MySQL

MySQL supports standard arithmetic operators for numerical calculations.

  

### Addition ( + )

Used to add two numbers.

```sql

SELECT 20 + 2;

```

```sql

SELECT 20 + 2 AS Total;

```

```sql

SELECT 20 + 2, 25 + 59, 30 + 30 AS Total;

```

* ⚠️ Repeating alias names:

```sql

SELECT 20 + 2 AS Total, 25 + 59 AS Total, 30 + 30 AS Total;

```

* MySQL automatically converts strings to numbers:

```sql

SELECT '1990' + '2000', '2005' + '2200';

```

* Add 4 to each student's percentage:

```sql

SELECT id, name, email, (percentage + 4) FROM student;

```

  

### Subtraction ( - )

Used to subtract one number from another.

```sql

SELECT 20 - 2;

```

  

```sql

SELECT 20 - 15, 30 - 14;

```

* Strings are converted to integers:

```sql

SELECT '2150' - '1550', 1200 - '200';

```

* Subtract 4 from student percentages:

```sql

SELECT id, name, email, (percentage - 4) FROM student;

```

  

### Multiplication ( * )

Used to multiply two numbers.

```sql

SELECT 10 * 2;

```

```sql

SELECT 5 * 3, 15 * 5;

```

* Strings treated as numbers:

```sql

SELECT 21 * '3', '250' * 5;

```

* Multiply percentage by 4:

```sql

SELECT id, name, email, (percentage * 4) FROM student;

```

  

### Division ( / )

Performs floating-point division.

```sql

SELECT 10 / 2;

```

```sql

SELECT 2 / 3, 3 / 5;

```

* Strings divided as numbers:

```sql

SELECT 250 / '19', '1225' / 2;

```

* Division by zero returns NULL:

```sql

SELECT 12 / 0;

```

* Divide percentage by 4:

```sql

SELECT id, name, email, (percentage / 4) FROM student;

```

  

### Integer Division (DIV)

Performs integer division, returning the quotient only (no remainder).

```sql

SELECT 20 DIV 3;

```

```sql

SELECT 5 DIV 2, -40 DIV 3, -100 DIV -7;

```

* Works with numeric strings:

```sql

SELECT '220' DIV 3, 400 DIV '65';

```

* Integer division of percentage:

```sql

SELECT id, name, email, (percentage DIV 4) FROM student;

```

  

### Modulus ( % or MOD() )

Returns the remainder of division.

```sql

SELECT 10 % 3;

```

```sql

SELECT 257 MOD 9, MOD(222, 19);

```

* Works with numeric strings:

```sql

SELECT '2345' MOD 23;

```

* Remainder when dividing percentage by 4:

```sql

SELECT id, name, email, (percentage % 4) FROM student;

```
# ## Mathematical Functions in MySQL

These functions are used to perform various mathematical operations in SQL queries.

  

### 🔢 Basic Functions

| Function         | Description                     | Example                       |

| :--------------- | :------------------------------ | :---------------------------- |

| `ABS(x)`         | Returns the absolute value of x | `SELECT ABS(-6);`             |

| `CEIL(x)`        | Smallest integer ≥ x            | `SELECT CEIL(8.45);`          |

| `FLOOR(x)`       | Largest integer ≤ x             | `SELECT FLOOR(8.45);`         |

| `ROUND(x)`       | Rounds x to nearest integer     | `SELECT ROUND(8.45);`         |

| `ROUND(x)`       | Rounds x up if decimal ≥ 0.5    | `SELECT ROUND(8.55);`         |

| `TRUNCATE(x, y)` | Truncates x to y decimal places | `SELECT TRUNCATE(8.4567, 2);` |

  

### 🧮 Power, Root, Log

| Function         | Description                 | Example                  |

| :--------------- | :-------------------------- | :----------------------- |

| `POW(x, y)`      | Returns x raised to power y | `SELECT POW(2, 8);`      |

| `POWER(x, y)`    | Same as POW                 | `SELECT POWER(2, 5);`    |

| `SQRT(x)`        | Square root                 | `SELECT SQRT(8);`        |

| `ROUND(SQRT(x))` | Rounded square root         | `SELECT ROUND(SQRT(8));` |

| `LOG(x)`         | Natural log (base e) of x   | `SELECT LOG(2);`         |

| `LN(x)`          | Natural log of x            | `SELECT LN(2);`          |

| `LOG10(x)`       | Base-10 log                 | `SELECT LOG10(1000);`    |

| `LOG2(x)`        | Base-2 log                  | `SELECT LOG2(8);`        |

### 🎲 Random & PI

| Function | Description | Example |

| :--- | :--- | :--- |

| `RAND()` | Random float between 0 and 1 | `SELECT RAND();` |

| `RAND() * 100` | Random float between 0 and 100 | `SELECT RAND() * 100;` |

| `ROUND(RAND()*100)` | Random integer (0-100) | `SELECT ROUND(RAND() * 100);` |

| `PI()` | Value of π | `SELECT PI();` |

  

### 📐 Trigonometry

| Function | Description | Example |

| :--- | :--- | :--- |

| `SIN(x)` | Sine of x (in radians) | `SELECT SIN(PI()/2);` |

| `COS(x)` | Cosine of x | `SELECT COS(0);` |

| `TAN(x)` | Tangent of x | `SELECT TAN(1);` |

| `COT(x)` | Cotangent of x | `SELECT COT(1);` |

| `ASIN(x)` | Arc sine | `SELECT ASIN(1);` |

| `ACOS(x)` | Arc cosine | `SELECT ACOS(0);` |

| `ATAN(x)` | Arc tangent | `SELECT ATAN(1);` |

| `ATAN2(y,x)` | Arc tangent of two variables | `SELECT ATAN2(1,1);` |

| `RADIANS(x)` | Degrees to radians | `SELECT RADIANS(180);` |

| `DEGREES(x)` | Radians to degrees | `SELECT DEGREES(PI());` |

  

### 🔧 Others

| Function | Description | Example |

| :--- | :--- | :--- |

| `MOD(x, y)` | Remainder of x + y | `SELECT MOD(10, 3);` |

| `SIGN(x)` | Sign of number (-1, 0, 1) | `SELECT SIGN(-20);` |

| `CRC32(x)` | CRC32 hash value for checksum | `SELECT CRC32('hello');` |

| `CONV(n, from_base, to_base)` | Convert between number bases | `SELECT CONV(10, 10, 2);` -> `'1010'` |

  

---

---
  

---

## 📅 Date and Time Functions in MySQL

MySQL provides a wide range of built-in functions to handle dates, times, and intervals.

  

### Get Current Date & Time

| Function | Description | Example |

| :--- | :--- | :--- |

| `CURRENT_DATE()` | Returns current date (YYYY-MM-DD) | `SELECT CURRENT_DATE();` |

| `SYSDATE()` | Current date and time of function call | `SELECT SYSDATE();` |

| `NOW()` | Returns current date and time | `SELECT NOW();` |

| `CURRENT_TIME()` | Returns current time (HH:MM:SS) | `SELECT CURRENT_TIME();` |

| `CURTIME()` | Returns current time | `SELECT CURTIME();` |

  

```sql

SELECT CURRENT_DATE();

```

```sql

SELECT SYSDATE();

```

```sql

SELECT NOW();

```

```sql

SELECT CURRENT_TIME();

```

```sql

SELECT CURTIME();

```

  

### 🗓️ Date Calculations

| Function | Description | Example |

| :--- | :--- | :--- |

| `DATE_ADD(date, INTERVAL x unit)` | Add interval to date | `SELECT DATE_ADD(NOW(), INTERVAL 5 DAY);` |

| `DATE_SUB(date, INTERVAL x unit)` | Subtract interval from date | `SELECT DATE_SUB(NOW(), INTERVAL 2 MONTH);` |

| `DATEDIFF(date1, date2)` | Difference in days | `SELECT DATEDIFF('2025-06-20', '2025-06-01');` |

| `TIMEDIFF(time1, time2)` | Difference between two times | `SELECT TIMEDIFF('12:30:00', '09:00:00');` |

  

### 🥝 Extract Date Parts

| Function | Description | Example |

| :--- | :--- | :--- |

| `DAY(date)` | Day of month | `SELECT DAY(NOW());` |

| `DAYNAME(date)` | Name of the day | `SELECT DAYNAME(NOW());` |

| `MONTH(date)` | Month number | `SELECT MONTH(NOW());` |

| `MONTHNAME(date)` | Name of the month | `SELECT MONTHNAME(NOW());` |

| `YEAR(date)` | Year number | `SELECT YEAR(NOW());` |

| `HOUR(time)` | Extract hour | `SELECT HOUR(NOW());` |

| `MINUTE(time)` | Extract minute | `SELECT MINUTE(NOW());` |

| `SECOND(time)` | Extract second | `SELECT SECOND(NOW());` |

| `QUARTER(date)` | Quarter of the year (1-4) | `SELECT QUARTER(NOW());` |

| `WEEK(date)` | Week number | `SELECT WEEK(NOW());` |

| `DAYOFYEAR(date)` | Day of the year (1-366) | `SELECT DAYOFYEAR(NOW());` |

  

### 🧩 Formatting Dates

| Function | Description | Example |

| :--- | :--- | :--- |

| `DATE_FORMAT(date, format)` | Format date | `SELECT DATE_FORMAT(NOW(), '%W, %M %e, %Y');` |

| `TIME_FORMAT(time, format)` | Format time | `SELECT TIME_FORMAT(NOW(), '%h:%i:%s %p');` |

| `STR_TO_DATE(str, format)` | Parse date string | `SELECT STR_TO_DATE('20-06-2025', '%d-%m-%Y');` |

  

### 🔄 Timestamps and Conversion

| Function | Description | Example |

| :--- | :--- | :--- |

| `UNIX_TIMESTAMP()` | Current timestamp in seconds since epoch | `SELECT UNIX_TIMESTAMP();` |

| `FROM_UNIXTIME(ts)` | Convert UNIX timestamp to datetime | `SELECT FROM_UNIXTIME(1718798400);` |

| `TO_DAYS(date)` | Convert date to number of days since year 0 | `SELECT TO_DAYS(NOW());` |

| `TO_SECONDS(date)` | Seconds since year 0 | `SELECT TO_SECONDS(NOW());` |

  

### 🌐 UTC and Time Zones

| Function | Description | Example |

| :--- | :--- | :--- |

| `UTC_DATE()` | Current date in UTC | `SELECT UTC_DATE();` |

| `UTC_TIME()` | Current time in UTC | `SELECT UTC_TIME();` |

| `UTC_TIMESTAMP()` | Current UTC date and time | `SELECT UTC_TIMESTAMP();` |

| `CONVERT_TZ(dt, from_tz, to_tz)` | Convert timezone | `SELECT CONVERT_TZ(NOW(), '+00:00', '+05:30');` |

  

---

-