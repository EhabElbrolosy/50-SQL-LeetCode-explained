## _Last Person to Fit in the Bus_

```sql
WITH CTE AS(
  SELECT *,
  SUM(weight) over (order by turn) as total_weight
  FROM Queue
)
SELECT person_name FROM CTE
WHERE total_weight <= 1000
ORDER BY total_weight DESC
limit 1
```
هنحسب الاول الوزن التراكمي (Cumulative SUM)
يعني اجمالي الوزن لحد اخر شخص ركب ( كل ما حد يركب الوزن التراكمي بيزيد تدريجيًا) 

عن طريق الكود دا ```SUM(weight) over (order by turn) ```

هنا ```OVER(ORDER BY turn)``` معناها: اجمع وزن كل شخص، بس بالترتيب اللي الناس فيه بتركب واحد ورا التاني ( اللي هو turn )

--- 
بعد ما حسبنا الوزن التراكمي هنبدأ نحدد مين هو اخر شخص مسموحله بالركوب 
(اللي هو حد ركب بعده هيبقى الوزن التراكمي اكبر من 1000 )

```sql
SELECT person_name FROM CTE
WHERE total_weight <= 1000
ORDER BY total_weight DESC
limit 1
```
كدا هنقول: حدد اسم الشخص اللي هيكون عنده الوزن التراكمي اصغر من او يساوي 1000

لحد هنا الكود هيعرض كل الناس اللي الوزن التراكمي عندها اقل من او يساوي 1000 احنا عايزين اعلى حد فيهم فقط

هنرتبهم تنازليا ونعمل limit 1 عشان نجيب اسم الشخص اللي عنده بيبقى الوزن التراكمي اعلى قيمة (اللي هي يساوي 1000 او اعلى قيمة بعد ال 1000)

![WhatsApp Image 2025-06-28 at 19 22 16_85ec0c7f](https://github.com/user-attachments/assets/ed509c2b-d7d1-4857-a660-d5148f42065a)
