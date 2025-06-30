## _Immediate Food Delivery II_

```sql
with cte as (SELECT customer_id, min(order_date)
as first_date from delivery group by customer_id)

SELECT 
  round(
  SUM(case when D.order_date = D.customer_pref_delivery_date then 1 end)::numeric / count(distinct D.customer_id) *100,2)
  as immediate_percentage from delivery D join CTE
  ON D.order_date = CTE.first_date
  and D.customer_id = CTE.customer_id
```
الcase عشان نحسب عدد الاوردرات اللي الorder_time فيها بيساوي ustomer_pref_delivery_date
ونقسم على عدد العملا الموجودين عشان نجيب النسبة

---

الشرط اللي هنزوده اللي هو ان الorder date = اول order date لكل عميل 

يعني بدل ما اقول ```where order_date = min(order_date) ```

ممكن نعمل cte نعرض فيه اول مرة كل عميل طلب فيها اوردر ونسيمها first_date يعني نقول ```min(order_date) .... group by customer_id``` 

ونعمل join بين الجدولين وهيبقى شرط الjoin 
ان الorder_date بتاع الجدول الاساسي بيساوي first_date من الCTE اللي عملناها 
```sql
 ON D.order_date = CTE.first_date
```
ونضيف شرط تاني طبعا ان الcustomer_id بتاع الجدول الاساسي = بتاع الCTE 
عشان لو ماعملناهوش كدا هنربط بيانات عميل بعميل تاني ليهم نفس التاريخ
