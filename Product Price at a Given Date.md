## _Product Price at a Given Date_


جزء من الحل متكرر .. هنحسب اخر سعر لكل منتج وفي حالة لو ماظهرش هنكتب قيمة معينة ( اللي هي 10 هنا )
```sql
SELECT 
  DISTINCT P1.product_id, 
  COALESCE(
  LAST_VALUE(P2.new_price) 
  OVER ( PARTITION BY P2.product_id ORDER BY P2.change_date
  ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
      ),10) AS price
FROM Products P1 
LEFT JOIN Products P2
ON P1.product_id = P2.product_id
AND P2.change_date <= '2019-08-16'
ORDER BY P1.product_id;
```



مطلوب نحسب اخر تحديث سعر لكل منتج لحد يوم 16-8-2019

الفكرة اللي استخدمتها اننا ممكن نقسم الاسعار ل partitions حسب كل product_id وناخد اخر قيمة في البارتيشن دا باستخدام last_value()a

عشان نجيب اخر value في ال partition لازم نزود الrange دا : ```ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING``` 
دور من اول البارتيشن لحد اخر صف في البارتيشن .. ودا اللي بحاول افهمه اكتر لأني كنت بجربها بالصدفة لما الكود ماشتغلش واشتغلت لما ضفتها مش فاهم اوي 

---


هنستخدم left join عشان نظهر كل المنتجات حتى لو ماحصلهاش تحديث (لو ماحصلهاش تحديث هنحط الرقم الافتراضي اللي هو 10 )

في الاول كنت بكتب ```'where P2.change_date <= '2019-08-16``` بدل ```AND P2.change_date <= '2019-08-16'``` 
الغلط اللي كان بيحصل ان الناتج برده ماكنش بيعرض المنتجات اللي القيم بتاعها null من الجدول التاني 
كان بيعرض كدا: (المنتج التالت ماكنتش بيظهر خالص)

                                                                                      | product_id | price |
                                                                                      | ---------- | ----- |
                                                                                      | 1          | 35    |
                                                                                      | 2          | 50    |


## قاعدة مهمة جدا 
سبب ان دا حصل اني ضيفت where clause مع left join
ايه المشكلة اللي بتحصل ؟
لو ضيفت where clause وطبقت الشرط على الجدول اليمين ساعتها هي بتعرض الصفوف اللي الشرط بتاعها اتحقق فقط يعني لو ماتحققش (المفروض بيظهر NULL) هي بتستبعده ومش بتعرضه خالص

## لما تستخدم LEFT JOINوبعدين تضيف شرط على الجدول اليمين في WHERE فا ده بيمنع ظهور الصفوف اللي مالهاش تطابق، لأنها بتبقى NULL والشرط بيستبعدها وبالتالي الـ LEFT JOIN بيتحول فعليًا لـ INNER JOIN.

حل دا اني بدل ما اكتب الشرط في جملة where هكتبه في شرط الjoin يعني هكتبه مع ON 
```sql
ON P1.product_id = P2.product_id
AND P2.change_date <= '2019-08-16'
```
