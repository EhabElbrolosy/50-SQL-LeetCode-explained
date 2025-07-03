## _Draw The Triangle 2 (Hackerrank)_

```sql
DECLARE @COUNT INT
SELECT @COUNT = 1;

WHILE @COUNT <= 5
    BEGIN print Replicate(concat('*', ' '),@COUNT) 
     set @COUNT = @COUNT + 1
    END
```

عشان نعمل الشكل دا المفروض هنستخدم loop 
```
* 
* * 
* * * 
* * * * 
* * * * *
```

في الاول هنكريت متغير اسمه @count قيمته المبدئية بتساوي 1 

هنبدأ الLOOP باستخدام ```WHILE```

```WHILE @COUNT <= 5``` يعني طالما قيمة المتغير اصغر من او يساوي 5 ابدأ اعمل حاجتين:

``` BEGIN print Replicate(concat('*', ' '),@COUNT) ``` 

1.ابدأ اعرض ناتج دمج '*' و ' ' (مسافة) وكرره بنفس عدد مرات @count 

دالة replicate بتكرر النص لعدد معين من المرات 

---

2. زود قيمة @count بمقدار 1
```sql
set @COUNT = @COUNT + 1
```
