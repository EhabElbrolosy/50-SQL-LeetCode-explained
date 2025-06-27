### 1. Row_Number() | RANK() | DENSRANK | NTILE

➖Row_number():


بيدي رقم فريد لكل صف بناءً على الترتيب لأنه بيتجاهل أي تكرار في القيم
1, 2, 3, 4
 
➖RANK():

بيدي نفس الرقم للصفوف اللي ليها نفس القيمة، يعني ما بيتجاهلش التكرارات بس بيحصل قفزة في الترتيب بعد التكرار.
 1, 2, 2, 4

➖DENSE_RANK():

زي RANK() ما بيتجاهلش التكرارات، لكن مش بيعمل قفز في الأرقام، الترتيب بيكمل زي ماهو
 1, 2, 2, 3

➖NTILE(n):

بيقسم الصفوف إلى n مجموعات متساوية أو قريبة في العدد، كل مجموعة بتاخد رقم حسب ترتيبها
 يعني لو عندك 6 صفوف وقلت NTILE(3) هتكون: 1, 1, 2, 2, 3, 3

 ---

### 2.Execution order in sql
```FROM / JOIN ---> WHERE ---> GROUP BY ---> HAVING ---> SELECT ---> ORDER BY```

---

### 3.GROUP BY 

لو هتستخدم GROUP BY، يبقى لازم كل الأعمدة اللي في SELECT (ماعدا دوال التجميع) تكون مكتوبة في GROUP BY برده 
يعني مثلا
```sql
SELECT name, id, COUNT(*) FROM table_name GROUP BY name, id;
```
الname, id مكتوبين مع select ومع group by 

---

###  4.remove duplicates
لو عايز تحذف الصفوف المكررة وتسيب أول صف بس ممكن استخدم ROW_NUMBER() وفلتر على اللي رقمه أكر من 1
ولازم تحطها في CTE أو subquery.
```sql
WITH CTE AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
  FROM Person
)
DELETE FROM Person WHERE id IN (
  SELECT id FROM CTE WHERE rn > 1
);
```
---

### 5. Alias name for subquery
   لما بعمل subquery داخل  FROM  لازم أديها alias name وإلا هتطلعلك error
```sql
select * from (select name from table) as alisa_name
```
عشان بيتعامل مع الsubquery على انه جدول مؤقت ولازم يكون الجدول له اسم

---
### 6. Declaring a variable (SQL server)
في العادي لما بنفترض variable بقيمة معينة بنكتب ```set @variable = value``` ولتكن رقم ثابت مثلا
بس لو عايزين الvalue دي تبقى query هنكتب 
```sql
select @variable = select col1,col2 from table_name
```
---
### 7.Case order

لو بنكتب case أي استثناء أو حالة شاذة لازم يتحط في الأول عشان ما يتغطاش بالشروط اللي قبله

---

### 8.calculating median

لو عايز احسب الmedian او الوسيط هستخدم ```percentile_cont(0.5) within group```
```sql
SELECT 
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY column_name) AS median_value
FROM 
  table_name;
```
ونفس الحال لو عايز احسب الربع الاول هستبدل ال (0.5) بـ (0.25)

---

### 9.counting NULL values

كل دوال التجميع زي sum, avg, count بتتجاهل الnulls 
معادا count(*) دي بتعد كل الصفوف حتى لو كانت null
عشان كدا لو حبينا نحسب عدد الnulls في عمود معين (مثلا name) هنقول 
'''sql
select count(*) - count(name) from ...

'''
دا هيطلع عدد القيم الnull بس

---

### 9.Join types

![Joins types](https://images.app.goo.gl/UiXS1)
