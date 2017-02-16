title: 不要总加班计划
date: 2016-01-04 02:07:18
tags:
---
加班呢，总的来讲就是节奏没把握好

最容易出现的原因如下：

被打断－这个地方要改一下
急－这个功能今晚要上
偷懒－反正明天才上，明天应该搞得定，今天可以玩点别的
.......

刚好在2016的开端
写一个脚本去检查每天八点半的时候公司电脑上还有没有跑node
如果有，说明我在加班，于是发送一条微博，说一下警示自己的话记录一下

为了实现这一点，申请了一个sae应用
问题还是在于登录授权这个验证手段

[地址](http://overtime.applinzi.com/getRequestToken.php)
捣鼓了一下感觉还行～

win下的定时监测任务

"系统工具"--->"任务计划程序"

overTime.bat

```

@echo off
tasklist|find "node.exe"
if "%errorlevel%"=="0" (
    telnet http://xxxxxx
) else (
    echo.donotrun
)
pause


```