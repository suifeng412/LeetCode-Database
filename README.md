# LeetCode-Database
+ [175. 组合两个表](#j1)



### <span id='j1'>175. 组合两个表</span>
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

题目：编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：  
`FirstName, LastName, City, State`
 ```
 # Write your MySQL query statement below
 SELECT FirstName,LastName,City,State 
 FROM person p 
 LEFT JOIN Address ad ON p.PersonId=ad.PersonId;
 
 ```




















