---
title: JavaScript开发技巧-超时与间歇
date: 2016-04-18 22:49:58
categories: js技巧
tags: 
  - JavaScript
  - setTimeout
  - setInterval
---

使用超时调用来模拟间歇调用时一种最佳模式,原因是最后一次间歇调用可能会在前一次间歇调用结束前启动，所以最好不要使用间歇调用。

## 间歇调用 
// no recommend
```javascript
var num = 0;
var max = 10;
var intervalId = null;
function incrementNumber() {
    num++;
    if(num == max) {
        clearInterval(intervalId);
        alert("Done!");
	}
}
intervalId = setInterval(incrementNumber, 500);
```

<!--more-->

## 超时调用
// recommend
```javascript
var num = 0;
var max = 10;
function incrementNumber() {
    num++;
    if(num < max) {
        setTimeout(incrementNumber, 500);  
    }else{
        alert("Done!");
    }
}
setTimeout(incrementNumber, 500);
```