# 🔸LeetCode SQL Questions
[LeetCode](https://leetcode.com/) is the platform to expand your knowledge and prepare for technical interviews.

I'll be solving SQL questions in MS SQL Server.

<details>
     <summary>:green_circle:Level: Easy</summary>

## Solved:heavy_check_mark: 
    
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
WHERE P1.email = P2.Email AND P1.Id > P2.Id;
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
```The average of the ratio between query rating and its position.```
We also define ```poor query percentage``` as:
```The percentage of all queries with rating less than 3.```
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

#### :zap:[1084. Sales Analysis III](https://leetcode.com/problems/sales-analysis-iii/)
Write an SQL query that reports the **products** that were **only** sold in the first quarter of ```2019```. That is, between ```2019-01-01``` and ```2019-03-31``` inclusive.

```sql
SELECT  DISTINCT S.product_id,P.product_name
FROM Sales AS S
LEFT JOIN Product AS P
ON s.product_id = p.product_id
WHERE S.product_id NOT IN (SELECT product_id
                          FROM Sales
                          WHERE sale_date < '2019-01-01' or sale_date > '2019-03-31')
```

**Output:**
| product_id | product_name |
| ---------- | ------------ |
| 1          | S8           |

#### :zap:[1667. Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table/)
Write an SQL query to fix the names so that only the first character is uppercase and the rest are lowercase.
Return the result table ordered by ```user_id```.
```sql
SELECT user_id, (UPPER(LEFT(name, 1)) + LOWER(SUBSTRING(name, 2, LEN(name)))) AS name
FROM Users
ORDER BY user_id;
```
**Output:**
| user_id | name  |
| ------- | ----- |
| 1       | Alice |
| 2       | Bob   |

#### :zap:[181. Employees Earning More Than Their Managers](https://leetcode.com/problems/employees-earning-more-than-their-managers/)
Write an SQL query to find the employees who earn more than their managers.
```sql
SELECT name AS Employee
FROM Employee as e
WHERE salary > (SELECT salary FROM Employee  WHERE id = e.managerID)
```
**Output:**
| Employee |
| -------- |
| Joe      |

#### :zap:[182. Duplicate Emails](https://leetcode.com/problems/duplicate-emails/)
Write an SQL query to report all the duplicate emails. Note that it's guaranteed that the email field is not NULL.
```sql
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(email) > 1; 
```
**Output:**
| email   |
| ------- |
| a@b.com |

#### :zap:[183. Customers Who Never Order](https://leetcode.com/problems/customers-who-never-order/)
Write an SQL query to report all customers who never order anything.
```sql
SELECT C.name AS Customers
FROM Customers AS C
LEFT JOIN Orders AS O
ON C.id = O.customerId
WHERE O.id IS NULL;
```
**Output:**
| Customers |
| --------- |
| Henry     |
| Max       |

#### :zap:[197. Rising Temperature](https://leetcode.com/problems/rising-temperature/)
Write an SQL query to find all dates' ```Id``` with higher temperatures compared to its previous dates (yesterday).
```sql
SELECT w.id AS Id
FROM Weather w
JOIN Weather yesterday
ON DATEDIFF(DAY, yesterday.recordDate, w.recordDate) = 1
WHERE w.temperature > yesterday.temperature;
```
**Output:**
| Id |
| -- |
| 2  |
| 4  |

#### :zap:[577. Employee Bonus](https://leetcode.com/problems/employee-bonus/)
Write an SQL query to report the name and bonus amount of each employee with a bonus **less than** ```1000```.

```sql
SELECT E.name, B.bonus
FROM Employee AS E
LEFT JOIN Bonus AS B
ON E.empId = B.empId
WHERE B.bonus < 1000 OR B.bonus IS NULL;
```
**Output:**
| name | bonus |
| ---- | ----- |
| Brad | null  |
| John | null  |
| Dan  | 500   |

#### :zap:[584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)
Write an SQL query to report the names of the customer that are not referred by the customer with ```id = 2```.
```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```
**Output:**
| name |
| ---- |
| Will |
| Jane |
| Bill |
| Zack |

#### :zap:[586. Customer Placing the Largest Number of Orders](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/)
Write an SQL query to find the ```customer_number``` for the customer who has placed **the largest number of orders**.
The test cases are generated so that **exactly one customer** will have placed more orders than any other customer.

```sql
SELECT TOP 1 customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(customer_number) DESC;
```
**Output:**
| customer_number |
| --------------- |
| 3               |

#### :zap:[607. Sales Person](https://leetcode.com/problems/sales-person/)
Write an SQL query to report the names of all the salespersons who did not have any orders related to the company with the name **"RED"**.
```sql
SELECT  DISTINCT S.name
FROM SalesPerson AS S
LEFT JOIN Orders AS O
ON S.sales_id = O.sales_id
WHERE S.sales_id NOT IN (
    SELECT DISTINCT o.sales_id
    FROM Orders o
    INNER JOIN Company c ON o.com_id = c.com_id
    WHERE c.name = 'RED'
)
```
**Output:**
| name |
| ---- |
| Alex |
| Amy  |
| Mark |

#### :zap:[620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)
Write an SQL query to report the movies with an odd-numbered ID and a description that is not ```"boring"```.
Return the result table ordered by ```rating``` **in descending order**.
```sql
SELECT *
FROM Cinema
WHERE id % 2 != 0 AND description NOT IN (
  SELECT description FROM Cinema WHERE description = 'boring'
)
ORDER BY rating DESC;
```
**Output:**
| id | movie      | description | rating |
| -- | ---------- | ----------- | ------ |
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |

#### :zap:[1050. Actors and Directors Who Cooperated At Least Three Times](https://leetcode.com/problems/actors-and-directors-who-cooperated-at-least-three-times/)
Write a SQL query for a report that provides the pairs ```(actor_id, director_id)``` where the actor has cooperated with the director at least three times.
```sql
SELECT actor_id, director_id
FROM ActorDirector
GROUP BY actor_id, director_id
HAVING COUNT(actor_id)>=3;
```
**Output:**
| actor_id | director_id |
| -------- | ----------- |
| 1        | 1           |

#### :zap:[1075. Project Employees I](https://leetcode.com/problems/project-employees-i/)
Write an SQL query that reports the **average** experience years of all the employees for each project, **rounded to 2 digits**.
```sql
SELECT p.project_id, ROUND(AVG(CAST(e.experience_years AS decimal(5,2))), 2) AS  average_years
FROM Project AS p
JOIN Employee AS e
ON p.employee_id = e.employee_id
GROUP BY p.project_id;
```
**Output:**
| project_id | average_years |
| ---------- | ------------- |
| 1          | 2             |
| 2          | 2.5           |

#### :zap:[1148. Article Views I](https://leetcode.com/problems/article-views-i/)
Write an SQL query to find all the authors that viewed at least one of their own articles.
Return the result table sorted by ```id``` in ascending order.

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id;
```
**Output:**
| id |
| -- |
| 4  |
| 7  |

#### :zap:[1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)
Write an SQL query to find the average selling price for each product. ```average_price``` should be **rounded to 2 decimal places**.
```sql
SELECT p.product_id, ROUND(SUM(p.price * u.units) / CONVERT(decimal(7,2), SUM(u.units)), 2) AS average_price
FROM Prices AS p
JOIN UnitsSold AS u 
ON p.product_id = u.product_id
WHERE u.purchase_date between p.start_date AND p.end_date
GROUP BY p.product_id;
```
**Output:**
| product_id | average_price |
| ---------- | ------------- |
| 1          | 6.96          |
| 2          | 16.96         |

#### :zap:[1327. List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/)
```sql
WITH CTE_unit AS (
  SELECT p.product_name, SUM(o.unit) AS unit
  FROM Products AS p
  JOIN Orders AS o
  ON p.product_id = o.product_id
  WHERE MONTH(o.order_date) = '02' AND YEAR(o.order_date) = '2020' 
  GROUP BY p.product_name
)

SELECT *
FROM CTE_unit
WHERE unit >=100
```

**Output:**
| product_name       | unit |
| ------------------ | ---- |
| Leetcode Kit       | 100  |
| Leetcode Solutions | 130  |

#### :zap:[1484. Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/)
Write an SQL query to find for each date the number of different products sold and their names.
The sold products names for each date should be sorted lexicographically.
Return the result table ordered by ```sell_date```.
```sql
with CTE_DISTINCT AS
(
    SELECT DISTINCT sell_date, product
    FROM Activities
)

SELECT sell_date, COUNT(DISTINCT product) AS num_sold, STRING_AGG(product, ',') WITHIN GROUP (ORDER BY product) AS products
FROM CTE_DISTINCT
GROUP BY sell_date
ORDER BY sell_date;
```
**Output:**
| sell_date  | num_sold | products                     |
| ---------- | -------- | ---------------------------- |
| 2020-05-30 | 3        | Basketball,Headphone,T-Shirt |
| 2020-06-01 | 2        | Bible,Pencil                 |
| 2020-06-02 | 1        | Mask                         |

#### :zap:[1527. Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition/)
Write an SQL query to report the patient_id, patient_name and conditions of the patients who have Type I Diabetes. Type I Diabetes always starts with ```DIAB1``` prefix.

```sql
SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions LIKE 'DIAB1%' OR conditions LIKE '% DIAB1%' OR conditions LIKE '%DIAB1';
```
**Output:**
| patient_id | patient_name | conditions   |
| ---------- | ------------ | ------------ |
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |

#### :zap:[1795. Rearrange Products Table](https://leetcode.com/problems/rearrange-products-table/)
Write an SQL query to rearrange the ```Products``` table so that each row has ```(product_id, store, price)```. If a product is not available in a store, do **not** include a row with that ```product_id``` and ```store``` combination in the result table.
```sql
SELECT product_id, 'store1' AS store, store1 AS price
FROM Products
WHERE store1 IS NOT NULL
UNION 
SELECT product_id, 'store2' AS store, store2 AS price
FROM Products
WHERE store2 IS NOT NULL
UNION 
SELECT product_id, 'store3' AS store, store3 AS price
FROM Products
WHERE store3 IS NOT NULL;
```
**Output:**
| product_id | store  | price |
| ---------- | ------ | ----- |
| 0          | store1 | 95    |
| 0          | store2 | 100   |
| 0          | store3 | 105   |
| 1          | store1 | 70    |
| 1          | store3 | 80    |


#### :zap:[1965. Employees With Missing Information](https://leetcode.com/problems/employees-with-missing-information/)
Write an SQL query to report the IDs of all the employees with **missing information**. The information of an employee is missing if:
- The employee's **name** is missing, or
- The employee's **salary** is missing.
Return the result table ordered by ```employee_id``` in **ascending order**.
```sql
--1--
SELECT employee_id
FROM Employees
WHERE employee_id NOT IN (SELECT employee_id FROM Salaries)

UNION

SELECT employee_id
FROM Salaries
WHERE employee_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;

--2--
SELECT concat(e.employee_id, s.employee_id) AS employee_id
FROM Employees AS e
FULL JOIN Salaries AS s
ON e.employee_id = s.employee_id
WHERE name IS NULL OR salary IS NULL
ORDER BY 1 ASC;
```
     
**Output:**
| employee_id |
| ----------- |
| 1           |
| 2           |

</details>

<details>
     <summary>:orange_circle:Level: Medium</summary>
     
## Solved:heavy_check_mark: 
#### :zap:[176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary/)
Write an SQL query to report the second highest salary from the ```Employee``` table. If there is no second highest salary, the query should report ```null```.
```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary != (SELECT MAX(salary) FROM Employee);
```
**Output:**
| SecondHighestSalary |
| ------------------- |
| 200                 |
</details>

 


