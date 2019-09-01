---
title: php 7 新特性整理
date: 2017-04-02 00:34:14
---

# 变量类型声明
此特性需要通过以下代码启用`严格类型校验模式`, 并且必须在文件的顶部, 所以`严格类型校验模式`是基于文件可配的.
```
declare(strict_types=1);
```

```php
class Student
{
    public function __construct()
    {
        $this->name = 'durban';
    }
}

$student = new Student();

function enroll(Student $student)
{
    echo $student->name;
}
```


返回值类型声明
null合并运算符
太空船操作符（组合比较符）
通过 define() 定义常量数组
匿名类
Unicode codepoint 转译语法
为unserialize()提供过滤
预期
Group use declarations
生成器可以返回表达式
Generator delegation
整数除法函数 intdiv()
会话选项
preg_replace_callback_array()
CSPRNG Functions