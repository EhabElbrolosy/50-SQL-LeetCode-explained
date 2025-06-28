## _Customer Who Visited but Did Not Make Any Transactions_

```sql
select V.customer_id, count(V.visit_id) as count_no_trans
from Visits V left join Transactions T
on V.visit_id = T.visit_id
where transaction_id is null
group by V.customer_id
```

فكرة متكررة هنستخدم left join عشان نعرض كل الزيارات من جدول visits حتى لو ماكنش ليها معاملات من الجدول التاني

هنضيف شرط ```where transaction_id is null``` عشان نجيب بس الزيارات اللي تمت بس ماحصلش فيها معاملات

ف الاخر هنحسب عدد الزيارات دي ( اللي هي ملهاش معاملات ) لكل عميل 
```sql
select V.customer_id, count(V.visit_id) FROM ... GROUP BY V.customer_id
```
