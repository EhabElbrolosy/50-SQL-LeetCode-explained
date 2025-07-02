## _Queries Quality And Percentage_

```sql
SELECT 
  query_name,
  round(AVG(
  cast(rating as float) /cast(position as float)),2) as quality,
  (round(
    cast(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) as float) / cast(COUNT(*)as float) *100
    ,2)) as poor_query_percentage
  from Queries 
GROUP BY query_name
```
المطلوب في الoutput ان كل الارقام تطلع ارقام عشرية في حين ان الاعمدة عندنا كلها INTEGER 

ف عشان كدا استخدمت ```(cast(... as float``` قبل كل ناتج ولو شغال postgresql هكتب ```::NUMERIC```

بس دا طبعا مش احسن حاجة هتعقد الكود شوية وهتخليه بطيء ف ممكن نستخدم طريقة تانية
```ROUND(AVG(rating * 1.0 / position), 2) AS quality,``` 

بدل  ما احول صيغة العمود باستخدام cast ضربت قيمة العمود في 1.0 دا كدا هيخلي العمود FLOAT

## لو ضربت عمود (عدد صحيح) في رقم عشري زي 1.0، النتيجة كلها هتتحول تلقائيًا إلى نوع عشري (float)

---

عشان نحسب عدد حالات الpoor queries ممكن نستخدم الcase دي 
```sql
sum (case when rating < 3 then 1 else 0 end
```
ونقسم الرقم على count(query_name) ونقرب الناتج طبعا حسب المطلوب round(....,2)

---

حل تاني عشان اخف التعقيد شوية حسبت نسبة quality في CTE منفصل وخدت الaverage في الجدول الاساسي

```sql
WITH CTE AS (
  SELECT *, (rating ::numeric / position ::numeric) AS percent
  FROM Queries
)

SELECT 
  query_name,
  ROUND(AVG(percent), 2) AS quality,
  round((sum (case when rating < 3 then 1 else 0 end) / count(query_name)::numeric)*100 ,2) as poor_query_percentage
FROM CTE
GROUP BY query_name;
```
