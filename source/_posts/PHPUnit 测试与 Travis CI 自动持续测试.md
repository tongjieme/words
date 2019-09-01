---
title: PHPUnit 测试与 Travis CI 自动持续测试
<!-- date: 2017-01-19 00:34:14 -->

---

对程序编写自动化的测试是非常有好处的, 这可以让我们每次迭代更新程序, 以及部署程序到线上生产环境之前提前发现问题, 从此不用发布到线上担惊受怕会出bug

PHP 测试可以使用 PHPUnit 测试框架

### 安装PHPUnit
```
composer require --dev phpunit/phpunit
```
安装好了之后, 可以试着运行 `vendor/bin/phpunit -v` 


### 目录结构
新建好下面的目录结构
```
├── composer.json
├── composer.lock
├── phpunit.xml         # phpunit 的配置文件
├── src                 # 需要被测试的代码
│   └── Animal.php
├── tests               # 测试用例存放的文件夹
│   ├── AnimalTest.php  # 测试用例
│   └── bootstrap.php   # phpunit 入口文件
└── vendor              # 第三方composer 组件
    ├── autoload.php
    ├── bin
    ├── composer
    ├── doctrine
    ├── myclabs
    ├── phpdocumentor
    ├── phpspec
    ├── phpunit
    ├── sebastian
    ├── symfony
    └── webmozart
```


### 准备要测试的代码
假设我们要测试`src/Animal.php`, 里面的构造方法是否成功初始化Animal 示例的名字
```
<?php
namespace App;

class Animal
{
    protected $name;

    public function __construct($name)
    {
        $this->setName($name);
    }

    /**
     * @return mixed
     */
    public function getName()
    {
        return $this->name;
    }

    /**
     * @param mixed $name
     */
    public function setName($name)
    {
        $this->name = $name;
    }


}
```

### 配置PHPUnit

修改 `phpunit.xml` 添加以下内容
```
<?xml version="1.0" encoding="UTF-8"?>
<phpunit bootstrap="tests/bootstrap.php">
  <testsuites>
    <testsuite name="Animal">
      <directory suffix="Test.php">tests</directory>
    </testsuite>
  </testsuites>
</phpunit>
```

我们制定tests文件里面以Test.php 结尾的文件都是测试用例
```
<testsuite name="Animal">
  <directory suffix="Test.php">tests</directory>
</testsuite>
```

### bootstrap.php
phpunit 会在运行测试的时候首先运行`tests/bootstrap.php`文件, 所以为了测试用例的编写便利, 可以在文件中添加composer 的自动加载
```
<?php
require dirname(__DIR__) . '/vendor/autoload.php';
```

### 测试用例(test case)
每个测试用例就是一个类, 每个类的方法就是一个测试, 测试用例组成测试组件(test suite)

在 `phpunit.xml` 配置里 我们配置了 `<directory suffix="Test.php">tests</directory>`, 所以`test` 文件夹就是我们存放测试用例的地方, 而且所有的测试用例文件应该以 Test.php结尾来命名
phpunit 会运行所有测试用例里面每个以test开头的方法, 每个方法都是一个测试
`tests/AnimalTest.php`
```
<?php
namespace App;

use PHPUnit\Framework\TestCase;

class AnimalTest extends TestCase
{
    public function testSetNameWithConstructor()
    {
        $animal = new Animal('James');
        $this->assertAttributeEquals('James', 'name', $animal);
    }
}
```

### 运行测试
```
vendor/bin/phpunit -c phpunit.xml
```
测试结果
```
PHPUnit 5.7.19 by Sebastian Bergmann and contributors.

.                                                                   1 / 1 (100%)

Time: 113 ms, Memory: 3.75MB

OK (1 test, 2 assertions)
```