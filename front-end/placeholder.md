title: ::placeholder
date: 2015-02-13 19:47:00
tags: [placeholder]
categories: [前端]
---

## 前言
今天在安卓下面又碰到 `::placeholder` 的问题（特别是Samsung手机），在网上看到不少文章，都说不要在`input`元素中设置 `line-height` 或者设置`input`的`line-height`为`normal`就能避免问题。我个人觉得我们没有真正的去理解里面的具体原因。

> 注：使用Samsung i939测试。

## 它应该叫啥？
`placeholder`可以叫它为属性、占位符，`placeholder`属性提供可描述输入字段预期值的提示信息。百度找不到好东西，只能Google了，`::placeholder`属性貌似还没有规范，不知道它的定位，到底是伪元素呢，还是伪类？请看下面`placeholder`目前的状况

## 目前的状况
- Webkit: ::-webkit-input-placeholder 伪元素;
- Trident(IE内核，IE10）: :-ms-input-placeholder 伪类;
- Gecko18-: :-moz-placeholder 伪类;
- Gecko19+: ::-moz-placeholder 伪元素;
- Presto: 不支持.

### 参考
1. [查看兼容情况](http://caniuse.com/#search=placeholder)
2. Gecko内核把`placeholder`从伪类变为伪元素的[原因](http://lists.w3.org/Archives/Public/www-style/2013Jan/0283.html)
3. 查看[__伪元素__与__伪类__的区别](http://segmentfault.com/blog/stephenlee/1190000000484493)

## 支持的样式
`placeholder`作为伪元素时，支持的样式属性有：

* `font`
* `color`
* `background`
* `word-spacing`
* `letter-spacing`
* `text-decoration`
* `vertical-align`
* `text-transform`
* `line-height`
* `text-indent`
* `opacity`

`placeholder`作为伪类时，也支持上面的大部分属性，但是没有`placeholder`伪元素灵活。

## Browser Support
<table class="browser-support-table">
<thead>
<tr>
<th class="chrome"><span>Chrome</span></th>
<th class="safari"><span>Safari</span></th>
<th class="firefox"><span>Firefox</span></th>
<th class="opera"><span>Opera</span></th>
<th class="ie"><span>IE</span></th>
<th class="android"><span>Android</span></th>
<th class="iOS"><span>iOS</span></th>
</tr>
</thead>
<tbody>
<tr>
<td class="yep" data-browser-name="Chrome">4+</td>
<td class="yep" data-browser-name="Safari">5+</td>
<td class="yep" data-browser-name="Firefox">4+</td>
<td class="nope" data-browser-name="Opera">None</td>
<td class="yep" data-browser-name="IE">10+</td>
<td class="yep" data-browser-name="Android">Any</td>
<td class="yep" data-browser-name="iOS">Any</td>
</tr>
</tbody>
</table>

## 其它资源
* [参考文档](http://css-tricks.com/almanac/selectors/p/placeholder/)
* [MDN docs](https://developer.mozilla.org/en-US/docs/Web/CSS/:-moz-placeholder)
* [IE Docs](https://msdn.microsoft.com/en-us/library/ie/hh772745(v=vs.85).aspx)
* [placeholder伪元素背后的原因](http://lists.w3.org/Archives/Public/www-style/2013Jan/0283.html)
* [Treehouse博客上面的placeholder见解](http://blog.teamtreehouse.com/the-css3-placeholder-pseudo-element)
* [Styling Placeholders](https://wiki.csswg.org/ideas/placeholder-styling)

## 实践

### 关于line-height
当`line-height`设置为`normal`，那么`line-height`到底有多少？W3C规范说是要基于元素字体样式，W3C官方推荐
`normal`这个应该在1.1和1.2之间。下面有原文引用具体可以[查看](http://www.w3.org/TR/2011/REC-CSS2-20110607/visudet.html#line-height)

> __normal__
>  Tells user agents to set the used value to a "reasonable" value based on the font of the element. The value has the same meaning as [<number>](http://www.w3.org/TR/2011/REC-CSS2-20110607/syndata.html#value-def-number). We recommend a used value for 'normal' between 1.0 to 1.2. The [computed value](http://www.w3.org/TR/2011/REC-CSS2-20110607/cascade.html#computed-value) is 'normal'.

### 如何解决
上面我们谈了这么多，接下来我们来聊聊怎么来解决实际问题，进过本人测试发现，不设置`height`，调整`line-height`属性一定程度上会改变`input`的值，有些人说不设置`line-height`或设置其为`normal`就行，其实这个方法是玩玩不可取的，至少本人在研究[Bootstrap 3](http://v3.bootcss.com/)发现，Bootstrap框架中都为`input`元素设置了`line-height`。我个人也觉得在开发高可用性组件是，`input`元素还是应该根据不同情况来调整`line-height`属性。比如框架中有输入框都有自己的规格自己的尺寸规范，可以有大中小三种，而这三种规格需要设置不同的`height`、`line-height`、`padding`、`font-size`等。那问题有来了我们应该怎么？

在 Bootstrap 中，全局设置了，以下是代码块。
```css
// Reset fonts for relevant elements
body {
  @line-height-base:        1.428571429
}
input,
button,
select,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}
```
# 从下面的Bootstrap代码片段可以看出，通过相关属性调节，即可达到修复bug的效果
```css
.form-control {
  width: 100%;
  height: 34px;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.42857143;
}
.input-sm,
.form-group-sm .form-control {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 3px;
}
.input-lg,
.form-group-lg .form-control {
  height: 46px;
  padding: 10px 16px;
  font-size: 18px;
  line-height: 1.33;
  border-radius: 6px;
}
```

## 关于作者

> * 如有疑问，欢迎您与我交流，QQ:854530770，喜欢NodeJS的童鞋可以加 329388789 (QQ群)一起学习
> * 作者：林皓轩
> * 网址：[zcoder.cn](http://zcoder.cn)
> * Weibo：[wnow20](http://weibo.com/wnow20)
> * GitHub: [wnow20](https://github.com/wnow20)

原创文章，转载请注明出处，尊重原创，净化网络环境从我做起，谢谢您的配合。顺道给大家推荐一个好用的VPN（[GreenVPN](http://gjsq.me/1472098)）。