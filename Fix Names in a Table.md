```sql
SELECT
    user_id,
    CONCAT(
        UPPER(
            LEFT(name,1)
            ),
        LOWER(
            SUBSTRING(name,2,length(name)-1))
            )AS name
FROM USERS
ORDER BY user_id
```
عشان نحدد اول حرف في الكلمة فقط هنستخدم ```LEFT(name,1)```  بعدين نحطها داخل UPPER() كدا اول حرف بس هيكون Capital
عايزين نخلي بقية الكلمة small. هنستخدم SUBSTRING عشان نستخرج عدد معين من الحروف من الكلمة 
```sql
SUBSTRING(name,2,length(name)-1
```
ال2 هي رقم الcharacter اللي هيبدأ من عنده تحديد الحروف
```,length(name)-1``` دا عدد الحروف هينتهي عندها 
اللي هو هنا عدد ال characters بتاع الكلمة باستثناء اول حرف لأنه capital
وطبعا نحط دا كله داخل LOWER() وندمج كل اللي فات دا باستخدام CONCAT 
