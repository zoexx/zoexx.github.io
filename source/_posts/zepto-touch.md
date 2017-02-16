title: 移动开发遇到的一些问题和解决方法
date: 2015-07-25 23:16:13
tags: mobile
---

### 为实现滑动翻页，使用zepto的touch事件需要手动将touch.js中的代码引入，并阻止touchmove事件的默认行为;

```

Zepto(function($){
	document.addEventListener('touchmove', function (event) {
		event.preventDefault();
	}, false);
	Zepto(document.body).swipeUp(function(e){
		console.debug(e.currentTarget);
	    nextPage(e.currentTarget);
	  });
	Zepto(document.body).swipeDown(function(e){
		prePage(e.currentTarget);
	});
});

```
### tap点透

换click，click慢300ms，用fastclick。

### 跟随scrollTop变化的导航栏

要结合touchmove touchstart touchend事件来响应，以避免导航变化延迟的问题


### 滑动弹出层内容

卡顿，调用原生滚动，回弹效果 -webkit-overflow-scrolling : touch;
事件冒泡，body内容也跟着滚动，阻止事件冒泡

```
var ScrollFix = function(elem) {
    var startY = startTopScroll = deltaY = undefined,

    elem = elem || elem.querySelector(elem);
    if(!elem)
        return;
    elem.addEventListener('touchstart', function(event){
        startY = event.touches[0].pageY;
        startTopScroll = elem.scrollTop;

        if(startTopScroll <= 0){
            elem.scrollTop = 1;
            event.stopPropagation();
        }

        if(startTopScroll + elem.offsetHeight >= elem.scrollHeight){
            elem.scrollTop = elem.scrollHeight - elem.offsetHeight - 1;
            event.stopPropagation();
        }
    }, false);

```