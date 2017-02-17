title: 用位运算实现多选
date: 2017-02-17 18:52:30
tags: javascript
---

>#### 在定制多选，组合选择的组件时，通常会使用用逗号分隔的字符串保存多个值（valueA,valueB,valueC）,在判断某一个选项的选中状态时，往往需要拆解，循环结果值。

>
直接上代码吧 ↓↓↓↓

```javascript
var options = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'] ,
    result  = 0 ; // 用一个int数值存储多选结果

function selected( value ){
    result += Math.pow( 2 , value );
}

function cancelSelected( value ){
    result -= Math.pow( 2 , value );
}

function getSelectedResult( result ){
    let selected = [] , index = 0 ;
    while( result > 0 ){
       result & 1 ? selected.push( options[index] ) : null ;
       result = result >>> 1 ;
       index ++ ;
    }
    console.log( selected.join(',') );
    return selected ;
}


selected( 0 )  ; // selected A
selected( 10 ) ; // selected K
selected( 11 ) ; // selected L
selected( 12 ) ; // selected M
selected( 20 ) ; // selected U
getSelectedResult( result );

cancelSelected( 0 );// cancel selected A
getSelectedResult( result );
console.log(result);
```

#### 这个方案在 result < Number.MAX_SAFE_INTEGER 内最多支持53个（选中的）多选项