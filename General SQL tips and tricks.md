### 1. Row_Number() VS RANK() VS DENSRANK() VS NTILE

➖Row_number():


بيدي رقم فريد لكل صف بناءً على الترتيب لأنه بيتجاهل أي تكرار في القيم
1, 2, 3, 4
 
➖RANK():

بيدي نفس الرقم للصفوف اللي ليها نفس القيمة، يعني ما بيتجاهلش التكرارات فا بيحصل قفزة في الترتيب بعد التكرار.
 1, 2, 2, 4

➖DENSE_RANK():

زي RANK() ما بيتجاهلش التكرارات، لكن مش بيعمل قفز في الأرقام، الترتيب بيكمل زي ماهو
 1, 2, 2, 3

➖NTILE(n):

بيقسم الصفوف إلى n مجموعات متساوية أو قريبة في العدد، كل مجموعة بتاخد رقم حسب ترتيبها
 يعني لو عندك 6 صفوف وقلت NTILE(3) هتكون: 1, 1, 2, 2, 3, 3

 ---

### 2.ترتيب التنفيذ في sql
FROM / JOIN ---> WHERE ---> GROUP BY ---> HAVING ---> SELECT ---> ORDER BY

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

### 5. Alias name
   لما بعمل subquery داخل  FROM  لازم أديها alias name وإلا هتطلعلك error
```sql
select * from (select name from table) as alisa_name
```
عشان كدا بيتعامل مع الsubquery على انه جدول مؤقت ولازم يكون الجدول له اسم
---

