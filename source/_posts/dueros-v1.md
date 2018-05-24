title: dueros_v1
date: 2017-12-05 10:54:30
tags: linux dueros
---


#### 这是一篇 2017-12-05 在dueros开发论坛记录的帖子 搬运过来

⭐⭐⭐⭐ 等了好久等了好久的 开 箱 ⭐⭐⭐⭐

![1512462305290081022.jpg](http://boscdn.bpc.baidu.com/v1/developer/1512462305290081022.jpg)

![1512462319791021976.jpg](http://boscdn.bpc.baidu.com/v1/developer/1512462319791021976.jpg)


立即下单的树莓派跟tf卡也在第二天收到~~~~

![1512462354443027445.jpg](http://boscdn.bpc.baidu.com/v1/developer/1512462354443027445.jpg)

刷镜像这一步就不啰嗦了，请参考[官方说明](https://developer.dueros.baidu.com/doc/device-devkit/intro_markdown)。

刷好镜像之后改wifi配置（此时依然是用电脑打开tf卡的目录）：

1. 在tf卡根目录新建一个ssh文件
1. 新建一个wpa_supplicant.conf文件，并加入wifi配置

```bash
touch ssh
touch wpa_supplicant.conf
vim wpa_supplicant.conf
```
      以下是文件内容：

```text
country=GB
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="wifi ssid"
    psk="password"
}
```


即可。

装卡，通电，找ip。

ip可以登录路由器的管理页面去找，

如果，有人跟我一样，

没有管理帐号，那么，可以用ipscan来找一找。

献上软件

链接: [https://pan.baidu.com/s/1i51UCN7](https://pan.baidu.com/s/1i51UCN7) 密码: kr4n

扫描一下局域网内的ip，找找有包含raspberry,linux之类字样的，

如果，有人跟我一样，

没有找到任何特征。

![1512462040931003322.png](http://boscdn.bpc.baidu.com/v1/developer/1512462040931003322.png)

那么就用排除法吧！

筛选之后，我这边剩余19个没有任何特征的ip，一个一个ssh过去，(┬＿┬)

![1512462057540082439.png](http://boscdn.bpc.baidu.com/v1/developer/1512462057540082439.png)

好歹是找到了。

用top观察一下下，dueros跑的很安逸。

![1512462067106027182.png](http://boscdn.bpc.baidu.com/v1/developer/1512462067106027182.png)

中午吃完午饭回来，摸摸pi的cpu，嗯，还是挺温暖的。

拔电源，连接上板子，给电，小度温柔的声音出来啦



“小度正在启动请稍候...”

“启动成功~”

“小度小度~”

“...”

“小度小度 ?”

“.....”

“小度小度 !”

“.....”



最怕空气突然安静.....

查看一下日志

```bash
tail -f /duer/duer_linux.log
```


果然是报错了......

![1512462125923052655.png](http://boscdn.bpc.baidu.com/v1/developer/1512462125923052655.png)

```text
Can't open "plughw:1,0" PCM device. No such file or directory
```

看上去好像是找不到什么设备，怀疑每一根链接线

立即在群里询问

经群里的小伙伴提示，换了根micro usb线，即可正常运行。

（之前用的是一根给设备充电的短线）

这个PCM device是什么意思呢，PCM是pulse code modulation，中文译名是脉冲编码调制。在这里我理解为录音和播放音频的设备（模拟信号和数字信号之间的转换），如有错误大家指出。

这里有一篇文章介绍[PCM是什么](http://blog.csdn.net/junglyfine/article/details/8665589)


![1512462153975069787.png](http://boscdn.bpc.baidu.com/v1/developer/1512462153975069787.png)

![1512462181885004181.png](http://boscdn.bpc.baidu.com/v1/developer/1512462181885004181.png)


((유∀유|||)) 好像我并没有呵呵呵的傻笑昂

这里也可以看到语音识别的返回信息（摊手）：


```text
[App-INFO ]-[2017-12-05 15:17:32.161]-----Duer server data:{
   "client_msg_id" : "aee7c92d-0b63-4283-b80f-940e6beec458",
   "cuid" : "7b4ac26c65ac73db4f30f334ac80b013",
   "id" : "1512458252_136cskpp6",
   "idx" : 4,
   "logid" : "15124582511915",
   "msg" : "ok",
   "result" : {
      "bot_id" : "talk_service",
      "bot_meta" : {
         "description" : "desc",
         "type" : "其他",
         "version" : "1.0.0"
      },
      "directives" : [
         {
            "header" : {
               "dialogRequestId" : "aee7c92d-0b63-4283-b80f-940e6beec458",
               "name" : "Speak",
               "namespace" : "SpeechSynthesizer"
            },
            "payload" : {
               "content" : [ "呵呵呵的傻笑。呵呵呵的傻笑" ],
               "speak_behavior" : "REPLACE_ALL",
               "token" : "1512458252_123fr0kcl",
               "type" : "Text"
            }
         }
      ],
      "display" : {
         "id" : "af9b0b03baa4665c7bb8002327edcf65",
         "url" : "http://xiaodu.baidu.com/saiya/dcsview?id=af9b0b03baa4665c7bb8002327edcf65&appid=dmB3C032D9FD3E4899";
      },
      "nlu" : {
         "domain" : "unknown",
         "intent" : "unknown",
         "slots" : {}
      },
      "speech" : {
         "content" : "呵呵呵的傻笑。呵呵呵的傻笑",
         "type" : "Text"
      },
      "views" : [
         {
            "content" : "呵呵呵的傻笑",
            "type" : "txt"
         },
         {
            "list" : [
               {
                  "src" : "https://ss3.baidu.com/-fo3dSag_xI4khGko9WTAnF6hhy/xiaodu/pic/item/d1160924ab18972be2049786eecd7b899e510abb.jpg";
               }
            ],
            "type" : "image"
         }
      ]
   },
   "se_query" : "呵呵呵呵呵呵呵呵呵呵呵呵",
   "session_id" : "86dc7a52-cb5d-4448-b9b4-f7610fd278bc",
   "speech_id" : "6495958712883389696",
   "status" : 0,
   "time" : 1512458252,
   "timeuse" : 748,
   "user_id" : "7b4ac26c65ac73db4f30f334ac80b013"
}
```

okay，下一步是用pythonSDK修改唤词。



Ps:

看过《her》之后一直对Samantha恋恋不忘呢。
