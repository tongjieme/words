---
title: MySQL 的储存引擎
date: 2017-03-15 00:34:14
---

mysql 的储存引擎简单的来说其实就是表的类型，其决定了此表的数据在服务器的储存方式

### 查看数据库支持的储存引擎
```
SHOW ENGINES;
```
![数据库支持的储存引擎](/img/1.png)

结果中的Support字段某行标注 default 表明mysql默认是用此储存引擎
要修改默认引擎可以在 my.ini 中修改 default-storage-engine=MYISAM 即可

常用的储存引擎有 InnoDB, MyISAM, MEMORY.

### InnoDb 
支持事务处理，支持外键，也有并发控制，如果对事务的完整性要求比较高，选择InnoDB 有它很大的优势

### MyISAM
插入数据的速度快，空间使用低，但不支持事务，如果表主要是用于插入新纪录和读取记录，选择MyISAM能提高处理效率

### MEMORY
所有数据都在内存中，数据的处理速度快，但安全性不高，如果对表需要很快的读写速度，而且对数据的安全性很低，选择MEMORY储存引擎是一个不错的选择

### 选择
对于非常复杂的应用，可以根据实际情况结合需求选择多种储存引擎的组合，已达到有效利用各自储存引擎的优势，避开其缺陷

### MySQL 储存引擎的完整对比

特性 | MyISAM | Memory | InnoDB | Archive | NDB
---- | ---- | ---- | ---- | ----
存储限制 |256TB | 内存大小 | 64TB | None | 384EB
事务支持 | No | No | Yes | No | Yes
锁粒度 | 表 | 表 | 行 | 行 | 行
并发控制 | No | No | Yes | No | No
B-tree 索引 | Yes | Yes | Yes | No | No
T-tree 索引 | No | No | No | No  | Yes
Hash 索引 | No  | Yes | No | No  | Yes
全文索引  | Yes | No  | Yes | No | No
聚索引 | No | No  | Yes | No | No
索引缓存  | Yes | N/A | Yes | No  | Yes
压缩数据 | Yes | No  | Yes  | Yes | No
数据加密 | Yes | Yes | Yes | Yes | Yes
集群数据库支持 | No | No | No | No  | Yes
主从同步  | Yes | Yes | Yes | Yes | Yes
外检支持 | No | No  | Yes | No  | Yes
备份与时间点恢复  | Yes | Yes | Yes | Yes | Yes
查询缓存支持 | Yes | Yes | Yes | Yes | Yes