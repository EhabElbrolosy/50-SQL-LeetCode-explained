## _Average Time Of Process Per Machine_

```sql
SELECT 
machine_id, round(
(sum( case when activity_type = 'end' then timestamp end )::NUMERIC  -
sum( case when activity_type = 'start' then timestamp end )::NUMERIC)/ count(distinct process_id)
,3) as processing_time
FROM Activity
GROUP BY machine_id 
```

مفروض نطرح end time - start time لكل machine_id

هنقسم الtime stamp ل start, end باستخدام الcase دس:
```sql
sum( case when activity_type = 'end' then timestamp end )::NUMERIC  -
sum( case when activity_type = 'start' then timestamp end )::NUMERIC)
```
كدا معانا مجموع الstart time ومجموع الend time نطرحهم من بعض ونقسم على عدد الprocess لكل machine 
دا كله GROUP BY machine_id 

``` count(distinct process_id)``` 

ونعمل للناتج round(...,3) 
لو سبنا ناتج البسط وناتج المقام INTEGER يبقى الناتج مش هيطلع فيه كسور ف لازم نحول البسط ل NUMERIC او DECIMAL عشان نظهر العلامات العشرية

---

طريقة تانية عشان نحسب المتوسط:
هناخد متوسط ( اعلى قيمة end_time ناقص اعلى قيمة start_time ) 
مش هنحتاج نقسم على عدد process 
```sql
WITH process_durations AS (
  SELECT 
    machine_id,
    process_id,
    MAX(CASE WHEN activity_type = 'start' THEN timestamp END) AS start_time,
    MAX(CASE WHEN activity_type = 'end' THEN timestamp END) AS end_time
  FROM Activity
  GROUP BY machine_id, process_id
)

SELECT 
  machine_id,
  ROUND(AVG(end_time - start_time)::NUMERIC, 3) AS average_processing_time
FROM process_durations
GROUP BY machine_id;

```

---

طريقة تالتة باستخدام self join
```sql
SELECT M1.machine_id, round(
avg(M1.timestamp - M2.timestamp)::NUMERIC,3) as processing_time
FROM activity M1 JOIN activity M2
ON M1.machine_id = M2.machine_id
and M1.process_id = M2.process_id
and M1.activity_type = 'end'
and M2.activity_type = 'start'
GROUP BY M1.machine_id
order by processing_time
```
