---
title: 前端编码规范(1/4)-通用规范
date: 2016-03-23 16:19:07
tags: 
---

## 适用范围

 HTML, Javascript and CSS / SCSS

 *** 

### 文件 / 资源名称
在同一个web工程中的所有文件名应该遵循同样命名规则。为了易读使用"-"符是最好想法，同样这也是权威url的使用规则。
(i.e. `//example.com/blog/my-blog-entry` or `//s.example.com/images/big-black-background.jpg`).

以字母开头，禁止使用数字开头（除非有版本等特殊要求）。

所有字母要小写

有种情况，如果资源存在继承关系(i.e. .min.js, .min.css)，这种情况建议使用"."分隔符

<!--more-->

**Bad**
```
MyScript.js
myCamelCaseName.css
i_love_underscores.html
1001-scripts.js
my-file-min.css
```
**Good**
```
my-script.js
my-camel-case-name.css
i-love-underscores.html
thousand-and-one-scripts.js
my-file.min.css
```

 *** 

### 协议

省略协议。

省略(`http:`, `https:`)from images and other media files, style sheets, and scripts，除非这两种协议都无法找到文件。

**Bad**
```
<script src="http://cdn.com/foundation.min.js"></script>
```

**Good**
```
<script src="//cdn.com/foundation.min.js"></script>
```

**Bad**
```
.example {
  background: url(http://static.example.com/images/bg.jpg);
}
```

**Good**
```
.example {
  background: url(//static.example.com/images/bg.jpg);
}
```

 *** 

### 文本缩进

缩进使用两个空格

```
<ul>
  <li>Fantastic</li>
  <li>Great</li>
  <li>
    <a href="#">Test</a>
  </li>
</ul>
```

```
@media screen and (min-width: 1100px) {
  body {
    font-size: 100%;
  }
}
```

```
(function(){
  var x = 10;

  function y(a, b) {
    return {
      result: (a + b) * x
    }

  }
}());
```

 *** 

### 注释

当注释时，不是写这段代码是什么，而要写这段代码为什么，并且写上想法，当然最好带上你的参照链接地址等等。

**Bad**
```
var offset = 0;

if(includeLabels) {
  // Add offset of 20
  offset = 20;
}
```

**Good**
```
var offset = 0;

if(includeLabels) {
  // If the labels are included we need to have a minimum offset of 20 pixels
  // We need to set it explicitly because of the following bug: http://somebrowservendor.com/issue-tracker/ISSUE-1
  offset = 20;
}
```

[JSDoc](http://usejsdoc.org/) or [YUIDoc](http://yui.github.io/yuidoc/)

 *** 

### 代码校验

对于JavaScript建议使用 JSLint / JSHint校验项目规则

[原文](https://github.com/gionkunz/chartist-js/blob/develop/CODINGSTYLE.md)