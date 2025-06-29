## _Investment in 2016_

```sql
WITH CTE AS(
select lat, lon, count(*) from Insurance 
group by lat, lon 
having count(*) = 1
)

SELECT ROUND(
  SUM(tiv_2016)::numeric
  ,2) as tiv_2016 
  from (
SELECT *, count(*) over (partition by tiv_2015) as num from Insurance
) sub
where num > 1
and (lat, lon) in (
  SELECT 
    lat, lon from CTE
)
```
---
شرح المطلوب: هنجمع اجمالي استثمارات 2016  اللي هي tiv_2016 للعملا بس بشرطين:

## السرط الاول:
استثمار العميل في 2015 (tiv_2015) يكون مساوي لاستثمار عميل تاني في نفس السنة
يعني فيه أكتر من عميل عندهم نفس قيمة الاستثمار في 2015

بمعنى اخر: بندور على العملاء اللي قيمة tiv_2015 بتاعتهم متكررة في الجدول
*(هنقارن صفوف نفس العمود ببعضها)*

---
```sql
SELECT *, count(*) over (partition by tiv_2015) as num from Insurance
```
الغرض من الكود دا انه هيظهرلنا عدد مرات تكرار كل قيمة في العمود ( لكن بدون تجميع) 
يعني دا هيبقى الناتج: 

                                                      | tiv_2015 |num|
                                                      | -------- | - |
                                                      | 10       | 3 |
                                                      | 10       | 3 |
                                                      | 10       | 3 |
                                                      | 20       | 1 |

رقم 10 اتكرر 3 مرات, رقم 20 اتكرر مرة واحدة وهكذا 

---
هنضيف بعدها شرط ```where num > 1``` عشان ناخد القيم اللي اتكررت بس..
هيبقى الكود كدا :
```SQL
SELECT ROUND(
  SUM(tiv_2016)::numeric
  ,2) as tiv_2016 
  from (
SELECT *, count(*) over (partition by tiv_2015) as num from Insurance
) sub
where num > 1
```
اجمع استثمارات tiv_2016 بشرط ان عدد تكرار كل قيمة من tiv 2015 اكبر من 1 

---

## الشرط التاني:
ان يكون الموقع الجغرافي مش متكرر في الجدول (lat, lon) ظهروا مرة واحدة 

هنحلها باستخدام cte 
```sql
WITH CTE AS(
select lat, lon, count(*) from Insurance 
group by lat, lon 
having count(*) = 1
```
لما نعمل (*)count ونعمل ```group by lat, lon``` ساعتها هيظهر عدد مرات تكرار اللوكيشن دا 
هنزود شرط ``` having count(*) = 1``` عشان نعرض الصفوف اللي اتكررت مرة واحدة بس (unique) 

اخر حاجة هناخد بس عمودين الlat, lon من الcte 
```
and (lat, lon) in (
  SELECT 
    lat, lon from CTE
```



## ملحوظة صغيرة:
مفروض نقرب الناتج لأقرب 2 من عشرة ... الارقام عندنا integer يعني اعداد صحيحة 
هنحولها ل decimal او ```(::numeric)``` عشان العلامات العشرية تظهر وبعدها نستخدم ```ROUND(...,2)```
