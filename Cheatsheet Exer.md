# EXERCISE
## 1. Map ER-Diagram -> Relational Schema
1) Regular Entity Type
2) Weak Entity Type: like (1) but PK = owner[PK] + partial key, draw referencing
3) 1:1 Relationship
    - Relationship have att => New table
    - Otherwise, add FK to 1 (total participation) side
4) 1:N Relationship: add FK to N-side
5) N:M Relationship: new table
6) Multivalues attributes: new table for each, PK = owner[PK] + self
7) N-ary Relationship: new table

## 3. Relational Algebra
- Complete set of Algebra Operation: $\sigma$, $\pi$, $-$, $\rho$, $\times$, $\cup$
- Operations: 
    - Unary: $\sigma_{condition}(R)$, $\pi_{att1, att2, ...}(R)$, $\rho_{name(name1, name2, ...)}(R)$
    - Binary: $R \underset{join condition}{\Join} S$ and (⟖, ⟕, ⟗), $R * S$, $R(A) \div S(B)$
    - Set theory: $R \cup S$, $R \cap S$, $R - S$, $R \times S$
    - Aggregate function: $\langle \textit{grouping attributes} \rangle \mathcal{F}_{func1 att1, func2 att2, ...} (R)$

## 4. Functional Dependency
- Candidate key:
    - Is SK: F<sup>+</sup> contains all atts of R
    - Is minimal
- Normalized method: 
- -> 1NF: remove multivalued & composite att
- -> 2NF: remove partial dependency from 1NF
- -> 3NF: remove transitive dependency from 2NF, for every X->A: X should be an SK/A is key attribute
- -> BCNF: 3NF first, then ensure for every X->A: X should be an SK

## 2. SQL
- Handle NULL comparison
    - `NULL` will result `UNKNOWN` in boolean operations (like `FALSE`, `UNKNOWN` is also rejected by `WHERE`)
    - `NVL(some_val, 0)`: if `some_val` = 0, then treat it as `NULL`
```sql
----------------TABLE----------------------------
CREATE TABLE table_name (
    --- attributes
    <col_name_1> <col_type> <constraints>, --- Example of constraint: NOT NULL , UNIQUE , ...
    --- PK
    <col_name_2> <col_type> PRIMARY KEY,
    PRIMARY KEY (CUSTOMER_ID, FIRST_NAME),
    CONSTRAINT PK_name PRIMARY KEY (CUSTOMER_ID, FIRST_NAME)
    --- FK
    FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPARTMENT(DEPARTMENT_ID)
        ON DELETE CASCADE,   --- or SET NULL
    CONSTRAINT FK_name FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPARTMENT(DEPARTMENT_ID)
);

DROP TABLE table_name;

ALTER TABLE table_name
    RENAME TO new_name;


------------COLUMN------------------------------
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


------------ROW---------------------------------
--- Insert row by row
INSERT INTO table_name(col1, col2, ..., coln) 
    VALUES (val1, val2, ..., valn);
--- Insert data from another table
INSERT INTO table_name(col1, col2)
    SELECT other1, other2 FROM other_table;
--- Update rows
UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;
--- Drop rows
DELETE FROM table_name WHERE condition;
--- Drop all rows
TRUNCATE TABLE table_name;  --- Solution 1: Faster, but cannot be rolled back
DELETE FROM table_name;  --- Solution 2: Safer with referential integrity, can be rolled back


-----------QUERIES--------------------------------
SELECT DISTINCT att1 AS "Name1", att2, att3 AS "Name 3", ... --- Aggregate: MIN(), MAX(), COUNT(), SUM(), AVG()
FROM table1 alias1
    JOIN table2 alias2 ON condition
    JOIN tableN aliasN ON condition
WHERE condition --- LIKE '%' ('*', '_') / replace condition by (NOT) EXIST
GROUP BY att1, att2, ...
HAVING group_condition
ORDER BY att1, att2, att3 ASC --- or DESC
LIMIT number_of_rows OFFSET start_index_in_returned_table;


-----------TRANSACTION-----------------------------
COMMIT;
ROLLBACK;
SAVEPOINT savepoint_name;
RELEASE SAVEPOINT savepoint_name;
ROLLBACK TO savepoint_name;
BEGIN TRANSACTION transaction_name;


---------VIEW-------------------------------
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition
WITH CHECK OPTION;
```