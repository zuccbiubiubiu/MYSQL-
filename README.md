# 增
## INSERT
insert into student_info value('1018' , '赵六' , '2003-08-02' , '男');
# 删
## 删库
DROP database testDB
## 删表
### 普通删表的记录
DELETE FROM table1
### 普通删掉整个表
DROP table table2
### 快速删除整个表
TRUNCATE TABLE table3
### 删除表中的某个列
ALTER TABLE table1 DROP COLUMN columns1
# 改
## UPDATE
UPDATE table_name SET field1 = new-value1, field2 = new-value2 [WHERE Clause];
## ALTER
ALTER TABLE table1 ADD columns1 DATE(数据类型)
# 查
## 查询前五条记录
SELECT * FROM tanle LIMIT 0,5
## LIMIT和OFFSET
### 从第一个索引开始查（索引从0开始），查三条记录
SELECT * FROM user LIMIT 1,3；
### 从第一条记录开始查（索引从0开始），查三条记录
SELECT * FROM user LIMIT 3 OFFSET 1；
## LIKE
### 查询城市第一个字母为 "a"、"c "或 "s "的所有记录。
SELECT * FROM Country WHERE City LIKE '[acs]%';
### 查询城市第一个字母以 "a "到 "f "开头的所有记录。
SELECT * FROM Country WHERE CIty LIKE '[a-f]%'
### 查询城市第一个字母不是 "a"、"c "或 "f "的所有记录。
SELECT * FROM Customers WHERE City LIKE '[!acf]%';
## UNION
### 默认保留不重复的查询结果
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
### 保留全部的查询结果
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;


# 函数
## coalesce [koʊˈæl.ɪs]
### 释义
COALESCE 是 SQL 中的一个函数，它的全称就是 COALESCE 函数。COALESCE 函数用于从一系列的参数中返回第一个非 NULL 值。如果所有的参数值都是 NULL，那么 COALESCE 函数将返回 NULL
### 用法
SELECT COALESCE(column1, column2, column3) FROM table_name;
