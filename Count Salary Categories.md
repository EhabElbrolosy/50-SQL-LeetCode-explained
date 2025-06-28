## _Count Salary Categories_ ( معقدة إلى حد ما وعايزة تركيز )
المشكلة الاساسية ان مفيش اي مرتبات Average salary ف بالتالي هي مش بتظهر خالص في الoutput 
احنا عايزين نظهر ان قيمتها بصفر 

الحل النهائي
```sql
SELECT t.category, count(income) as accounts_count
FROM (values ('Low Salary'), ('Average Salary'), ('High Salary'))as T(category)
left join(
SELECT *,
  CASE
    WHEN income < 20000 THEN 'Low Salary'
    when income between 20000 and 50000 then 'Average Salary'
    ELSE 'High Salary'
  END as category
FROM Accounts ) as S
on T.category = S.category
GROUP BY t.category
order by accounts_count desc
```
المطلوب اننا نحسب عدد المرتبات في كل تصنيف من التصنيفات دي (Low Salary, Average Salary, High Salary)
اول خطوة خالص هنصنف المرتبات حسب الcategories المطلوبة باستخدام الcase statement دي
```sql
SELECT *,
  CASE
    WHEN income < 20000 THEN 'Low Salary'
    when income between 20000 and 50000 then 'Average Salary'
    ELSE 'High Salary'
  END as category
```
طبعا هنتعامل مع الcase على انها جدول جديد (subquery or CTE) عشان نعرف نحدد منها الاعمدة المطلوبة 

---
دلوقتي هنبدأ نشوف عدد المرتبات داخل كل تصنيف منهم... بس لو استخدمنا الكود دا هيظهر معانا مشكلة رخمة جدا 
```sql
SELECT 
  category,count(*) as accounts_count
FROM(
SELECT *,
  CASE
    WHEN income < 20000 THEN 'Low Salary'
    when income between 20000 and 50000 then 'Average Salary'
    ELSE 'High Salary'
  END as category
FROM Accounts
) as SUBQUERY
GROUP BY category
```
بسبب ان مفيش ارقام بين ال20000 و 50000 ف بالتالي مفيش اي Average Salary يعني المفروض قيمته بصفر
بس لو جينا نشوف ناتج الكود هيظهر كدا 

                                                    | category       | accounts_count |
                                                    | -------------- | -------------- |
                                                    | High Salary    | 3              |
                                                    | Low Salary     | 1              |


بدل من كدا 

                                                    | category       | accounts_count |
                                                    | -------------- | -------------- |
                                                    | High Salary    | 3              |
                                                    | Low Salary     | 1              |
                                                    | Average Salary | 0              |






---
الحل اللي جربته في الاول لكن مانفعش هو اني استخدم ```COALESCE(count(*), 0)```
يعني بقوله لو ناتج الcount(*) دي طلع بnull اكتب صفر 

طبعا الحل مانفعش عشان اصلا مفيش اي صفوف الcategory بتاعها Average salary ف بالتالي لما استخدمت ```coalesce``` ماشتغلتش
عشان coalesce بتشتغل لما تكون قيمة الصف بnull فقط لكن في الحالة دي الصف نفسه اصلا مش موجود 

---
هنعمل حل مجنون دلوقتي: 
هنضيف الفبمة اللي مش بتظهر دي (بالعافية) عن طريق function اسمها value 

هنقول 
```sql
SELECT ....FROM (values ('Low Salary'), ('Average Salary'), ('High Salary'))as T(category)
```
الكود دا عمل ايه بقى ؟ هيضيف القيم التلاتة دي في جدول جديد اسمه T وفي عمود اسمه category
يعني بقى عندنا جدول جديد فيه عمود واحد اسمه category فيه 3 صفوف هم Low Salary, Average Salary, High Salary

الهدف من الحركة دي اننا لو عملنا دلوقتي left join بين جدول الT والsubquery اللي عملناه قبل كدا 
ساعتها هيظهر كل القيم من جدول الT وفي حالة لو القيمة دي ملهاش قيمة مقابلة في الجدول التاني ... هيظهر الناتج بصفر وهو دا المطلوب بقى

هنعمل شرط الjoin بين الجدولين ان T.category = S.category

وفي الاخر هنحدد من دا كله الاعمدة المطلوبة بس اللي هم ```...SELECT t.category, count(income) as accounts_count FROM```
ونعمل في الاخر ```group by t.category``` وبس كدا ياعم
