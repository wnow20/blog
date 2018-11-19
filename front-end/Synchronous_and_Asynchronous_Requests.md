title: Ajax同步和异步请求
date: 2017-01-29 12:30:00
tags: [ajax,同步]
categories: [前端]
---

## Ajax同步和异步请求

### 同步
```JS
var request = new XMLHttpRequest();
request.open('GET', 'http://www.mozilla.org/', false);
request.send(null);
if (request.status === 200) {
  console.log(request.responseText);
}
```

### 异步
```JS
var oReq = new XMLHttpRequest();
oReq.open("GET", "http://www.mozilla.org/", true);
oReq.onreadystatechange = function (oEvent) {
  if (oReq.readyState === 4) {
    if (oReq.status === 200) {
      console.log(oReq.responseText);
    } else {
      console.log("Error", oReq.statusText);
    }
  }
};
oReq.send(null);
```

### 不得不使用同步请求的情况

在少数情况下,只能使用同步模式的XMLHttpRequest请求.比如在 window.onunload和window.onbeforeunload 事件处理函数中.

在页面unload事件处理函数中使用异步的XMLHttpRequest会引发这样的问题:当响应返回之后,页面已经不复存在,所有变量和回调函数也已经销毁.结果只能引起一个错误 ,“函数未定义”.

解决办法是在这里使用同步模式的请求,这样的话,当请求完成之前,页面不会被关闭.

