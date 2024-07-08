- [小tips](#小tips)
  - [学习资源](#学习资源)
  - [单行注释](#单行注释)
  - [多行注释](#多行注释)
  - [内部注释](#内部注释)
- [增](#增)
  - [INSERT](#insert)
- [删](#删)
  - [删库](#删库)
  - [删表](#删表)
- [改](#改)
  - [UPDATE](#update)
  - [ALTER](#alter)
- [查](#查)
  - [LIMIT和OFFSET](#limit和offset)
  - [LIKE](#like)
  - [UNION](#union)
  - [EXISTS](#exists)
  - [CASE](#case)
  - [IFNULL()](#ifnull)
  - [coalesce](#coalesce)
  - [REGEXP](#REGEXP)
- [MYSQL 数据库](#mysql-数据库)
  - [Constraints](#constraints)
  - [INDEX](#index)
  - [AUTO_INCREMENT](#auto_increment)
  - [DATES](#DATES)
- [窗口函数](#窗口函数)
  - [语法](#语法)
  - [种类](#种类)

# 小tips
## 学习资源
W3school(https://www.w3schools.com/) 该笔记的大部分知识来源于此
## 单行注释
```sql
-- Select all:
SELECT * FROM Customers;
```
## 多行注释
```sql
/*Select all the columns
of all the records
in the Customers table:*/
SELECT * FROM Customers;
```
## 内部注释
```sql
SELECT CustomerName, /*City,*/ Country FROM Customers;

```
# 增
## INSERT
### 简单插入值
```sql
insert into student_info value('1018' , '赵六' , '2003-08-02' , '男');
```
### 插入查询的值
```sql
INSERT INTO Customers (CustomerName, City, Country)
SELECT SupplierName, City, Country FROM Suppliers;
```
# 删
## 删库
```sql
DROP database testDB`
```
## 删表
### 普通删表的记录
```sql
DELETE FROM table1
```
### 普通删掉整个表
```sql
DROP table table2
```
### 快速删除整个表
```sql
TRUNCATE TABLE table3
```

# 改
## UPDATE
```sql
UPDATE table_name SET field1 = new-value1, field2 = new-value2 [WHERE Clause];
```
## ALTER
### 添加属性列
```sql
ALTER TABLE table1 ADD columns1 DATE(数据类型)
```
### 更改数据类型
```sql
ALTER TABLE table1
MODIFY COLUMN columns1 YEAR
```
### 删除表中的某个列
```sql
ALTER TABLE table1
DROP COLUMN columns1
```
# 查
## LIMIT和OFFSET
### 从第一个索引开始查（索引从0开始），查三条记录
```sql
SELECT * FROM user LIMIT 1,3；
```
### 从第一条记录开始查（索引从0开始），查三条记录
```sql
SELECT * FROM user LIMIT 3 OFFSET 1；
```
### 查询前五条记录
```sql
SELECT * FROM tanle LIMIT 0,5
```
## LIKE
### 查询城市第一个字母为 "a"、"c "或 "s "的所有记录。
```sql
SELECT * FROM Country WHERE City LIKE '[acs]%';
```
### 查询城市第一个字母以 "a "到 "f "开头的所有记录。
```sql
SELECT * FROM Country WHERE CIty LIKE '[a-f]%'
```
### 查询城市第一个字母不是 "a"、"c "或 "f "的所有记录。
```sql
SELECT * FROM Customers WHERE City LIKE '[!acf]%';
```
## UNION
### 默认保留不重复的查询结果
```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```
### 保留全部的查询结果
```sql
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```
## EXISTS
### 返回 TRUE，并列出产品价格小于 20 的供应商
```sql
SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.supplierID AND Price < 20);
```
### 编写解决方案，报告购买了产品 "A"，"B" 但没有购买产品 "C" 的客户的 customer_id 和 customer_name
```sql
法一
SELECT c.customer_id, c.customer_name
FROM Customers c
WHERE EXISTS (
    SELECT 1
    FROM Orders o
    WHERE o.customer_id = c.customer_id AND o.product_name = 'A'
)
AND EXISTS (
    SELECT 1
    FROM Orders o
    WHERE o.customer_id = c.customer_id AND o.product_name = 'B'
)
AND NOT EXISTS (
    SELECT 1
    FROM Orders o
    WHERE o.customer_id = c.customer_id AND o.product_name = 'C'
);
法二
SELECT o.customer_id, c.customer_name
FROM Orders o
LEFT JOIN Customers c ON o.customer_id = c.customer_id
GROUP BY o.customer_id
HAVING 
    SUM(IF(product_name = 'A', 1, 0)) > 0 AND
    SUM(IF(product_name = 'B', 1, 0)) > 0 AND
    SUM(IF(product_name = 'C', 1, 0)) = 0
```
## CASE
### 释义
>CASE 语句通过条件，并在满足第一个条件时返回一个值（就像 if-then-else 语句）。 因此，一旦某个条件为真，它就会停止读取并返回结果。 
如果没有条件为真，则返回 ELSE 子句中的值。 如果没有 ELSE 部分，也没有条件为真，则返回 NULL。
###  下面的 SQL 语句通过条件，并在满足第一个条件时返回一个值：
```sql
SELECT OrderID, Quantity,
CASE
    WHEN Quantity > 30 THEN 'The quantity is greater than 30'
    WHEN Quantity = 30 THEN 'The quantity is 30'
    ELSE 'The quantity is under 30'
END AS QuantityText
FROM OrderDetails;
```

## IFNULL()
### 释义
>如果表达式为 NULL，MySQL IFNULL() 函数会返回一个替代值。
### 下面的示例中，如果值为 NULL，则返回 0：
```sql
SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0))
FROM Products;
```
## coalesce [koʊˈæl.ɪs]
### 释义
>COALESCE 是 SQL 中的一个函数，它的全称就是 COALESCE 函数。COALESCE 函数用于从一系列的参数中返回第一个非 NULL 值。如果所有的参数值都是 NULL，那么 COALESCE 函数将返回 NULL

>**功能与IFNULL类似**
### 用法
```sql
SELECT COALESCE(column1, column2, column3) FROM table_name;
```
## REGEXP(正则表达式)
- ^ 匹配输入字符串的开始位置。如果设置了 RegExp 对象的 Multiline 属性，^ 也匹配 '\n' 或 '\r' 之后的位置。
查找 name 字段中以 'st' 为开头的所有数据：
```sql
SELECT name FROM person_tbl WHERE name REGEXP '^st';
```
- $ 匹配输入字符串的结束位置。如果设置了RegExp 对象的 Multiline 属性，$ 也匹配 '\n' 或 '\r' 之前的位置。
查找 name 字段中以 'os' 为开头的所有数据：
```sql
SELECT name FROM person_tbl WHERE name REGEXP 'os$';
```
还有更多的REGEXP操作符设置，详见菜鸟教程(https://www.runoob.com/mysql/mysql-regexp.html)
# MYSQL 数据库
## Constraints 
### 属性值
- NOT NULL  确保列中不能有 NULL 值
- UNIQUE    确保列中的所有值都是不同的
- PRIMARY KEY  NOT NULL 和 UNIQUE 的组合。 唯一标识表中的每一行
- FOREIGN KEY  防止破坏表之间链接的操作
- CHECK   确保列中的值满足特定条件
- DEFAULT 如果没有指定值，则为列设置默认值
- CREATE INDEX 用于快速创建和从数据库中检索数据
### 语法
```sql
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
);
```
### 要命名 UNIQUE 约束并在多个列上定义 UNIQUE 约束，请使用以下 SQL 语法：
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```
### PRIMARY KEY
#### 创建 "Persons "表时，以下 SQL 将在 "ID "列上创建 PRIMARY KEY：
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);
```
#### 要对 PRIMARY KEY 约束进行命名，并在多个列上定义 PRIMARY KEY 约束，请使用以下 SQL 语法：
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT PK_Person PRIMARY KEY (ID,LastName)
);
```
#### 要在已创建表格的情况下为 "ID "列创建 PRIMARY KEY 约束，请使用以下 SQL:
```sql
ALTER TABLE Persons
ADD PRIMARY KEY (ID);
```
### FOREIGN KEY
#### 释义
>FOREIGN KEY 约束用于防止破坏表之间链接的操作。 FOREIGN KEY 是一个表中的字段（或字段集合），它指向另一个表中的主键。 具有外键的表称为子表，具有主键的表称为引用表或父表。

![image](https://github.com/zuccbiubiubiu/MYSQL-/assets/111670275/2f6bfa74-ff8e-4d29-b008-9cf249c006b4)
>上表中，"订单 "表中的 "PersonID "列指向 "人员 "表中的 "PersonID "列。 人员 "表中的 "PersonID "列是 "人员 "表中的 PRIMARY KEY。 订单 "表中的 "PersonID "列是 "订单 "表中的 FOREIGN KEY。 FOREIGN KEY 约束可以防止向外键列插入无效数据，因为它必须是父表中包含的值之一。
#### 创建 "订单 "表时，以下 SQL 将在 "PersonID "列上创建一个 FOREIGN KEY：
```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```
#### 要对 FOREIGN KEY 约束进行命名，并在多个列上定义 FOREIGN KEY 约束，请使用以下 SQL 语法：
```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
);
```
### CHECK
#### 释义
>CHECK 约束用于限制列的取值范围。 如果在列上定义了 CHECK 约束，该列将只允许某些值。 如果在表上定义了 CHECK 约束，则可以根据行中其他列的值来限制某些列的值。
####
创建 "人员 "表时，下面的 SQL 将在 "年龄 "列上创建 CHECK 约束。 该 CHECK 约束确保个人的年龄必须是 18 岁或以上：
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CHECK (Age>=18)
);
```
## INDEX
### 在表上创建索引。 允许重复值：
```sql
CREATE INDEX index_name
ON table_name(column1,column2,...)
```
### 在表上创建唯一索引
```sql
CREATE UNION index_name
ON table1(column1,column2)
```
## AUTO_INCREMENT
### 以下 SQL 语句将 "Personid "列定义为 "Persons "表中的自动递增主键字段：
```sql
CREATE TABLE Persons (
    Personid int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (Personid)
);
```
### 要让 AUTO_INCREMENT 序列从另一个值开始，请使用以下 SQL 语句：
```sql
ALTER TABLE Persons AUTO_INCREMENT=100;
```
## DATES
### 种类
- DATE  format YYYY-MM-DD
- DATETIME  format: YYYY-MM-DD HH:MI:SS
- TIMESTAMP  format: YYYY-MM-DD HH:MI:SS
- YEAR  format YYYY or YY
### 注意点
![image](https://github.com/zuccbiubiubiu/MYSQL-/assets/111670275/07b8d3db-0dd0-4fed-9cd6-9f3774835ac7)
![image](https://github.com/zuccbiubiubiu/MYSQL-/assets/111670275/aa251f44-96c9-47e8-a2ec-d498e8f92cae)
所以最好不要加入时间部分
# 窗口函数 (OLAP函数（Online Anallytical Processing，联机分析处理）)
## 语法
><窗口函数>over(partition by 分组字段 order by 排序字段)
>注：分组和排序字段不是必须项，视问题情况而定
## 种类
- 聚合窗口函数 avg、sum、count、max、min等
- 排序窗口函数是rank、dense_rank、row_number
- 偏移窗口函数是lag、lead
### 聚合函数
#### 实例
- **找出某产品线活跃用户数比产品组内平均活跃用户数多的日期**
![image](https://github.com/zuccbiubiubiu/MYSQL-/assets/111670275/4e9ceffb-4722-4c80-8dee-b611ee1071d0)
#### 第一步 先根据日期和产品线分组，计算某天某产品线的活跃用户数
```sql
select concat(substr(dt,1,4),'-',substr(dt,5,2),'-',substr(dt,7,2)) as stat_date
      ,product_id
      ,count(distinct user_id) as active_user_num
from user_log
where dt between '20230404' and '20230430'
and product_id is not null
group by stat_date,product_id
```
#### 第二步 用avg窗口函数求产品线组内该段时间的平均活跃用户数
```sql
select *
      ,round(avg(active_user_num)over(partition by product_id),2) as avg_active_user_num  --切记要用产品分组
from 
(
    select concat(substr(dt,1,4),'-',substr(dt,5,2),'-',substr(dt,7,2)) as stat_date
          ,product_id
          ,count(distinct user_id) as active_user_num
    from user_log
    where dt between '20230404' and '20230430'
    and product_id is not null
    group by stat_date,product_id
) as t1
```
#### 第三步过滤出active_user_num≥avg_active_user_num行记录
```sql
select stat_date
      ,product_id
      ,active_user_num
      ,avg_active_user_num
from 
(
    select *
          ,round(avg(active_user_num)over(partition by product_id),2) as avg_active_user_num
    from 
    (
        select concat(substr(dt,1,4),'-',substr(dt,5,2),'-',substr(dt,7,2)) as stat_date
              ,product_id
              ,count(distinct user_id) as active_user_num
        from user_log
        where dt between '20230404' and '20230430'
        and product_id is not null
        group by stat_date,product_id
    ) as t1
) as t2
where active_user_num>=avg_active_user_num  --关键条件
order by product_id,stat_date
```
- **累计求和**
```sql
select *
      ,sum(active_cnt)over(partition by product_id order by stat_date) as acc_active_cnt
      --有排序，得出的是累计求和，无排序，得出的是组内总和
      ,sum(active_cnt)over(partition by product_id) as sum_active_cnt
from 
(
    select concat(substr(dt,1,4),'-',substr(dt,5,2),'-',substr(dt,7,2)) as stat_date
          ,product_id
          ,count(user_id) as active_cnt
    from user_log
    where dt between '20230404' and '20230430'
    and product_id is not null
    group by stat_date,product_id
) as t1
```
### 排序窗口函数
#### 区分
- rank：值相等时会重复，会产生空位，比如某列有13，12，12，11这4个数，用rank排序该列时，排序结果为1、2、2、4
- dense_rank：值相等时会重复，不会产生空位，还是上面的例子，用dense_rank排序该列时，排序结果为1、2、2、3
- row_number：值相等时不会重复，不会产生空位，还是上面的例子，用row_number排序该列时，排序结果为1、2、3、4
#### 实例
详见该文(https://zhuanlan.zhihu.com/p/629460362)






