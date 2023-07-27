```sql
(1)SELECT子语句 
SELECT * FROM person; 
SELECT name,dep FROM person 
SELECT name n,dep d FROM person 

(2)FROM子语句 
SELECT * FROM person; 
SELECT * FROM person t;  
SELECT t.id ,t.'name' FROM person t;  
(3)WHERE子语句 
比较运算符 
SELECT * FROM job WHERE age > 20 
SELECT * FROM job WHERE age >= 20 
SELECT * FROM job WHERE age < 20 
SELECT * FROM job WHERE age <= 20 
SELECT * FROM job WHERE age = 20 
SELECT * FROM job WHERE age != 20 
逻辑运算符 
SELECT * FROM job WHERE age > 20 AND age < 25  AND  sex = '男' 
SELECT * FROM job WHERE positon = '美工' or positon = '实习生'   //@1 
数字区间运算符 
SELECT * FROM job WHERE age BETWEEN 18 AND 19    //包括18和19  
SELECT * FROM job WHERE age NOT BETWEEN 18 AND 19   //逻辑非 
集合运算符 
SELECT * FROM job WHERE positon in ('美工','实习生')     //和@1等价 
SELECT * FROM job WHERE age in (18,20,27)      
模糊查询运算符 
匹配符：_ 下划线 匹配一个任意字符   % 百分号 匹配零个或者多个任意字符。注意like模糊查询只能用于字符类型。 
SELECT * FROM job WHERE name LIKE '郭_'      //郭名，郭大，郭奎 
SELECT * FROM job WHERE name LIKE '郭%'          //郭名，郭大，郭奎，郭飞翔，郭大牛 
SELECT * FROM job WHERE name LIKE '&郭%'        //郭名，郭大，郭奎，郭飞翔，郭大牛，小郭，。。。带就算 
空NULL  解释 ：表示空的意思，未知，没有。不是0，不是空字符串。 
SELECT * FROM person WHERE dep IS NULL    //注意：判断空必须用is 
SELECT * FROM person WHERE dep IS NOT NULL  
(4)GROUP BY 分组子语句 
分组的理解：相同的是一组。按照组来显示数据。几组就显示几条记录。 
分组的注意事项：分组的时候，SELECT显示的字段有约束条件。1是分组的组名，2是聚合函数 
SELECT sex FROM job GROUP BY sex 
SELECT sex, MAX(age) 最大,MIN(age) 最小,COUNT(*) 个数,SUM(age) 求和,AVG(age) 求平均 FROM job GROUP BY sex 

(5)HAVING 分组过滤的条件语句。注意跟WHERE的区分。WHERE对每一条记录进行过滤筛选，HAVING是对组进行过滤筛选，分组之后才能使用HAVING。 
SELECT source s，count (*) c FROM job GROUP BY s HAVING c (*) >10  

(6)ORDER BY 排序 
SELECT * FROM job ORDER BY age;   //默认按照升序排序    ASC升序关键字 
SELECT * FROM job ORDER BY age DESC;   //实现降序    DESC升序关键字 


同时使用6个语句的查询语句 
SELECT positon,count(*) 
FROM job 
WHERE age > 20 
GROUP BY position  
HAVING count(*) > 10 
ORDER BY count(*) DESC 

```
