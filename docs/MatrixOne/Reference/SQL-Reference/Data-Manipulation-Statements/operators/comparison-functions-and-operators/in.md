# **IN**

## **语法说明**

`IN` 运算符可以在 `WHERE` 语句中指定特定的多个值，本质上是多个 `OR` 条件的简写。

## **语法结构**

```
> SELECT column1, column2, ...
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

## **示例**

``` sql
create table t2(a int,b varchar(5),c float, d date, e datetime);
insert into t2 values(1,'a',1.001,'2022-02-08','2022-02-08 12:00:00');
insert into t2 values(2,'b',2.001,'2022-02-09','2022-02-09 12:00:00');
insert into t2 values(1,'c',3.001,'2022-02-10','2022-02-10 12:00:00');
insert into t2 values(4,'d',4.001,'2022-02-11','2022-02-11 12:00:00');

mysql> select * from t2 where a in (2,4);
a	b	c	d	e
2	b	2.0010	2022-02-09	2022-02-09 12:00:00
4	d	4.0010	2022-02-11	2022-02-11 12:00:00

mysql> select * from t2 where a not in (2,4);
a	b	c	d	e
1	a	1.0010	2022-02-08	2022-02-08 12:00:00
1	c	3.0010	2022-02-10	2022-02-10 12:00:00

mysql> select * from t2 where b not in ('e',"f");
a	b	c	d	e
1	a	1.0010	2022-02-08	2022-02-08 12:00:00
2	b	2.0010	2022-02-09	2022-02-09 12:00:00
1	c	3.0010	2022-02-10	2022-02-10 12:00:00
4	d	4.0010	2022-02-11	2022-02-11 12:00:00

mysql> select * from t2 where e not in ('2022-02-09 12:00:00') and a in (4,5);
a	b	c	d	e
4	d	4.0010	2022-02-11	2022-02-11 12:00:00

```

## **限制**

* 目前，`IN` 左侧只支持常数列表。
* `IN` 左侧只支持单列数据，不支持多列组成的元组。
* `IN`目前未对`NULL`值进行很好地处理，右侧不能使用 `NULL`值。
