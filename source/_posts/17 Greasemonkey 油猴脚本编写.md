---
title: Greasemonkey 油猴脚本编写
---

## 起始脚本
```js
// ==UserScript==
// @name test： test
// @icon            https://xxx/icon.jpg
// @grant           GM_xmlhttpRequest
// @author          AC
// @create          2015-11-25
// @run-at          document-start
// @version         13.3
// @connect         *
// @include         /^https?://jamestung.net/.*/
// @home-url        https://greasyfork.org/zh-TW/scripts/XXX
// @namespace       10086@qq.com
// @copyright       2017, AC
// @description     这是描述
// @lastmodified    2017-12-04
// @feedback-url    https://greasyfork.org/zh-TW/scripts/XXX
// @note            备注
// @grant           GM_getValue
// @require         https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js
// ==/UserScript==

(function () {
    var ACMO = window.MutationObserver || window.WebKitMutationObserver || window.MozMutationObserver;
    var option = {'childList': true, 'subtree': true};
    var observer = new ACMO(function (records) {

    });

    observer.observe(document.body, option);


    $('body').html("empty body");
})();
```

## 用户脚本元键值
https://greasyfork.org/zh-CN/help/meta-keys

## 官方用户脚本元键值 列表
https://wiki.greasespot.net/Metadata_Block