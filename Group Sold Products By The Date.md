## _Group Sold Products By The Date_

```sql
SELECT 
  sell_date, 
  count(distinct product) as num_sold ,
  string_agg(distinct product, ',') as products
from Activities
group by sell_date
```

الجديد هنا اني محتاج اجمع قيم نفس العمود في صف واحد (قيم اكتر من صف من عمود products اكتبهم مع بعض في نفس الcell)

هنستخدم ```string_agg(products, ',')``` الproducts هو اسم العمود والarguments التاني هو الفاصل بين كل قيمة منهم اللي هي ال(,)

ونكتب معاها distinct عشان نشيل التكرارات لو المنتج اتباع مرتين 
