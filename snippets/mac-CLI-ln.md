title: Mac的ln命令
date: 2016-10-30 12:52:00
tags: [代码片段]
categories: [代码片段]

---

今天在开发Node.js项目的过程中,死活不能创建文件夹(目录)的软连接(Symbolic Link).

不生效的命令如下:

```shell
$ ln -s ./public/ static
```

后来发现, 原来文件夹参数竟然要绝对路径 - - 

即:

```shell
$ ln -s /User/ge/workspace/projectName/public/ static
```
通过以上命令就能在当前目录中创建指定的软连接
