# _Delete Duplicate Emails_

```sql
WITH CTE AS(
    SELECT
    id, email,
    row_number() over (partition by email ORDER BY id) as num
FROM Person
)
delete from Person
WHERE id in (
    SELECT id from CTE
    WHERE num > 1 
)
```
```## ليها طرق كتيرة بس اللي استخدمته كان windows function باستخدام row_number  ```

