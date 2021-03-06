# SQL

### Courses
- DataCamp course:  [Intro to SQL for Data Science](https://www.datacamp.com/courses/intro-to-sql-for-data-science)


## `SELECT`ing columns

### Notes
- Querying is an essential skill for a data scientist, since the data you need for your analyses will often live in databases.
- In this query, SELECT and FROM are called keywords. In SQL, keywords are not case-sensitive, which means you can write the same query as:
```sql
select name
from people;
```
- it's good practice to **make SQL keywords uppercase** to distinguish them from other parts of your query, like column and table names.
- It's also good practice (but not necessary for the exercises in this course) to **include a semicolon at the end of your query**. This tells SQL where the end of your query is!

### Select multiple columns
- To select multiple columns from a table, simply separate the column names with commas!
- For example, this query selects two columns, name and birthdate, from the people table:
```sql
SELECT name, birthdate
FROM people;
```
### Select distinct columns
- Often your results will include many duplicate values. If you want to select all the unique values from a column, you can use the DISTINCT keyword.
- This might be useful if, for example, you're interested in knowing which languages are represented in the films table:
```sql
SELECT DISTINCT language
FROM films;
```

### Count number of rows in a table
```sql
SELECT COUNT(*)
FROM people;
```

## Filtering columns

### `BETWEEN`
Checking for ranges like this is very common, so in SQL the BETWEEN keyword provides a useful shorthand for filtering values within a specified range. This query is equivalent to the one above:
```sql
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;
```

### `WHERE IN`
```sql
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```

### `IS NULL` ---> missing
```sql
SELECT name
FROM people
WHERE deathdate IS NULL;
```

### `LIKE` and `NOT LIKE`
The % wildcard will match zero, one, or many characters in text. For example, the following query matches companies like 'Data', 'DataC' 'DataCamp', 'DataMind', and so on:
```sql
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```
Where the second letter of name has an "r"
```sql
SELECT name
FROM people
WHERE name LIKE '_r%';
```
---

## Aggregate Functions

### `AVG`, `SUM`, `MIN`
```sql
SELECT AVG(budget)
FROM films;
```

### `WHERE`

---

## ARITHMETIC
However, the following gives a result of 1:
```sql
SELECT (4 / 3);
```

This gives you the result you would expect: 1.333.
```sql
SELECT (4.0 / 3.0) AS result;
```

## `AS` aliasing (renaming)
Aliases are helpful for making results more readable!
```sql
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;
```

## Subtraction
```sql
SELECT title,
       gross-budget AS net_profit
FROM films;
```

```sql
-- get the count(deathdate) and multiply by 100.0
-- then divide by count(*)
SELECT COUNT(deathdate)* 100.0 / COUNT(*) AS percentage_dead
FROM people;
```

```sql
SELECT MAX(release_year) - MIN(release_year) AS difference
FROM films;
```

```sql
SELECT (MAX(release_year) - MIN(release_year))/10 AS number_of_decades
FROM films;
```

## Part 4:  Sorting, Grouping and Joins

### `ORDER BY`
By default ORDER BY will sort in ascending order. If you want to sort the results in descending order, you can use the DESC keyword. For example,

```sql
SELECT title
FROM films
ORDER BY release_year DESC;
```

```sql
SELECT title
FROM films
WHERE release_year IN (2000, 2012)
ORDER BY release_year;
```
```sql
SELECT *
FROM films
WHERE release_year NOT IN (2015)
ORDER BY duration;
```
```sql
SELECT title, gross
FROM films
WHERE title LIKE 'M%'
ORDER BY title;
```

### Sorting single columns:  `DESC`  (descending order)
```sql
SELECT name
FROM people
ORDER BY name DESC;
```

### Sorting multiple columns
```sql
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```

### `GROUP BY`
Now you know how to sort results! Often you'll need to aggregate results. For example, you might want to count the number of male and female employees in your company. Here, what you want is to group all the males together and count them, and group all the females together and count them. In SQL, GROUP BY **allows you to group a result by one or more columns**:
```sql
SELECT sex, count(*)
FROM employees
GROUP BY sex
ORDER BY count DESC;
```
```text
sex	count
male	15
female	19
```

Commonly, `GROUP BY` is used with aggregate functions like `COUNT()` or `MAX()`. Note that `GROUP BY` always goes after the `FROM` clause!

**Note:**  A word of warning: SQL will return an error if you try to `SELECT` a field that is not in your `GROUP BY` clause without using it to calculate some kind of value about the entire group.

**Note:**  `ORDER BY` always goes after `GROUP BY`

```sql
SELECT release_year, AVG(duration)
FROM films
GROUP BY release_year;
```
```sql
SELECT imdb_score, COUNT(*)
FROM reviews
GROUP BY imdb_score;
```

### `HAVING` --> used for filtering
This means that if you want to filter based on the result of an aggregate function, you need another way! That's where the HAVING clause comes in. For example,
```sql
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
```
NOT VALID:  cannot use **aggregate** functions in `WHERE` clause:  
```sql
WHERE COUNT(title) > 10;
```

#### Practice
```sql
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000
ORDER BY avg_gross DESC;
```

```sql
-- select country, average budget, average gross
SELECT country, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
-- from the films table
FROM films
-- group by country 
GROUP BY country
-- where the country has more than 10 titles
HAVING COUNT(country) > 10
-- order by country
ORDER BY COUNTRY
-- limit to only show 5 results
LIMIT 5;
```

### `JOIN`
```sql
SELECT title, imdb_score
FROM films
JOIN reviews
ON films.id = reviews.film_id
WHERE title = 'To Kill a Mockingbird';
```




