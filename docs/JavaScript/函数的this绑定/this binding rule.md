---
sidebar_position: 2
---
# this绑定规则



## this指向谁和定义时的位置没关系，只与函数被调用的方式有关系

**以下是四种调用方式**

* 默认绑定
* 隐式绑定
* 显示绑定
* new绑定

**接下来我们来详细介绍这四种绑定方式**



### 默认绑定

什么情况下使用默认绑定呢？

* 独立函数调用

  >独立的函数调用我们可以理解成函数没有被绑定到某个对象上进行调用（也就是挂载到window对象上的）
  >
  >所以this都指向window

默认绑定的总结规则:

**this的指向与定义的位置,还有调用的位置无关,只与调用方式有关**

* 如何理解这句话，我们来看下面的代码

  ```javascript
  function hu(callback) {
      callback();
  }
  function fn() {
      console.log(this);
  }
  hu(fn);
  ```

  ```js title="输出结果"
  Window {0: Window, window: Window, self: Window, document: document, name: '', location: Location, …}
  ```

  * 根据总结规则,来理解这段代码:
    定义时fn函数在hu函数外面,调用时fn函数在hu函数里面,但hu函数调用fn函数的方法还是默认绑定

* 对比上面的代码,来对默认绑定的总结规则有更好的理解:

  ```javascript
  function fn() {
      console.log(this);
  }
  let obj = {action: function(callback) {
      callback();
  }}
  obj.action(fn);
  ```

  ```js title="输出结果"
  Window {0: Window, window: Window, self: Window, document: document, name: '', location: Location, …}
  ```

  虽然fn函数是作为obj.action的实参传进去的,调用的位置在obj对象的属性里的,但是this的指向只与调用方式有关,而obj对象属性对fn的调用方式仍然是默认绑定

<br/>

### 隐式绑定

另外一种比较常见的调用方式是通过某个对象进行调用的

>也就是它的调用位置中，是通过某个对象发起的函数调用

* **那么this指向发起函数调用的对象**

<br/>

### 显示绑定

隐式绑定有一个前提条件:

>必须在调用的对象内部有一个对函数的引用（比如一个属性）

* 如果没有这样的引用，在进行调用时，会报找不到该函数的错误
* 正是通过这个引用，间接的将this绑定到了这个对象上



如果我们不希望在` 对象内部` 包含这个函数的引用，同时又希望在这个对象上进行强制调用，该怎么做呢？

>JavaScript所有的函数都可以使用call和apply方法（这个和Prototype有关）

call和apply的区别，其实非常简单

* 除了第一个参数都要求是一个对象
* 后面的参数是传入函数的实参，apply为数组，call为参数列表

```javascript
function sum(num1,num2) {
    console.log(num1 + num2,this);
}

sum.call(this要绑定的对象,实参1,实参2);
sum.apply(this要绑定的对象,[实参1,实参2])
```



如果我们想多次调用引用固定指向的this指针的函数，如果一直用call或者apply给该函数绑定固定this指针指向的对象，会比较繁琐

>所以引出了bind方法

bind方法会返回一个新的函数，需要新的函数变量名来接收，并且是返回固定了this所绑定的对象，不管这个新函数是不是作为独立函数调用

bind（）方法不仅可以固定this的绑定对象，**还可以固定部分函数的形参**

```javascript
let sum = (x,y) => x + y;
let obj = {name:"kobe",age:38};
let sum2 = sum.bind(obj,1);

把this指向obj对象,同时把形参x固定为1传给sum2。
所以sum2（5）；=>返回6
```



有些时候，我们会调用一些JavaScript的内置函数，或者一些第三方库中的内置函数

```javascript
setTimeout(function() {
    console.log(this);
}, 2000)
```

数组的五个迭代方法

这5个迭代方法都能传入第二个参数,而这第二个参数则是用来绑定this的指向的


[点击这里跳转到五个迭代方法](http://localhost:3000/JavaScript/%E6%B7%B1%E5%85%A5%E5%87%BD%E6%95%B0%E6%89%A7%E8%A1%8C/Introduction%20to%20functions#%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E9%AB%98%E7%BA%A7%E5%87%BD%E6%95%B0%E6%96%B9%E6%B3%95)

<br/>

### new绑定

与面对对象的思想有关

JavaScript中的函数可以当做一个类的构造函数来使用，也就是使用new关键字

重点是构造函数的知识点

* 这些内置函数会要求我们传入另外一个函数



## 绑定的优先级

学习了四条规则，接下来开发中我们只需要去查找函数的调用  应用了哪条规则即可，但是如果一个函数调用位置应用了多条规则，优先级谁更高呢?

* 默认绑定的优先级最低
* 显示绑定优先级高于隐式绑定
  * call和apply的优先级比隐式绑定高很好理解
  * 但是bind是创建一个新的函数,理应来说与旧函数不一起比较了
    我们仍可以用下面代码证明bind优先级高于隐式绑定:
    ```javascript
    function foo() {
    	console.log(this)
    }
    var obj = {
        name: "obj",
        foo:foo.bind("aaa")
    }
    
    obj.foo();
    ```
    ```js title="输出结果"
    String {'aaa'}
* new绑定优先级高于隐式绑定
    ```javascript
    var obj = {
        name: "obj",
        foo: function() {
            console.log(this);
        }
    }
    
    var f = new obj.foo;
    f();
    ```
    ```js title="输出结果"
    foo {}
* new绑定优先级高于bind

    * new关键字不能和apply还有call一起使用，但new可以跟bind比较:
      ```javascript
      function foo(){
          console.log(this);
      }
      var bar = foo.bind('aaa');
      
      var obj = new bar();
      ```
      ```js title="输出结果"
      foo {}