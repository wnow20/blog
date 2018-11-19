title: Table样式设置
date: 2014-12-23 12:01:00
tags: [table, 前端, 标签, 样式设置]
categories: [前端]
---

## table标签的的三个属性
> 我个人在工作中一下三个属性比较重要。

### border 规定表格边框的宽度。
像素为单位，设置table下面所有可显示标签的边框。包括table，th，td，颜色可用样式定义，这里存在一个相关的样式属性

    table {
        border-collapse: collapse/separate;
    }

### cellpadding 规定单元边沿与其内容之间的空白。
像素或百分比为单位，设置每个单元格之间的填充（padding）属性，对应样式其实很简单在样式文件中直接设置相应的padding即可。

### cellspacing 规定单元格之间的空白。
单元格间距(Cell spacing)，对应的样式有

    table {
      border-spacing: 0;
    }




## 关于作者

> * 如有疑问，欢迎您与我交流，QQ:854530770，喜欢NodeJS的童鞋可以加 329388789 (QQ群)一起学习
> * 作者：林皓轩
> * 网址：[zcoder.cn](http://zcoder.cn)
> * Weibo：[wnow20](http://weibo.com/wnow20)
> * GitHub: [wnow20](https://github.com/wnow20)

原创文章，转载请注明出处，尊重原创，净化网络环境从我做起，谢谢您的配合。顺道给大家推荐一个好用的VPN（[GreenVPN](http://gjsq.me/1472098)）。