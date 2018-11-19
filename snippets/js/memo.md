title: JavaScript中常用的方法
auther: wnow20
date: 2015-1-13 14:35:00
tags: [代码片段, JavaScript, 转换]
categories: [代码片段]

---
## 10进制16进制转换
```js
// 16进制转10进制
var decNum = 160;
decNum.toString(16).toUpperCase(); // 'A0'

// 10进制转16进制
parseInt('A0', 16); // 160
```

## 字母数字转换

```js
// 字符 → 数字
'A'.charCodeAt(); // 65
'a'.charCodeAt(); // 97
// 数字 → 字符
String.fromCharCode(65); // 'A'
String.fromCharCode(97); // 'a'
```


## 关于作者

> * 如有疑问，欢迎您与我交流，QQ:854530770，喜欢NodeJS的童鞋可以加 329388789 (QQ群)一起学习
> * 作者：林皓轩
> * 网址：[zcoder.cn](http://zcoder.cn)
> * Weibo：[wnow20](http://weibo.com/wnow20)
> * GitHub: [wnow20](https://github.com/wnow20)

原创文章，转载请注明出处，尊重原创，净化网络环境从我做起，谢谢您的配合。顺道给大家推荐一个好用的VPN（[GreenVPN](http://gjsq.me/1472098)）。
