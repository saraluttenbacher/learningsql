# Notes

- Database used in course = Quiz Data
  - Two tables
    - people - 1000 fake quiz participants
    - states - states in USA
- In github coderspace:
  - ctrl+shift+P opens command bar
  - run command for SQLite: open database, select database and open
  - create a .sql file named working, save code there. right click and select run code to run.

 ## What is a database?

 - collection of information and relationships between data
  - stored in columns (fields) and rows (records) = tables
- all records have the same fields
- database = tables and the relationships we set between the tables
- schema = layout of fields, tables, and relationships

## What is SQL?
- Structured Query Language - manipulate and define data in database
  - how we write the questions we want to ask the database
- Write a question a computer can understand
  - series of smaller questions chained together
- SQL is used in alot of databases
- Dual role of SQL - can be used as a Data Manipulation Language or Data Definition Language
  - CRUD records
  - edit schema
  

### SQL Statements
- something you write in SQL to get info from or change a database is a statement
- SQL is whitespace independant (ignore white spaces)
- Statements made of clauses
- clauses made of key words, operators, field names, table names, and predicates
  - key words and operators are ALL CAPS
- Semicolon at end of every statement


## Select Statement
- Tells SQL we want it to choose specific piece of data
- Select statement can also return text
- ```SQL
  SELECT field_name FROM table_name;
  ```
- Single Quotes '' will return text
- ```SQL
  SELECT 'Hello, World!;
  ```
- Select multiple fields by listing them out and seperating them with a comma
- ```SQL
  SELECT first_name, last_name FROM people;
  ```
  - You can select the fields in whatever order you would like
- * is a wildcard character. It will select all the columns in a table
- ```SQL
  SELECT * FROM people;
  ```


## WHERE Statement
- Filter results from SELECT with a WHERE statement
- ```SQL
  SELECT * FROM people WHERE state_code='CA';
  ```
  - Beware: Some fields are set up as case sensitive. Searching for 'ca' returns nothing
- Use whitespace/newlines to make code easier for humans to read
- ```SQL
  SELECT first_name, last, name, shirt_or_hat
  FROM people
  WHERE
  shirt_or_hat='shirt';
  ```
- ORDER MATTERS!!! SELECT, then FROM, then WHERE. SQL will not understand a different clause order
- ```SQL
  /* Improper Statement */
  WHERE shirt_or_hat='shirt'
  FROM people
  SELECT first_name, last_name, shirt_or_hat;
  ```

## Logical Statements and Operators
- The logical statement AND lets you set multiple different criteria for a filter.
- ```SQL
  SELECT first_name, last_name FROM people WHERE state_code='CA' AND shirt_or_hat='shirt';
  ```
- ```SQL
  SELECT team, first_name, last_name FROM people WHERE state_code='CA' AND shirt_or_hat='shirt' AND team='Angry Ants';
  ```
- Adding an ! before an = tells SQL you want to exclude that value from your query
- ```SQL
  SELECT team, first_name, last_name FROM people WHERE state_code='CA' AND shirt_or_hat='shirt' AND team!='Angry Ants';
  ```
  - You can also use <> and IS NOT for not equal to.

 - You can use IS and IS NOT in place of an equals symbol
- ```SQL
  SELECT team, first_name, last_name FROM people WHERE state_code='CA' AND shirt_or_hat='shirt' AND team IS 'Angry Ants';
  ```
- ```SQL
  SELECT team, first_name, last_name FROM people WHERE state_code='CA' AND shirt_or_hat='shirt' AND team IS NOT 'Angry Ants';
  ```
- Use OR to return either of two criteria
- ```SQL
  SELECT team, first_name, last_name FROM people WHERE state_code='CA' OR state_code='CO' AND shirt_or_hat='shirt' AND team IS 'Angry Ants';
  ```
- Use parenthesis to tell SQL what order to read the clauses in
- ```SQL
  SELECT team, first_name, last_name, shirt_or_hat, state_code FROM people WHERE (state_code='CA' OR state_code='CO') AND shirt_or_hat='shirt' AND team IS 'Angry Ants';
  ```

## LIKE Operator

- Use the LIKE operator and the wildcard % to return results where part of the string is a match
- ```SQL
  SELECT first_name, last_name, state_Code
  FROM people
  WHERE state_code LIKE 'C%';
  ```
  - % can come before, after, or on both sides of the constant
  - % can be any length of text
  - some fields are case sensitive and some are not - depends on how field is set up

## LIMIT Operator

- limit the number of results that get returned with LIMIT
- ```SQL
  SELECT * FROM people
  WHERE company
  LIKE '%LLC'
  LIMIT 5;
  ```
  - returns the first n records
- use OFFSET after limit to return n records after the first k records
- ```SQL
  SELECT *
  FROM people
  WHERE company
  LIKE '%LLC'
  LIMIT 10 OFFSET 5;
  ```

