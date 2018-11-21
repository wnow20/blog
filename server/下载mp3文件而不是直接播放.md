title: 下载mp3文件而不是直接播放
date: 2015-01-01 02:30:00
tags: [Java, 文件下载, MP3, Content-Disposition, MIME]
categories: [后端]
---

## 该本文适合人群
Web工程师适合看本文，由于本人是做`Java`的，以下的代码都是基于`Java`的，但是我会讲基本原理

## 需求
在浏览器中，打开一个输出是MP3文件流的`url`，要求是直接下载，但是有个浏览器有自带的播放功能，会出现浏览器直接打开文件。

## 解决办法
### Content-Disposition

rfc2616#section-19.5.1: http://tools.ietf.org/html/rfc2616#section-19.5.1
WIKI: http://en.wikipedia.org/wiki/MIME#Content-Disposition


什么是[Content-Disposition]()?，`Content-disposition` 是 [MIME]() 协议的扩展，MIME 协议指示 MIME 用户代理如何显示附加的文件，`Content-disposition`可以在用户下载时指定文件名字。实际使用中`Content-disposition`虽然只是能指定名字，但是浏览器默认直接下载，而不需要再指定`Content-Type`；但是如果不指定`Content-disposition`，直接设置响应头`Content-Type: application/octet-stream`也可以下载，但是这种下载没有指定文件名，用户下载下来的文件名是请求路径(Request URL)。

### 设置response-header(响应头)

    Content-Disposition: attachment; filename="filename.ext"

### Java代码示例

    // 使客户端直接下载
    // 响应的格式是:
    // Content-Type: application/octet-stream
    response.setContentType("application/octet-stream");

    // 为客户端下载指定默认的下载文件名称
    // 响应的格式是:
    // Content-Disposition: attachment;filename="[文件名]"
    response.setHeader("Content-Disposition",
            "attachment;filename=\"" + f.getName() + "\"");

## 如果请求路径是静态文件
这个时候问题就简单了，我们只要加一个过滤器，对需要请求地址，设置响应头`Content-Type: application/octet-stream`，即可。

## 其他
我在解决这个问题的过程中，发现 [stackoverflow.com](http://stackoverflow.com/) 上面答案很多，但是我使用百度却只搜到两条信息，而且写的都不好，这说明两个问题：1、百度有的时候不好用，2、国内没有这样的环境，即使大家解决问题了，没有注意积累，没有分享出来。

下面是 stackoverflow.com 上面的解答，很不错，分享一下。

http://stackoverflow.com/questions/3401650/stop-mp3-file-from-streaming-in-browsers/3401658#3401658

[Content-Disposition]: http://tools.ietf.org/html/rfc2616#section-19.5.1
[MIME]: http://www.w3school.com.cn/media/media_mimeref.asp
