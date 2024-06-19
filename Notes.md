# Intermediate SQL Notes

## Test


--SELECT gender, AVG(salary) as avg_salary
--FROM employee_demographics dem
--JOIN employee_salary sal
--	ON dem.employee_id = sal.employee_id
--GROUP BY gender;


--SELECT gender, AVG(salary) OVER(PARTITION BY gender)
--FROM employee_demographics dem
--JOIN employee_salary sal
--	ON dem.employee_id = sal.employee_id;


--SELECT gender, 
--SUM(salary) OVER(PARTITION BY gender ORDER BY dem.employee_id) AS Rolling_Total
--FROM employee_demographics dem
--JOIN employee_salary sal
--	ON dem.employee_id = sal.employee_id;

--SELECT dem.employee_id, dem.first_name, dem.last_name, gender, salary,
--ROW_NUMBER() OVER(PARTITION BY gender ORDER BY salary DESC) AS row_num,
--RANK() OVER(PARTITION BY gender ORDER BY salary DESC) AS rank_num,
--DENSE_RANK() OVER(PARTITION BY gender ORDER BY salary DESC) AS dense_rank_num
--FROM employee_demographics dem
--JOIN employee_salary sal
--	ON dem.employee_id = sal.employee_id;

-- CTE Common Table Expressions
--WITH CTE_Example AS
--(
--SELECT gender, AVG(salary)as avg_sal, MAX(salary)as max_sal, MIN(salary)as min_sal, COUNT(salary)as count_sal
--FROM employee_demographics dem
--JOIN employee_salary sal
--	ON dem.employee_id = sal.employee_id
--GROUP BY gender
--)
--SELECT AVG(avg_sal)
--FROM CTE_Example;

--Getting the same thing with a Subquery
--SELECT AVG(avg_sal)
--FROM (SELECT gender, AVG(salary)as avg_sal, MAX(salary)as max_sal, MIN(salary)as min_sal, COUNT(salary)as count_sal
--FROM employee_demographics dem
--JOIN employee_salary sal
--	ON dem.employee_id = sal.employee_id
--GROUP BY gender
--) AS example_subquery;


-- Multiple CTEs in one Query
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
