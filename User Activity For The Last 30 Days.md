## _User Activity For The Last 30 Days_

```sql
select activity_date as day, count (distinct user_id) as active_users
from activity
WHERE activity_date Between date '2019-07-27' - interval '29 days' and date '2019-07-27'
group by activity_date
```

هنحسب عدد الاشخاص اللي سجلوا في كل يوم ```count (distinct user_id) .... group by activity_date```

الشرط اللي هنضيفه هنا هو اننا نفلتر على نعرض دا على مدة 30 يوم اخرهم يوم 2019-07-27

---
# باستخدام postgresql :
```sql
WHERE activity_date Between date '2019-07-27' - interval '29 days' and date '2019-07-27'
```
هيعرض اللي التواريخ اللي بين يوم 2019-07-27 و (اخر 29 يوم قبل التاريخ دا)
يعني لما اكتب ```'2019-07-27' - interval '29 days'``` كدا يعني اطرح 29 يوم من التاريخ دا

لازم نكتب ```date``` قبل التاريخين عشان نحولهم لتاريخ ... لأننا لما نقول '2019-07-27' ف هو كدا string ف لازم نحوله لتاريخ ونطرح منه 29 يوم

معلومة مهمة: between بتحسب البداية والنهاية .. مش بتحسب اللي بينهم بس 
عشان كدا كتبنا 29 يوم بدل 30 لأنها بتعد التاريخ معاهم اللي هو 2019-07-27

---

# باستخدام SQL SERVER :
```sql
WHERE activity_date BETWEEN DATEADD(DAY, -29, '2019-07-27') AND '2019-07-27'
```

```DATEADD(DAY, -29, '2019-07-27')``` يعني اطرح 29 يوم من التاريخ دا

