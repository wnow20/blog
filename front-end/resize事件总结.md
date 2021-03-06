title: resize事件总结
date: 2014-12-10 16:16:00
tags: [resize, onresize, 事件, 前端, JavaScript]
categories: [前端]
---

> 通过原生JS和jQuery两个方便讲resize事件

_onresize 事件会在窗口或框架（iframe）被调整大小时发生。除了window对象和body元素，[其它元素][]虽然有的支持，但不会主动触发_

## resize的使用方式

1. 直接在元素中定义属性
2. 直接给onresize赋值（强制，入侵式事件注册）
    * 可以给window和document.body的onresize赋值
    * 如window.onresize=function(){}，document.body.onresize=function(){}
    * 但需要注意的是，赋值之后，window.onresize === document.body.onresize 为true
3. 使用事件监听（事件队列，一个元素同一事件可以绑定多次）
    * 只对window有作用
    * 如window.addEventListener("resize",fn);

## 手动触发
* jQuery: $(selector).resize()
* window.onresize();
* dispatchEvent，详情请参考[Creating and triggering events][]

## 插件解决方案 [jQuery-Resize][]
* [jQuery-Resize文档][]
* [jQuery-Resize案例][]

[其它元素]: http://www.w3school.com.cn/jsref/event_onresize.asp
[jQuery-Resize]: https://github.com/cowboy/jquery-resize
[jQuery-Resize文档]: http://benalman.com/code/projects/jquery-resize/docs/
[jQuery-Resize案例]: http://benalman.com/code/projects/jquery-resize/examples/resize/
[Creating and triggering events]: https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events