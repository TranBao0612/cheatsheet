# SQL COMMAND
## DEFINITION
- DDL (Data Definition Language): defining, altering & deleting DB structures such as tables, indexes or schemas.
- DML (Data Manipulation Language): fetch data from the database.
    + High-level, non-procedural (declarative) *(e.g. relational language SQL)*
    + Low-level, procedural: retrieve 1 record/once
- TCL (Transaction Control Language): control transaction.
- DQL (Data Query Language): manipulate the data stored in database tables.
- DCL (Data Control Language): control access to data in the database by granting or revoking permissions.
## COMMANDS
- DDL: `CREATE`, `DROP`, `ALTER`, `TRUNCATE`, `COMMENT`, `RENAME ... TO ...`
- DML: `INSERT`, `UPDATE`, `DELETE`
- TCL: `COMMIT`, `SAVEPOINT`, `ROLLBACK`, `BEGIN TRANSACTION`
- DQL: `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `DISTINCT`, `ORDER BY`, `LIMIT`
- DCL: `GRANT`, `REVOKE` 

&nbsp;

# 1. NOTE
## 1.1 Domain type
| Type       | Description                                                               |
|------------|---------------------------------------------------------------------------|
| `char(n)`    | string of fixed length                                                  |
| `varchar(n)` or `varchar2(n)` | string with max length n                               |
| `int` (or `integer`), `smallint` | (small) integer (length is machine-dependent)       |
| `real` or `double precision` | floating-point or double-precision floating-point numbers, with machine-dependent precision |
| `number(p, s)` | fixed-point number with user-specified precision, consists of: maximum total of p digits, s digits on the right side on decimal point, maximum p-s digits on the left side                               |
| `float(n)` | floating-point, user-specified precision of at least n digits.            |
| `date`     | yyyy-mm-dd                                                                |
| `time`     | hh:mm:ss                                                                  |
| `blob`     | variable length binary data, up to 128 TB                                 |
| `clob`     | timestamp with time zone                                                  |

&nbsp;

## 1.2 Handle NULL comparison
- `NULL` will result `UNKNOWN` in boolean operations (like `FALSE`, `UNKNOWN` is also rejected by `WHERE`)
- `NVL(some_val, 0)`: if `some_val` = 0, then treat it as `NULL` *(useful in not equal <> compasrison)*

## 1.3. Set operations
- Does not keep duplicated rows.
- Only works with tables which match number of columns and domain type of each column
- Example: `UNION`, `INTERSECT`, `MINUS`
- Note: the set operation `UNION ALL` **does keep duplications**.

&nbsp;

# 2. Operations with syntax
## 2.1. Create, Delete, Modify TABLE
### CREATE
- Create table:
```sql
CREATE TABLE table_name (
    <col_name_1> <col_type> <constraints>,
    <col_name_2> <col_type> <constraints>,
    ...,
    <col_name_n> <col_type> <constraints>
);
--- Example of constraint: NOT NULL , UNIQUE , ...
```
- Set primary key (3 ways)
```sql
CREATE TABLE CUSTOMERS (
    CUSTOMER_ID NUMBER(22) PRIMARY KEY, --- 1st, for atomic key without name
    FIRST_NAME VARCHAR2(10) NOT NULL,
    PRIMARY KEY (CUSTOMER_ID, FIRST_NAME), --- 2nd, for composite key without name
    CONSTRAINT PK_name PRIMARY KEY (CUSTOMER_ID, FIRST_NAME) --- 3rd, set name for primary key
);
```
- Set foreign key (2 ways) + enforce referential integrity constraints (2 ways)
```sql
CREATE TABLE CUSTOMERS (
    CUSTOMER_ID NUMBER(22) PRIMARY KEY, 
    DEPARTMENT_ID NUMBER(22) NOT NULL,
    FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPARTMENT(DEPARTMENT_ID), --- 1st, for composite key without name
    CONSTRAINT FK_name FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPARTMENT(DEPARTMENT_ID), --- 2nd, set name for foreign key
    FOREIGN KEY (DEPARTMENT_ID) 
        REFERENCES DEPARTMENT(DEPARTMENT_ID) 
        ON DELETE SET NULL, --- If references is deleted, set null
    FOREIGN KEY (DEPARTMENT_ID) 
        REFERENCES DEPARTMENT(DEPARTMENT_ID) 
        ON DELETE CASCADE, --- If references is deleted, delete whole rows that store the deleted references
);
```
### DELETE
```sql
DROP TABLE table_name;
```
### MODIFY
- Add, delete constraints of TABLE:
```sql
--- Add
ALTER TABLE table_name
    ADD PRIMARY KEY (col_name);
