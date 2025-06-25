## Consecutive Numbers

```sql
WITH CTE as(
    SELECT id, num,
    lead(num,1) over (order by id) as following_1,
    lead(num,2) over (order by id) as following_2
FROM Logs
order by id)
SELECT distinct num as consecutivenums
from CTE where num = following_1 
and num = following_2
```
عايزين الرقم من عمود num اللي متكرر 3 مرات ورا بعضه
```sql
lead(num,1) over (order by id) as following_1,
lead(num,2) over (order by id) as following_2
```

-هنقارن قيمة الnum من الصف الحالي بقيمته من الصفين اللي بعده ف هنعمل عمودين يمثلوا الصفين اللي بعدهم باستخدام lead() 

-ضيفنا where clause شرطها ان الnum الحالي يساوي قيمة num في الصفين اللي بعده ```where num= following_1 and num = following_2```

- لو كنا عملنا الlead داخل subquery ساعتها هيجيب ناتج غلط لأنه مش هيكون مرتبط بالصف الحالي. يعني في كل مرة هيقارن الnum بالصف الاول فقط
---
حل تاني ممكن ينفع في بعض الحالات لكن مش دقيق اوي
```sql
WITH CTE as(
    SELECT id,
    case when
    ABS(
        (sum(num) over (order by id rows between current row and 2 following))/num
     ) = 3
    THEN 'Yes' else 'no'
    end as case_1
FROM Logs
order by id
)
SELECT id as ConsecutiveNums
from CTE WHERE case_1 ='Yes';
```
الشرط هنا ان مجموع الصف الحالي ```current row``` والصفين اللي بعده ``` and 2 following``` = قيمة num من الصف الحالي
لكن الحل دا مش هينفع لأن احتمال يكون في 3 قيم (مختلفة) متتالية مجموعها يساوي قيمة num 
