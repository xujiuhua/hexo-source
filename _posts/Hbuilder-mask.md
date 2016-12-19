---
title: Hbuilder开发APP(01)-遮罩蒙版
date: 2016-03-24 14:58:10
categories: APP开发
tags: 
  - hbuilder
  - mui
  - mask
  - h5+
  - webview
---

首先要理清概念，遮罩蒙版是属于HTML5+规范中Webview的配置[WebviewStyle](http://www.html5plus.org/doc/zh_cn/webview.html#plus.webview.WebviewStyle)，

可实现功能：
1、弹出框popover(e.g.新闻分享弹出框)
2、侧滑菜单
3、新手引导页
后续会逐一介绍

mask: (String 类型 )窗口的遮罩
用于设置Webview窗口的遮罩层样式，遮罩层会覆盖Webview中所有内容，包括子webview，并且截获webview的所有触屏事件，`此时Webview窗口的点击操作会触发maskClick事件`。 字符串类型，可取值： rgba格式字符串，定义纯色遮罩层样式，如"rgba(0,0,0,0.5)"，表示黑色半透明； "none"，表示不使用遮罩层； 默认值为"none"，即无遮罩层。

<!--more-->

***

下面介绍两种实现方式。

## **H5+实现**

Example:

```html
<!DOCTYPE html>
<html>
	<head>
	<meta charset="utf-8">
	<title>Webview Example</title>
	</head>
	<body>
		Webview窗口页面加载完成事件
		<br/>
		点击窗口关闭遮罩层
	</body>
	<script type="text/javascript">
		var ws = null;
		// H5 plus事件处理
		function plusReady(){
			ws = plus.webview.currentWebview();
			// 显示遮罩层
			ws.setStyle({mask:"rgba(0,0,0,0.5)"});
			// 点击关闭遮罩层
			ws.addEventListener("maskClick",function(){
				ws.setStyle({mask:"none"});
			},false);
		}
		if(window.plus){
			plusReady();
		}else{
			document.addEventListener("plusready",plusReady,false);
		}
	</script>
</html>
```

***

## **MUI封装实现**
遮罩蒙版常用的操作包括：创建、显示、关闭，如下代码：
```javascript
// callback为用户点击蒙版时自动执行的回调；
// 此方法就如H5+中监听maskClick事件
var mask = mui.createMask(callback);
mask.show();//显示遮罩
mask.close();//关闭遮罩
```

Example:

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <!-- 引入css -->
        <link rel="stylesheet" type="text/css" href="../css/mui.min.css"/>
        <title>主界面</title>
    </head>
    <body>
    	这是mui主界面
    </body>
    <script src="../js/mui.min.js"></script>
	<script type="text/javascript">
		var mask = mui.createMask(_closeMask);
		mui.plusReady(function() {
			mask.show();
		});
		// 此方法执行完成后，会自动关闭遮罩，
		// return false可以阻止事件发生，不关闭遮罩
		function _closeMask() {
			// doSomething();
			
			// 注意：此处不能调用mask.close()
			// mask.close()等价于执行了一次回调_closeMask
			// 如此就会进入死循环
//  			mask.close();
		}
	</script>
</html>
```
