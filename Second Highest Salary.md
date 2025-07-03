## _Second Highest Salary_

```sql
SELECT NULLIF(
  (SELECT DISTINCT salary FROM Employee
    ORDER BY salary DESC
    OFFSET 1 LIMIT 1
  ), 
  NULL
) AS SecondHighestSalaryy

```
---


نبدا بشرح الsubquery:

```sql
SELECT distinct salary FROM Employee
ORDER BY salary DESC
OFFSET 1 LIMIT 1
```
هيعرض المرتبات من غير تكرار ```(distinct)``` وهيرتبهم تنازليا (يعني حاليا اول قيمة هي اكبر قيمة)
هنعمل ```offset 1 ``` دي هتعمل skip لاول قيمة تقابلها وتخش ف اللي بعدها

وكمان ```limit 1 ``` عشان يكتفي بقيمة واحدة بس 

كدا حددنا تاني اكبر قيمة في عمود الsalary 

---
الجزء اللي بعده عايزين نقول فيه ( في حالة ان مفيش تاني اكبر سالاري يبقى الناتج null )

```sql
SELECT nullif( (select ...... ),null) as SecondHighestSalary
```
كدا هخليه يعرض في الناتج ```null``` 

استخدام nullif() : لو value_1 دي بتساوي value_2 اعرض null
```nullif(value_1, value_2)```

---
## ناخد بالنا ان offset و limit مش مدعومين في sql server 

عشان كدا ممكن تتحل بطريقة تانية اسهل بكتير ماجتش ف بالي ( غاششها ) 
```sql
  SELECT max(salary) as SecondHighestSalary
  FROM Employee
  WHERE salary <> (
      SELECT max(salary)
      FROM Employee
  );
```
اكبر salary لا يساوي max(salary)

![WhatsApp Image 2025-06-28 at 19 22 16_85ec0c7f](https://github.com/user-attachments/assets/ed509c2b-d7d1-4857-a660-d5148f42065a)
