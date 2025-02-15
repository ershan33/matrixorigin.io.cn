# **DAYOFYEAR()**

## **函数说明**

返回日期所对应在一年中的天数，返回值在 1-366 之间。

## **函数语法**

```
> DAYOFYEAR(expr)
```

## **参数释义**

|  参数   | 说明  |
|  ----  | ----  |
| expr  | 必要参数。需要提取天数的 date 格式的输入值 |

## **示例**

```sql
drop table if exists t1;
create table t1(a date, b datetime,c varchar(30));
insert into t1 values('2022-01-01','2022-01-01 01:01:01','2022-01-01 01:01:01');
insert into t1 values('2022-01-01','2022-01-01 01:01:01','2022-01-01 01:01:01');
insert into t1 values(20220101,'2022-01-01 01:01:01','2022-13-13 01:01:01');
insert into t1 values('2022-01-02','2022-01-02 23:01:01','2022-01-01 23:01:01');
insert into t1 values('2021-12-31','2021-12-30 23:59:59','2021-12-30 23:59:59');
insert into t1 values('2022-06-30','2021-12-30 23:59:59','2021-12-30 23:59:59');

mysql> select distinct dayofyear(a) as dya from t1;
+------+
| dya  |
+------+
|    1 |
|    2 |
|  365 |
|  181 |
+------+
4 rows in set (0.00 sec)

mysql> select * from t1 where dayofyear(a)>120;
+------------+---------------------+---------------------+
| a          | b                   | c                   |
+------------+---------------------+---------------------+
| 2021-12-31 | 2021-12-30 23:59:59 | 2021-12-30 23:59:59 |
| 2022-06-30 | 2021-12-30 23:59:59 | 2021-12-30 23:59:59 |
+------------+---------------------+---------------------+
2 rows in set (0.01 sec)

mysql> select * from t1 where dayofyear(a) between 1 and 184;
+------------+---------------------+---------------------+
| a          | b                   | c                   |
+------------+---------------------+---------------------+
| 2022-01-01 | 2022-01-01 01:01:01 | 2022-01-01 01:01:01 |
| 2022-01-01 | 2022-01-01 01:01:01 | 2022-01-01 01:01:01 |
| 2022-01-01 | 2022-01-01 01:01:01 | 2022-13-13 01:01:01 |
| 2022-01-02 | 2022-01-02 23:01:01 | 2022-01-01 23:01:01 |
| 2022-06-30 | 2021-12-30 23:59:59 | 2021-12-30 23:59:59 |
+------------+---------------------+---------------------+
4 rows in set (0.00 sec)
```

## **限制**

* 目前 `DAYOFYEAR()` 只支持 `date` 类型。
* 目前 `date` 格式只支持`yyyy-mm-dd` 和 `yyyymmdd`的数据格式。  
