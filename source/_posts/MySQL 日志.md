---
title: MySQL 日志

---
mysql 的错误信息和日常操作都会记录在日志文件中， 分析这些文件可以知道数据库的具体运行情况和哪些地方可以进行优化

MySQL日志有四种
二进制日志     记录了除查询语句的操作，二进制文件形式记录
错误日志      记录了数据库启动，停止，运行错误等信息
通用查询日志               记录用户所有的操作
慢查询日志     记录比较慢超出设定时间的操作

### 二进制日志
二进制日志非常有用，当数据库异常停止后, 或遭到破坏，可以通过二进制日志记录修复数据库

二进制日志记录默认是关闭的，需要通过设置 my.cnf 开启

```bash
[mysqld]
log-bin=/usr/local/mysql/log/bin.log
```

### 利用二进制日志还原数据库
二进制日志记录了除查询以外所有操作语句，利用 mysqlbin 读取二进制日志 将结果pipe 进 mysql 命令即可还原数据库
```bash
mysqlbin log.000001 | mysql -u root -p
mysqlbin log.000002 | mysql -u root -p
```
注意： 还原顺序一定要从 编号比较小的开始还原

### 错误日志
错误日志主要的作用是作为当数据库出现异常时，用来分析错误原因的资料
错误日志可以通过 my.cnf 里的 log-error 选项设置
```bash
[mysqld]
log-error=/usr/local/mysql/log/error.log
```

错误日志是文本形式存储的，所以可以直接cat 查看即可

### 慢查询日志
通过调查慢查询日志，可以找出执行效率低的查询语句
此日志默认关闭，需要通过配置my.cnf 后重启数据库以启用
```bash
[mysqld]
log-slow-queries=/usr/local/mysql/log/slow.log      #日志保存路径
long_query_time=n                              #查询所需时间，单位秒
```
