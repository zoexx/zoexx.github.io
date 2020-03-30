title: 身份与隐私 authorize
date: 2020-03-30 17:07:15
tags: ideas
---

[webkit 近日宣布为了保护隐私即将禁止所有第三方网站的cookie](https://webkit.org/blog/10218/full-third-party-cookie-blocking-and-more/)

那么未来使用跨域名服务，CORS + jwt 简单易维护可能成为首选方案。

使用简单请求也可以尽可能的减少preflight option请求
```
GET, HEAD, POST
```
POST 可配合 Content-Type:text/plain, multipart/form-data 和 application/x-www-form-urlencoded 使用。

中心化的账户授权管理也是一个减少业务代码量的方法：

[idea](https://cdn.zoeservers.com/blog/Blank%20Diagram.png)