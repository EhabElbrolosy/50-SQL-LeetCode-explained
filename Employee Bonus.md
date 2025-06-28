## _Employee Bonus_

```sql
select E.name, B.bonus from 
Employee E left join Bonus B
ON E.empId = B.empId
WHERE bonus < 1000
OR bonus is null
```

عشان استخدمنا left join ف احتمال ان في موظفين في جدول employee ملهمش bonus في الجدول التاني ف طبيعي القيم بتاعهم هتبقى null

لما نزود الشرط بتاع ```WHERE bonus < 1000``` ساعتها القيم الnull دي مش هتظهر في الناتج ودا غلط طبعا
### (جملة WHERE مش بتعرض اي صف قيمته NULL )

ف هنزود شرط زيادة ان  ```OR bonus is null``` عشان نضمن ان الناس اللي  ملهاش bonus خالص تظهر عادي
