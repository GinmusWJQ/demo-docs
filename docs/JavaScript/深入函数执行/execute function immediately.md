---
sidebar_position: 3
---

# 立即执行函数

声明一个函数，并**立即**调用这个函数，此时这个函数就是立即执行函数，简单来说就是定义函数之后立即执行该函数。

## 定义立即执行函数有两种形式

* **普通函数形式**

  1. 第一种写法:

  ```javascript
  (function foo ( ) {console.log("Hello World!")} ( ) )
  ```
  2. 第二种写法:

  ```javascript
  (function foo( ) {console.log("Hello World!")} ) ( )
  ```

* **匿名函数形式**

  1. 第一种写法:

  ```javascript
  (function ( ) {console.log("我是匿名函数。")} ( ) )
  ```
  2. 第二种写法:

  ```javascript
  (function ( ) {console.log("我是匿名函数。")} ) ( )
  ```



我们可以发现无论是普通函数形式和匿名函数形式都有两种写法,而这两种写法没有本质的区别



## 那立即执行函数有什么作用呢?

1. 保证其中定义的变量不会污染全局命名空间
2. 可以让函数定义之后马上调用,使代码更加简单
