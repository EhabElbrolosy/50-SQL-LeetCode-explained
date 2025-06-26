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
## ليها طرق كتيرة بس اللي استخدمته كان windows function باستخدام row_number

-لو استخدمنا ```()row_number``` وعملنا ```partition by email ``` ساعتها كل ايميل هياخد رانك رقم 1

-لكن لو الايميل اتكرر هيبقى الرانك بتاعه رقم 2 ودا اللي هنمسحه ( هنرمز للعمود دا باسم num مثلا )

-بعدها هنمسح من الجدول اي صف فيه ال num اكبر من 1 

-هنا ضروري نعمل ال subquery اللي بعد delete 
عشان لما نستخدم جملة ```WHERE id in``` لازم اللي ييجي بعد in يبقى عبارة عن عمود واحد بس، فا ماينفعش نقوله from CTE عشان هو فيه اكتر من عمود

---
## حل تاني من غير windows function لكن مش الانسب هنا 
```sql
WITH CTE AS(
    SELECT email, COUNT(*) 
    from Person 
    GROUP BY email
    having count(*) > 1
)
DELETE FROM Person
WHERE email in (
    SELECT email FROM CTE
)
```
ممكن نستخدم الفكرة دي لو عايزين نمسح القيم المكررة فقط بس مش هينفع مع ال case دي 
