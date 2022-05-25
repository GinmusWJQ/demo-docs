---
sidebar_position: 4
---

# ES6箭头函数

> 箭头函数的标准编写格式:let/const/var  函数名 = (实参...) => {函数执行体}

```javascript
//用let定义
let ginmus = (args) => {
    console.log(args)
}
//const 和 var同理
```

## 箭头函数的作用

简单来说就是优化我们的代码,让代码更加简洁,结构更加清晰



## 箭头函数编写格式的优化

* 优化一: 如果只有一个实参参数,那么（）可以省略掉
  ```javascript
  let ginmus = arg => {
      console.log(args)
  }
  ```
  
* 优化二: 如果函数执行体中只有一行代码, 那么可以省略大括号
  ```javascript
  let ginmus = arg => console.log(args) 
  ```
  * 并且这行代码的返回值会作为整个函数的返回值,同时如果那一行代码是return一个值,那么return也可以省略掉
  
* 优化三: 如果函数执行体只有返回一个对象, 那么需要给这个对象加上（）
  ```javascript
  //这是里都是错误的写法
  let ginmus = arg => {
      return {name: "ginmus"}
  }
  //根据上一个优化规则:优化二
  //如果return省略掉,代码就会长成这样
  let ginmus = arg => {name: "ginmus"}
  /*之所以要加括号，是因为对象要用{ }括起来，而函数执行体也要用{ }括起来，如果省略掉函数执行体外的{ }，但返回的一个对象用{ }括起来，那么则会造成歧义，编译器不知道{ }里的到底是函数执行体，还是一个对象了*/
  ```
  ```javascript
  //这是是正确的写法
  let ginmus = arg => {
      return ({name: "ginmus"});
  }
  /*（）表示括号里的东西是一个整体，（{name：“ginmus”}）当用（）括住{ }，编译器就会知道，原来{ }里的东西是一个对象*/
  ```



## 箭头函数还有一个很重要的知识点

就是箭头函数的this规则,我们将在`函数的this绑定`中仔细讲解
