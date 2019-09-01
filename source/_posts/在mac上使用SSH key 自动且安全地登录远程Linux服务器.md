---
title: 在Mac上使用SSH key 自动且安全地登录远程Linux服务器
---
通常登陆远程Linux经常需要输入密码, 频繁输入密码比较低效率且有被暴力破解的风险, SSH key提供了免密码且更加安全登陆服务器的方法,以下为设置过程

## 生成私钥和公钥

在终端输入
```bash
ssh-keygen -t rsa
```

输入生成地址及使用口令 可选择性回车跳过
```bash
Enter file in which to save the key (/home/demo/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
```

终端生成过程类似这样
```bash
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/demo/.ssh/id_rsa.
Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
The key fingerprint is:
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
The key's randomart image is:
+--[ RSA 2048]----+
|          .oo.   |
|         .  o.E  |
|        + .  o   |
|     . = = .     |
|      = S = .    |
|     o + = +     |
|      . o + o .  |
|           . o   |
|                 |
+-----------------+
```

这样就生成了 id_rsa(private key 私钥) 和 id_rsa.pub (public key 公钥)



## 存放私钥和公钥
id_rsa 存放在客户端电脑, 存放路径为 ~/demo/.ssh/id_rsa 
id_rsa.pub 存放在服务端, 存放路径为 ~/demo/.ssh/authorized_keys
使用SSH命令可以便捷地自动复制公钥到远程服务端
```bash
cat ~/.ssh/id_rsa.pub | ssh user@123.45.56.78 "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```
### Tips
多个公钥可以同时存放在服务器的 ~/demo/.ssh/authorized_keys 文件里, 以换行符隔开即可

## 完成
至此, 已经可以顺利自动登录远程服务器
```bash
James:Desktop jackwin$ ssh demo@123.456.1.1 
Last login: Sun Jan 22 02:56:18 2017 from 183.1.1.1
[demo@instance-11 ~]$ 
```

### 问题排除
比较常见的问题一般为权限设置, 一定要确保客户端和服务端的密钥文件及其文件夹是可被 SSH 进程可阅读的
如有需要可通过以下命令修改权限
```bash
chmod 755 authorized_keys
chmod 700 id_rsa
chmod 700 id_rsa.pub
```

#### 示例:
服务端
```bash
demo@instance-11 .ssh]$ ls -lah
total 12K
drwx------. 2 demo demo 4.0K Jan 22 02:52 .
drwxr-xr-x. 4 demo demo 4.0K Jan  8 16:21 ..
-rwxr-xr-x. 1 demo demo  394 Jan 22 02:53 authorized_keys
```
客户端
```bash
James:Desktop jackwin$ ls -lah ~/.ssh
total 72
drwx------   7 jackwin  staff   238B Jun 22  2015 .
drwxr-xr-x@ 90 jackwin  staff   3.0K Jan 22 10:16 ..
-rw-------@  1 jackwin  staff   1.6K May  4  2015 id_rsa
-rw-r--r--   1 jackwin  staff   394B Jun 14  2015 id_rsa.pub
-rw-r--r--@  1 jackwin  staff    18K Jan 22 10:50 known_hosts
```