## ORDER BY
- by default, SQL will return the results in the order that they are stored in the database
- Use ORDERBY to sort the result alphanumerically by the specified field.
- ```SQL
  SELECT first_name, last_name
  FROM people
  ORDER BY first_name;
  ```
  - Ascending order by default
  - You can add ASC or DESC after the column name to specify ascending or descending order. 
  - You can add multiple criteria to order by
 
## LENGTH, DISTINCT, COUNT

- to get the number of characters in each row for a specified column, use the LENGTH() command
- ```SQL
  SELECT first_name, LENGTH(first_name)
  FROM people;
  ```
- to get every unique value from a field, use DISTINCT()
- ```SQL
  SELECT DISTINCT(first_name)
  FROM people;
  ```
- Use COUNT to get the number of rows in your dataset that match your filter criteria
- ```SQL
  SELECT COUNT(company)
  FROM people
  WHERE state_code LIKE '%LLC';
  ```
  - this will return the number of companies that end in LLC in your dataset

## JOIN

- use the join command to merge data from two different tables using a common value/key
- ```SQL
  SELECT first_name, state_code 
  FROM people 
  JOIN states ON people.state_code=states.state_abbrev;
  ```
- After joining, you can continue your query, joining other tables or using WHERE clauses
- Use the table.column name convention for clarity
- Dirty join - join implicitly by selecting data from two different tables in the SELECT clause
  - Can cause issues, cross joining. Be careful
- nickname tables in the FROM clause
  - ```SQL
    SELECT ppl.first_name, st.state_name 
    FROM people ppl, states st 
    WHERE ppl.state_code=st.state_abbrev;
    ```
### Types of Joins
- Cross Join - one row of table A for each row of table B
  - Table is size of table A x table B
  - not matched on a specified value
- Inner Join - one row of table A for each matching row in table B
  - only includes rows from one table that matched up with rows from another table (inner values)
  - if no match is found, those rows are dropped (outer values)
- Left Outer Join - all rows from A, matching rows from B
  - ```SQL
    SELECT people.first_name, people.last_name, people.state_code, states.state_name 
    FROM states 
    LEFT JOIN people 
    ON people.state_code=states.state_abbrev;
    ```
  - if key isn't found in right table, NULL values returned
- Right Outer Join - all rows from B, matching rows from A

## 02_03

```SQL
/* Incorrect statement */
SELECT first_name, COUNT(first_name) FROM people;
```

```SQL
SELECT first_name, COUNT(first_name) 
FROM people 
GROUP BY first_name;
```

```SQL
/* Incorrect statement */
SELECT first_name, COUNT(first_name) 
FROM people 
GROUP BY last_name;
```

```SQL
SELECT last_name, COUNT(last_name) 
FROM people 
GROUP BY last_name;
```

```SQL
SELECT state_code, COUNT(state_code) 
FROM people 
GROUP BY state_code;
```

```SQL
/* Incorrect Statement */
SELECT state_code, quiz_points, COUNT(quiz_points)
FROM people
GROUP BY quiz_points
```

```SQL
SELECT state_code, quiz_points, COUNT(quiz_points)
FROM people
GROUP BY state_code, quiz_points
```

## 02_05

```SQL
SELECT states.state_name, COUNT(people.shirt_or_hat) 
FROM states 
JOIN people ON states.state_abbrev=people.state_code 
WHERE people.shirt_or_hat='hat'
GROUP BY people.shirt_or_hat, states.state_name;
```

```SQL
SELECT states.division, people.team, count(people.team) 
FROM states
JOIN people ON states.state_abbrev=people.state_code 
GROUP BY states.division, people.team;
```

## 03_02

```SQL
SELECT 4+2;
```

```SQL
SELECT 1/3;
```

```SQL
SELECT first_name, quiz_points FROM people WHERE quiz_points > 70;
```

```SQL
SELECT first_name, quiz_points FROM people WHERE quiz_points >= 70;
```

```SQL
SELECT first_name, quiz_points FROM people WHERE quiz_points >= 70 ORDER BY quiz_points;
```

```SQL
SELECT first_name, quiz_points FROM people WHERE quiz_points <= 70 ORDER BY quiz_points;
```

```SQL
SELECT MAX(quiz_points), MIN(quiz_points) FROM people;
```

```SQL
SELECT SUM(quiz_points) FROM people;
```

```SQL
SELECT team, COUNT(*), SUM(quiz_points), SUM(quiz_points)/COUNT(*) FROM people GROUP BY team;
```

```SQL
SELECT team, COUNT(*), SUM(quiz_points), AVG(quiz_points) FROM people GROUP BY team;
```

## 03_03

```SQL
/* Improper statement */
SELECT first_name, last_name, quiz_points FROM people WHERE quiz_points=MAX(quiz_points);
```

```SQL
SELECT first_name, last_name, quiz_points FROM people WHERE quiz_points=(SELECT MAX(quiz_points) FROM people);
```

```SQL
SELECT * FROM people WHERE state_code=(SELECT state_abbrev FROM states WHERE state_name='Minnesota');
```

