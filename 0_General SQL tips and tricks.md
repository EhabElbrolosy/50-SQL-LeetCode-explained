
### 1. Row_Number VS RANK VS DENSRANK VS NTILE

Row_number():


بيدي رقم فريد لكل صف بناءً على الترتيب لأنه بيتجاهل أي تكرار في القيم
1, 2, 3, 4
 
RANK():

بيدي نفس الرقم للصفوف اللي ليها نفس القيمة، يعني ما بيتجاهلش التكرارات بس بيعمل skip في الترتيب بعد التكرار.
 1, 2, 2, 4

DENSE_RANK():

زي RANK() ما بيتجاهلش التكرارات، لكن مش بيعمل skip في الأرقام، الترتيب بيكمل زي ماهو
 1, 2, 2, 3

NTILE(n):

بيقسم الصفوف إلى n مجموعات متساوية أو قريبة في العدد، كل مجموعة بتاخد رقم حسب ترتيبها
 يعني لو عندك 6 صفوف وقلت NTILE(3) هتكون: 1, 1, 2, 2, 3, 3

 ---
### 2. String functions (SQL SERVER)

LEN()

بتحسب عدد الأحرف (بدون المسافات في النهاية) ```LEN(column_name)```

LTRIM()

بتمسح المسافات من بداية النص ```LTRIM(column_name)```

RTRIM()

بتمسح المسافات من نهاية النص ```RTRIM(column_name)```

LEFT()

استخراج عدد characters من اليسار ```LEFT(column_name, n)``` هنا n تعبر عن عدد الحروف اللي عايزينها من ع الشمال

RIGHT()

استخراج عدد characters من اليمين ```RIGHT(column_name, n)```

SUBSTRING()

استخراج جزء معين من النص ```(SUBSTRING(column_name, start, length```

REPLACE()

بتبدل جزء من النص بنص تاني ```(REPLACE(column_name, 'old', 'new'```

CHARINDEX()

بتعمل lookup او بتدور على اول مكان لظهور حرف او كلمة ```(CHARINDEX('text', column_name```

يعني مثلا لو كتبت ```SELECT CHARINDEX('SQL', 'Welcome to SQL Server basics') AS Position```
لنتيجة: 12 عسان كلمة "SQL" تبدأ عند الحرف رقم 12 في النص.

PATINDEX()

نفس فكرة like بتدور على اول مكان لتطابق نص او حرف معين ```(PATINDEX('%pattern%', column_name```

UPPER(), LOWER()

بيخلوا الحرف او الكلمة capital او small


REPLICATE()

بتكرر النص لعدد معين من المرات ```REPLICATE(column_name, n)```

CONCAT()

بتدمج اكتر من نص مع بعض ```CONCAT(col1, col2, ...)``` (الconcat ليه طرق تانية بس دي ابسطهم )

CAST()

بتغير الformat بتاع العمود لأي صيغة ممكن نخلي نص او تاريخ او decimal او اي حاجة ```CAST(column_name AS VARCHAR(50)))```


---
### 3.Date functions (SQL SERVER) برده

GETDATE()

بتعرض التاريخ والوقت دلوقتي حالا ``` select GETDATE()```


DATEPART(part, date)

بتعرض جزء معين من التاريخ سواء شهر او سنة او يوم ```SELECT DATEPART(YEAR, GETDATE());```



YEAR() / MONTH() / DAY() 

بيجيب الجزء المطلوب من التاريخ يعني لو عايز اجيب الشهر من الوقت الحالي هقول ```SELECT MONTH(GETDATE())``` النتيجة هتبقى 6 اللي هو رقم الشهر



DATENAME(part, date)

بترجع الجزء المطلوب كنص (مثلاً اسم اليوم أو اسم الشهر) ```SELECT DATENAME(MONTH, GETDATE()) ``` الناتج هيبقى JUNE اللي هو اسم الشهر الحالي 



DATEDIFF(part, start, end)


بيسحب الفرق بين تاريخين بوحدة زمنية معينة ```SELECT DATEDIFF(DAY, '2025-01-01', '2025-06-28');``` يعني دا هيحسب عدد الايام بين التاريخين دول


DATEADD(part, number, date)

بيزود تاريخ بوحدة زمنية معينة على تاريخ معين يعني مثلا ```SELECT DATEADD(DAY, 7, GETDATE());``` هيقولي التاريخ بعد 7 ايام من النهاردة


EOMONTH(date)


هيعرض اخر يوم من نفس الشهر من التاريخ ```SELECT EOMONTH('2025-06-10');  -- 2025-06-30```


GETDATE() و SYSDATETIME() بيرجعوا التاريخ والوقت معًا.

DATEPART و DATENAME بيشتغلوا على أي تاريخ، مش بس التاريخ الحالي.

استخدم DATEADD + DATEDIFF لو عايز تحسب مدة أو تعدل على التاريخ.





