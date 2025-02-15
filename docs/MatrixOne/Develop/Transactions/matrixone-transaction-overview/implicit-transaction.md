# 隐式事务

在 MatrixOne 的事务类别中，隐式事务还遵循以下规则：

## 隐式事务规则

- 在 `AUTOCOMMIT` 发生变化的时候，隐式事务未提交的情况下，mo 会报错并提示用户需要提交后变化。
- `AUTOCOMMIT=0`，且在当前没有活跃事务的情况下，DDL（Data Definition Language，数据库定义语言）与参数配置文件均可执行，但 `CREATE` 以外的行为不能回滚。
- 在 `AUTOCOMMIT=1` 的情况下，每一条 DML（Data Manipulation Language，数据操纵语言）语句都是一个单独的事务，在执行后立即提交。
- 在 `AUTOCOMMIT=0` 的情况下，每一条 DML 语句都不会在执行后立即提交，需要手动进行 `COMMIT` 或 `ROLLBACK`，如果在尚未提交或回滚的状态下退出客户端，则默认回滚。
- 在 `AUTOCOMMIT=0` 的情况下，DML 与 DDL 可以同时存在，但是 DDL 仅限于 `CREATE`，其他一律无法支持。
- 在隐式事务存在未提交的内容时，开启一个显式事务，会强制提交之前未提交的内容。

## 隐式事务示例

例如，在上一个[显式事务](explicit-transaction.md)结束后，继续对 *t1* 插入数据 (4,5,6)，即成为一个隐式事务。而该隐式事务是否立即提交，取决于 `AUTOCOMMIT` 参数的值：

```
START TRANSACTION;
insert into t1 values(1,2,3);
COMMIT;
//此处开始一个隐式事务
insert into t1 values(4,5,6);
```
