title: phantomjs nodejs 爬数据
date: 2017-05-23 14:38:50
tags: 
---

> 运营童鞋想要对知乎的相关数据做分析

- 抓取第一波数据花费 3 小时左右
- 优化数据&改良代码  2 小时左右
- 抓取第二波数据     10 分钟

需要抓取数据的页面有两个

1. 搜索**结果页面**（问题列表以及该问题的高票回答列表）
1. 1中可得到的**问题页面**内**关注量**以及**浏览量**

界面1 知乎中加载更多用的是服务端渲染，直接返回html字符串，要解析这部分数据呢需要结合界面中的标签，用正则去提取，想到这里，反正界面已经在眼前何不直接操作dom取数据呢？

```javascript
document.querySelector();
```

于是在这一步中只需要会打开控制台，会找dom class的关系，可轻易得到问题的列表 

```javascript
[{
	"title": "牙齿矫正对个人外貌变化有多大影响？",
	"topicUrl": "https://www.zhihu.com/question/49519516",
	"commentAmount": "483 条评论",
	"vote": "1223",
	"author": "Vivian",
	"authorHome": "https://www.zhihu.com/people/vivian-74-91",
	"badge": null,
	"badgeSummary": null,
	"bio": "一无所是的空想者",
	"answerDigest": "用本人的真实照片来回答这个提问照片记录了整牙前后的完整变化,包括牙齿和脸型,29岁才开始矫正,历时一年半,期间经历了戴着牙套拍婚纱照,举办婚礼,旅行…... 什么感觉?要上天啊哈哈哈哈哈~所以,可能angelababy 真的只是做了牙齿矫正而已!一直觉得自己牙齿不…显示全部",
	"answerUrl": "https://www.zhihu.com/question/49519516/answer/137252689",
	"publishTime": "2016-12-23",
	"updateTime": "2017-01-02",
},
...others
]
```

之前看过许多phantomjs的应用文章，依靠构建一个浏览器运行环境模拟用户行为，可以做自动化测试，ui截图，爬虫等。

界面2 是根据[topicUrl](https://www.zhihu.com/question/49519516)在问题详情页抓取**关注者（人数）**，**被浏览（次数）**以及问题所属的tag，这一步就需要phantomjs来跑循环了。


```sh  
phantomjs fetchAnswerjs 0 150 
```
phantomjs fetchAnswerjs 0( arg[1] start_index ) 150( arg[2] times )

以下是简要流程

```javascript
// page用来打开指定url并执行数据抓取
var page = require('webpage').create(), 
// 读取上一步得到的列表.json数据 根据topicUrl抓取页面
    fs = require("fs"),
// system.args [array] 取terninal传递的参数 
// 使用了两个参数（ start_index , times ）来控制整个循环  
    system = require('system');

    // function startFetch( url )
	page.open(  url , function (status) {
	    if (status !== 'success') {
	        console.log('FAIL to load the address');
	    }else{    	
	    	
		    var data = page.evaluate(function () {
                // document.querySelector( everything );
                return { ...data }
		    });
            fullData[index] = { ...fullData[index] , data }
            fs.write( filePathName , JSON.stringify(fullData) );
			afterFetch();
			return ;
	    }

	})
    function afterFetch(){
        // compare current_index with start_index and times
        // try fullData[current_index]
        start( fullData[current_index + 1] ) or phantom.exit();
    }

// phantom 错误处理 常遇到界面404的情况 可以根据报错情况跳过
phantom.onError = function(msg, trace) {
    var msgStack = ['PHANTOM ERROR: ' + msg];
    if (trace && trace.length) {
        msgStack.push('TRACE:');
        trace.forEach(function(t) {
            msgStack.push(' -> ' + (t.file || t.sourceURL) + ': ' + t.line + (t.function ? ' (in function ' + t.function +')' : ''));
        });
    }
    console.error(msgStack.join('\n'));
    phantom.exit(1);
};
```

![运行时的截图](http://7xndda.com1.z0.glb.clouddn.com/zhihu.gif)

要的数据都已经抓好啦，下一步用nodejs改一下标题，转换成csv文件方便运营同学用excel做分析，其实改标题也可以在phantomjs里面完成。

```javascript
// 也是用到 fs
let dataList = require('./afterFormat.json')

let result = dataList.map( item => dealData(item) ) 

fs.writeFile( filetitle , JSON.stringify( result ) , 'utf8' , function(err){
  if (err) throw err;
  console.log('The file has been saved!');
});

```

![导出的文件截图](http://7xndda.com1.z0.glb.clouddn.com/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170523162357.png)

打算丢给运营同学用神箭手自己去玩 : )