ALTER TABLE table_name
    ADD CONSTRAINT PK_name PRIMARY KEY (col_name);
ALTER TABLE table_name
    ADD FOREIGN KEY (col_name) REFERENCES ref_table(ref_col);
ALTER TABLE table_name
    ADD CONSTRAINT FK_name FOREIGN KEY (col_name) REFERENCES ref_table(ref_col);
--- Delete
ALTER TABLE table_name
    DROP CONSTRAINT key_name;
--- Rename
ALTER TABLE table_name
    RENAME TO new_name;
```
- Add, delete, modify columns:
```sql
--- Add & Delete
ALTER TABLE table_name
    ADD column_name datatype constraints;
ALTER TABLE table_name
    DROP COLUMN column_name;
--- Rename
ALTER TABLE table_name
    RENAME COLUMN old_name TO new_name;
--- Modify
ALTER TABLE table_name
    MODIFY COLUMN column_name datatype constraints;
```

---

## 2.2. Create, Delete, Modify ROWS
```sql
--- Insert row by row
INSERT INTO table_name --- Must insert all cols
    VALUES (val1, val2, ...);
INSERT INTO table_name(col1, col2, ..., coln)
    VALUES (val1, val2, ..., valn); --- Nhét đủ số cột mình ghi là được
--- Insert data from another table
INSERT INTO table_name
    SELECT * FROM other_table;
INSERT INTO table_name(col1, col2)
    SELECT other1, other2 FROM other_table;
--- Update rows
UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;
--- Drop rows
DELETE FROM table_name WHERE condition;
--- Drop all rows
    --- Solution 1: Faster, but cannot be rolled back
TRUNCATE TABLE table_name;
    --- Solution 2: Safer with referential integrity, can be rolled back
DELETE FROM table_name;
```

---

## 2.3. Queries
```sql
--- Example with appropriate keywords
SELECT DISTINCT att1 AS "Name1", att2, att3 AS "Name 3", ...
FROM table1 alias1
    JOIN table2 alias2 ON condition
    JOIN tableN aliasN ON condition
WHERE condition LIKE ''
GROUP BY att1, att2, ...
HAVING group_condition
ORDER BY att1, att2, att3 ASC --- or DESC
LIMIT number_of_rows OFFSET start_index_in_returned_table; --- max number of rows to be returned, OFFSET is optional
``` 
- `AS` rename the column in the query and not affect the original table
- `LIKE` check if value contain the substring ''
    + `'%'` or `'*'`: n chars
    + `'_'`: 1 char
- `GROUP BY` with more than 2 atts means treating that tuples as 1 single value
- group_condition of `HAVING` is for those cannot specified by `WHERE` (Ex: Aggregate functions)
- `ORDER BY` order rows by attributes with priority decreasing from left to right, can specify order condition more detail by `ORDER BY att1 ASC, att2 DESC, att3 ASC, ...`
- A few aggregate functions in sql: `MIN()`, `MAX()`, `COUNT()`, `SUM()`, `AVG()`
- Result table return by grouped aggregate functions have:
    + Grouping attribute(s)
    + Aggregated values
    + Any attributes that is functional dependent on set of grouping attribute(s)
- **NOTE**: Aggregate functions ignore null values (except for `COUNT(*)`).
```sql
SELECT attributes
FROM table1 p1
WHERE condition AND NOT EXISTS ( --- or EXISTS
    SELECT attributes
    FROM table2 p2
    WHERE condition
        AND p2.att2 = p1.att1    --- Join with outer query because using NOT EXISTS
);
```

---

## 2.4. Transactions: COMMIT, ROLLBACK, SAVEPOINTS
```sql
COMMIT;
ROLLBACK;

