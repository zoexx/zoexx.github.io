title: 在发布之前的检查清单
date: 2018-11-27 14:06:43
tags:
---

> 每一个前端项目发布前的检查清单

1. fix lint 保持代码合乎 **规范**
2. run build & Analyzer 检查代码中的依赖项，尽可能的 **按需引入**
3. 静态资源开启gzip
4. 视情况 引入前端错误&性能监控
5. SEO优化项：
    - 使用 a tag (router-link in vue) 代替 js写的跳转
    - 合理使用html标签，不要全是div
    - 合理设置index.html 中 keywords, description 的内容

6. 检查meta标签
```html
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />

    <!-- 👇针对内核浏览器优先使用“急速”模式 文档-> http://se.360.cn/v6/help/meta.html -->
    <meta name="renderer" content="webkit|ie-comp|ie-stand">

    <!-- 👇禁止缩放 wkwebview中会失效 -->
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0;" name="viewport" />

    <!-- 👇禁止百度转码 -->
    <meta http-equiv="Cache-Control" content="no-siteapp" />
```
7. 升级浏览器的代码

```html
<noscript>
    <strong>We're sorry but our site doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
</noscript>
<!--[if lt IE 9]><div class="alert alert-danger topframe" role="alert">你的浏览器实在<strong>太太太太太太旧了</strong>，放学别走，升级完浏览器再说 <a target="_blank" class="alert-link" href="http://browsehappy.com">立即升级</a></div><![endif]-->
```

8. 加入webpack打包时间方便排除缓存错误

```html
console.log('update date => ${new Date()}');
```

9. 修改favicon.ico

10. 本地起一个静态服，使用chrome edge ie11，测试打好包的代码， 如果有单元测试可以跑一下

11. 在readme中补充发布，变更记录