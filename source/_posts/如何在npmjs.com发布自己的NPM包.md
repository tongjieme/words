---
title: 如何在npmjs.com发布自己的NPM包
---

## 登陆 NPM 账户
``` bash
npm login
npm config ls #确认是否已成功登陆
```
## cd 到想发布的文件夹

``` bash
cd NPM_package
npm init #初始化创建package.json 文件
```

## 发布

``` bash
npm publish
```

## 完成
到这里查看是否成功发布
[https://www.npmjs.com/~](https://www.npmjs.com/~)