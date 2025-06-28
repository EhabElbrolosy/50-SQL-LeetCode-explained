## _Replace Employee ID With The Unique Identifier_

```sql
select UNI.unique_id, EMP.name 
from Employees EMP left join EmployeeUNI UNI
on EMP.id=UNI.id
```
هنعرض اسامي كل الموظفين الموجودين في جدول Employees والunique_id بتاع كل موظف

بس لو الموظف دا ملوش unique_id من الجدول التاني ساعتها الناتج يظهر ب null

هنستخدم LEFT JOIN ( هيعرض كل الصفوف من الجدول اللي ع الشمال اللي هو هنا Employees والقيمة المقابلة له من الجدول التاني ولو ملوش قيمة هيظهر NULL )
