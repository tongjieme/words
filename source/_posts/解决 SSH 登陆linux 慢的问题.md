---
title: 解决 SSH 登陆linux 慢的问题
---



SSH 登陆慢的原因有很多，首先应该使用 `-vvv` 来让SSH 打印详细的连接信息

比较常见的原因，可能是服务器端的ssh配置启用了 `反向DNS 解析 (reverse DNS lookup)` 一般来说，它对安全的提高的作用不是很大，所以可以考虑把它关闭来提高SSH登陆的速度

还有另外一个原因，GSSAPI 身份校验超时, 关闭此校验也可以提高登陆速度

### 关闭reverse DNS lookup方法
```
echo "UseDNS no" >> /etc/ssh/sshd_config
```

### 关闭 GSSAPI 身份校验
```
echo "GSSAPIAuthentication no" >> /etc/ssh/sshd_config
```

### 最后重启ssh服务
```
service sshd restart
```