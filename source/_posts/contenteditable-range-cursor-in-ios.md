title: 爬坑：WordPress-Editor-iOS 插入图片光标位置控制
date: 2017-05-26 10:39:03
tags: hybird
---

> WordPress-Editor-iOS 插入图片后光标位置没有移到图片后面咋办？

问题如下（这是wordpress-ios 使用了[WordPress-Editor-iOS](https://github.com/wordpress-mobile/WordPress-Editor-iOS)作为富文本编辑器）：

![WordPress-Editor-iOS 插入图片后光标位置没有移到图片后面咋办](http://cdn.zoeservers.com/blog/contenteditable-range.gif)

分析插入图片的代码（精简版）:
```javascript
    function insertImage( ...args ){

        var sel=window.getSelection()
        var range = sel.getRangeAt(0)
        var img = document.createElement('img')
        img.src='https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/bd_logo1_31bdc765.png'

        // 实际代码在img外面包了一层span来做样式，这里直接用img类测试可以得到一样的效果
        range.insertNode(img)

        ...otherThings
    }

```

上面代码中 insertNode 这个方法是在当前选区的开始处插入 node ， 那么如果我们手动将img加入选区，再将 range 的光标折叠（range.collapse()），使光标移动到最后，应该可以解决问题。

加入log，发现插入img之后，它是包含在选区内的

```javascript

console.log(range.collapsed,range.startOffset,range.endOffset)
range.insertNode(img)
console.log(range.collapsed,range.startOffset,range.endOffset)

```

那么这里直接执行 range.collapse() ， 可以看到在chrome中已经可以达到效果[👉👉👉例子👈👈👈](http://zoeservers.com/example/range.html)

```javascript

range.insertNode(img)
range.collapse()

```

然而，app中仍然不起作用，甚至会在插入第一张图片时失去焦点，在 stackoverflow 中找到新建选区的方式去选中：

```javascript

range.insertNode(img)
range.collapse()

//Create a range (a range is a like the selection but invisible)
var newrange = document.createRange();
//Select the entire contents of the element with the newrange
newrange.setStart(img,0);
//collapse the range to the end point. false means collapse to end rather than the start
newrange.collapse(false);
//get the selection object (allows you to change selection)
var selection = window.getSelection();
//remove any selections already made
selection.removeAllRanges();
selection.addRange(newrange);

```

rang 还有一个 selectNodeContents 方法可以用来选中整个编辑区域，折叠后可以讲光标移动到最后

新建选区后，app中依然无解--光标会在插入图片后，从图片后面跳到图片前面，这时候按一下键盘中的删除，图片神奇的被删掉了 = =|||

如果，这是WKwebview的bug 那我......也只能忍了

尝试在图片后面加一个 br/span 然后选中 br/span 去改变光标位置，测试完败......

那么，假设，这是span这个节点的问题，换做是textNode.....

```javascript

range.deleteContents();

var text = document.createTextNode('描述这张图片~')
range.insertNode(text)
range.insertNode(img)
range.collapse()

//Create a range (a range is a like the selection but invisible)
var newrange = document.createRange();
//Select the entire contents of the element with the newrange
newrange.setStart(img,0);
//collapse the range to the end point. false means collapse to end rather than the start
newrange.collapse(false);
//get the selection object (allows you to change selection)
var selection = window.getSelection();
//remove any selections already made
selection.removeAllRanges();
selection.addRange(newrange);

```

如下图（自家app），只能曲线救国了

![在图片后加入textNode并选中](http://cdn.zoeservers.com/00f93ddce9b2a40ea2e4f2cd5a975a93.gif)



参考：

[MDN > web apis > range](https://developer.mozilla.org/en-US/docs/Web/API/range)
[JS Range HTML文档/文字内容选中、库及应用介绍](http://www.zhangxinxu.com/wordpress/2011/04/js-range-html%E6%96%87%E6%A1%A3%E6%96%87%E5%AD%97%E5%86%85%E5%AE%B9%E9%80%89%E4%B8%AD%E3%80%81%E5%BA%93%E5%8F%8A%E5%BA%94%E7%94%A8%E4%BB%8B%E7%BB%8D/)