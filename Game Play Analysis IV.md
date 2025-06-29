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
