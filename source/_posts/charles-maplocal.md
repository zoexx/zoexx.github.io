title: charles map local 使用简介（mac）
date: 2016-05-12 19:11:49
tags: tools
---

有位射鸡湿朋友要给个已上线的网站改样式文件，想用最简单快速见效的方式，于是写一篇代理本地文件的教程。

#### 软件介绍：

Charles 是一款Mac上的HTTP代理服务器、HTTP监视器、反向代理服务器，今天和大家分享最新的3.11.2版本，完美兼容最新的OS X 10.11系统，Charles可以让开发者监视查看所有连接互联网的HTTP通信，包括请求，响应和HTTP头信息等等，俗称“抓包”工具，对于Web开发人员来说是一款很有价值的辅助工具，具有Firefox插件，非常不错！

[官网地址](https://www.charlesproxy.com/)

#### 1. 设置系统代理

在charles的菜单栏 proxy > Mac OS X proxy 打上勾

（如果没有使用shadow socket 之类的代理 可以跳过）

配合chrome使用时要将 

系统代理地址 改为 127.0.0.1 

端口设置为charles的代理端口

（可在charles的 Proxy > Proxy Settings 中设置端口， 默认是 8888）

系统偏好设置 > 网络 > 高级…(正在使用的网络) > 代理 > Web代理（HTTP）

![set proxy](http://7xndda.com1.z0.glb.clouddn.com/charles_1.png)

如果使用ss需要经常切换代理，可以使用一款chrome的插件 [Proxy SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=zh-CN)

![Proxy SwitchyOmega](http://7xndda.com1.z0.glb.clouddn.com/charles_2.png)

#### 2.Map Local

设置好代理之后，用chrome浏览网站，可以在charles中看到请求：

![request](http://7xndda.com1.z0.glb.clouddn.com/charles_3.png)

![map local](http://7xndda.com1.z0.glb.clouddn.com/charles_4.png)


替换之后可以写一些测试语句看是否生效
接下来 就可以做你爱做的事情啦

[关于charles更详细的中文教程](http://blog.devtang.com/2015/11/14/charles-introduction/)