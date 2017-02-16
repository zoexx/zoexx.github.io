title: javascriptCore
date: 2016-01-01 18:31:20
tags: javascript
---
### 浏览器的多条进程（Webkit和Gecko为例）：

javascript引擎线程
界面渲染线程
浏览器事件触发线程
Http请求线程

### js引擎

> JavaScriptCore 执行 一系列步骤 来解释和优化脚本：
> 它进行词法分析，就是将源代码分解成一系列具有明确含义的符号或字符串。
> 然后用语法分析器分析这些符号，将其构建成语法树。
> 接着四个 JIT（Just-In-Time）进程开始参与进来，分析和执行解析器所生成的字节码。

> Google 的 V8 引擎 是用 C++ 编写的，它也能够编译并执行 JavaScript 
> 源代码、处理内存分配和垃圾回收。它被设计成由两个编译器组成，可以把源码直接编译成机器码：
> Full-codegen：输出未优化代码的快速编译器
> Crankshaft: 输出执行效率高、优化过的代码的慢速编译器
> 如果 Crankshaft 确定需要优化的代码是由 Full-codegen 生成的未优化代码，它就会取代 Full-codegen，这个过程叫做“crankshafting”。


[V8为什么这么快](http://blog.jobbole.com/19310/)
[javascriptCore参考文章-实现](http://blog.csdn.net/horkychen/article/details/7888647)
[SpiderMonkey参考文章](http://blog.csdn.net/horkychen/article/details/7888647)


[书签－js探秘](http://www.nowamagic.net/librarys/veda/detail/1579)

ES6

[阮一峰对ES6的介绍](http://www.nodeclass.com/api/ECMAScript6.html#intro)

