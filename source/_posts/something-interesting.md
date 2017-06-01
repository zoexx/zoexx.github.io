title: 记录一些有趣的运用
date: 2017-05-26 17:53:03
tags: javascript
---


- 检测资源加载

```javascript
// 桌面端适用于 chrome Firefox IE Opera （Edge和Safari不支持）
// 常见移动端 仅Android Webview, Firefox Mobile (Gecko)25.0+,Chrome for Android支持
// 所有css+js资源
var allResources = Array.from(document.getElementsByTagName('script'))  
    .map(script => script.src)    
    .concat(
        Array.from(document.getElementsByTagName('link'))
            .filter((link) => link.rel === 'stylesheet')
            .map((link) => link.href)
    );
// 加载成功的css+js资源
var loadedResources = performance.getEntriesByType('resource')  
    .filter((res) => {
        var url = res.name;
        var urlWithoutParam = url.split('?')[0];
        return ['script', 'link'].indexOf(res.initiatorType) !== -1 &&
            [/\.css$/, /\.js$/].some((reg)  => reg.test(urlWithoutParam));
    })).map((res) => res.name);
// 加载失败的css+js资源
var failedResources = allResources.filter((url) => loadedResources.indexOf(url) === -1);  
```

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