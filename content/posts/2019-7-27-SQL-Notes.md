+++
draft = false
date = 2019-07-27
title = "SQL Notes"
description = "Some notes for SQL"
tags = ["Programming Languages"]
+++

* [Basics](#basics)
* [SELECT](#select)
* [DELETE](#delete)
* [UPDATE](#update)
* [ALTER](#alter)
* [Summary Queries](#summary-queries)
* [References](#references)

## Basics

1. Create and use the database (MySQL as an example)

    ```sql
    CREATE DATABASE gregs_list;
    USE gregs_list;
    CREATE TABLE doughnut_list
    (
        doughnut_id INT NOT NULL AUTO_INCREMENT,
        doughnut_name VARCHAR(10) DEFAULT NULL,
        doughnut_type VARCHAR(8) DEFAULT NULL,
        PRIMARY KEY (doughnut_id),
        FOREIGN KEY (doughnut_type) REFERENCES foo(doughnut_type)
    ); -- Variable Char, up to 10 and 8 chars.
    DESC doughnut_list; -- Describe the table.
    -- DROP TABLE doughnut_list; -- Delete the table.
    INSERT INTO doughnut_list (doughnut_name, doughnut_type) VALUES ('hello', 'world'); -- The order doesn't matter, but they must match
    -- We can also insert multiple rows at onces (...), (...)
    SELECT * FROM doughnut_list; -- Show the table. * means all columns.
    SELECT doughnut_name FROM doughnut_list; -- Just show the name column
    SELECT * FROM doughnut_list WHERE doughnut_name = 'hello' AND doughnut_type = 'world'; -- Show the table selectively. = instead of ==
    -- Create a copy of a table
    CREATE TABLE foo_copy AS
    SELECT * FROM foo;
    ```

2. For `INSERT`, there are three variations from above:

    * Changing the order of the columns, as long as the values match them in the same order.
    * Omitting column names altogether, as long as we add all the values in the same order.
    * Leaving some columns out, as long as the values we're adding match the columns.

3. `\'` or `''` to insert data with the single quote.

4. Single line comments: `--`; multi-line comments: `/**/`.

5. In the terminal:

    ```sh
    mysql -u root -p ...
    show databases;
    use foo;
    show tables;
    ```

6. We can `INSERT` `DEFAULT` and `NULL` as well.

7. `INSERT INTO foo SELECT * FROM bar` is also possible.

8. Convert data types with `CAST(x AS foo)` and `CONVERT(x, foo)`.

9. `FORMAT(number, decimal)` formats the number. e.g. `FORMAT(2.3401, 2)` gives 2.34.

10. `CHAR(9)` is tab, `CHAR(10)` is line feed, `CHAR(13)` is carriage return. `CHAR(13, 10)` is CRLF.

11. An index speeds up the queries by letting the DB go directly to a row instead of doing a full scan. MySQL creates indexes for primary keys, foreign keys, and unique keys by default. Avoid creating indexes on columns that are updated frequently as it slows down the insert, update, and delete.

    ```sql
    CREATE INDEX invoice_date_index ON invoices (invoice_date);
    ```

12. `DECIMAL(5, 2)` means 5 digits in total with 2 decimal points inside (e.g. 123.45).

## SELECT

1. Both `<>` and `!=` mean not equal.

2. Use `IS NULL` to select null ones: `WHERE doughnut_name IS NULL;`.

3. `WHERE location LIKE '%CA';` selects all the values that end with CA. `%` replaces anything while `_` replaces one letter.

4. `WHERE value BETWEEN 1 AND 10;`.

5. `WHERE language IN ('Java', 'Python', 'C', 'C++');` is equivalent to using `OR`. Plus, we have `WHERE language NOT IN ('Java', 'Python', 'C', 'C++');`.

6. `NOT` goes right after `WHERE` when used with `BETWEEN` and `LIKE` : `WHERE NOT location LIKE '%CA';` `WHERE NOT value BETWEEN 1 AND 10;`.

7. `ORDER BY foo DESC, bar;` can be added to the end (DESC means descending.)

8. Select from two tables:

    ```sql
    SELECT o.OrderID, c.CustomerName, c.OrderDate
    FROM Orders o
    JOIN Customers c ON o.CustomerID=c.CustomerID;
    -- Self-join is also possible
    -- join a table from another db: foo.table
    -- Or do the implicit join
    SELECT o.OrderID, c.CustomerName, c.OrderDate
    FROM Orders o, Customers c
    WHERE o.CustomerID=c.CustomerID;
    -- Or with USING
    SELECT foo, bar
    FROM a
    JOIN b USING (same_column);
    -- Or natural join (based on columns with same names)
    SELECT foo, bar
    FROM a
    NATURAL JOIN b;
    -- Cross join creates the Cartesian product
    SELECT foo, bar
    FROM a CROSS JOIN b;
    ```

9. `LIMIT 100` limits the number of rows to 100. `LIMIT 2, 3` gets 3 rows that start with the third row (rows are also 0-index based).

10. We can also use `AS` like `SELECT invoice - payment - credit AS balance_due` and `SELECT CONCAT(first_name, ' ', last_name) AS full_name`. Simple alias are also possible `SELECT foo AS bar`.

11. Use `/` for division and `DIV` for integer division. Use `%` or `MOD` for modulo.

12. `LEFT`, `DATE_FORMAT`, and `ROUND`:

    ```sql
    -- LEFT for string prefixes
    SELECT first_name, last_name, CONCAT(LEFT(first_name, 1), LEFT(last_name, 1)) AS initials FROM foo;
    SELECT invoice_date, DATE_FORMAT(invoice_date, '%m/%d/%y') AS 'MM/DD/YY', DATE_FORMAT(invoice_date, '%e-%b-%Y') AS 'DD-Mon-YYYY'
    FROM invoices ORDER BY invoice_date;
    -- Round to the nearest integer
    ROUND(total)
    -- Round to one decimal place
    ROUND(total, 1)
    ```

13. `SELECT DISTINCT foo` eliminates duplicates.

14. `LIKE '%foo'` matches zero or more chars and  `LIKE '-foo'` matches one char.

15. Union.

    ```sql
    -- No duplicates
    SELECT column_name(s) FROM table1
    UNION
    SELECT column_name(s) FROM table2;
    -- Allow duplicates
    SELECT column_name(s) FROM table1
    UNION ALL
    SELECT column_name(s) FROM table2;
    ```

## DELETE

1. `DELETE FROM doughnut_list WHERE doughnut_type = 'bar';`.

2. `DELETE FROM doughnut_list;` deletes the entire table.

3. Use `SELECT` first to make sure what we're deleting is correct.

## UPDATE

1. `UPDATE doughnut_list SET doughnut_type = 'barbar' WHERE doughnut_type = 'bar';`.

2. Without `WHERE`, every possible one will be updated.

3. Use comma to seperate different columns after `SET`.

4. It can do basic math: `SET cost = cost + 1`.

## ALTER

1. `ALTER TABLE doughnut_list ADD COLUMN doughnut_id INT NOT NULL AUTO_INCREMENT FIRST, ADD PRIMARY KEY (doughnut_id);`

2. `FIRST`, `SECOND`, `LAST`, `BEFORE foo`, `AFTER foo` can be used.

3. `ALTER TABLE doughnut_list RENAME TO foo;`.

4. `ALTER TABLE doughnut_list CHANGE COlUMN foo bar INT;` changes foo to bar with int.

5. `ALTER TABLE doughnut_list MODIFY COLUMN foo VARCHAR(10);` changes the data type. We can also do `FIRST` or `LAST`.

6. `ALTER TABLE doughnut_list DROP COLUMN foo;` deletes a column.

## Summary Queries

1. `AVG()`, `SUM()`, `MIN()`, `MAX()`, `COUNT()` are aggregate functions.

## References

* [Head First SQL](https://www.goodreads.com/book/show/1782447.Head_First_SQL?from_search=true&from_srp=true&qid=LbMGiCAF6g&rank=1)
* [Murach's MySQL](https://www.goodreads.com/book/show/14543964-murach-s-mysql?from_search=true&from_srp=true&qid=OPrQio6dvx&rank=2)