## 03_04

```SQL
SELECT first_name, last_name FROM people;
```

```SQL
SELECT LOWER(first_name), UPPER(last_name) FROM people;
```

```SQL
SELECT LOWER(first_name), SUBSTR(last_name, 1, 5) FROM people;
```

```SQL
SELECT REPLACE(first_name, 'a', '-') FROM people;
```

```SQL
SELECT quiz_points FROM people ORDER BY quiz_points;
```

```SQL
SELECT quiz_points FROM people ORDER BY CAST(quiz_points AS CHAR);
```

```SQL
SELECT MAX(CAST(quiz_points AS CHAR)) FROM people;
```

```SQL
SELECT MAX(CAST(quiz_points AS INT)) FROM people;
```

## 03_05

```SQL
SELECT first_name, last_name FROM people;
```

```SQL
SELECT first_name, UPPER(last_name) FROM people;
```

```SQL
SELECT first_name as firstname, UPPER(last_name) as surname FROM people;
```

```SQL
SELECT first_name as firstname, UPPER(last_name) as surname FROM people WHERE firstname='Laura';
```

## 03_07

```SQL
SELECT state_code, max(quiz_points) AS maxpoints, avg(quiz_points) AS avgpts 
FROM people 
GROUP BY state_code 
ORDER BY avgpts DESC;
```

## 04_01

```SQL
INSERT INTO people (first_name) VALUES ('Bob');
```

```SQL
SELECT * FROM people;
```

```SQL
INSERT INTO people 
(first_name, last_name, state_code, city, shirt_or_hat)
VALUES 
('Mary', 'Hamilton', 'OR', 'Portland', 'hat');
```

```SQL
SELECT * FROM people;
```

```SQL
/* Improper Statement */
INSERT INTO people 
(first_name, last_name) 
VALUES 
('George', 'White'), 
('Jenn', 'Smith'), 
('Carol');
```

```SQL
INSERT INTO people 
(first_name, last_name) 
VALUES 
('George', 'White'), 
('Jenn', 'Smith'), 
('Carol', NULL);
```

```SQL
SELECT * FROM people;
```

## 04_02

```SQL
/* Incorrect Statement */
UPDATE people SET last_name = 'Morrison' WHERE first_name='Carlos';
```

```SQL
SELECT last_name FROM people WHERE first_name='Carlos';
```

```SQL
UPDATE people SET last_name = 'Morrison' WHERE last_name='Morrrison';
```

```SQL
SELECT last_name FROM people WHERE first_name='Carlos' AND city='Houston';
```

```SQL
UPDATE people SET last_name='Morrison'  WHERE first_name='Carlos' AND city='Houston';
```

```SQL
SELECT * FROM people WHERE id_number=175;
```

```SQL
UPDATE people SET last_name='Morrison' WHERE id_number=175;
```

```SQL
SELECT * FROM people;
```

```SQL
SELECT * FROM people WHERE company='Fisher LLC';
```

```SQL
UPDATE people SET company='Megacorp Inc' WHERE company='Fisher LLC';
```

```SQL
SELECT * FROM people WHERE company='Fisher LLC';
```

```SQL
SELECT * FROM people WHERE company='Megacorp Inc';
```

## 04_03

```SQL
/* Incorrect Statement */
DELETE FROM people;
```

```SQL
SELECT * FROM people;
```

```SQL
SELECT * FROM people WHERE id_number=1001;
```

```SQL
DELETE FROM people WHERE id_number=1001;
```

```SQL
SELECT * FROM people;
```

```SQL
SELECT * FROM people WHERE quiz_points IS NULL;
```

```SQL
DELETE FROM people WHERE quiz_points IS NULL;
```

```SQL
SELECT * FROM people;
```

```SQL
INSERT INTO people (first_name, last_name, city, state_code, shirt_or_hat, quiz_points, team, signup, age)
VALUES
("Walter", "St. John", "Buffalo", "NY", "hat", "93", "Baffled Badgers", "2021-01-29", NULL),
("Emerald", "Chou", "Topeka", "KS", "shirt", "92", "Angry Ants", "2021-01-29", 34);
```

```SQL
SELECT * FROM people;
```

```SQL
UPDATE people SET shirt_or_hat='shirt' WHERE first_name='Bonnie' AND last_name='Brooks';
```

```SQL
SELECT * FROM people WHERE first_name='Bonnie' AND last_name='Brooks';
```

```SQL
UPDATE people SET shirt_or_hat='shirt' WHERE first_name='Bonnie' AND last_name='Brooks';
```

```SQL
SELECT * FROM people WHERE first_name='Bonnie' AND last_name='Brooks';
```

```SQL
SELECT * FROM people WHERE first_name='Lois' AND last_name='Hart';
```

```SQL
DELETE FROM people WHERE first_name='Lois' AND last_name='Hart';
```

```SQL
SELECT * FROM people WHERE first_name='Lois';
```
