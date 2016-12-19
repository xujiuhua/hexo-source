---
title: Read JavaScript-02
date: 2016-03-14 16:46:01
tags: [javascript]
---
##### **ECMAScript数据类型**
　　5种简单数据类型（也称为基本数据类型）Undefined、Null、Boolean、Number、String，还有一种负责类型Object
###### **typeof操作符**

- "undefined"——如果这个值未定义；
- "boolean"——如果这个值是布尔值；
- "number"——如果这个值是数值；
- "string"——如果这个值是字符串；
- "null"——如果这个值是对象或者null；
- "function"——如果这个值是函数；

<!-- more -->
>typeof操作符的操作数，可以是变量也可以是数值字面量
```javascript
alert(typeof message);
alert(typeof 57);
```
###### **Undefined类型**
```javascript
var message;
// var age
alert(message); // "undefined"
alert(age); // 产生错误
--------------------------------------
var message; 
// var age
alert(typeof message); // "undefined"
alert(typeof age); // "undefined"
```
###### **Null类型**
```javascript
var car = null;
alert(typeof car); // "object"
```
如果定义的变量准备在将来用户保存对象，那么最好将改变量初始化null而不是其他值，这样一来，只要直接检测null值就知道改变量是否保存了一个对象的引用。
###### **Boolean类型**

各种数据类型也Boolean的转换规则，如下表格

| 数据类型        | 转换为true的值   |  转换为false的值  |
| --------   | -----:  | :----:  |
| Boolean     | true |   false     |
| String        |   任何非空字符串   |   ""(空字符串)   |
| Number        |    任何非零数字（包括无穷大）    |  0和nan  |
| Object        |    任何对象    |  null  |
| Undefined        |    n/a   |  undefined  |

###### **Number类型**

 1. 整数

八进制，第一位必须是0，然后是八进制数字序列（0~7），如果字面中的数据超出了范围，那么前导0将被忽略，后面的数字被当做十进制解析
```javascript
alert(070); // 八进制的56
alert(079); // 无效的八进制，解析为79
```
十六进制，前面两位必须是0x,后跟（0~9或A~F),其中字母A~F可大写也可小写
```javascript
var hexNum1 = 0xA; // 十六进制10
var hexNum2 = 0x1f; // 十六进制31
```
在进行算数计算时，所有八进制和十六进制都会被转换为十进制数值。

 2. 浮点数

```javascript
var floatNum = 3.125e7; // 31250000（3.125*10^7);
--------------------------------------
var a = 0.1;
var b = 0.2;
alert(a + b); // 0.30000000000000004
```

>永远不要测试某个特定的浮点数值。

范围：
Number.MIN_VALUE:5e-324
Number.MAX_VALUE:1.7976931348623157e+308

3. NaN

(Not a Number),有两个非同寻常的特点：
- 任何涉及NaN的操作都会返回NaN（NaN/0）
- NaN与任何值都不相等，包括NaN

```javascript
alert(isNaN(NaN)); //true
alert(isNaN(10)); //false
alert(isNaN("10")); //false�
alert(isNaN("blue")); //true
alert(isNaN(true)); //false
```

4. 数值转换

`Number()、parseInt()、parseFloat()`
Number()可用于任何数据类型

```javascript
var num1 = Number("Hello world!");  //NaN
var num2 = Number("");              //0
var num3 = Number("000011");        //11
var num4 = Number(true);            //1
```
parseInt()、parseFloat()只能把字符串转换为数值
```javascript
var num1 = parseInt("1234blue"); // 1234
var num2 = parseInt(""); // NaN
var num3 = parseInt("0xA"); // 10
var num4 = parseInt(22.5); // 22
var num5 = parseInt("070"); // 56
var num6 = parseInt("70"); // 70
var num7 = parseInt("0xf"); // 15
---------------------------------
var num1 = parseInt("10", 2); //2 
var num2 = parseInt("10", 8); //8 
var num3 = parseInt("10", 10); //10
var num4 = parseInt("10", 16); //16
```
>无论在什么情况下，都明确指定基数

parseFloat()只解析十进制，因此没有第二个基数
```javascript
var num1 = parseFloat("1234blue"); //1234 
var num2 = parseFloat("0xA"); //0
var num3 = parseFloat("22.5"); //22.5
var num4 = parseFloat("22.34.5"); //22.34
var num5 = parseFloat("0908.5"); //908.5
var num6 = parseFloat("3.125e7"); //31250000
```
###### **String类型**

 1. 字符字面量
　特殊的字符字面量，也叫转义序列

```javascript
var text = "This is the letter sigma: \u03a3.";
alert(text.length);// 28�
```
有28个字符，其中6个字符长的转义序列`\u03a3`表示一个字符

 2. 字符串特点
 　　字符串是不可变的
 
 3. 字符串转换
 　　toString(),null和undefined无此方法。多数情况不传参数，但在调用数值的toString()时，可以传表示进制的参数

```javascript
var num = 10;
alert(num.toString()); // "10"
alert(num.toString(2)); // "1010"
alert(num.toString(8)); // "12"
alert(num.toString(10)); // "10"
alert(num.toString(16)); // "a"
```
在不知道要转换的值是不是null或undefined的情况下，可以使用转型函数String()

```javascript
var value1 = 10;
var value2 = true;
var value3 = null;
var value4;
alert(String(value1)); // "10"
alert(String(value2)); // "true"
alert(String(value3)); // "null"
alert(String(value4)); // "undefined"
```
###### **Object对象**
一组数据和功能的结合
