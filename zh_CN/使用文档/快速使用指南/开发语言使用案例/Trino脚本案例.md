# DSS用户测试样例：Trino

DSS用户测试样例的目的是为平台新用户提供一组测试样例，用于熟悉DSS的常见操作，并验证DSS平台的正确性

![img.png](../images/useCase/img.png)

## 基本SQL语法

### 简单查询

```sql
select * from dept;
```

### Join连接

```sql
select * from emp
left join dept
on emp.deptno = dept.deptno;
```

### 聚合函数

```sql
SELECT count(*), nationkey FROM customer GROUP BY nationkey;
```

### data_diff

date_diff(unit, timestamp1, timestamp2) → bigint

```sql
SELECT date_diff('second', TIMESTAMP '2020-03-01 00:00:00', TIMESTAMP '2020-03-02 00:00:00');
-- 86400

SELECT date_diff('hour', TIMESTAMP '2020-03-01 00:00:00 UTC', TIMESTAMP '2020-03-02 00:00:00 UTC');
-- 24

SELECT date_diff('day', DATE '2020-03-01', DATE '2020-03-02');
-- 1

SELECT date_diff('second', TIMESTAMP '2020-06-01 12:30:45.000000000', TIMESTAMP '2020-06-02 12:30:45.123456789');
-- 86400

SELECT date_diff('millisecond', TIMESTAMP '2020-06-01 12:30:45.000000000', TIMESTAMP '2020-06-02 12:30:45.123456789');
-- 86400123
```

### 插入

```sql
INSERT INTO cities VALUES (2, 'San Jose'), (3, 'Oakland');
```

### 建表

```sql
CREATE TABLE orders (
  orderkey bigint,
  orderstatus varchar,
  totalprice double,
  orderdate date
)
WITH (format = 'ORC')
```
