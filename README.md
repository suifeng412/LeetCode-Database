# LeetCode-Database
+ [175. 组合两个表](#j1)
+ [176. 第二高的薪水](#j2)
+ [177. 第N高的薪水](#j3)
+ [178. 分数排名](#j4)
+ [180. 连续出现的数字](#j5)
+ [181. 超过经理收入的员工](#j6)
+ [182. 查找重复的电子邮箱](#j7)
+ [183. 从不订购的用户](#j8)
+ [184. 部门工资最高的员工](#j9)
+ [185. 部门工资前三高的员工](#j10)
+ [196. 删除重复的电子邮箱](#j11)
+ [197. 上升的温度](#j12)
+ [262. 行程和用户](#j13)
+ [595. 大的国家](#14)



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
 
<span style="color: #0099ff">答案：</span>
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

 ConsecutiveNums |
---|
 1 |              

<span style="color: #0099ff">答案：</span>
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


### <span id='j6'>181. 超过经理收入的员工</span>
题目：
Employee 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。   
给定 Employee 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。  

 Id | Name  | Salary | ManagerId    
---|---|---|--- 
 1  | Joe   | 70000  | 3         
 2  | Henry | 80000  | 4         
 3  | Sam   | 60000  | NULL      
 4  | Max   | 90000  | NULL      

eg.  
在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

 Employee |
---|
 Joe |      

<span style="color: #0099ff">答案：</span>
```Mysql
SELECT e.Name Employee 
FROM
Employee e
LEFT JOIN Employee m ON e.ManagerId = m.id
WHERE e.Salary > m.Salary;
```



### <span id='j7'>182. 查找重复的电子邮箱</span>

题目：编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。   

 Id | Email   
---|---
 1  | a@b.com 
 2  | c@d.com 
 3  | a@b.com 

eg.   
根据以上输入，你的查询应返回以下结果：

 Email   
--- |
 a@b.com 

```mysql
# Write your MySQL query statement below
SELECT Email FROM Person GROUP BY Email HAVING count(id)>1;
```



### <span id='j8'>183. 从不订购的用户</span>
题目：某网站包含两个表，Customers 表和 Orders 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。  

Customers 表：  

 Id | Name  
---|---
 1  | Joe   
 2  | Henry 
 3  | Sam   
 4  | Max   


Orders 表：  

 Id | CustomerId 
---|---
 1  | 3          
 2  | 1          

eg.  
例如给定上述表格，你的查询应返回：  

 Customers 
---|
 Henry     
 Max       
  
```mysql
# Write your MySQL query statement below
# 方法一
SELECT c.Name Customers FROM Customers c
LEFT JOIN Orders o ON c.id=o.CustomerId 
WHERE o.CustomerId is null;

# 方法二
SELECT Name Customers FROM Customers 
WHERE id not in(SELECT CustomerId FROM Orders);
```



### <span id='j9'>184. 部门工资最高的员工</span>
题目：  
Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。  

 Id | Name  | Salary | DepartmentId 
---|---|---|---
 1  | Joe   | 70000  | 1            
 2  | Henry | 80000  | 2            
 3  | Sam   | 60000  | 2            
 4  | Max   | 90000  | 1            

Department 表包含公司所有部门的信息。  

 Id | Name     
---|---
 1  | IT       
 2  | Sales    
 
编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。  

eg.   

 Department | Employee | Salary 
---|---|---
 IT         | Max      | 90000  
 Sales      | Henry    | 80000  
 
```mysql
# Write your MySQL query statement below
# 方法一
SELECT d.Name Department,tab2.Name Employee,tab2.Salary 
FROM 
(
    SELECT e.DepartmentId,e.Salary,e.Name
    FROM 
    (   
        SELECT max(Salary) Salary,DepartmentId FROM Employee GROUP BY DepartmentId
    ) tab
    LEFT JOIN Employee e ON e.Salary=tab.Salary and e.DepartmentId=tab.DepartmentId
) tab2  
LEFT JOIN Department d  ON tab2.DepartmentId = d.id 
WHERE tab2.DepartmentId>0 and d.id>0;

# 方法二
select d.Name Department,e.Name Employee,e.Salary
from Employee e 
left join Department d ON e.DepartmentId=d.Id
left join 
(
    select max(Salary) Salary,DepartmentId from Employee group by DepartmentId
) as s
ON s.Salary=e.Salary AND s.DepartmentId=e.DepartmentId 
where 
s.Salary is not null AND d.Name is not null;

# 方法三
select d.Name Department,e.Name Employee,e.Salary from Employee e 
left join Department d ON e.DepartmentId=d.Id
where 
(
    select count(distinct e2.Salary) from Employee e2 where e.DepartmentId=e2.DepartmentId AND e2.Salary>=e.Salary
) <= 1 
AND d.Name is not null;
```



### <span id='j10'>185. 部门工资前三高的员工</span>
题目：  
Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id 。  

 Id | Name  | Salary | DepartmentId 
---|---|---|---
 1  | Joe   | 70000  | 1            
 2  | Henry | 80000  | 2            
 3  | Sam   | 60000  | 2            
 4  | Max   | 90000  | 1            
 5  | Janet | 69000  | 1            
 6  | Randy | 85000  | 1     
 
Department 表包含公司所有部门的信息。  

 Id | Name     
---|---
 1  | IT       
 2  | Sales    

编写一个 SQL 查询，找出每个部门工资前三高的员工。例如，根据上述给定的表格，查询结果应返回：  

 Department | Employee | Salary 
---|---|---
 IT         | Max      | 90000  
 IT         | Randy    | 85000  
 IT         | Joe      | 70000  
 Sales      | Henry    | 80000  
 Sales      | Sam      | 60000  
        
```mysql
# Write your MySQL query statement below
# 第一钟方法
SELECT d.Name Department, e.Name Employee, e.Salary 
FROM Employee e
LEFT JOIN Department d ON e.DepartmentId = d.Id
WHERE 
d.Id>0 AND
(SELECT COUNT(distinct Salary) FROM Employee WHERE DepartmentId=e.DepartmentId AND Salary >=e.Salary )<=3
ORDER BY d.Name;

# 第二种方法
select d.Name Department, e.Name Employee, e.Salary 
from Employee e
left join Department d on e.DepartmentId = d.Id
where 
e.Salary>=
ifnull(
(
    select distinct Salary from Employee where      
    DepartmentId=e.DepartmentId order by Salary desc limit 2,1
) 
, 0    )
and d.Name is not null;
```


### <span id='j11'>196. 删除重复的电子邮箱</span>
题目：编写一个 SQL 查询，来删除 Person 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。   

 Id | Email            
---|---
 1  | john@example.com 
 2  | bob@example.com  
 3  | john@example.com 

eg.  
例如，在运行你的查询语句之后，上面的 Person 表应返回以下几行:  

 Id | Email            
---|---
 1  | john@example.com 
 2  | bob@example.com  

```mysql
# Write your MySQL query statement below
DELETE FROM `Person` 
WHERE Id  not in 
( 
    SELECT id FROM
        (SELECT min(Id) id FROM `Person` GROUP BY Email) tab
);
```


### <span id='j12'>197. 上升的温度</span>
题目：给定一个 Weather 表，编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 Id。  

 Id(INT) | RecordDate(DATE) | Temperature(INT) 
---|---|---
       1 |       2015-01-01 |               10 
       2 |       2015-01-02 |               25 
       3 |       2015-01-03 |               20 
       4 |       2015-01-04 |               30 

eg.  
例如，根据上述给定的 Weather 表格，返回如下 Id:  

 Id |
---|
  2 
  4 
  
```mysql
# Write your MySQL query statement below
# 方法一
SELECT w.Id   FROM Weather w
WHERE 
(
    SELECT count(1) FROM Weather WHERE  RecordDate=date_sub(w.RecordDate, interval 1 day) AND Temperature<w.Temperature
);

# 方法二
select w.Id from Weather w
left join Weather w2 ON w2.RecordDate=date_sub(w.RecordDate, interval 1 day) and w2.Temperature<w.Temperature
where w2.Id is not null;
```


### <span id='j13'>262. 行程和用户</span>
题目：Trips 表中存所有出租车的行程信息。每段行程有唯一键 Id，Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。Status 是枚举类型，枚举成员为 (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’)。   

 Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at
---|---|---|---|---|---
 1  |     1     |    10     |    1    |     completed      |2013-10-01
 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01
 3  |     3     |    12     |    6    |     completed      |2013-10-01
 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01
 5  |     1     |    10     |    1    |     completed      |2013-10-02
 6  |     2     |    11     |    6    |     completed      |2013-10-02
 7  |     3     |    12     |    6    |     completed      |2013-10-02
 8  |     2     |    12     |    12   |     completed      |2013-10-03
 9  |     3     |    10     |    12   |     completed      |2013-10-03
 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03

Users 表存所有用户。每个用户有唯一键 Users_Id。Banned 表示这个用户是否被禁止，Role 则是一个表示（‘client’, ‘driver’, ‘partner’）的枚举类型。  

 Users_Id | Banned |  Role  
---|---|---
    1     |   No   | client 
    2     |   Yes  | client 
    3     |   No   | client 
    4     |   No   | client 
    10    |   No   | driver 
    11    |   No   | driver 
    12    |   No   | driver 
    13    |   No   | driver 

写一段 SQL 语句查出 2013年10月1日 至 2013年10月3日 期间非禁止用户的取消率。基于上表，你的 SQL 语句应返回如下结果，取消率（Cancellation Rate）保留两位小数。  

 Day    | Cancellation Rate 
---|---
 2013-10-01 |       0.33        
 2013-10-02 |       0.00        
 2013-10-03 |       0.50        

```mysql
# Write your MySQL query statement below
# 方法一
select 
t.Request_at Day,
FORMAT(
count(t.Status='cancelled_by_driver' or t.Status='cancelled_by_client' or null)/count(1), 2)+0 `Cancellation Rate`
from Trips t
left join Users c on c.Users_Id=t.Client_Id
left join Users d on d.Users_Id=t.Driver_Id 
where 
t.Request_at BETWEEN '2013-10-01' AND '2013-10-03'
and c.Banned='No' and d.Banned='No'
group by t.Request_at;

# 方法二
SELECT tab2.Request_at Day,if(FORMAT(cancelled/total, 2),0+cast(FORMAT(cancelled/total, 2) as char),0) `Cancellation Rate` FROM
(
SELECT count(distinct t.Id) total,Request_at
FROM Trips t
LEFT JOIN Users u ON u.Users_Id=t.Client_Id 
WHERE u.Banned = 'No' AND t.Request_at>='2013-10-01' AND t.Request_at<='2013-10-03' AND t.Request_at<='2013-10-03' 
GROUP BY Request_at   
) tab2
LEFT JOIN 
(   
SELECT count(distinct t.Id) cancelled,Request_at
FROM Trips t
LEFT JOIN Users u ON u.Users_Id=t.Client_Id 
WHERE u.Banned = 'No' AND t.Request_at>='2013-10-01' AND t.Request_at<='2013-10-03' AND t.Status !='completed' 
GROUP BY Request_at
) tab1
ON tab1.Request_at=tab2.Request_at;
```



### <span id='j14'>595. 大的国家</span>
题目：
这里有张 World 表  

 name            | continent  | area       | population   | gdp           
---|---|---|---|---
 Afghanistan     | Asia       | 652230     | 25500100     | 20343000      
 Albania         | Europe     | 28748      | 2831741      | 12960000      
 Algeria         | Africa     | 2381741    | 37100000     | 188681000     
 Andorra         | Europe     | 468        | 78115        | 3712000       
 Angola          | Africa     | 1246700    | 20609294     | 100990000     

如果一个国家的面积超过300万平方公里，或者人口超过2500万，那么这个国家就是大国家。  
编写一个SQL查询，输出表中所有大国家的名称、人口和面积。  

eg.  
例如，根据上表，我们应该输出:  

 name         | population  | area         
---|---|---
 Afghanistan  | 25500100    | 652230       
 Algeria      | 37100000    | 2381741      

```mysql
# Write your MySQL query statement below
select name,population,area from World 
where population>25000000 or area>3000000;
```






