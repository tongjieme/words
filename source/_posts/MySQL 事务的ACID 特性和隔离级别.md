---
title: MySQL 事务的ACID 特性和隔离级别

---

事务是一系列的SQL语句的集合, 定义了数据库系统中一个操作的结果在何时以何种方式对其他并发操作可见, 
事务具有回滚的特点, 如果事务中的某一条SQL语句无法成功执行, 那么所有语句都不会执行, 并且不会提交

例子: 在下面的事务中, 无论是 STATEMENT A 还是 B, 其中一个没有成功执行, 则所有操作都不会执行, 数据库并没有任何改变
```sql
START TRANSACTION;
STATEMENT A
STATEMENT B
COMMIT;
```

### 事务的ACID
事务的四个特征 `原子性`, `一致性`, `隔离性`, `持久性`
#### atomicity 原子性
原子是自然界一个不可分割的粒子, 事务具有原子性, 这意味着事务里的SQL语句只能全成功执行或全失败, 这一特性有助于数据的完整性
#### consistency 一致性
如果事务最终没有执行到COMMIT 语句, 那么所有修改都不应该保存到数据库
#### isolation 隔离性
同一时间可能有多个事务在操作同一个数据表或者某一行的数据, 在没执行到COMMIT语句前, 事务与事务之间的数据更改相互是透明不可见的
#### durability 持久性
执行到COMMIT 语句后, 事务所做的修改都会保存到数据库中



### 隔离级别
#### READ UNCOMMITTED 未提交读
最低的隔离级别, 性能影响最小, 不过性能也好不了太多, 而且有产生`脏读`的可能性, 事务可以看到其他事务“尚未提交”的修改
#### READ COMMITTED 提交读
一个事务从开始知道提交之前, 所做的任何修改对其它事务来说都是不可见的. 
#### REPEATABLE READ 可重复读
此级别是MySQL默认的事务级别, 同一事务中多次读取同样记录的结果是一致的.
#### SERIALIZABLE 可串行化 也叫 可序列化
级别最高, 所有事务都以串行化执行, MySQL对事务操作的所有行都会加锁,解决了幻读的问题
如果对数据局即时性及一致性有很高要求, 可考虑此级别, 
#### 隔离级别的查看与设置
```sql
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
SET GLOBAL tx_isolation='REPEATABLE-READ';
SET SESSION tx_isolation='SERIALIZABLE';
SET tx_isolation='SERIALIZABLE';
```

`my.cnf`
```
[mysqld]
transaction-isolation = REPEATABLE-READ
```

### 读现象
#### 脏读
当一个事务允许读取另外一个事务修改但未提交的数据时，就可能发生脏读。
#### 不可重复读
#### 幻(影)读
在事务执行过程中，当两个完全相同的查询语句执行得到不同的结果集。这种现象称为“幻(影)读（phantom read）”

### 总结表格
隔离级别 | 脏读可能性| 不可重复读可能性| 幻(影)读可能性
---- | ---- | ---- | ---- 
未提交读   | yes        | yes             | yes
提交读    | no         | yes             | yes
可重复读  | no        | no               | yes
可串行化  | no        | no              | no