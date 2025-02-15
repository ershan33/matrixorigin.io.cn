# **Derived Tables**

## **语法描述**

当 `SELECT` 语句的 `FROM` 从句中使用独立子查询时，我们也经常将其称为派生表，因为实际上外部查询将子查询的结果当作了一个数据源。

## **语法结构**

每个 FROM 子查询的表都必须要有一个名字，因此[AS]操作符是必须的。子查询的SELECT列表中每个列也必须要有一个唯一的名字。

```
> SELECT ... FROM (subquery) [AS] name ...
```

## **示例**

```sql
> CREATE TABLE tb1 (c1 INT, c2 CHAR(5), c3 FLOAT);
> INSERT INTO tb1 VALUES (1, '1', 1.0);
> INSERT INTO tb1 VALUES (2, '2', 2.0);
> INSERT INTO tb1 VALUES (3, '3', 3.0);
> select * from tb1;
+------+------+--------+
| c1   | c2   | c3     |
+------+------+--------+
|    1 | 1    | 1.0000 |
|    2 | 2    | 2.0000 |
|    3 | 3    | 3.0000 |
+------+------+--------+
3 rows in set (0.03 sec)

> SELECT sc1, sc2, sc3 FROM (SELECT c1 AS sc1, c2 AS sc2, c3*3 AS sc3 FROM tb1) AS sb WHERE sc1 > 1;
+------+------+--------+
| sc1  | sc2  | sc3    |
+------+------+--------+
|    2 | 2    | 6.0000 |
|    3 | 3    | 9.0000 |
+------+------+--------+
2 rows in set (0.02 sec)
``````sql
CREATE TABLE tb1 (c1 INT, c2 CHAR(5), c3 FLOAT);
INSERT INTO tb1 VALUES (1, '1', 1.0);
INSERT INTO tb1 VALUES (2, '2', 2.0);
INSERT INTO tb1 VALUES (3, '3', 3.0);

mysql> select * from tb1;
+------+------+--------+
| c1   | c2   | c3     |
+------+------+--------+
|    1 | 1    | 1.0000 |
|    2 | 2    | 2.0000 |
|    3 | 3    | 3.0000 |
+------+------+--------+
3 rows in set (0.03 sec)

mysql> SELECT sc1, sc2, sc3 FROM (SELECT c1 AS sc1, c2 AS sc2, c3*3 AS sc3 FROM tb1) AS sb WHERE sc1 > 1;
+------+------+--------+
| sc1  | sc2  | sc3    |
+------+------+--------+
|    2 | 2    | 6.0000 |
|    3 | 3    | 9.0000 |
+------+------+--------+
2 rows in set (0.02 sec)
```

- **Subquery with Join**:

```sql
> create table t1 (libname1 varchar(21) not null primary key, city varchar(20));
> create table t2 (isbn2 varchar(21) not null primary key, author varchar(20), title varchar(60));
> create table t3 (isbn3 varchar(21) not null, libname3 varchar(21) not null, quantity int);
> insert into t2 values ('001','Daffy','Aducklife');
> insert into t2 values ('002','Bugs','Arabbitlife');
> insert into t2 values ('003','Cowboy','Lifeontherange');
> insert into t2 values ('000','Anonymous','Wannabuythisbook?');
> insert into t2 values ('004','BestSeller','OneHeckuvabook');
> insert into t2 values ('005','EveryoneBuys','Thisverybook');
> insert into t2 values ('006','SanFran','Itisasanfranlifestyle');
> insert into t2 values ('007','BerkAuthor','Cool.Berkley.the.book');
> insert into t3 values('000','NewYorkPublicLibra',1);
> insert into t3 values('001','NewYorkPublicLibra',2);
> insert into t3 values('002','NewYorkPublicLibra',3);
> insert into t3 values('003','NewYorkPublicLibra',4);
> insert into t3 values('004','NewYorkPublicLibra',5);
> insert into t3 values('005','NewYorkPublicLibra',6);
> insert into t3 values('006','SanFransiscoPublic',5);
> insert into t3 values('007','BerkeleyPublic1',3);
> insert into t3 values('007','BerkeleyPublic2',3);
> insert into t3 values('001','NYC Lib',8);
> insert into t1 values ('NewYorkPublicLibra','NewYork');
> insert into t1 values ('SanFransiscoPublic','SanFran');
> insert into t1 values ('BerkeleyPublic1','Berkeley');
> insert into t1 values ('BerkeleyPublic2','Berkeley');
> insert into t1 values ('NYCLib','NewYork');
> select * from (select city,libname1,count(libname1) as a from t3 join t1 on libname1=libname3 join t2 on isbn3=isbn2 group by city,libname1) sub ;
+----------+--------------------+------+
| city     | libname1           | a    |
+----------+--------------------+------+
| NewYork  | NewYorkPublicLibra |    6 |
| SanFran  | SanFransiscoPublic |    1 |
| Berkeley | BerkeleyPublic1    |    1 |
| Berkeley | BerkeleyPublic2    |    1 |
+----------+--------------------+------+
4 rows in set (0.00 sec)
``````sql
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
insert into t3 values('000','NewYorkPublicLibra',1);
insert into t3 values('001','NewYorkPublicLibra',2);
insert into t3 values('002','NewYorkPublicLibra',3);
insert into t3 values('003','NewYorkPublicLibra',4);
insert into t3 values('004','NewYorkPublicLibra',5);
insert into t3 values('005','NewYorkPublicLibra',6);
insert into t3 values('006','SanFransiscoPublic',5);
insert into t3 values('007','BerkeleyPublic1',3);
insert into t3 values('007','BerkeleyPublic2',3);
insert into t3 values('001','NYC Lib',8);
insert into t1 values ('NewYorkPublicLibra','NewYork');
insert into t1 values ('SanFransiscoPublic','SanFran');
insert into t1 values ('BerkeleyPublic1','Berkeley');
insert into t1 values ('BerkeleyPublic2','Berkeley');
insert into t1 values ('NYCLib','NewYork');

mysql> select * from (select city,libname1,count(libname1) as a from t3 join t1 on libname1=libname3 join t2 on isbn3=isbn2 group by city,libname1) sub ;
+----------+--------------------+------+
| city     | libname1           | a    |
+----------+--------------------+------+
| NewYork  | NewYorkPublicLibra |    6 |
| SanFran  | SanFransiscoPublic |    1 |
| Berkeley | BerkeleyPublic1    |    1 |
| Berkeley | BerkeleyPublic2    |    1 |
+----------+--------------------+------+
4 rows in set (0.00 sec)
```

## **限制**

MatrixOne 暂不支持选择多列进行子查询。
