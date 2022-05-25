---
sidebar_position: 1
---
# apply、call、bind函数的实现



## 用JS代码自己实现一个类似call函数的方法:

```javascript
//给Function的原型添加一个方法
Function.prototype.hycall = function(thisArags,...arr) {//call方法第一个参数是要给this绑定指向对象,所以定义多一个形参
    //获取需要执行的函数(因为哪个函数调用的这个方法,this就指向谁,然后自定义一个变量来接收它)
    var fn = this;

    thisArags = (thisArags !== null && thisArags !== undefined) ? Object(thisArags) : window;
    thisArags.fn = fn;
    thisArags.fn(...arr);

}

function foo() {
    console.log(this);
}

//系统的函数的call方法
foo.call("ginmus");

//自己实现的函数的hycall方法
foo.hycall("ginmus");
```

```js title="输出结果"
sum函数被执行 100 [String: 'ginmus']
sum函数被执行 100 [String: 'ginmus'] { fn: [Function: sum] }
```

* 总结:其实本质就是把需要执行的函数添加到形参的属性,然后由形参来调用,这样就绑定好一个this指向

## 用JS代码自己实现一个类似apply函数的方法:

```javascript
//给function的原型添加一个方法
Function.prototype.hyapply = function(thisArags,argArary) {
    //首先得获取需要执行的函数
    var fn = this;

    thisArags = (thisArags !== null && thisArags !== undefined) ? Object(thisArags) : window;
    thisArags.fn = fn;
    thisArags.fn(...argArary);

}

function sum(num1,num2) {
    console.log("sum函数被执行",num1 + num2,this)
}
//系统的函数的apply方法
sum.apply("ginmus",[10,20]);

//自己实现的函数的hyapply方法
sum.hyapply("ginmus",[10,20]);
```

```js title="输出结果"
sum函数被执行 100 [String: 'ginmus']
sum函数被执行 100 [String: 'ginmus'] { fn: [Function: sum] }
```

* 总结:和call方法的实现差不多,只是形参的类型变了

## 用JS代码自己实现一个类似bind函数的方法:

```javascript
//给Function的原型添加一个方法
Function.prototype.hybind = function(thisArags,...argArary) {
    //获取需要执行的函数(因为哪个函数调用的这个方法,this就指向谁,然后自定义一个变量来接收它)
    var fn = this;

    thisArags = (thisArags !== null && thisArags !== undefined) ? Object(thisArags) : window;
    function proxyFn(...args) {
        thisArags.fn = fn;
        //这一步很细节
        var finalArgs = [...argArary,...args]
        thisArags.fn(...finalArgs)
        
    }

    return proxyFn;

}

function sum(num1,num2,num3,num4) {
    console.log("sum函数被执行",num1 + num2 + num3 + num4,this)
}
//系统的函数的bind方法
let result = sum.bind("ginmus",10,20);
result(30,40);

//自己实现的函数的hybind方法
let result2 = sum.hybind("ginmus",10,20);
result2(30,40)
```

```js title="输出结果"
sum函数被执行 100 [String: 'ginmus']
sum函数被执行 100 [String: 'ginmus'] { fn: [Function: sum] }
```

* 细节:就是argArary和args合并