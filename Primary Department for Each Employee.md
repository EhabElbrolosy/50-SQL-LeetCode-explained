## _Primary Department for Each Employee_

الحل اللي حليت بيه بس لا أنصح به, معقد ع الفاضي
```sql
WITH CTE AS (
  select *, count(*) over (partition by employee_id) as num
  from Employee
)
, CTE2 as (
select 
  employee_id,
  case 
    when num > 1 and primary_flag ='Y' then department_id
    when num = 1 and primary_flag ='N' then department_id
  end as department_id
FROM CTE
)
select * from CTE2 where department_id is not null
```
---
حل تاني ابسط واسهل 
```sql
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag='Y' OR 
    employee_id in
    (SELECT employee_id
    FROM Employee
    Group by employee_id
    having count(employee_id)=1)
```

الـ subquery بتختار الموظفين اللي موجودين في قسم واحد بس عن طريق تجميعهم بـ employee_id وتصفية اللي عدد مرات ظهورهم = 1 يعني بتحدد الموظفين اللي ليهم قسم واحد فقط وبالتالي ده يعتبر قسمهم الأساسي

