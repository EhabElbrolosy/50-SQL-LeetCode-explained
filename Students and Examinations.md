# Students and Examinations

```sql 
WITH CTE as(
    SELECT
        E1.id,    
        E1.name,
        COUNT(E2.managerId) as Total
    FROM Employee E1 Join Employee E2
    ON E1.id = E2.managerID
    group by E1.id,E1.name
)
SELECT name FROM CTE 
where Total >= 5
```
- ممكن نعمل Self join على الجدول نفسه يعني كأننا بنتعامل معاه على انه جدولين منفصلين
جدول E1 بيمثل المديرين و E2 بيمثل الموظفين

- الJoin condition هنا هو ان يكون الid بتاع الموظف يساوي الID بتاع المدير
يعني E1.id = E2.managerID علشان نجيب كل موظف مع مديرع

- بعد كده بنستخدم COUNT(E2.managerID) لحساب عدد الموظفين تحت كل مدير
بعدها هنعرض أسماء المدراء اللي عندهم 5 موظفين أو أكتر باستخدام where Total >= 5

- سبب استخدام الcte هو انه عايز في الoutput الاسم فقط
ف احنا هنعمل الquery كله في cte لوحده وبعدها ناخد منه الname بس

- في الcte لازم نكتب فيه ال` E1.id` عشان يعدي من التيست في حالة ان في اكتر من مدير بنفس الاسم
