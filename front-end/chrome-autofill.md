title: 去掉chrome记住密码后自动填充表单的黄色背景
auther: wnow20
date: 2015-1-14 10:53:00
tags: [chrome, webkit, 自动填充, html, css]
categories: [前端]

---
##写本文起因
chrome浏览器自动填充表单的黄色背景高亮（`#FAFFBD`）一直困扰着我，我之前没想着理它，可是最近一个登陆框，需要用到图标，于是我草率的直接设置在`input`元素里面，结果问题就来了，很难看很难看，因此还是总结一下。

这个问题，在2008年的时候就已经存在了，隔了好几年了，在chromium上面可以找到 [Issue 46543](https://code.google.com/p/chromium/issues/detail?id=46543)，但是官方好像没有理这个问题，英文没怎么看懂，谁理解的，可以给大家分享一下。

## 思路一： 打补丁

Webkit内核的浏览器有一个`-webkit-autofill`私有属性，

通过审查元素可以看到这是由于chrome会默认给自动填充的input表单加上input:-webkit-autofill私有属性，然后对其赋予以下样式：
```css
input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill {
  background-color: rgb(250, 255, 189); /* #FAFFBD; */
  background-image: none;
  color: rgb(0, 0, 0);
}
```

因此我们就会想到覆盖这个样式，代码如下，但是依然不能覆盖原有的背景、字体颜色。需要注意的是：加 `!important` 依然不能覆盖原有的背景、字体颜色，除了chrome默认定义的`background-color`，`background-image`，`color`不能用 `!important` 提升其优先级以外，其他的属性均可使用`!important`提升其优先级。

```css
input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill {
  background-color: #FFFFFF;
  background-image: none;
  color: #333;
  /* -webkit-text-fill-color: red; //这个私有属性是有效的 */
}
input:-webkit-autofill:hover {
  /* style code */
}
input:-webkit-autofill:focus {
  /* style code */
}
```

### 情景一：input文本框是纯色背景的
解决办法：
```css
input:-webkit-autofill {
  -webkit-box-shadow: 0 0 0px 1000px white inset;
  -webkit-text-fill-color: #333;
}
```

### 情景二：input文本框是使用图片背景的
解决办法：
```js
if (navigator.userAgent.toLowerCase().indexOf("chrome") >= 0)
{
  var _interval = window.setInterval(function () {
    var autofills = $('input:-webkit-autofill');
    if (autofills.length > 0) {
      window.clearInterval(_interval); // 停止轮询
      autofills.each(function() {
        var clone = $(this).clone(true, true);
        $(this).after(clone).remove();
      });
    }
  }, 20);
}
```

下面的js不是太靠谱
```js
if (navigator.userAgent.toLowerCase().indexOf("chrome") >= 0) {
  $(window).load(function(){
    $('input:-webkit-autofill').each(function(){
      var clone = $(this).clone(true, true);
      $(this).after(clone).remove();
    });
  });
}
```

## 思路二： 关闭浏览器自带填充表单功能
设置表单属性 `autocomplete="off/on"` 关闭自动填充表单，自己实现记住密码
```html
<!-- 对整个表单设置 -->
<form autocomplete="off" method=".." action="..">
<!-- 或对单一元素设置 -->
<input type="text" name="textboxname" autocomplete="off">
```
网上大部分文章是使用Cookie实现记住用户名、密码。不管是在前端，还是后端都可以实现。本文不对Cookie存储展开讨论。可自行[谷歌](http://www.google.com/ncr)

## 其他
stackoverflow.com上面也有类似的回答
[Google Chrome form autofill and its yellow background](http://stackoverflow.com/questions/2920306/google-chrome-form-autofill-and-its-yellow-background)
[Override browser form-filling and input highlighting with HTML/CSS](http://stackoverflow.com/questions/2338102/override-browser-form-filling-and-input-highlighting-with-html-css)
