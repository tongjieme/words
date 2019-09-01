---
title: Ubuntu 16.04 安装nginx
---


## 安装 依赖库
#### gcc g++
```sh
apt-get install build-essential -y
apt-get install libtool -y
```

#### pcre
```sh
apt-get update
apt-get install libpcre3 libpcre3-dev -y
```

#### zlib
```sh
apt-get install zlib1g-dev -y
```

#### ssl
```sh
apt-get install openssl -y
```





## 下载源码：
```sh
wget http://nginx.org/download/nginx-1.11.3.tar.gz
```sh
## 解压：
```sh
tar -zxvf nginx-1.11.3.tar.gz
```

## 进入解压目录：
```sh
cd nginx-1.11.3
```

## 配置：
```sh
./configure --prefix=/usr/local/nginx 
```

## 编辑nginx：
```sh
make
```


## 安装nginx：
```sh
sudo make install
```

## 启动nginx：
```sh
sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

## 查看nginx进程：
```sh
ps -aux | grep nginx
```

## 其它资料
nginx 官方安装指引
https://www.nginx.com/resources/wiki/start/topics/tutorials/install/

安装配置选项列表
https://www.nginx.com/resources/wiki/start/topics/tutorials/installoptions/