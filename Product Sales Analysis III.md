## _Product Sales Analysis III_

( الproblem دي عليها كلام كتير ان فيها غلط او خطأ في الصياغة على حسب كلام الناس في الDiscussion )

```sql
SELECT
  product_id, year as first_year, quantity, price
FROM Sales
WHERE (product_id, year) in(
  SELECT product_id, MIN(year)
  FROM Sales
  GROUP BY product_id
)
```
---

في الاول هنعمل subquery او CTE فيه *عمودين*: السنة الاولى من البيع لكل منتج + اسم المنتج
```sql
SELECT product_id, MIN(year)
  FROM Sales
  GROUP BY product_id
```
---
هنحدد الاعمدة المطلوبة اللي هم ```product_id, year as first_year, quantity, price``` من جدول الSales
ونضيف where clause : لما تكون السنة واسم المنتج موجودين في الsubquery اللي عملناه قبل كدا

في العادي لما بنقول where col1 in () غالبا اللي بييجي بعد in هو query فيه عمود واحد

بس في الحالة دي عايزين نقول where col1, col2 in (query فيه عمودين )

عشان نحلها هنحط العمودين داخل اقواس كدا ```WHERE (product_id, year) in (select ....)```
