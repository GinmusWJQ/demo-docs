---
sidebar_position: 4
---
# 了解组合函数



## 组合函数的概念

组合（Compose）函数是在JavaScript开发过程中一种对函数的使用技巧、模式

* 比如我们现在需要对某一个数据进行函数的调用，执行两个函数fn1和fn2，这两个函数是依次执行的
* 那么如果每次我们都需要进行两个函数的调用，操作上就会显得重复
* 那么是否可以将这两个函数组合起来，自动依次调用呢
* 这个过程就是对函数的组合，我们称之为 组合函数（Compose Function）



## 通用组合函数的实现

```javascript
 function hyCompose(...fns) {
     //判断数组里面是否都是函数,如果不是就报错
  if(fns.some((value) => {
    return typeof value !== "function";
  })){
    throw new TypeError("Expercted arguments are functions")
  }

  function compose(...args) {
    var index = 0;
    var result = fns.length? fns[index](...args): args;
    while( ++index < fns.length) {
      result = fns[index](...result)
    }
    return result;
  }
  return compose;
}

function double(...args) {
    var doubleArags = args.map(function(value){
      return value * 2;
    })
    return doubleArags;
  }
  
  function square(...args) {
    var squareArags = args.map(function(value){
      return value ** 2;
    })
    return squareArags;
  }
  

var newFn = hyCompose(double,square)
console.log(newFn(10,20,30));
```
