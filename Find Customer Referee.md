## _Find Customer Referee_

```sql
SELECT name
FROM Customer
WHERE referee_id <> 2
or referee_id IS NULL;
```
لما بنعمل الشرط الاول اللي هو ```WHERE referee_id <> 2```
ساعتها مش هيظهر اي صف قيمته ب null 
### _عشان جملة where بتتجاهل اي صف ناتج شرطه ```NULL```_

قاعدة :

(جملة WHERE بترجع الصفوف اللي شرطها = TRUE فقط.
لو الشرط نتيجته FALSE أو UNKNOWN (زي لما تقارن مع NULL) الصف ما بيرجعش)

ف هنضيف كمان شرط ```or referee_id IS NUL``` عشان نظهر القيم الnull في الناتج
