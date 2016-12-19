---
title: JavaScript高级程序设计-Math
date: 2016-04-08 21:10:05
categories: JavaScript高级程序设计（第3版）
tags: 
  - JavaScript
  - Math
---
# Math对象的方法

## **min()和max()**
```javascript
var max = Math.max(3, 54, 32, 16);
alert(max); //54
var min = Math.min(3, 54, 32, 16);
alert(min); //3
```
数组中的最大值和最小值
```javascript
var values = [1, 2, 3, 4, 5, 6, 7, 8];
var max = Math.max.apply(Math, values);
```

***

<!--more-->

## **random()**
从某个整数范围内，随机选择一个值
```javascript
var randomValue = Math.floor(Math.random()*可能的总数 + 第一个值);
e.g.
// 选择1-10之间的值
var val1 = Math.floor(Math.random()*10 + 1);
// 选择2-10之间的值
var val2 = Math.floor(Math.random()*9 + 2);
```
应用：
```javacript
function selectFrom(lowerValue, upperValue) {
	var choices = upperValue - lowerValue + 1;
	return Math.floor(Math.random() * choices + lowerValue);
}
var colors = ["red", "green", "blue", "yellow", "black", "purple", "brown"];
var color = colors[selectFrom(0, colors.length-1)];
```

## **舍入方法、对象的属性、其他方法**
见：JavaScript高级程序设计（第3版）
