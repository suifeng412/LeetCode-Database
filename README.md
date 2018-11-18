# LeetCode-Database
+ [175. 组合两个表](#j1)
+ [176. 第二高的薪水](#j2)
+ [177. 第N高的薪水](#j3)



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































