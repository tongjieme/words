---
title: ubuntu 16.04 xenial 配置国内软件源
date: 2017-01-05 00:34:14
---


之所以要修改配置软件源，一方面是 Ubuntu 默认的软件更新源对我们来说比较慢，另一方面可能是升级以及安装软件时当前源出错。

修改配置ubuntu 软件源的几个步骤

# 备份软件源的配置
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
```

# 编辑源文件
```
sudo vi /etc/apt/sources.list
```

把里面的文本全部清空, 并写入以下文本

```
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
##测试版源
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
# 源码
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
##测试版源
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
# Canonical 合作伙伴和附加
deb http://archive.canonical.com/ubuntu/ xenial partner
deb http://extras.ubuntu.com/ubuntu/ xenial main
```

# 然后刷新源列表
```
sudo apt-get update
```