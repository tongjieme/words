---
title: Mac禁用内存压缩
---

内存压缩功能是OSX 10.9 Mavericks增加的新特色,在内存不够用的情况下利用CPU压缩内存数据,以改善当前可用内存不足的情况,
内存压缩功能在某种程度上增加了CPU的负担,若果CPU运行速度比较吃紧的情况下,可以通过禁用内存压缩功能舒缓CPU压力.

以下为禁用方法

在终端输入:
```Bash
sudo nvram boot-args="vm_compressor=1" # vm_compressor默认值为 4,这里设为1表示禁用内存压缩
sysctl -a vm.compressor_mode # 如果这里输出是 1 的话表示禁用成功
```
#### 问题排除
在新版 macOS Sierra 10.12.1 中, 必须在recovory 模式中才能禁用, 
recovory 模式 进入方法为: 开机按住 `cmd + R`