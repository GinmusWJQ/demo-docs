---
sidebar_position: 2
---

# 作用域的嵌套

**首先**，理解作用域链,与原型链不同！！

## 先思考一个问题：作用域链是什么?

```
作用域链是:VO + parentScope
```

* 在全局变量中,VO就是GO,而且GO没有parentScope

* 对函数来说,VO是AO,而parentScope是在代码编译的时候确定的
  * **区别于执行时确立** 

* 该如何理解**"区别于执行时确立"** 这句话呢?请看下面的代码:

```javascript
var message = "Hello World";

function foo() {
    console.log(message);
}

function bar() {
    var message = "Hello Bar"
    foo();
}

bar();
//打印出的内容是Hello World还是Hello Bar?
```

答案是:**Hello World**   而不是Hello Bar

原因是:

* 在编译过程中,foo函数的作用域链是VO + parentScope,而就在编译的时刻,foo的父级作用域是GO,而不是bar函数的AO,就算执行的时候,foo函数是由bar函数的调用,foo函数的parentScope也不会是bar函数的AO.

总结:

* 作用域链与调用位置无关,只与定义位置有关