SAVEPOINT savepoint_name;
RELEASE SAVEPOINT savepoint_name;
ROLLBACK TO savepoint_name;

BEGIN TRANSACTION transaction_name; --- Starts a new transaction. All SQL commands after this are treated as part of the same transaction until COMMIT or ROLLBACK is used.
```
- `COMMIT`: makes permanent change to DB.
- `ROLLBACK`: rollback to latest commit, double rollback doesn't do anything (rollback not behave like undo).
- `SAVEPOINT`: create savepoint
    + Can create as many savepoints as wish.
    + Can Rollback to any savepoint, as long as that savepoint hasn't been released (deleted).
    + All savepoints are released automatically after `COMMIT`.
    + After rollback, all savepoints after the current point are released.
- `BEGIN TRANSACTION`:
    + New transaction increase transaction count by 1, commit decrease transaction count by 1.
    + SQL Server lock transactions and use read commited to prevent conflict for concurrent transaction. 

---

## 2.5. View and Others
### VIEW
```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition
WITH CHECK OPTION;
```
- View is a saved query, it updates automatically if the table which it retrieves data from update.
- Update data through a view is possible if it is a single view (view from 1 table, no join operation) without any aggregate function
    + View involving join may be updatable, but not guaranteed.
- `WITH CHECK OPTION` is optional. It enforce: "If update the underlying table through view, and the update make any row currently in the view disappear from the view, then that update is rejected."
---
### TRIGGER
```sql
--- View existing triggers
SHOW TRIGGERS;
--- DML Trigger: INSERT, UPDATE, DELETE
CREATE TRIGGER trigger_name
BEFORE UPDATE ON table_name --- Can replace BEFORE with AFTER or INSTEAD OF, concatenate DML with 'OR'
FOR EACH ROW
BEGIN
    [SQL statemenr to be executed]
END;
--- Example
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100),
    updated_at TIMESTAMP
);
CREATE TRIGGER update_timestamp
BEFORE UPDATE OR DELETE ON users
FOR EACH ROW
BEGIN
    SET NEW.updated_at = CURRENT_TIMESTAMP;
END;
```
- `INSTEAD OF` replace the code with defined code.
- Possible usage of `TRIGGER`: add updated timestamp, insert default value if violate domain constraints, ...
```sql
--- DDL Trigger: CREATE, ALTER, DROP TABLE
CREATE TRIGGER trigger_name
ON DATABASE --- for DDL events in current DB only, use ON SERVER for server-level DDL events
FOR CREATE_TABLE --- or ALTER_TABLE/ DROP_TABLE or all 3, concatenate DDL with ','
AS 
BEGIN
   PRINT 'you can not create table in this database';
   ROLLBACK;
END;
--- Example
CREATE TRIGGER prevent_table_creation
ON DATABASE
FOR CREATE_TABLE, ALTER_TABLE, DROP_TABLE --- AFTER is default in this case, Oracle supports BEFORE, SQL Server doesn't
AS 
BEGIN
   PRINT 'you can not create, drop and alter table in this database';
   ROLLBACK;
END;
```
### ASSERTION
```sql
CREATE ASSERTION assertion_name
CHECK(condition);
```
- Define global integrity constraint *(e.g. semantic)*
- Most DBMS only runs assertion to check only at commit time, not per-row-operation.