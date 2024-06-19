# Intermediate SQL Notes
```sql
SELECT gender, AVG(salary) as avg_salary
FROM employee_demographics dem
JOIN employee_salary sal
	ON dem.employee_id = sal.employee_id
GROUP BY gender;
```

```sql
SELECT gender, AVG(salary) OVER(PARTITION BY gender)
FROM employee_demographics dem
JOIN employee_salary sal
	ON dem.employee_id = sal.employee_id;
```

```sql
SELECT gender, 
SUM(salary) OVER(PARTITION BY gender ORDER BY dem.employee_id) AS Rolling_Total
FROM employee_demographics dem
JOIN employee_salary sal
	ON dem.employee_id = sal.employee_id;
```

```sql
SELECT dem.employee_id, dem.first_name, dem.last_name, gender, salary,
ROW_NUMBER() OVER(PARTITION BY gender ORDER BY salary DESC) AS row_num,
RANK() OVER(PARTITION BY gender ORDER BY salary DESC) AS rank_num,
DENSE_RANK() OVER(PARTITION BY gender ORDER BY salary DESC) AS dense_rank_num
FROM employee_demographics dem
JOIN employee_salary sal
	ON dem.employee_id = sal.employee_id;
```

### CTE Common Table Expressions
```sql
WITH CTE_Example AS
(
SELECT gender, AVG(salary)as avg_sal, MAX(salary)as max_sal, MIN(salary)as min_sal, COUNT(salary)as count_sal
FROM employee_demographics dem
JOIN employee_salary sal
	ON dem.employee_id = sal.employee_id
GROUP BY gender
)
SELECT AVG(avg_sal)
FROM CTE_Example;
```
### Getting the same thing with a Subquery
```sql
SELECT AVG(avg_sal)
FROM (SELECT gender, AVG(salary)as avg_sal, MAX(salary)as max_sal, MIN(salary)as min_sal, COUNT(salary)as count_sal
FROM employee_demographics dem
JOIN employee_salary sal
	ON dem.employee_id = sal.employee_id
GROUP BY gender
) AS example_subquery;
```


### Multiple CTEs in one Query
```sql
WITH CTE_Example AS
(
SELECT employee_id, gender, birth_date
FROM employee_demographics
WHERE birth_date > '1985-01-01'
),
CTE_Example2 AS
(
SELECT employee_id, salary
FROM employee_salary
WHERE salary > 50000
)
SELECT *
FROM CTE_Example
JOIN CTE_Example2
	ON CTE_Example.employee_id = CTE_Example2.employee_id;
```




### Temporary Tables (Tables only viewable to the session they are created in)
	- Specific syntax needed for SQL Server
	- Temp Tables are used in more advanced procedures 
## First Ex. Not as popular
```sql
CREATE TABLE #temp_table
( first_name varchar(50),
last_name varchar(50),
favorite_movie varchar(100),
)
```
```sql
INSERT INTO #temp_table
VALUES('Michael', 'Norris', 'Casino')

SELECT *
FROM #temp_table;

SELECT *
FROM employee_salary;

SELECT *
INTO #salary_over_50k
FROM employee_salary
WHERE salary >= 50000;
```

### Stored Procedures (way to save SQL code to reuse)
	## Specific syntax needed for SQL Server

```sql
CREATE PROCEDURE large_salaries
AS
BEGIN
	SELECT *
	FROM employee_salary
	WHERE salary >= 50000;
END;

EXEC large_salaries
```


```sql
CREATE PROCEDURE large_salaries2
AS
BEGIN
	SELECT *
	FROM employee_salary
	WHERE salary >= 50000;
	SELECT*
	FROM employee_salary
	WHERE salary >= 10000;
END
GO


EXEC large_salaries2
GO
```

	 
