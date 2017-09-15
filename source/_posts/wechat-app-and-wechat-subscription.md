title: 小程序绑定微信公众号之后的能力
date: 2017-09-15 10:56:15
tags: 微信小程序
---

## 公众号的连接能力 

### [卡券-小程序打通](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1499332673_Unm7V)

1.小程序内领取卡券  
2.小程序内查看卡券  
3.小程序开卡组件  
4.卡券内跳转小程序

### [自定义菜单跳转小程序](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141013)

### [个性化菜单跳转小程序](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1455782296)

### [群发图文中插入小程序](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1481187827_i0l21)

- 小程序卡片跳转小程序，代码示例：
- **小程序卡片的封面图链接，图片必须为1080*864像素**
 
```html
<mp-miniprogram data-miniprogram-appid="wx123123123" data-miniprogram-path="pages/index/index" data-miniprogram-title="小程序示例" data-progarm-imageurl="http://mmbizqbic.cn/demo.jpg"></mp-miniprogram>
```

- 文字跳转小程序，代码示例：

```html
<p><a data-miniprogram-appid="wx123123123" data-miniprogram-path="pages/index" href="">点击文字跳转小程序</a></p>
```

- 图片跳转小程序，代码示例：

```html
<p><a data-miniprogram-appid="wx123123123" data-miniprogram-path="pages/index" href=""><img data-src="http://mmbiz.qpic.cn/mmbiz_jpg/demo/0?wx_fmt=jpg"></a></p>
```

### 可利用 unionId 让关联的公众号下发消息通知 [模版消息跳转小程序](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1433751277)


### [会员卡跳小程序](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1451025283)

## 小程序中相关的场景值

|sence | 说明 |
|--    |--    |
|1020  |公众号 profile 页相关小程序列表
|1035  |公众号自定义菜单
|1043  |公众号模板消息
|1058  |公众号文章
|1067  |公众号文章广告
|1074  |公众号会话下发的小程序消息卡片