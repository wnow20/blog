title: JavaScript识别浏览器
date: 2014-12-13 18:02:00
tags: [JavaScript, 识别, 浏览器, 兼容, userAgent]
categories: [前端]
---

> 漫谈JavaScript判断浏览器类型及版本

## 判断是否为IE浏览器

在 IE10 以及 IE10 之前前 使用 `!!window.ActiveXObject` 既可以判断，不过现在出了 IE11 版本，测试发现 IE11 中，`!!window.ActiveXObject === true;  //=> false`，不能用了。以前还有个老办法，使用 [User-agent][]( 用户代理字符串) 判断，很不幸的告诉，在 ie11 的 `User-agent` 中已删除兼容（"兼容"）和浏览器 ("MSIE") 令牌。大家以前的代码不能准确判断了，这里查看[关于 IE11 User-agent 字符串更改的微软官方文档][]

## 现成代码
伸手党可以直接复制下面的代码，[原文链接](http://www.cnblogs.com/leadzen/archive/2008/09/06/1285764.html)

    <script>
    var Browser = {};
    var ua = navigator.userAgent.toLowerCase();
    var s;
    (s = ua.match(/rv:([\d.]+)\) like gecko/)) ? Browser.ie = s[1] : // 不一定准确，基本么关系
    (s = ua.match(/msie ([\d.]+)/)) ? Browser.ie = s[1] :
    (s = ua.match(/firefox\/([\d.]+)/)) ? Browser.firefox = s[1] :
    (s = ua.match(/chrome\/([\d.]+)/)) ? Browser.chrome = s[1] :
    (s = ua.match(/opera.([\d.]+)/)) ? Browser.opera = s[1] :
    (s = ua.match(/version\/([\d.]+).*safari/)) ? Browser.safari = s[1] : 0;

    if (Browser.ie) document.write('IE: ' + Browser.ie);
    if (Browser.firefox) document.write('Firefox: ' + Browser.firefox);
    if (Browser.chrome) document.write('Chrome: ' + Browser.chrome);
    if (Browser.opera) document.write('Opera: ' + Browser.opera);
    if (Browser.safari) document.write('Safari: ' + Browser.safari);
    </script>

## 其它辨别方法
* IE支持创建ActiveX控件，因此她有一个其他浏览器没有的东西，就是ActiveXObject函数（IE11中已不支持）。
* Opera提供了专门的浏览器标志，就是window.opera属性。
* Firefox中的DOM元素都有一个getBoxObjectFor函数，用来获取该DOM元素的位置和大小（IE对应的中是getBoundingClientRect函数）。这是Firefox独有的，判断它即可知道是当前浏览器是Firefox。
* Safari浏览器中有一个其他浏览器没有的openDatabase函数，可做为判断Safari的标志。
* Chrome有一个MessageEvent函数，但Firefox也有。不过，好在Chrome并没有Firefox的getBoxObjectFor函数，根据这个条件还是可以准确判断出Chrome浏览器的。

## 识别IE11的做法
在极少数情况下，必须唯一地标识 IE11。 使用 [Trident 令牌](http://msdn.microsoft.com/zh-cn/library/ie/ms537503(v=vs.85).aspx) 来执行此操作。

许多浏览器检测技术在浏览器更新时，可能会导致误报。 （例如，对 `attachEvent` 方法的支持不妨碍对 `addEventListener event` 的支持。） 为获得最佳结果，在你需要时 [检测功能][] 并使用 [渐进增强][] 为不支持所需功能的浏览器或设备提供简化体验。检测功能请看下文

# 如何检测功能而非浏览器
> [原文链接](http://msdn.microsoft.com/zh-cn/library/ie/hh273397(v=vs.85).aspx)

> 传统上来讲，浏览器检测，典型的实施方法是执行单一的比较操作（通常涉及用户代理 (User-Agent) 头信息），然后对该浏览器支持的功能提出多项设计假设。 然而，事实证明功能检测是一种效用较高、所需维护较少的方法。

## 检测 DOM 对象和属性
最常见的功能检测方法是在 DOM 中查找与要使用的功能相关的对象或属性。 例如，以下代码示例显示如何确定浏览器是否支持 `addEventListener `方法，以便定义事件处理程序。

    function registerEvent( sTargetID, sEventName, fnHandler ) 
    {
       var oTarget = document.getElementById( sTargetID );
       if ( oTarget != null ) 
       {
          if ( oTarget.addEventListener ) {   
             oTarget.addEventListener( sEventName, fnToBeRun, false );
          } else {
            var sOnEvent = "on" + sEventName; 
            if ( oTarget.attachEvent ) 
            {
               oTarget.attachEvent( sOnEvent, fnHandler );
            }
          }
       }
    }

若要确定是否有对象、属性、特性或方法能够帮助检测某项功能，请参阅定义要使用的功能的规范。
例如，Internet Explorer 9 支持导航计时规范中的 `performance` 对象。 直到撰写本文之前，本规范定义了 "window.performance" 属性。 因此，可以通过 `window` 对象上是否存在 `performance ` 属性来确定当前 Web 浏览器是否支持导航计时规范，如以下代码示例所示。

    if ( window.performance ) {
        showLoadTimes( window.performance );
    }

## 创建对象来支持功能
某些功能只有在由浏览器呈现的情况下才能在 DOM 中检测出来。 例如，Internet Explorer 9 仅在采用 IE9 标准模式显示网页时才支持 audio 元素。 如果采用 IE5 (Quirks) 模式显示（或通过早期版本的 Internet Explorer 显示）包含 audio 元素的网页，则 audio 元素将作为未知（通用）元素呈现。

    function supportsAudio() 
    {
        var o = document.createElement( 'audio' );
        return ( o.canPlayType ); 
    }

## 搜索 DOM 功能
[hasFeature][] 方法明确支持各个规范或者某一规范所定义的功能集。 
    bResult = document.implementation.hasFeature("org.w3c.svg", "1.0")

这里推荐一篇相关文章：
---------------------------
[如何创建有效的回退策略](http://msdn.microsoft.com/zh-cn/library/ie/hh273396(v=vs.85).aspx)


## 关于作者

> * 如有疑问，欢迎您与我交流，QQ:854530770，喜欢NodeJS的童鞋可以加 329388789 (QQ群)一起学习
> * 作者：林皓轩
> * 网址：[zcoder.cn](http://zcoder.cn)
> * Weibo：[wnow20](http://weibo.com/wnow20)
> * GitHub: [wnow20](https://github.com/wnow20)

原创文章，转载请注明出处，尊重原创，净化网络环境从我做起，谢谢您的配合。顺道给大家推荐一个好用的VPN（[GreenVPN](http://gjsq.me/1472098)）。

本文地址：<http://my.oschina.net/u/225690/blog/355836>

[User-agent]: http://msdn.microsoft.com/zh-cn/library/ie/ms537503(v=vs.85).aspx
[检测功能]: http://msdn.microsoft.com/zh-cn/library/ie/hh273397(v=vs.85).aspx
[渐进增强]: http://en.wikipedia.org/wiki/Progressive_Enhancement
[关于 IE11 User-agent 字符串更改的微软官方文档]: http://msdn.microsoft.com/zh-cn/library/ie/hh869301(v=vs.85).aspx
[hasFeature]: http://msdn.microsoft.com/zh-cn/library/ie/ms536446(v=vs.85).aspx