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
