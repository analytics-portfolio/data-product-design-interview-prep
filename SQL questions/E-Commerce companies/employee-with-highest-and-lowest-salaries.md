
Question: Workers With The Highest And Lowest Salaries
Source: stratascrach

You have been asked to find the employees with the highest and lowest salary across whole dataset.

Your output should include the employee's ID, salary, and employee's department, as well as a column salary_type that categorizes the output by:

'Highest Salary' represents the highest salary

'Lowest Salary' represents the lowest salary


worker table:
department:         text
first_name:         text
joining_date:       datetime
last_name:          text
salary:             bigint
worker_id:          bigint

affected_from:      datetime
worker_ref_id:      bigint
worker_title:       text


--- SOLUTION ---
WITH top_last_salaries AS 
    (SELECT *, 
            RANK() OVER (ORDER BY salary) AS lowest_sal, 
            RANK() OVER (ORDER BY salary DESC) AS highest_sal
    FROM worker) 

SELECT worker_id, 
       salary, 
       department,
       CASE
           WHEN highest_sal = 1 THEN 'Highest Salary'
           ELSE  'Lowest Salary'
       END AS salaries_rank
       FROM top_last_salaries
       WHERE lowest_sal = 1
            OR highest_sal = 1
