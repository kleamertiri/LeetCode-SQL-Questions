# 🔸LeetCode SQL Questions
[LeetCode](https://leetcode.com/) is the platform to expand your knowledge and prepare for technical interviews.

I'll be solving SQL questions in MS SQL Server.


## Level: Easy
#### :zap:[1873. Calculate Special Bonus](https://leetcode.com/problems/calculate-special-bonus/)
Write an SQL query to calculate the bonus of each employee. The bonus of an employee is ```100%``` of their salary if the ID of the employee is **an odd number** and **the employee name does not start with the character** ```'M'```. The bonus of an employee is ```0``` otherwise.
Return the result table ordered by ```employee_id```.
```sql
SELECT employee_id, 
CASE
    WHEN employee_id % 2 != 0 AND NAME NOT LIKE 'M%' THEN salary
    ELSE 0
END AS bonus
FROM Employees
ORDER BY employee_id;
```
**Output:**
| employee_id | bonus |
| ----------- | ----- |
| 2           | 0     |
| 3           | 0     |
| 7           | 7400  |
| 8           | 0     |
| 9           | 7700  |

#### :zap:[610. Triangle Judgement](https://leetcode.com/problems/triangle-judgement/)
Write an SQL query to report for every three line segments whether they can form a triangle.
Return the result table in **any order**.
```sql
SELECT x, y, z,
CASE 
    WHEN x + y > z AND x + z > y AND z + y > x THEN 'Yes'
    ELSE 'No'
END AS triangle
FROM Triangle;
```

**Output:**
| x  | y  | z  | triangle |
| -- | -- | -- | -------- |
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |

#### :zap:[196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)
Write an SQL query to **delete** all the duplicate emails, keeping only one unique email with the smallest ```id```. Note that you are supposed to write a ```DELETE``` statement and not a ```SELECT``` one.

```sql
DELETE P1
FROM Person P1, Person P2
WHERE P1.email = P2.Email AND P1.Id>P2.Id;
```
**Output:**
| id | email            |
| -- | ---------------- |
| 1  | john@example.com |
| 2  | bob@example.com  |


#### :zap:[1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/) 
Write an SQL query to find the number of times each student attended each exam.
Return the result table ordered by **student_id** and **subject_name**.
```sql

SELECT st.student_id, st.student_name, sb.subject_name, COUNT(exam.subject_name) AS attended_exams
FROM Students AS st
CROSS JOIN Subjects AS sb
LEFT JOIN Examinations AS exam
ON st.student_id = exam.student_id AND sb.subject_name = exam.subject_name
GROUP BY st.student_id, st.student_name, sb.subject_name

```
**Output:**
| student_id | student_name | subject_name | attended_exams |
| ---------- | ------------ | ------------ | -------------- |
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |



#### :zap:[1211.Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/)
We define ```query``` quality as:
```
The average of the ratio between query rating and its position.
```
We also define ```poor query percentage``` as:
```
The percentage of all queries with rating less than 3.
```
Write an SQL query to find each ```query_name```, the ```quality``` and ```poor_query_percentage```.

Both ```quality``` and ```poor_query_percentage``` should be **rounded to 2 decimal places**.
```sql
SELECT query_name,
        ROUND(SUM(rating / CONVERT(DECIMAL(5,2), position)) / COUNT(query_name),2) AS quality,
        ROUND(SUM(IIF(RATING < 3, 1, 0)) * 100 /CONVERT(DECIMAL(5,2),COUNT(query_name)), 2)  AS poor_query_percentage

FROM Queries
GROUP BY query_name;
```
**Output:**
| query_name | quality | poor_query_percentage |
| ---------- | ------- | --------------------- |
| Cat        | 0.66    | 33.33                 |
| Dog        | 2.5     | 33.33                 |

#### :zap:[619.Biggest Single Number](https://leetcode.com/problems/biggest-single-number/)
A **single number** is a number that appeared only once in the ```MyNumbers``` table.
Write an SQL query to report the largest **single number**. If there is no single number, report ```null```.
```sql
SELECT MAX(x.num) AS num
FROM(
  SELECT num, COUNT(num) AS count_num
  FROM MyNumbers
  GROUP BY num
  HAVING COUNT(num) < 2
) AS x
```

**Output:**
| num |
| --- |
| 6   |

#### :zap:[1179. Reformat Department Table](https://leetcode.com/problems/reformat-department-table/)
Write an SQL query to reformat the table such that there is a department id column and a revenue column **for each month**.
```sql
SELECT  id, 
SUM(IIF(month = 'Jan', revenue, NULL)) AS Jan_Revenue,
SUM(IIF(month = 'Feb', revenue, NULL)) AS Feb_Revenue,
SUM(IIF(month = 'Mar', revenue, NULL)) AS Mar_Revenue,
SUM(IIF(month = 'Apr', revenue, NULL)) AS Apr_Revenue,
SUM(IIF(month = 'May', revenue, NULL)) AS May_Revenue,
SUM(IIF(month = 'Jun', revenue, NULL)) AS Jun_Revenue,
SUM(IIF(month = 'Jul', revenue, NULL)) AS Jul_Revenue,
SUM(IIF(month = 'Aug', revenue, NULL)) AS Aug_Revenue,
SUM(IIF(month = 'Sep', revenue, NULL)) AS Sep_Revenue,
SUM(IIF(month = 'Oct', revenue, NULL)) AS Oct_Revenue,
SUM(IIF(month = 'Nov', revenue, NULL)) AS Nov_Revenue,
SUM(IIF(month = 'Dec', revenue, NULL)) AS Dec_Revenue
FROM Department
GROUP BY id;
```
**Output:**

| id | Jan_Revenue | Feb_Revenue | Mar_Revenue | Apr_Revenue | May_Revenue | Jun_Revenue | Jul_Revenue | Aug_Revenue | Sep_Revenue | Oct_Revenue | Nov_Revenue | Dec_Revenue |
| -- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| 1  | 8000        | 7000        | 6000        | null        | null        | null        | null        | null        | null        | null        | null        | null        |
| 2  | 9000        | null        | null        | null        | null        | null        | null        | null        | null        | null        | null        | null        |
| 3  | null        | 10000       | null        | null        | null        | null        | null        | null        | null        | null        | null        | null        |

#### :zap:[1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/)
Write an SQL query to find the daily active user count for a period of ```30``` days ending ```2019-07-27``` inclusively. A user was active on someday if they made at least one activity on that day.
```sql
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date between DATEADD(day, -29, '2019-07-27') AND '2019-07-27'
GROUP BY activity_date
HAVING COUNT(DISTINCT user_id) > 0;
```
**Output:**
| day        | active_users |
| ---------- | ------------ |
| 2019-07-20 | 2            |
| 2019-07-21 | 2            |


