---
sidebar_position: 3
---

# 规则之外的this

我们讲到的规则已经足以应付平时的开发，但是总有一些语法，超出了我们的规则之外。

## 忽略显示绑定

如果在显示绑定中，我们传入一个`null`或者`undefined`，那么这个显示绑定会被忽略，使用默认规则



## 间接函数引用

创建一个函数的 间接引用，这种情况使用默认绑定规则:

```javascript
function foo() {
    console.log(this);
}

var obj = {
    name: 'obj',
    foo:foo
}

var obj2 = {
    name:'obj2'
}

obj.foo();
(obj2.bar = obj.foo)()
```

```js title="输出结果"
{name: 'obj', foo: ƒ}
Window {0: Window, window: Window, self: Window, document: document, name: '', location: Location, …}
```

分析:

* 赋值(obj2.bar = obj1.foo)的结果是foo函数
* foo函数被直接调用，那么是默认绑定,指向window



## 箭头函数的this也是在规则之外的

为什么说箭头函数的this是在规则之外呢

* 因为箭头函数不使用this的四种标准规则（也就是不绑定this），而是根据外层作用域来决定this
* 无论是默认绑定，还是隐式绑定，还是显式绑定，this指向的都是window

因为箭头函数并不绑定this对象，那么this引用就会从上层作用于中找到对应的this,有点类似于作用域链,只与定义时的位置有关,与调用的方式还有调用的位置都没关系

