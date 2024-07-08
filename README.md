# 小tips
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
### 删除表中的某个列
```sql
ALTER TABLE table1 DROP COLUMN columns1
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
### 删除属性列
```sql
ALTER TABLE table1
DROP COLUMN columns1
```
### 更改数据类型
```sql
ALTER TABLE table1
MODIFY COLUMN columns1 YEAR
# 查
## 查询前五条记录
```sql
SELECT * FROM tanle LIMIT 0,5
```
## LIMIT和OFFSET
### 从第一个索引开始查（索引从0开始），查三条记录
```sql
SELECT * FROM user LIMIT 1,3；
```
### 从第一条记录开始查（索引从0开始），查三条记录
```sql
SELECT * FROM user LIMIT 3 OFFSET 1；
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
# MYSQL 数据库
## Constraints
- NOT NULL - Ensures that a column cannot have a NULL value
- UNIQUE - Ensures that all values in a column are different
- PRIMARY KEY - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
- FOREIGN KEY - Prevents actions that would destroy links between tables
- CHECK - Ensures that the values in a column satisfies a specific condition
- DEFAULT - Sets a default value for a column if no value is specified
- CREATE INDEX - Used to create and retrieve data from the database very quickly




