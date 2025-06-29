## _Monthly Transactions I_

```sql
-- Write your PostgreSQL query statement below
select to_char(trans_date, 'YYYY-MM') as month,
country,
count(*) as trans_count,
coalesce(sum(case when state ='approved' then 1 else 0 end ),0) as approved_count,
sum(amount) as trans_total_amount,
coalesce(sum(case when state ='approved' then amount end),0) as approved_total_amount
from transactions
group by month, country
order by month
```
هنحدد الاعمدة المطلوبة:

### 1- Month
عشان نستخرج الشهر والسنة فقط من تاريخ معين هنستخدم

```select to_char(trans_date, 'YYYY-MM') as month``` لو شغالين POSTGRESQL

```SELECT FORMAT(trans_date, 'YYYY-MM') as month ``` لو شغالين SQL SERVER 

كدا هنغير صيغة التاريخ من (يوم شهر سنة) ل (شهر سنة)

---

### 2- country

---

### 3-number of transactions

هنعبر عنها ب count(*) لأن كل صف يعبر عن معاملة 
```sql
count(*) as trans_count
```

---

### 4- total amount

```sql
sum(amount) as trans_total_amount
```
---

### 5-approved_count

عشان نتجنب استخدام subqueries و where كتير ممكن نستخدم case اسهل بمراحل
```sql
coalesce(sum(case when state ='approved' then 1 else 0 end ),0)
```
دا هيجمع عدد الصفوف اللي فيهم الحالة approved ولازم نحطه في ```coalesce(...,0)``` عشان نعدي من الtest_case في حالة لو مفيش approved يعرض 0 بدل null

---

### 6=approved_total_amount

نفس الفكرة في حالة لو الحالة بتاع المعاملة approved اجمع الamount ونحطها داخل coalesce برده 

```sql
coalesce(sum(case when state ='approved' then amount end),0)
```

---

## في الاخر هنعمل group by بكل الاعمدة اللي ظهرت مع الaggregation functions اللي هم month, country
