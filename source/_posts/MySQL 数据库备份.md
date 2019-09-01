---
title: MySQL 数据库备份与还原
date: 2017-04-08 00:34:14
---

出于数据安全考虑， 需要定期对数据进行备份。
备份 mysql 数据库可以用mysql自带的mysqldump 命令和第三方的mysqlhotcopy 备份

### mysqldump
备份整个数据库的：
```
mysqldump -u username -p dbname > BackupName.sql
```

如需单独备份其中的表：
```
mysqldump -u username -p dbname table1 table2 > BackupName.sql
```

当然也可以一次备份多个数据库：
```
mysqldump -u username -p --databases dbname1 dbname1 > BackupName.sql
```

### mysqlhotcopy
mysqlhotcopy 和mysqldump 相比有个好处 备份过程可以不用停止mysql服务器，而且备份的速度比较快，但相应也有缺点，就是只能备份MyISAM 储存引擎的表，不能备份 InnoDB 储存引擎的表
使用命令如下：
```
mysqlhotcopy [option] dbname1 dbname2 backupDir/
```
更多选项可以查看帮助文档
```
mysqlhotcopy --help
```

### 定期备份
在linux里实现定期mysql备份很容易，可以使用linux 的 crontab 定时任务 运行备份命令shell脚本即可



### 还原
可以直接使用mysql命令还原数据库
```
mysql -u root -p [dbname] < backup.sql
```


