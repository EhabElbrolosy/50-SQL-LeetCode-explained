## _Department Top 3 Salaries_

```sql
select Department,  Employee, salary FROM (
select 
  D.name as Department,
  E.name as Employee,
  salary,
  dense_rank() over (partition by D.name order by salary desc) as S_rank
from Department D join Employee E
ON D.id = E.departmentId
) subquery
WHERE S_rank <=3
order by salary desc
```

هنشوف اعلى 3 مرتبات في كل قسم باستخدام dense_rank()
```sql
dense_rank() over (partition by D.name order by salary desc) as S_rank
```
دا هيكون الناتج 

                                                | Department | salary | S_rank |
                                                | ---------- | ------ | ------ |
                                                | IT         | 90000  | 1      |
                                                | IT         | 85000  | 2      |
                                                | IT         | 85000  | 2      |
                                                | IT         | 70000  | 3      |
                                                | IT         | 69000  | 4      |
                                                | Sales      | 80000  | 1      |
                                                | Sales      | 60000  | 2      |

دلوقتي لما نعمل فلتر ```WHERE S_rank <=3``` كدا هنحدد اعلى 3 في كل قسم 
 وضروري جدا ان اننا نرتبهم تنازليا جوا dense_rank عشان اعلى مرتب ياخد رقم 1 والتاني 2 وهكذا 

 ---

بعد ما حسبنا الترتيب باستخدام dense_rank()، حطينا الاستعلام ده كله جوا Subquery علشان نقدر نستخدم العمود الجديد S_rank في شرط WHERE.

استخدمنا WHERE S_rank <= 3 علشان نفلتر أعلى 3 موظفين في كل قسم حسب المرتب
 
