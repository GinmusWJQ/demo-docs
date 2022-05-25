---
sidebar_position: 1
---

# JavaScript编译和执行

假如我们有下面一段代码，它在JavaScript中是**如何**被执行的呢？

```javascript
var name = 'ginmus';
function foo() {
    var name = 'foo';
    console.log(name);
}

var num1 = 20;
var num2 = 30;
var result = num1 + num2;

console.log(result);

foo();
```

分为以下*两个步骤:*

1. 初始化全局对象
2. 执行上下文(调用栈)



## 初始化全局对象

js引擎会在执行代码之前，会在堆内存中创建一个全局对象：**Global Object（GO）**

* 该对象**所有**的作用域（scope）都可以访问
* 里面会包含`Date`、`Array`、`String`、`Number`、`setTimeout`、`setInterval`等等
* 其中还有一个window属性指向自己



##执行上下文(调用栈)

<span>js引擎内部有一个执行上下文栈（Execution Context Stack，**简称ECS**），它是用于执行代码的调用栈。同样也称为EC Stack</span>

* EC Stack调用栈里存放的执行上下文
  1. Global EC
  2. Function EC
  3. Eval EC
* 同时还会生成变量对象
  * VO全局变量对象`存在与EC Stack调用栈中`
  * AO(Active Object)私有变量对象
  * GO(Global Object)全局对象

<span>那么这个调用栈要执行谁呢？  `=>`   执行的是全局的代码块</span>

* 全局的代码块为了执行会构建一个 Global Execution Context（GEC）

* GEC会 被放入到ECS中 执行。	***GEC被放入到ECS中里面包含两部分内容：***

  * **第一部分：**在代码执行前，在parser转成AST的过程中，会将全局定义的变量、函数等加入到VO(GO)对象中，但是并不会赋值

    * 这个过程也称之为变量的作用域提升（hoisting）

    * 在函数加入到VO(GO)对象中去的时候,GO中的函数变量名赋值了函数对象的内存地址,而函数对象中也存了父级作用域(parentScope)的地址,也就是指向GO

      > [跳转到更多的作用域链的相关知识](#作用域的嵌套)

  * **第二部分：**在代码执行中，对变量赋值，或者执行其他的函数

<span>但遇到函数该怎么执行呢,?</span>

* 在执行的过程中执行到一个函数时，就会根据函数体创建一个函数执行上下文 Functional Execution Context  简称(FEC）
* 并且压入到EC Stack中

  - FEC被放入到ECS中里面包含**三部分内容：**

    - **第一部分：**在解析函数成为AST树结构时，会创建一个Activation Object（AO）：
      - AO中包含`形参`、`arguments`、`函数定义`、`指向函数对象`、`定义的变量`
      - AO包含在指向函数对象的变量的同时该函数对象也存着父级作用域(parentScope的地址,也指向回了AO

    - **第二部分：**作用域链：由VO（在函数中就是AO对象）和父级VO组成，查找时会一层层查找
    - **第三部分：**this绑定的值：这个我们后续会详细解析



## 总结

> 总结一下上述过程,让编译过程更加清晰直白。

我们以时间线来串联起上述内容,现初步分为

`开始编译时`、`编译结束,开始执行代码`、`代码开始执行后遇到函数被调用,但在执行函数之前`、`开始执行函数内代码`

#####开始编译时

* 第一步先创建GO对象
* 第二步:构建一个全局执行上下文(GEC),然后压入EC Stack
* 第三步:并且使EC Stack中的VO指向堆内存中的GO,也就是这里,开始作用域提升,
* 第四步:并且在堆内存中创建函数对象,GO对象里的函数变量存着函数对象的地址,并且函数对象的地址也存着父级作用域(parentScope)的地址,也就是指向GO

##### 编译结束,开始执行代码

* 逐行执行函数内代码,给AO中没有赋值的对象赋值

##### 代码开始执行后遇到函数被调用,但在执行函数之前,

* 第一步:构建函数执行上下文(FEC)然后压入EC Stack
* 第二步:构建AO对象,并使VO指向AO

##### 开始执行函数内代码

* 逐行执行代码,给GO中没有赋值的对象赋值
  	
