title: 考验基础的面试题
date: 2017-04-12 17:32:31
tags: javascript
---

偶然看到一篇解析面试题目的文章，考验了javascript的函数，变量，属性，原型对象，优先级等要点。

有同行认为这在实战中没有用到，不用理解，甚至认为是脑筋急转弯，我并不认同这样的观点，在实际调试，理解程序的过程中，对于这些写法的误解或是一知半解极易把自己带入坑。


```javascript
function Foo() {
    getName = function () { console.log(1); };
    return this;
}
Foo.getName = function () { console.log(2);};
Foo.prototype.getName = function () { console.log(3);};
var getName = function () { console.log(4);};
function getName() { console.log(5);}
 
//请写出以下输出结果：
Foo.getName();
// 2 访问公有属性
getName();
// 4 window.getName()
Foo().getName();
// 1 window.getName()
getName();
// 1 window.getName()
new Foo.getName();
// 2 访问公有属性
new Foo().getName();
// 1 (new Foo()).getName()
new new Foo().getName();
// 3 new (new Foo()).getName()
```

[原文中有详细解释](http://www.codeceo.com/article/one-javascript-interview.html)