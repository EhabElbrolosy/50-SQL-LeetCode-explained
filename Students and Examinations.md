##Students and Examinations##

```sql SELECT 
    S.student_id,
    S.student_name,
    SUB.subject_name, 
    count(E.subject_name) AS attended_exams
FROM Students S CROSS JOIN 
Subjects SUB JOIN Examinations E ON 
S.student_id = E.student_id AND SUB.subject_name = E.subject_name  
GROUP BY S.student_id, S.student_name,SUB.subject_name
ORDER BY S.student_id, SUB.subject_name
```
