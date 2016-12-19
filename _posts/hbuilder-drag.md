---
title: Hbuilder开发APP(02)-侧滑(webview模式)
date: 2016-03-24 22:42:25
categories: APP开发
tags: 
  - hbuilder
  - mui
  - mask
  - h5+
  - webview
---

mui提供了两种侧滑导航实现：webview模式和div模式，两种模式各有优劣，适用于不同的场景。[两者区别](http://dev.dcloud.net.cn/mui/ui/#offcanvas)。

这里介绍webview模式的侧滑

***

## **实现步骤**

 1. 创建两个页面：主页面和侧滑页面
 2. 主页面预加载侧滑页面
 3. 主页面监听遮罩的`maskClick`事件
 4. 打开侧滑菜单
 5. 关闭侧滑菜单

***

<!--more-->

## **JavScript**

```javascript
var main = null,
	menu = null;
var showMenu = false;

mui.plusReady(function() {
	main = plus.webview.currentWebview();
	main.addEventListener('maskClick', closeMenu);
	//处理侧滑导航，为了避免和子页面初始化等竞争资源，延迟加载侧滑页面；
	setTimeout(function() {
		menu = mui.preload({
			id: 'drag-h5-menu',
			url: 'drag-h5-menu.html',
			styles: {
				left: 0,		// 窗口水平向右的偏移量
				width: '70%',
				zindex: -1
			},
			show: {
				aniShow: 'none'
			}
		});
	}, 200);
});

function closeMenu() {
	if(showMenu) {
		main.setStyle({
			mask: 'none',
			left: 0
		});
		showMenu = false;
		menu.hide();
	}
}

function openMenu() {
	if(!showMenu) {
		// 设置透明遮罩，防止菜单页被点击，
		// 因为菜单页是隐藏在后面，不加透明遮罩有可能点击。
		// 当菜单界面显示出来时，去掉遮罩
		menu.setStyle({
			mask: 'rgba(0,0,0,0)'
		});
		
		menu.show('none', 0, function() {
			//主窗体开始侧滑并显示遮罩
			main.setStyle({
				mask: 'rgba(0,0,0,0.4)',	// 显示遮罩
				left: '70%'				    // 窗口水平向右的偏移量
			});
			menu.setStyle({
				mask: 'none'
			});
			showMenu = true;
		});
	}
}
```
***

## **代码解释**

 - 监听遮罩点击事件

```javascript
main.addEventListener('maskClick', closeMenu);
```
 - 菜单窗口偏移

```javascript
left: 0,        // 菜单窗口水平向右的偏移量
width: '70%'    // 菜单窗口宽度
```
 - 打开侧滑菜单

```javascript
// 菜单页增加一个透明遮罩
menu.setStyle({
    mask: 'rgba(0,0,0,0)'
});

// 菜单显示，则主界面要向右偏移，并且增加一层遮罩
main.setStyle({
    mask: 'rgba(0,0,0,0.4)',    // 显示遮罩
    left: '70%'                 // 窗口水平向右的偏移量
});

// 移除菜单页上遮罩，否则菜单页不能被操作
 menu.setStyle({
    mask: 'none'
});
```