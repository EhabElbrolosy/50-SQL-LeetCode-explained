```sql
SELECT *
FROM Cinema
WHERE id %2 <> 0
AND description not in ('boring')
ORDER BY rating desc
```
هنحدد الid اللي رقمه فردي بس 
```sql
WHERE id %2 <> 0
```

يعني هنضيف شرط ان لازم باقي قسمة الid على 2 لا يساوي 0 

_ (2%) يعني باقي قسمة الرقم على 2 *
