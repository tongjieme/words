---
title: Nodejs 使用Pug.js 实现多语言应用
---

Pug.js(jade) 是一款非常简洁且高性能的模板引擎, 可用于Nodejs和浏览器, 此文介绍如何使用它在nodejs环境下实现多语言访问

首先体验一下其简洁的语法:
```pug
doctype html
html(lang="en")
  head
    title= pageTitle
    script(type='text/javascript').
      if (foo) bar(1 + 5)
  body
    h1 Pug - node template engine
    #container.col
      if youAreUsingPug
        p You are amazing
      else
        p Get on it!
      p.
        Pug is a terse and simple templating language with a
        strong focus on performance and powerful features.
```

转化成
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Pug</title>
    <script type="text/javascript">
      if (foo) bar(1 + 5)
    </script>
  </head>
  <body>
    <h1>Pug - node template engine</h1>
    <div id="container" class="col">
      <p>You are amazing</p>
      <p>Pug is a terse and simple templating language with a strong focus on performance and powerful features.</p>
    </div>
  </body>
</html>
```
以下介绍如何实现多语言功能
### 安装 express pug locale 及创建文件树
```bash
cd your_project
npm i express pug locale --save
touch app.js
mkdir views
touch zh-cn.json
touch en-us.json
```
以下为最终的文件树
```bash
tree -I 'node_modules'
.
├── app.js
├── lang
│   ├── en-us.json
│   └── zh-cn.json
├── package.json
└── views
    └── index.pug
```
## 集成到express
```js
var express = require('express');
var app = express();
var fs = require("fs");

app.set('view engine', 'pug');

app.listen(3333, () => {
  console.log('Example app listening on port 3333!');
});
```
## 语言相关配置
app.js
```js
var locale = require("locale");

app.use(locale(['en-us', 'zh-cn']));
locale.Locale["default"] = 'en-us';
```
lang/zh-cn.json
```json
{
  "Home": "首页",
  "Services": "服务",
  "Products": "产品",
  "About_us": "关于我们",
  "Contact_us": "联系我们"
}
```
lang/zh-cn.json
```json
{
  "Home": "Home",
  "Services": "Services",
  "Products": "Products",
  "About_us": "About us",
  "Contact_us": "Contact_us"
}
```
## 加载语言文件
```js
var lang = {};
fs.readdirSync(`${__dirname}/lang`).forEach(file => {
  if(file.indexOf('.') !== 0) {
    // 忽略 . 开头的文件
    var language_flag = file.split('.')[0];
    lang[language_flag] = JSON.parse(fs.readFileSync(`${__dirname}/lang/${file}`));
  }
});
```
## 动态识别用户当前环境
由于使用locale包, 识别用户语言环境变得相当简单
```js
app.get('/', (req, res) => {
  req.locale // locale 已经根据用户语言环境与本应用支持的语言衡量出最佳匹配语言
});
```
## 渲染相应语言html
app.js
```js
app.get('/', (req, res) => {
  res.render('index', lang[req.locale])
});
```
index.pug
```pug
html
  head
    meta(charset="utf-8")
  body
    ul
      li= Home
      li= Services
      li= Products
      li= About_us
      li= Contact_us
```
## 完整的app.js 代码
```js
var express = require('express');
var app = express();
var fs = require("fs");
var locale = require("locale");

app.use(locale(['en-us', 'zh-cn']));
locale.Locale["default"] = 'en-us';

app.set('view engine', 'pug');



var lang = {};
fs.readdirSync(`${__dirname}/lang`).forEach(file => {
  if(file.indexOf('.') !== 0) {
    // 忽略 . 开头的文件
    var language_flag = file.split('.')[0];
    lang[language_flag] = JSON.parse(fs.readFileSync(`${__dirname}/lang/${file}`));
  }
});

app.get('/', (req, res) => {
  res.render('index', lang[req.locale])
});


app.listen(3333, () => {
  console.log('Example app listening on port 3333!');
});
```

最终的文件树
```bash
tree -I 'node_modules'
.
├── app.js
├── lang
│   ├── en-us.json
│   └── zh-cn.json
├── package.json
└── views
    └── index.pug
```
## 最终效果
![pug_multi_language](/img/pug_multi_language.png)

#### 附常见几款模板引擎性能对比 Table
Template | Syntax | Streaming | Asynchronous | Auto-escape
---- | ---- | ---- | ---- | ----
[dustjs-linkedin](https://github.com/linkedin/dustjs) | Text | ✔ | ✔ | ✔
[doT](https://github.com/olado/doT) | Text | ✖ | ✖ | ✖
[handlebars](https://github.com/wycats/handlebars.js) | Text | ✖ | ✖ | ✔
[pug](https://github.com/pugjs/pug) | Short-hand HTML | ✖ | ✖ | ✔
[marko](https://github.com/marko-js/marko) | HTML/Concise HTML | ✔ | ✔ | ✔
[nunjucks](http://mozilla.github.io/nunjucks/) | Text | ✖ | ✔ | ✖
[react](https://facebook.github.io/react/)<sup>1</sup> | JSX | ✖ | ✖ | ✔
[swig](http://mozilla.github.io/nunjucks/) | Text | ✖ | ✖ | ✔

性能对比资料来源: [templating-benchmarks](https://github.com/marko-js/templating-benchmarks/)









