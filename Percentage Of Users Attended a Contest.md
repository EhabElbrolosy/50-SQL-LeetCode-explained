# _Percentage Of Users Attended a Contest_


```sql
DECLARE @total_students int
select @total_students = count(*) from Users;

SELECT contest_id,
round(
(cast(count(distinct user_id) as decimal(10,2)) / @total_students *100)
,2)  as percentage
FROM register 
group by contest_id
ORDER BY percentage desc, contest_id ASC

```
- حليتها بطريقة مختلفة شوية عملت variable اسمه ```@total_students``` بيساوي اجمالي عدد الطلبة من جدول users

- بعد كدا الكود الاساسي بيحسب نسبة عدد الطلبة المسجلين في كل contest من اجمالي عدد الطلبة 

- يعني ```count(distinct user_id) / @total_students *100``` بيعد الطلبة الموجودين في كل contest (بدون تكرار) ويقسمهم على الvariable اللي عملناه اللي هو اجمالي عدد الطلبة 
هنسمي الناتج دا باسم Percentage 


- لو سبناها كدا هيبقى الحل مش صح عشان الارقام كلها integer ف كدا مش هيظهر اعداد عشرية زي المطلوب بمعنى ان لو الناتج 0.66 هيظهر 1 او 0 لكن مش هيظهر عدد في النص 
فا هنعمل للناتج cast(percenrage AS DECIMAL(10,2) كدا هنحول الناتج من رقم صحيح لرقم عشري 

- وفي الاخر هنحط دا كله داخل ``` round(...,2)``` عشان نقرب الرقم 

- وماننساش نعمل order by صح زي ماهو مطلوب ```ORDER BY percentage desc, contest_id ASC```