---
### 4. Execution order in sql
```FROM / JOIN ---> WHERE ---> GROUP BY ---> HAVING ---> SELECT ---> ORDER BY```

---

### 5. GROUP BY 

لو هتستخدم GROUP BY، يبقى لازم كل الأعمدة اللي في SELECT (ماعدا دوال التجميع) تكون مكتوبة في GROUP BY برده 
يعني مثلا
```sql
SELECT name, id, COUNT(*) FROM table_name GROUP BY name, id;
```
الname, id مكتوبين مع select ومع group by 
## _لكن القاعدة دي مش ضرورية في MY SQL ممكن اعمل group by باعمدة مش موجودة في الselect في mysql فقططط_

---

### 6. SQL Typing Pattern

 ترتيب الكتابة بيبقى 
 ```WHERE ---> GROUP BY ---> HAVING ---> ORDER BY ---> LIMIT ```

 لو كتبت مثلا group by قبل where هيقولي error 
 
 ---

###  7. remove duplicates
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

### 8. Alias name for subquery
   لما بعمل subquery داخل  FROM  لازم أديها alias name وإلا هتطلعلك error
```sql
select * from (select name from table) as alisa_name
```
عشان بيتعامل مع الsubquery على انه جدول مؤقت ولازم يكون الجدول له اسم

---
### 9. Regex in SQL

---
### يعني إيه Regex أصلاً؟

 طريقة بنستخدمها علشان ندوّر على نمط معين في الكلام .... يعني بدور على جزء معين متكرر داخل الكلام والطريقة دي موجودة في لغات برمجة تانية 

 | الرمز              | المعنى                                                       |
|--------------------|------------------------------------------------------------------------|
| `.`                | أي حرف واحد بس                                                        |
| `^`                | بداية السطر أو الكلمة                                                 |
| `$`                | نهاية السطر أو الكلمة                                                 |
| `*`                | الحاجة اللي قبلها تتكرر 0 أو أكتر من مرة                             |
| `+`                | تتكرر 1 أو أكتر من مرة                                                |
| `?`                | تتكرر 0 أو 1 بس                                                       |
| `[abc]`            | أي حرف من التلاتة دول (a أو b أو c)                                   |
| `[^abc]`           | أي حرف **مش** a أو b أو c                                             |
| `[a-z]`            | أي حرف صغير من a لـ z                                                 |
| `[A-Z]`            | أي حرف كابيتال من A لـ Z                                              |
| `[A-Za-z]`         | أي حرف إنجليزي سواء صغير أو كبير                                      |
| `[0-9]`            | أي رقم من 0 لـ 9                                                      |
| `[A-Za-z0-9]`      | أي حرف أو رقم بس                                                      |
| `[^A-Za-z0-9]`     | أي حاجة غير الحروف والأرقام (يعني رموز خاصة مثلاً)                   |
| `\d`               | رقم (زي `[0-9]`)                                                       |
| `\D`               | حاجة مش رقم                                                           |
| `\w`               | حرف أو رقم أو شرطة سفلية (_)                                          |
| `\W`               | حاجة مش حرف ولا رقم                                                   |
| `\s`               | مسافة أو Tab أو Newline                                               |
| `\S`               | حاجة مش مسافة                                                         |
| `{n}`              | تتكرر n مرات بالضبط                                                  |
| `{n,}`             | تتكرر على الأقل n مرات                                               |
| `{n,m}`            | تتكرر من n لـ m مرات                                                  |
| `|`                | أو – يعني كذا أو كذا (OR)                                             |
| `( )`              | تجميع – بنستخدمه لما نحب نجمع جزء معين من النمط (Grouping)          |
| Newline                                    |                           |
| `\S`           | حاجة مش مسافة                                              |                           |
| `{n}`          | تتكرر n مرات بالضبط                                        |                           |
| `{n,}`         | تتكرر على الأقل n مرات                                     |                           |
| `{n,m}`        | تتكرر من n لـ m مرات                                       |                           |
| \`             | \`                                                         | أو – يعني كذا أو كذا (OR) |
| `( )`          | تجميع – بنستخدمه لما نحب نجمع جزء معين من النمط (Grouping) |                           |


---
| الرمز | معناها                                            |
| ----- | ------------------------------------------------- |
| `~`   | regex **حساس لحالة الأحرف** (case-sensitive)      |
| `~*`  | regex **مش حساس لحالة الأحرف** (case-insensitive) |
| `!~`  | عكس الـ regex                                     |
| `!~*` | عكس الـ regex بدون حساسية للحروف                  |


###  لو عايز أجيب كل الأسامي اللي بتبدأ بحرف A:

```sql
WHERE name REGEXP '^A'
```

###  لو عايز أجيب الكلمات اللي فيها 3 أرقام ورا بعض:

```sql
WHERE code REGEXP '[0-9]{3}'
-- أو
WHERE code REGEXP '\\d{3}'
```

###  لو عايز أجيب أي حاجة فيها رقم وكلمة مع بعض:

```sql
WHERE text REGEXP '[A-Za-z]+[0-9]+'
```

###  لو عايز أجيب القيم اللي فيها رموز خاصة (مش حروف ولا أرقام):

```sql
WHERE value REGEXP '[^A-Za-z0-9]'
```

### ✳️ لو عايز أجيب الكلمات اللي فيها "cat" أو "dog":

```sql
WHERE sentence REGEXP 'cat|dog'
```

###  لو عايز أتأكد إن الكود بيتكون من حرف كابيتال وبعده رقمين:

```sql
WHERE code REGEXP '^[A-Z][0-9]{2}$'
```

---
### 10. Declaring a variable (SQL server)
في العادي لما بنفترض variable بقيمة معينة بنكتب ```set @variable = value``` ولتكن رقم ثابت مثلا
بس لو عايزين الvalue دي تبقى query هنكتب 
```sql
select @variable = select col1,col2 from table_name
```
---


### 11. Case order

لو بنكتب case أي استثناء أو حالة شاذة لازم يتحط في الأول عشان ما يتغطاش بالشروط اللي قبله

---

### 12. second highest value
لو عايز اجيب تاني أعلى قيمة مممن استخدم offset
```sql
SELECT id FROM student
ORDER BY id DESC
OFFSET 1 LIMIT 1;
```
كدا هيعمل skip لأول قيمة ويخش في اللي بعدها 

---
### 13. LEAST() and GREATEST()

LEAST(a, b)

قيم في نفس الصف (أعمدة)	أصغر قيمة بين العمودين أو أكثر

GREATEST(a, b)

قيم في نفس الصف (أعمدة)	أكبر قيمة بينهم

MIN(col)

عدة صفوف	أصغر قيمة داخل العمود كله

MAX(col)

عدة صفوف	أكبر قيمة داخل العمود كله


---
### 14. calculating median

لو عايز احسب الmedian او الوسيط هستخدم ```percentile_cont(0.5) within group```
```sql
SELECT 
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY column_name) AS median_value
FROM 
  table_name;
