## _Game Play Analysis IV.md_

```sql
DECLARE @NUM decimal
SELECT @NUM = count(distinct player_id) from Activity;

WITH CTE as(
  SELECT *, min(event_date) over(partition by player_id order by event_date) as minimum from Activity
)
, CTE2 AS(
select  A1.player_id, count(A2.event_date) as percentaged
FROM CTE A1 join CTE A2
ON A1.player_id = A2.player_id
and A1.event_date= dateadd(day, 1, A2.minimum)
group by A1.player_id
)
select ROUND(count(*)/@NUM,2) as fraction from CTE2
```

أول حاجة بنحسب كام لاعب موجودين في الجدول كله وبنخزن العدد ده في متغير اسمه @NUM.
بعدها بنعمل CTE فيه كل السجلات وكل لاعب بنحطله أول يوم دخل فيه اللعبة باستخدام ```min(event_date)```

 هنقارن كل لاعب بنفسه ونشوف:
هل اللاعب ده دخل تاني في اليوم اللي بعد أول يوم دخل فيه على طول ولا لا
يعني لو أول دخول كان 1 يناير، بنشوف هل دخل كمان يوم 2 يناير.

لو حصل كده، بنعد اللاعب ده

في الآخر بنقسم عدد اللاعبين اللي دخلوا تاني بعد أول يوم
على إجمالي عدد اللاعبين كلهم، وده بيدينا النسبة المطلوبة، وبنقربها لأقرب علامتين عشريتين

---
CTE1 عشان نحسب اول تاريخ دخول لكل لاعب

CTE2 عشان نحسبب عدد اللاعبين اللي سجلوا بعد اول يوم على طول```and A1.event_date= dateadd(day, 1, A2.minimum)```
