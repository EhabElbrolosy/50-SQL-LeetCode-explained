## _Average Selling Price_

```sql
select
    P.product_id,
    COALESCE(
            ROUND(
                (SUM(price * units))::numeric / NULLIF (SUM(units),0)
            ,2)
        ,0)
    AS average_price
from prices P 
LEFT JOIN UnitsSold U 
    ON P.product_id = U.product_id 
    AND U.purchase_date between P.start_date AND P.end_date
GROUP BY P.product_id;
```
حساب متوسط السعر لكل منتج ولكن لو المنتج ماتباعش هنستبدل قيمته ب0

```sql COALESCE(
            ROUND(
                (SUM(price * units))::numeric / NULLIF(SUM(units),0)
            ,2)
        ,0)
    AS average_price
```

- في حالة لو المنتج ماتباعش هنبدل القيمة ب 0 ف هنستخدم (0,...)COALESCE 

- الdata type بتاع الاعمدة هي INTEGER ف لو جينا نشتغل عليها هيطلع النواتج ارقام صحيحة مع ان الحل المطلوب عبارة عن ارقام عشرية. (::numeric) (او cast(...) as decimal لو شغالين sql server)
هنحول الاعمدة دي ل numeric عشان الارقام العشرية تظهر في الناتج

- استخدام NULLIF عشان نضمن ان في حالة لو الناتج في المقام صفر يبقى هيرجع null 
بمعنى: لو المنتج ماتباعش خالص استبدل الناتج ( اللي هو صفر ) ب null عشان مايطلعش ERROR 

- لازم نضيف في شرط ال join ان ال purchase date بين start date and end date
 لو كنا ضفنا الشرط دا في جملة where كان ساعتها ال left join هيتم غلط لأن الشرط متنافي معاه ف لازم نضيفها في شرط ال join 