```
ونفس الحال لو عايز احسب الربع الاول مثلا هستبدل ال (0.5) بـ (0.25)

---

### 15. counting NULL values

كل دوال التجميع زي sum, avg, count بتتجاهل الnulls 
معادا count(*) دي بتعد كل الصفوف حتى لو كانت null
عشان كدا لو حبينا نحسب عدد الnulls في عمود معين (مثلا name) هنقول 
```sql
select count(*) - count(name) from...
```

دا هيطلع عدد القيم الnull بس

---
### 16. WHERE clause and nulls

جملة WHERE مش تعرض الصفوف اللي نتيجة الشرط فيها NULL، لأنها تعتبرها UNKNOWN مش TRUE
لو عايز تظهر الصفوف اللي فيها NULL، لازم تستخدم IS NULL أو IS NOT NULL

---

### 17. Join types

![460017715-e2bf8b95-fa1c-4911-92db-6b4411cec14a](https://github.com/user-attachments/assets/2cf772e2-37e3-41f6-ab11-61f59057c0d9)

---

### 18.CREATE, UPDATE, DELETE

 أولًا: التعامل مع هيكل الجدول (Structure)
 1. إضافة عمود جديد إلى جدول:
```sql
ALTER TABLE Employees
ADD Salary INT;
```
 2. تعديل نوع بيانات عمود موجود:
```sql
ALTER TABLE Employees
ALTER COLUMN Salary DECIMAL(10,2);\
```
 3. حذف عمود:
```sql
ALTER TABLE Employees
DROP COLUMN Salary;
```
 ثانيًا: التعامل مع البيانات داخل الجدول
 4. إضافة صف (بيانات جديدة):
```sql
INSERT INTO Employees (ID, Name, Age)
VALUES (1, 'Ahmed', 30);
```
 5. تعديل بيانات صف معين:
```sql
UPDATE Employees
SET Age = 31
WHERE ID = 1;
```
 6. حذف صف معين:
```sql
DELETE FROM Employees
WHERE ID = 1;
```
 ثالثًا: أوامر إضافية سريعة
 7. حذف كل البيانات من الجدول (مع بقاء الهيكل):
```sql
DELETE FROM Employees;
```
أو:

```sql
TRUNCATE TABLE Employees;
 TRUNCATE أسرع، لكنه ما بيسمحش بـ WHERE ولا بيرجع صفوف محذوفة.
```
 8. إنشاء جدول جديد:
```sql
CREATE TABLE Employees (
  ID INT PRIMARY KEY,
  Name NVARCHAR(100),
  Age INT
);
```
