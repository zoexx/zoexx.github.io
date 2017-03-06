title: 前端经验价值几何
date: 2017-03-06 18:25:18
tags: javascript
---

# 从写一个方法去反思

今日看到一篇文章[《为何你的前端经验不值钱》](http://mp.weixin.qq.com/s/lRflZqb8qBIjPYlcTXRWjw)，自测发现自己仅做到了“可用”，欠缺对于“健壮，可靠，宽容，精益求精”的思考。在实际工作中极易给自己或者后人留坑，用一篇日志提醒一下自己。

```
/**
 * **************************************************** 
 * 根据所给正整数n求一个由n个[2,32]不重复整数构成的整数数组 
 * ****************************************************
 **/


function getRandomArr( n ){

    if ( !n ){ return [] }
    n = parseInt(n);
    if ( !n || isNaN(n) ){ return [] }
    if ( n > 31 || n < 0 ){
        console.warn('the argument is out of range');
        return [];
    }

    const Max = 32 , Min = 2 ;

    let count = 0 ,
        resultMap = {} ;

    while( count < n ){
        let r = Math.floor(Math.random() * (Max - Min)) + Min ;
        if ( !resultMap[r] ){
            resultMap[r] = true ;
            count ++ ;
        } 
    }

    return Object.keys( resultMap ).map( nub => parseInt(nub) );
}
```