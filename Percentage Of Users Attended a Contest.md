# _Percentage Of Users Attended a Contest_



DECLARE @total_students int
select @total_students = count(*) from Users;

SELECT contest_id,
round(
(cast(count(distinct user_id) as decimal(10,2)) / @total_students *100)
,2)  as percentage
FROM register 
group by contest_id
ORDER BY percentage desc, contest_id ASC


حليتها بطريقة مختلفة شوية عملت variable اسمه ```@total_students``` بيساوي اجمالي عدد الطلبة من جدول users

الكود الاساسي بعد كدا بيحسب نسبة عدد الطلبة الموجودين في كل contest  (بدون تكرار) من اجمالي عدد الطلبة 

يعني ```count(distinct user_id) as decimal(10,2)) / @total_students *100)```
