# LeetCode-Database
+ [175. 组合两个表](#j1)
+ [176. 第二高的薪水](#j2)



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

 ```
 # Write your MySQL query statement below
 SELECT FirstName,LastName,City,State 
 FROM person p 
 LEFT JOIN Address ad ON p.PersonId=ad.PersonId;
 
 ```

### <span id='j2'>176. 第二高的薪水</span>
编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。  

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
```
# Write your MySQL query statement below 
select IFNULL((select Distinct Salary from Employee order by Salary DESC limit 1,1),null) as SecondHighestSalary 
```






























