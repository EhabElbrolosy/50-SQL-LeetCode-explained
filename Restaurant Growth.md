## _Restaurant Growth_

# شرح مهم لل cumulative amount

```sql
with CTE as (select visited_on, sum(amount) as total_amount from customer group by visited_on)
select 
  visited_on,
  round(sum(total_amount)over (order by visited_on rows between 6 preceding and current row),2) as amount,
  round(avg(total_amount)over (order by visited_on rows between 6 preceding and current row),2) as average_amount
FROM CTE
order by visited_on ASC
offset 6
```
المطلوب اننا نعرض حاجتين : 
cumulative amount paid : يعني مجموع المدفوعات التراكمي

cumulative average amount paid : متوسط المدفوعات التراكمي

بس في خلال الاسبوع اللي فات فقط

---

بشكل عام عشان نحسب اي حاجة تراكمية وليكن مثلا المتوسط التراكمي هنستخدم ```avg(...) over (partition by ... order by ... )``` يعني هندمج بين دالة avg والwindows functions

الاساسي هنا اننا نستخدم over عشان نبدأ نعمل window لكن partition by , order by مش اساسيين

---

## وظيفة partition by :
يعني الwindow دي هتتقسم لكل ايه ..PARTITION BY معناها إننا بنقسم البيانات إلى مجموعات فرعية (partitions) بناءً على العمود اللي بنحدده، وكل مجموعة بتُطبّق عليها دالة النافذة بشكل مستقل.

فلو كتبنا: ```AVG(salary) OVER(PARTITION BY name ORDER BY hire_date)```
وعمود name فيه أسماء متكررة،
هيتحسب المتوسط التراكمي (rolling average) لكل اسم على حدة،
يعني كل ما الاسم يفضل نفسه، الحساب هيكمل،
لكن أول ما يظهر اسم جديد، يبدأ التراكمي من أول وجديد

## وظيفة order by:
بيحدد ترتيب الصفوف اللي هتتطبّق عليها دالة النافذة
يعني بنقوله: رتب الصفوف الأول وبعد كده طبق الدالة بشكل تراكمي أو حسب الترتيب دا
لو كتبت ```SUM(sales) OVER(ORDER BY sale_date) ``` هيحسب مجموع تراكمي للمبيعات، لكن بترتيب التواريخ كل صف هيشوف اللي قبله، ويجمع عليهم

---

لغاية كدا حسبنا التراكمي للwindow كلها لكن هنا احنا عايزين نحسب التراكمي لاخر 7 صفوف بس 

هنبدأ نضيف شرط جديد 
```sql
rows between 6 preceding and current row
```
 يعني احسب التراكمي دا على ال6 صفوف اللي فاتت مع الصف الحالي ( يعني الاجمالي 7 صفوف )
 ودا المطلوب عشان التقسيم يطلع مظبوط
 ## لما نستخدم rows between .... لازم اللي ييجي الاول بعد between هو اللي قبل الصف الحالي 

 يعني نقول ``` rows between 6 preceding and 6 following ```  مش العكس (نبدأ بالماضي الاول)

 ---

 في الاخر استخدمت offset 6 عشان نعمل skip لاول اسبوع عشان مايظهرش في ال output

