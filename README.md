# 增
## INSERT
insert into student_info value('1018' , '赵六' , '2003-08-02' , '男');
# 删
# 改
## UPDATE
UPDATE table_name SET field1 = new-value1, field2 = new-value2 [WHERE Clause];
# 查
## 查询前五条记录
SELECT * FROM tanle LIMIT 0,5
## LIMIT和OFFSET
### 从第一个索引开始查（索引从0开始），查三条记录
SELECT * FROM user LIMIT 1,3；
### 从第一条记录开始查（索引从0开始），查三条记录
SELECT * FROM user LIMIT 3 OFFSET 1；
# 函数
## coalesce [koʊˈæl.ɪs]
### 释义
COALESCE 是 SQL 中的一个函数，它的全称就是 COALESCE 函数。COALESCE 函数用于从一系列的参数中返回第一个非 NULL 值。如果所有的参数值都是 NULL，那么 COALESCE 函数将返回 NULL
### 用法
SELECT COALESCE(column1, column2, column3) FROM table_name;
