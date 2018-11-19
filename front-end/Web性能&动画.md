title: Web性能&动画
date: 2016-10-30 12:52:00
tags: [前端, 性能, 动画]
categories: [前端]

---

## 动画
### GPU加速

```less
transform: translate3d(0, 0, 0);
// 或
translateZ(0)
```

CSS3 Perspective 属性

> 注：3D变形会消耗更多的内存与功耗，应确实有性能问题时才去使用它，兼在权衡

计算量大的任务使用Web Worker

## 建议
 + 尽可能少的使用box-shadows与gradients
    box-shadows与gradients往往都是页面的性能杀手，尤其是在一个元素同时都使用了它们，所以拥抱扁平化设计吧。
 + 尽可能的让动画元素不在文档流中，以减少重排
 + 分离读写操作
 
## setTimeout 与 setInterval

### 更新延迟
Timer计算延时的精确度不够。延时的计算依靠的是浏览器的内置时钟，而时钟的精确度又取决于时钟更新的频率(Timer resolution)。IE8及其之前的IE版本更新间隔为15.6毫秒。假设你设定的setTimeout延迟为16.7ms，那么它要更新两个15.6毫秒才会该触发延时。这也意味着无故延迟了 15.6 x 2 - 16.7 = 14.5毫秒。

## requestAnimationFrame
[Window.requestAnimationFrame()](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)

## 工具
 + Chrome开发人员工具 - Timeline
 + Chrome开发人员工具 - Rendering
 + [JavaScript Performance Monitor](https://github.com/mrdoob/stats.js)
 + [CSS Triggers](https://csstriggers.com/)
 + [Fastdom.js](https://github.com/wilsonpage/fastdom)
 + [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)

## 缓存

## CSS样式性能优化
雪碧图

## 书籍
[JavaScript网页动画设计](https://www.amazon.cn/dp/B019WODCYU) - Velocity.js作者 Julian Shapiro
[WebKit技术内幕](https://www.amazon.cn/dp/B00KWGEHH4)
[Web性能实践日志](https://www.amazon.cn/dp/B00K4RUL94)

## 参考资料
 + [高性能 CSS3 动画](https://www.qianduan.net/high-performance-css3-animations/)
 + [Rendering: repaint, reflow/relayout, restyle](http://www.phpied.com/rendering-repaint-reflowrelayout-restyle/)
 + [DOM layout 性能](http://madscript.com/performance/dom-layout-performance/) - 元彦
 + [How (not) to trigger a layout i`n WebKit](http://gent.ilcore.com/2011/03/how-not-to-trigger-layout-in-webkit.html)
 + [Web动画性能指南](http://alexorz.github.io/animation-performance-guide/)
 + [渲染性能](https://developers.google.com/web/fundamentals/performance/rendering/)
 + GPU硬件加速 - [使用渲染层合并属性](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)
 + 浏览器渲染优化 - [Browser Rendering Optimization](https://www.udacity.com/course/browser-rendering-optimization--ud860)
 + [简化绘制的复杂度、减小绘制区域](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)
 + [优化性能](https://developers.google.com/web/fundamentals/performance/?hl=zh-cn)
 + [高性能滚动 scroll 及页面渲染优化](http://web.jobbole.com/86158/)
 + [CSS vs JS动画：谁更快？](http://zencode.in/19.CSS-vs-JS%E5%8A%A8%E7%94%BB%EF%BC%9A%E8%B0%81%E6%9B%B4%E5%BF%AB%EF%BC%9F.html)
 + Timer - [Accuracy of JavaScript Time](http://ejohn.org/blog/accuracy-of-javascript-time/)
 + Timer - [Analyzing Timer Performance](http://ejohn.org/blog/analyzing-timer-performance/)
 + Timer - [How JavaScript Timers Work](http://ejohn.org/blog/how-javascript-timers-work/)
 + [Javascript高性能动画与页面渲染](http://www.infoq.com/cn/articles/javascript-high-performance-animation-and-page-rendering)
 + [动画](https://developers.google.com/web/fundamentals/design-and-ui/animations/?hl=zh-cn)
 + [What forces layout / reflow](https://gist.github.com/paulirish/5d52fb081b3570c81e3a) 同CSS Triggers
 + [PageSpeed Insights规则](https://developers.google.com/speed/docs/insights/rules)
 + Profiles - [Improving Web App Performance With the Chrome DevTools Timeline and Profiles](https://addyosmani.com/blog/performance-optimisation-with-timeline-profiles/)
 + [浏览器的渲染原理简介](http://coolshell.cn/articles/9666.html)
 + [Webkit](https://webkit.org/)
 + [Best Practices](https://developers.google.com/speed/articles/compressing-javascript)
