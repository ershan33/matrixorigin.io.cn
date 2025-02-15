# **FULL JOIN**

## **语法说明**

``FULL JOIN`` 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行。

``FULL JOIN`` 关键字结合了 ``LEFT JOIN`` 和 ``RIGHT JOIN`` 的结果。

**说明**：在一些数据库中： ``FULL JOIN`` 等同于 ``FULL OUTER JOIN``。

## **语法结构**

```
> SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name=table2.column_name;

```

## **示例**

```sql
drop table if exists t1,t2,t3;
create table t1 (libname1 varchar(21) not null primary key, city varchar(20));
create table t2 (isbn2 varchar(21) not null primary key, author varchar(20), title varchar(60));
create table t3 (isbn3 varchar(21) not null, libname3 varchar(21) not null, quantity int);
insert into t2 values ('001','Daffy','Aducklife');
insert into t2 values ('002','Bugs','Arabbitlife');
insert into t2 values ('003','Cowboy','Lifeontherange');
insert into t2 values ('000','Anonymous','Wannabuythisbook?');
insert into t2 values ('004','BestSeller','OneHeckuvabook');
insert into t2 values ('005','EveryoneBuys','Thisverybook');
insert into t2 values ('006','SanFran','Itisasanfranlifestyle');
insert into t2 values ('007','BerkAuthor','Cool.Berkley.the.book');
insert into t3 values ('000','NewYorkPublicLibra',1);
insert into t3 values ('001','NewYorkPublicLibra',2);
insert into t3 values ('002','NewYorkPublicLibra',3);
insert into t3 values ('003','NewYorkPublicLibra',4);
insert into t3 values ('004','NewYorkPublicLibra',5);
insert into t3 values ('005','NewYorkPublicLibra',6);
insert into t3 values ('006','SanFransiscoPublic',5);
insert into t3 values ('007','BerkeleyPublic1',3);
insert into t3 values ('007','BerkeleyPublic2',3);
insert into t3 values ('001','NYC Lib',8);
insert into t1 values ('NewYorkPublicLibra','NewYork');
insert into t1 values ('SanFransiscoPublic','SanFran');
insert into t1 values ('BerkeleyPublic1','Berkeley');
insert into t1 values ('BerkeleyPublic2','Berkeley');
insert into t1 values ('NYCLib','NewYork');

mysql> select city,libname1,count(libname1) as a from t3 full join t1 on libname1=libname3 join t2 on isbn3=isbn2 group by city,libname1;
+----------+--------------------+------+
| city     | libname1           | a    |
+----------+--------------------+------+
| NewYork  | NewYorkPublicLibra |    6 |
| SanFran  | SanFransiscoPublic |    1 |
| Berkeley | BerkeleyPublic1    |    1 |
| Berkeley | BerkeleyPublic2    |    1 |
+----------+--------------------+------+
```
