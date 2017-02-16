title: 域名
date: 2016-01-06 00:47:36
tags:
---

周末心血来潮在万网给自己买了俩域名，zoexx.me zoe.gift

买完域名是需要DNS来解析它的，把它指向一个地址

> DNS（Domain Name System，域名系统），因特网上作为域名和IP地址相互映射的一个分布式数据库，能够使用户更方便的访问互联网，而不用去记住能够被机器直接读取的IP数串。

[DNSPOD](https://www.dnspod.cn)提供即时生效的服务 试用 还挺快

// to do...
解析设置 ： 记录类型，主机记录，解析线路，记录值，MX优先级，TTL

A
CNAME
www

给github page 上的博客绑定域名，只需要在解析设置中添加一条CNAME的记录，然后在repository的根目录下新建一个包含域名的CNAME文件，稍等片刻，在地址栏键入域名，可以跳转到github 如果未设置好CNAME 会显示github找不到repository的页面，如果未设置好解析，会提示找不到服务器。
