# 🔸LeetCode SQL Questions
[LeetCode](https://leetcode.com/) is the platform to expand your knowledge and prepare for technical interviews.

I'll be solving SQL questions in MS SQL Server.

## Level: Easy
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

