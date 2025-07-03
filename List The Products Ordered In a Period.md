## _List The Products Ordered In a Period_

```sql
SELECT product_name, unit
FROM (
  SELECT 
    product_name, 
    SUM(unit) AS unit,
    DATENAME(month, order_date) AS month_name,
    DATENAME(year, order_date) AS year_num
  FROM Products P 
  JOIN Orders O ON P.product_id = O.product_id
  GROUP BY product_name, DATENAME(month, order_date), DATENAME(year, order_date)
) AS sub_2
WHERE 
  unit >= 100 AND 
  month_name = 'February' AND 
  year_num = '2020';
```
الحل اللي استخدمته لكن غير مختصر 

---

حل تاني ابسط 
```sql
SELECT 
  P.product_name, 
  SUM(O.unit) AS unit
FROM Products P
JOIN Orders O ON P.product_id = O.product_id
WHERE 
  MONTH(O.order_date) = 2 AND 
  YEAR(O.order_date) = 2020
GROUP BY 
  P.product_name
HAVING 
  SUM(O.unit) >= 100;
```
هنجيب المنتجات اللي اتطلب منها 100 وحدة أو أكتر في فبراير 2020
استخدمنا:

``` sql
WHERE MONTH(order_date) = 2 AND YEAR(order_date) = 2020
```
عشان نحدد الطلبات اللي كانت في فبراير 2020
وبعدين:
```sql
GROUP BY product_name
HAVING SUM(unit) >= 100
```
علشان نجمع عدد الوحدات لكل منتج ونفلتر اللي العدد بتاعه 100 أو أكتر بس
في الآخر SELECT جاب اسم المنتج ومجموع الوحدات المطلوبة
