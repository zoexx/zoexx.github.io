title: 记录一些有趣的运用
date: 2017-05-26 17:53:03
tags: javascript
---

- 监听页面可见

```javascript
document.addEventListener('visibilitychange', function () {
    document.title = document.hidden ? '崩溃了，来看一下' : '😈zoeying😈';
});
```

- 字符画 附上一个生成字符画的网站[👉Text Ascii Art Generator](http://patorjk.com/software/taag/)

```javascript
console.log(`
 ▄▄▄▄▄▄   ████▄ ▄███▄          ▄▄▄▄▄   ██   ▀▄    ▄          ▄  █ ▄█ 
▀   ▄▄▀   █   █ █▀   ▀        █     ▀▄ █ █    █  █          █   █ ██ 
 ▄▀▀   ▄▀ █   █ ██▄▄        ▄  ▀▀▀▀▄   █▄▄█    ▀█           ██▀▀█ ██ 
 ▀▀▀▀▀▀   ▀████ █▄   ▄▀      ▀▄▄▄▄▀    █  █    █            █   █ ▐█ 
                ▀███▀                     █  ▄▀                █   ▐ 
                                         █                    ▀      
                                        ▀                            
`);
```