---
title: "jQuery's data()"
date: 2016-03-10 23:45:10
tags: jQuery
---
#### 1、官方解释：
在匹配元素上存储任意相关数据 或 返回匹配的元素集合中的第一个元素的给定名称的数据存储的值。
<!--more-->

#### 2、作用效果
方法允许我们在DOM元素上绑定任意类型的数据,避免了循环引用的内存泄漏风险。

#### 3、方法
``` javascript
    .data( key, value )
        .data( key, value )
        .data( obj )
    .data( key )
        .data( key )
        .data()
```
#### 4、HTML5 data-*
从jQuery 1.4.3起， HTML 5 data- 属性 将自动被引用到jQuery的数据对象中。嵌入式破折号处理属性（ attributes）的方式在 jQuery 1.6 中已经改变，以使之符合W3C HTML5 规范.
必须是驼峰式写法：
<span style="color:red;">he second statement of the code above correctly refers to the data-last-value attribute of the element. In case no data is stored with the passed key, jQuery searches among the attributes of the element, converting a camel-cased string into a dashed string and then prepending data- to the result. So, the string lastValue is converted to data-last-value.<span>
```html
<div data-role="page" data-last-value="43" data-hidden="true" data-options='{"name":"John"}'></div>
```
``` javascript
$( "div" ).data( "role" ) === "page";
$( "div" ).data( "lastValue" ) === 43;
$( "div" ).data( "hidden" ) === true;
$( "div" ).data( "options" ).name === "John";
```

#### 5、数据安全
```
<div id="test">test</div>
```
```javascript
$("#test").data("safe", "数据安全");
```
从Dom节点中，是没办法看到绑定的数据。但是可以获取到