## _Moving Rating_

مطلوب نعمل 2 queries سهلين اصلا وهنضمهم مع بعض في عمود واحد باستخدام ```UNION ALL```

```sql
-- Write your PostgreSQL query statement below
SELECT name as results FROM (
(select
  U.name, count(M.movie_id) 
FROM Users U LEFT join MovieRating M
ON U.user_id = M.user_id
GROUP BY U.name
order by count(M.movie_id) desc, U.name asc
LIMIT 1)

UNION ALL

(SELECT 
  title,
  AVG(rating)::NUMERIC as total
FROM Movies M1 LEFT JOIN MovieRating M2
ON M1.movie_id = M2.movie_id
WHERE TO_CHAR(created_at, 'yyyy-mm') = '2020-02'
GROUP BY title
ORDER BY total desc, title ASC
LIMIT 1))
```
الquery الاول بيحسب عدد الافلام لكل user 

هنا ضروري اننا نعمل select user من جدول ال users مش من اي جدول تاني .. لأن دا الجدول اللي فيه كل بيانات المستخدمين سواء عملوا rate او ماعملوش

في المطلوب دا مش ضروري نعمل left join ممكن join عادي بس عملتها عشان الدقة مش اكتر

---

هنضيف الشرط ان التاريخ لازم يكون ف شهر 2 سنة 2020 بس.. ممكن تتعمل بطرق كتيرة زي 

```sql
where created_at BETWEEN '2020-02-01' and '2020-02-28'
```
وهنا between بتحسب من اول يوم 1 قبراير لحد 28 

يعني مش بتحسب اللي بينهم بس 

---

```sql
WHERE TO_CHAR(created_at, 'yyyy-mm') = '2020-02'
```
بيحول فورمات التاريخ لشهر وسنة او حسب منا عايز

لو شغال sql server : 
```
WHERE FORMAT(created_at, 'yyyy-MM') = '2020-02'
```

طريقة تانية باستخدام like
```sql
 where MovieRating like '2020-02%'
```

