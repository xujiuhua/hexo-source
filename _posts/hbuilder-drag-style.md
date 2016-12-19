---
title: Hbuilder开发APP(03)-侧滑方式
date: 2016-03-28 09:24:55
categories: APP开发
tags: 
  - hbuilder
  - mui
  - mask
  - h5+
  - webview
---

## **侧滑方式**

 - 主界面移动，菜单不动
 - 主界面不动，菜单移动
 - 整体移动（整体滑动暂不支持android手机，因为两个页面的移动动画，无法保证同步性）

## **方式一：主界面移动，菜单不动**

>主界面的zindex必须大于侧滑界面的zindex

### 主界面预加载侧滑页面
主界面样式：
```javascript
main.setStyle({
	zindex: 2
});
```

侧滑页面样式：
```javascript
styles: {
	left: 0,		// 窗口水平向右的偏移量
	width: '70%',   // 侧滑页面只显示70%
	zindex: 1
}
```

<!--more-->

### 打开侧滑
原理：menu.show显示菜单界面，但是因为菜单界面的zindex比主界面zindex低，所以仍然无法看到菜单界面，show方法回调函数设置主界面样式，然后可以看到主界面慢慢滑动，菜单界面真正显示出来。
```javascript
menu.show('none', 0, function() {
	//主窗体开始侧滑并显示遮罩
	main.setStyle({
		mask: 'rgba(0,0,0,0.4)',   // 显示遮罩
		left: '70%'				   // 窗口水平向右的偏移量
	});
});
```

### 关闭侧滑
```javascript
main.setStyle({
	mask: 'none',               // 去除遮罩
	left: 0                     // 窗口水平向右的偏移量
});
// 隐藏菜单界面，必须延迟隐藏，才能看到移动的效果
setTimeout(function() {
	menu.hide();
}, 200);
```

***

## **方式二：主界面不动，菜单移动**

>主界面zindex必须小于侧滑界面zindex

### 主界面预加载侧滑界面
主界面样式：
```javascript
main.setStyle({
	zindex: 1
});
```

侧滑页面样式：
```javascript
styles: {
	left: '-70%',	 // 把侧滑界面向左偏移70%
	width: '70%',    // 侧滑页面只显示70%
	zindex: 2
}
```

### 打开侧滑
原理：先调用侧滑界面show方法，因为侧滑界面zindex大于主界面zindex，所以窗口在主界面上，然后执行回调函数设置侧滑界面left=0,效果是侧滑界面left(-70%-->0%)显示出来。
```javascript
menu.show('none', 0, function() {
	//主窗体开始侧滑并显示遮罩
	main.setStyle({
		mask: 'rgba(0,0,0,0.4)'	// 显示遮罩
	});
	menu.setStyle({
		mask: 'none',
		left: 0
	});
});
```

### 关闭侧滑
```javascript
main.setStyle({
	mask: 'none'
});
showMenu = false;
menu.setStyle({
	left: '-70%'
});
//等窗体动画结束后，隐藏菜单webview，节省资源；
setTimeout(function() {
	menu.hide();
}, 200);
```

***

## **总结**

 - 注意两种方式中主界面和侧滑界面zindex大小。
 - 菜单关闭一定要在动画结束后执行，否则感觉很突兀。
 - 理解侧滑菜单显示和关闭的先后步骤。

**[源码](https://github.com/xujiuhua/Mui-Mask)**
