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
