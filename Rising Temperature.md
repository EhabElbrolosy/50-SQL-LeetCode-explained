## _Rising Temperature_

```sql
WITH CTE AS (
  SELECT 
    id,
    recordDate,
    temperature,
    LAG(temperature) OVER (ORDER BY recordDate) AS previous_TEMP,
    lag(recordDate) over (order by recordDate) as previous_day
  FROM Weather
)
SELECT id
FROM CTE
WHERE temperature > previous_TEMP
and datediff(day, previous_day, recordDate) = 1
```
هنقارن درحة حرارة اليوم بدرحة اليوم اللي قبله 

هنعمل cte فيه قيمة درجة الحرارة من اليوم اللي قبله عن طريق ```LAG(temperature) OVER (ORDER BY recordDate) AS previous_TEMP``` 
لو جينا نقارن دلوقتي بينهم عن طريق الشرط دا ```WHERE temperature > previous_TEMP``` الحل هيشتغل عادي لكن هيطلعلنا test رخم بيفترض ان الايام في الجدول مش ورا بعض



ف احنا كدا عايزين نقارن بين الايام المتتالية فقط ( اللي الفرق بينها يوم واحد ) 

ف هنضيف في الcte قيمة اليوم اللي قبل اليوم الحالي : ```lag(recordDate) over (order by recordDate) as previous_day``` 
وهنزود شرط ان الفرق بين اليوم الحالي واليوم السابق = 1: ```and datediff(day, previous_day, recordDate) = 1```

كدا الكود هيقارن درجة حرارة اليوم باليوم اللي قبله على طول وهيعرض الايام اللي الحرارة فيها اعلى من حرارة اليوم اللي قبها



![WhatsApp Image 2025-06-28 at 19 22 16_85ec0c7f](https://github.com/user-attachments/assets/ed509c2b-d7d1-4857-a660-d5148f42065a)
