---
title: Dynamic-Javascript
date: 2016-06-28 22:23:39
categories: javascript
tags:
  - JavaScript
  - 动态
---

# 动态操作js

## 动态加载
在需要加载js的地方，直接写入一下代码
```javascript
document.write('<script src="js/jquery-3.0.0.js" type="text/javascript" charset="utf-8"><\/script>');
```
Example:
```javascript
<head>
    <meta charset="utf-8" />
    <title></title>
    <script type="text/javascript">
        window.jQuery || document.write('<script src="js/jquery-3.0.0.js" type="text/javascript" charset="utf-8"><\/script>');
    </script>
    <script src="js/jquery.jsonp.js" type="text/javascript" charset="utf-8"></script>
</head>
```
***

<!--more-->

## 动态更改
`<script src='' id="s1"></script>`
```javascript
var s1 = document.getElementById("s1");
s1.src = "js/jquery-3.0.0.js";
```

## 动态创建
```javascript
var oHead = document.getElementsByTagName('HEAD').item(0); 
var oScript= document.createElement("script"); 
oScript.type = "text/javascript"; 
oScript.src="test.js"; 
oHead.appendChild( oScript); 
```

## 按需加载
```javascript
window.jQuery || document.write('<script src="js/jquery-3.0.0.js" type="text/javascript" charset="utf-8"><\/script>');
```