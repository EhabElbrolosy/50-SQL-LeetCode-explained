## _Triangle judgment_ ##

``` sql
select 
    *,
    Case
        WHEN (x + y > z) and (x + z > y) and (y + z > x) THEN 'Yes'
        Else 'No'
    End
    as triangle
FROM Triangle
```
- المطلوب نحكم على الشكل اذا كان مثلث ولا لا من خلال اطوال اضلاعه X,Y,Z
  
- من شروط ان الشكل يكون مثلث هو ان مجموع اي ضلعين اكبر من الضلع التالت ف هنعبر عن دا ب case

- _لو مجموع الضلع الاول والتاني اكبر من التالت
ومجموع التاني والتالت اكبر من الاول
ومجموع الاول والتالت اكبر من التاني
```Then``` الشكل مثلث
```Else``` مش مثلث_

---

ممكن نحلها باستخدام if بدل من case كدا
```sql
SELECT *, 
  IF(x + y > z AND y + z > x AND z + x > y, 'Yes', 'No') AS triangle 
FROM Triangle;
```
لكن الحل دا مش مدعوم في postgresql موجود بس في my sql و sql server تقريبا
