---
sidebar_position: 2
---
# 认识arguments
## arguments 是一个对应于传递给函数的实参的类数组(array-like)对象。

> array-like意味着它不是一个数组类型，而是一个对象类型

* 虽然它拥有数组的一些特性，比如说length，比如可以通过index索引来访问
* 但是它却没有数组的一些方法，比如forEach、map等



## 所以一般情况下,我们都会把arguments转换为数组

那么将arguments转换为数组有以下常见三种方法:

* **转换方式一:自己用for循环遍历**
  
  ```javascript
  var length = arguments.length;
  var arr2 = [];
  
  var i = 0;
  for(i=0; i < length; i++) {
      arr2.push(arguments[i])
  }
  console.log(arr2);
  ```
  <br/>
* **转换方式二:用系统给的方法遍历 *(有两种写法)***
  
  ```javascript
  //写法一:
  var arr = Array.prototype.slice.call(arguments);
  
  //写法二
  var arr = [].slice.call(arguments);
  ```
  这种转换方式涉及到call的知识,还有slice方法的原理
  * slice的原理就是遍历一遍调用它的对象,然后创建新数组存放符合遍历条件的数.
  * 也就是谁调用slice,slice就遍历谁,那么再回到本质的问题,谁调用slice,就是隐形绑定slice的this指向,那么如果直接用call来指定slice的this指向对象,那么slice就直接遍历call指向的对象
  * 同时slice理应要传入两个参数,分别是遍历的起始索引,遍历结束时的索引,如果都不传入的话,slice默认从头到尾遍历一遍
  * slice被Array.prototype调用,本质上与被空数组"[  ]"调用是一样的
<br/>
<br/>
* **转换方式三:用ES6新增方法转换 *(同样也有两种写法)***
  
  ```javascript
  //写法一:
  const arr = Array.from(arguments);
  
  //写法二
  const arr = [...arguments];
  ```



## 但是要注意,箭头函数没有arguments的

如果给箭头函数定义arguments了,那么就会往上一直找父级的arguments,一直找到全局对象

那么全局对象有arguments吗?要分两种情况

1. 在浏览器中,全局对象是没有arguments,运行会报错的
2. 但在Node中,全局对象是arguments  (大家可以自己去打印看一下)
   
   ```js title="Node中的arguments大概长成这样:"
   [
     {},
     [Function: require] {
       resolve: [Function: resolve] { paths: [Function: paths] },
       main: Module {
         id: '.',
         path: '#',
         exports: {},
         filename: '#',      
         loaded: false,
         children: [],
         paths: [Array]
       },
       extensions: [Object: null prototype] {
         '.js': [Function (anonymous)],
         '.json': [Function (anonymous)],
         '.node': [Function (anonymous)]
       },
       cache: [Object: null prototype] {
         '#': [Module]       
       }
     },
     Module {
       id: '.',
       path: '#',
       exports: {},
       filename: '#',        
       loaded: false,
       children: [],
       paths: [
         '#',
         '#',
         '#',
         '#',
         '#'
       ]
     },
     '#',
     '#'
   ]


<br/>
如果想获得箭头函数的参数怎么办?

* 用剩余数组  ...args