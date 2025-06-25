## _Project Employees_
```sql
SELECT
    project_id,
    ROUND(SUM(years)/ count(employee_id),2) AS average_years
FROM(
    SELECT 
        project_id,
        E.employee_id,
        SUM(experience_years)/ count(E.employee_id) as years
    FROM Project as P join Employee AS E
    on P.employee_id = E.employee_id
    GROUP BY project_id, E.employee_id
)
GROUP BY project_id
```
هنعمل ```GROUP BY``` على مرحلتين:

مرة هنحسب عدد سنوات الخبرة لكل موظف في المشروعين يعني ```GROUP BY project_id, E.employee_id```  ودا اللي داخل الsubquery
بمعنى ان كدا التجميع بقى على مستوى الموظفين مش على مستوى المشروعين
 
 ف هنعمل Group by للproject_id بس 
يعني بدل ما سنوات الخبرة متجمعة لكل موظف داخل المشروعين, احنا هنجمعهم لكل مشروع ```GROUP BY project_id```

---
                                  التجميع على مستوى الموظفين (داخل Subquery)
                                                    
                                    | project_id | employee_id | years |
                                    | ---------- | ----------- | ----- |
                                    | 1          | 2           | 2     |
                                    | 1          | 1           | 3     |
                                    | 2          | 4           | 2     |
                                    | 2          | 1           | 3     |
                                    | 1          | 3           | 1     |
---
                                                    
                                      التجميع على مستوى الproject
                                                         
                                        | project_id | average_years |                               
                                        | ---------- | ------------- |
                                        | 1          | 2             |
                                        | 2          | 2.5           |
