title: gulp传参数
date: 2016-01-05 02:13:38
tags: gulp
---
webpack多了之后 如果在开发环境下gulp要watch以及每次自动执行的范围太大，很慢
只想对正在开发的任务进行监听跟构建
这时候呢可以单独写一个任务
但是希望能灵活一些
这里想到传参数给gulp的task


```
    if (gulp.env.choose){
        var map = {
           "map":"map/main",
           "compare":"acitvityCompareGuidance/main",
           "face":"faceScore/main",
           "assistant":"assistant/main",
        }
        var temp = map[gulp.env.choose];
        if (temp){
            console.log('webpack choose :' + temp);
            webpackSrc = mapFiles([temp], 'js');
        }else{
            console.log('webpack choose undefined , please check the map');
        }
    }

```

执行方法
```
 gulp webpack-build --choose map

```
原理

gulp下有个evn 属性可以接受参数,evn 属性值对应一个对象:

```
{ _: [] }

```

其中 _ 属性值是一个空数组,当我们制定gulp 任务时,任务名会默认填充 _ 属性值的数组.

```
gulp kendo

gulp.env: { _: [ 'kendo'] }

gulp kendo xxxx

gulp.env: { _: ['kendo','xxxx'] }

gulp kendo --choose value

gulp.env: { _: ['choose','value'] }

```
