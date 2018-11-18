# LeetCode-Database
+ [175. 组合两个表](#j1)
+ [176. 第二高的薪水](#j2)
+ [177. 第N高的薪水](#j3)
+ [178. 分数排名](#j4)
+ [180. 连续出现的数字](#j5)



### <span id='j1'>175. 组合两个表</span>
题目：编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：  
`FirstName, LastName, City, State`

表1：Person

列名 | 类型
---|---
PersonId | int
FirstName | varchar
LastName | varchar

PersonId 是上表主键

表2：Address

 列名 | 类型    
---|---
AddressId  |int     
PersonId   | int     
City       | varchar 
State      | varchar 

AddressId 是上表主键

<span style="color: #0099ff">答案：</span>
 ```Mysql
 # Write your MySQL query statement below
 SELECT FirstName,LastName,City,State 
 FROM person p 
 LEFT JOIN Address ad ON p.PersonId=ad.PersonId;
 ```

### <span id='j2'>176. 第二高的薪水</span>
题目：编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。  

表1：Employee 

 id | Salary     
---|---
1  | 100     
2  | 200     
3  | 300 

eg.   
例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。  

 SecondHighestSalary |  
---|
200  | 

<span style="color: #0099ff">答案：</span>
```Mysql
# Write your MySQL query statement below 
select IFNULL((select Distinct Salary from Employee order by Salary DESC limit 1,1),null) as SecondHighestSalary 
```

### <span id='j3'>177. 第N高的薪水</span>
题目：编写一个 SQL 查询，获取 Employee 表中第 n 高的薪水（Salary）。  

 id | Salary    
---|---
1  | 100     
2  | 200     
3  | 300

 例如上述 Employee 表，n = 2 时，应返回第二高的薪水 200。如果不存在第 n 高的薪水，那么查询应返回 null。 

 SecondHighestSalary |  
---|
200  | 

<span style="color: #0099ff">答案：</span>
```Mysql
# 创建函数getNthHighestSalary，参数N为INT，返回类型为INT
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
# 开始
BEGIN
# 定义M为INT类型
Declare M INT;
# M为N-1，在查询语句直接写(N-1)会报语法错误
Set M = N-1;
  RETURN (
      # Write your MySQL query statement below.
      # 使用limit查询，第N-1条的下一条就是N，为第N高的工资
      select DISTINCT Salary from Employee order by Salary DESC limit M, 1
  );
END
```


### <span id='j4'>178. 分数排名</span>
题目：编写一个 SQL 查询来实现分数排名。如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。  

表1：  

 Id | Score     
---|---
1  | 3.50     
2  | 3.65    
3  | 4.00
4  | 3.85
5  | 4.00
6  | 3.65

eg.   
例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：  

 Score  | Rank     
---|---
 4.00  | 1    
 4.00  | 1    
 3.85  | 2    
 3.65  | 3    
 3.65  | 3    
 3.50  | 4    
 

 ```Mysql
 # Write your MySQL query statement below
 # 方法一
 select Score,
 (select count(distinct Score) from Scores as s2 where s2.Score >= s1.Score) Rank 
 from Scores as s1
 order by Score DESC;

# 方法二
 select s.Score, count(distinct s2.Score) Rank
 from Scores s
 left join Scores s2 ON s2.Score >= s.Score
 group by s.Id
 order by s.score Desc;
```


### <span id='j5'>连续出现的数字</span>
题目：编写一个 SQL 查询，查找所有至少连续出现三次的数字。  

 Id  | Num     
---|---
 1  |  1  
 2  |  1  
 3  |  1  
 4  |  2  
 5  |  1  
 6  |  2  
 7  |  2  

eg.  
例如，给定上面的 Logs 表， 1 是唯一连续出现至少三次的数字。

 ConsecutiveNums 
---
 1               

```Mysql
# Write your MySQL query statement below
# 方法一
select distinct tab.Num ConsecutiveNums from
(
select 
distinct l2.Num, 
(select l3.id from Logs l3 where l3.Num=l2.Num and l3.id=(l2.id+1)) id3,
(select l1.id from Logs l1 where l1.Num=l2.Num and l1.id=(l2.id-1)) id1
from Logs l2 
) tab 
where tab.id1>=0 and tab.id3>=0;

# 方法二
select  distinct l.Num ConsecutiveNums from
Logs l
left join Logs l2 ON l2.id = (l.id-1) AND l2.Num=l.Num
left join Logs l3 ON l3.id = (l.id+1) AND l3.Num=l.Num
where 
l2.id is not null
AND l3.id is not null;
```




















