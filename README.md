# 增
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
