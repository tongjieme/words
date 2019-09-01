---
title: 在centos 部署私有git 服务端
---

## 新增一个名为git 的用户
然后更改密码
服务端执行
``` bash
sudo useradd git
sudo passwd git
```

## 然后安装git
服务端执行
CentOS/Fedora: yum install git
Ubuntu/Debian: apt-get install git

## 生成SSH Key
本地电脑执行
``` bash
ssh-keygen -C "youremail@mailprovider.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/flynn/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again: 
Your identification has been saved in foo_rsa.
Your public key has been saved in foo_rsa.pub.
The key fingerprint is:
ab:cd:ef:01:23:45:67:89:0a:bc:de:f0:12:34:56:78 flynn@en.com
The key's randomart image is:
+--[ RSA 2048]----+
|    o+-+  ..     |
|  E o            |
|   . ++.o..      |
|    o o H .      |
|   . .   =       |
|    . =o.o=      |
| o .             |
|  .              |
|     = o  .      |
+-----------------+
```


## 将生成的SSH Key放回服务端的git用户下
服务端执行
``` bash
sudo su git
mkdir ~/.ssh && touch ~/.ssh/authorized_keys
nano ~/.ssh/authorized_keys
```
然后粘贴第一步生成的foo_rsa.pub 内容到 服务端的~/.ssh/authorized_keys 文件中
保存

## 生成一个本地仓
服务端执行
``` bash
git init --bare my-project.git
```

## 然后clone到本地电脑
本地电脑执行
``` bash
git clone ssh://git@server_ip:ssh_port/path/to/my-project.git
```

## 完成
至此, 你已经可以执行git相关指令了
