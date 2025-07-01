## _Customers Who Bought All Products_

```sql
SELECT customer_id FROM
(
select customer_id, count(product_key) as total FROM Customer
GROUP BY 1
having count(distinct product_key) = (select count(distinct product_key)from product)
) as sub
```

group by 1 
1 يعني اول عمود في الselect اللي هو هنا customer_id

الشرط دا ```having count(distinct product_key) = .... ``` يعني عدد المنتجات الفريدة اللي اشتراها العميل بيساوي نفس عدد المنتجات الفريدة من جدول products


لو ماستخدمناش distinct : لو العميل اشترى نفس المنتج 3 مرات مثلا ساعتها هيبان انه اشترى 3 منتجات ف كدا الشرط مش هيتحقق

---
نفس الحل باستخدام join 
```sql
SELECT c.customer_id
FROM Customer c
JOIN Product p ON c.product_key = p.product_key
GROUP BY c.customer_id
HAVING COUNT(DISTINCT c.product_key) = (SELECT COUNT(DISTINCT product_key) FROM Product);
```
