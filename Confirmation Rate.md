## _Confirmation Rate_

```sql
select 
    S.user_id, 
    round(coalesce(
    sum(case when action ='confirmed' then 1 else 0 end)::NUMERIC
    /nullif(count(action)::NUMERIC,0),0),2) as confirmation_rate
from Signups S left outer join Confirmations C
on S.user_id = C.user_id
GROUP BY S.user_id
order by confirmation_rate asc
```
هنقسم عدد الconfirmed action على total actions لكل user 

طريقة سهلة عشان نحسب عدد الconfirmed من غير مانستخدم cte او joins هي اننا نعمل case 
```sql
sum(case when action ='confirmed' then 1 else 0 end)
```
 الكود هيحسب مجموع ناتج الشرط دا: ( لو لقيت في الصف كلمة confirmed يبقى اكتب 1) كدا هيبقى عندي مجموع الرسايل الconfirmed فقط

لحد هنا الكود مش هيشتغل لأن احتمال يطلع المقام بصفر... كدا عايزين نقوله لو ناتج المقام بصفر يبقى اكتب قيمة تانية
هنقوله ```nullif(count(action)),0``` لو ناتج المقام بصفر خليه ```null``` 

دلوقتي في حالةلو الناتج null .... 
فا القسمة على null بتساوي null احنا عايزين الناتج يبقى بصفر بقى بدل null

ف هنرجع نستخدم ```coalesce(...,0) ``` يعني لو الناتج دا طلع ب null اكتب 0 

لو شفنا ناتج القسمة دلوقتي هيطلع ارقام صحيحة مش كسور لأن الاعمدة عندنا كلها INTEGER
ف دلوقتي هنحول ناتج البسط ل Decimal عشان الكسور تظهر

لو شغالين SQL SERVER هنستخدم ```Cast() as decimal(10,2)``` ولو شغالين postgresql هنكتب ```::NUMERIC``` بعد الناتج

كدا الحمدلله الارقام هتظهر بالكسور وماننساش نقرب الناتج ```ROUND(...,2)```  زي المطلوب


---
استخدمنا ال left join عشان نظهر كل اسامي المستخدمين من الجدول الاول حتى لو ملوش قيمة في الجدول التاني

لو كنت استخدمت join بس كان هيجيب المشترك بينهم فقط مش كلهم ودا مش اللي احنا عايزينه

---
حل تاني ( مش بتاعي ) بس عجبني 
بدل ما يحول العمود ل decimal كتبت ال value = 1.00
```sql
SELECT 
  s.user_id,
  ROUND(AVG(CASE WHEN action = 'confirmed' THEN 1.00 ELSE 0.00 END),2) AS confirmation_rate
FROM Signups s LEFT JOIN Confirmations c ON s.user_id = c.user_id
GROUP BY s.user_id
```
