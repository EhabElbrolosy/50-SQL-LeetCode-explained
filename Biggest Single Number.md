## _Biggest Single Number_

```sql
SELECT max(num) as num FROM(
  SELECT num, count(*) as R
  from MyNumbers
  group by num
  having count(*) =1
) as subquery
```
هنجيب الاول القيم الغير مكررة فقط عن طريق count(*) 

الفكرة هنا لما نقول ```SELECT num, count(*) as R from MyNumbers group by num having count(*) =1```
الناتج بتاعها هيبقى كل رقم اتكرر مرة واحدة هياخد رقم 1 لو اتكرر 3 مرات هياخد رقم 3 

ف هنزود شرط ```having count(*) =1 ``` كدا حددنا الارقام للي ظهرت مرة واحدة

هنكتب اللي فات دا كله داخل SUBQUERY او CTE ونجيب اعلى قيمة منه ```SELECT max(num)...f ```
### وماننساش لو استخدمنا subquery زي الحل دا نديله alias name 
---

فكرت احلها في الاول باستخدام row_number 
``` sql
SELECT max(num) as num from(
SELECT num,
row_number() over (partition by num order by num) as rank
from MyNumbers
)
where rank = 1
```

الحل مش مظبوط لأن كدا كل الارقام سواء اتكررت او ماتكررتش هتاخد رقم 1 وبعدين لو اتكررت تاني هتاخد رقم 2 

ف لو جيت اعمل فلتر على اللي ظهر جنبه رقم 1 هيظهرلي اكبر رقم في الجدول لأن كل الارقام ظهر معاها 1 اصلا
