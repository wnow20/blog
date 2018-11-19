title: JavaScript正则表达式通常用法
auther: wnow20
date: 2015-1-13 13:31:00
tags: [代码片段, JavaScript]
categories: [代码片段]

---

```js
//正则表达式
/\bjavascript\b/ig    // 匹配单词"javascript"，"i"忽略大小写，"g"全文搜索，全文匹配
var text = "testing: 1, 2, 3";        // 文本示范
var pattern = /\d+/g;                 // 匹配所有包含一个或多个数字的实例
pattern.test(text);                   // => true: 匹配成功
text.search(pattern);                 // => 9: 首次匹配成功的位置
text.match(pattern);                  // => ["1", "2", "3"]: 所有匹配组成的数组
text.replace(pattern, "#");           // => "testing: #, #, #"
text.split(/\D+/);                    // => ["", "1", "2", "3"]: 用非数字字符街区字符串
```

## 关于作者

> * 如有疑问，欢迎您与我交流，QQ:854530770，喜欢NodeJS的童鞋可以加 329388789 (QQ群)一起学习
> * 作者：林皓轩
> * 网址：[zcoder.cn](http://zcoder.cn)
> * Weibo：[wnow20](http://weibo.com/wnow20)
> * GitHub: [wnow20](https://github.com/wnow20)

原创文章，转载请注明出处，尊重原创，净化网络环境从我做起，谢谢您的配合。顺道给大家推荐一个好用的VPN（[GreenVPN](http://gjsq.me/1472098)）。
