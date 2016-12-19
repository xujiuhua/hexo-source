---
title: HHTML5之Audio and Video
date: 2016-05-05 17:00:02
categories: H5
tags: 
  - JavaScript
  - H5
  - audio
  - video
---


　　本人做一个项目需要使用音乐播放。然后涉及到音频自动播放，想想H5已经实现多媒体audio和video,所以直接采用了audio元素。


[TOC]

# **pc**
PC端比较容易，直接调用`play`方法，如下代码，页面加载完成后，直接调用方法即可，此方法在pc端和QQ浏览器完美运行。
但是移动端浏览器问题就出现了，不信可以试试。

```javascript
html:
<audio id="audio" src="source/oneminute.mp3" preload="auto">
</audio>

js:
window.onload = function() {
    play();
}
function play() {
    var audio = document.getElementById("audio");
    audio.play();
}
```

<!--more-->

# **移动端**
移动端一个audio对象的第一次播放，必须是一个用户触发的行为。
如果你不触摸或者点击屏幕，想用方法来启动第一次播放是无法实现的。

解决办法：
对于同一个audio对象，只在其`初次播放`时需要用户行为触发，之后就可以操作了，比如touchstart则我们全局使用同一个audio对象，用户触发播放时候使用空的播放，之后替换src。

```javascript
html:
<audio id="audio" src=""></audio>
<audio id="audio2" src=""></audio>
<button id="button" style="width: 100px;height: 100px;">播放</button>

js:
// 用户点击启动播放控件
function play() {
    // 结果控件
    var audio = document.getElementById("audio");
    audio.src = "source/oneminute.mp3";
    audio.play();
    
    setTimeout(function() {
        
    // 这里播放在移动端Fail,因为audio2没有通过点击方法来         
    // 触发，而是通过一个延迟方法间接执行。
    // var audio2 = document.getElementById("audio2");
    // audio2.src = "source/win.mp3";
    // audio2.play();
        
    // 这里播放在移动端Success，因为play方法直接启动控件，         
    // 这里只是替换src
       audio.src = "source/win.mp3";
       audio.play();
    }, 3000);
}
            
document.getElementById("button").onclick = function() {
    play();
}

```

> 切记：移动端一个audio对象的第一次播放，必须是一个用户触发的行为

