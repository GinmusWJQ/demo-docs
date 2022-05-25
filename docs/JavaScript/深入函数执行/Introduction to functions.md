---
sidebar_position: 1
---
# 函数的前言简介

在JavaScript中，函数是非常重要的，并且是一等公民！



## 函数的使用是非常灵活的



## 函数可以作为另外一个函数的参数，也可以作为另外一个函数的返回值来使用

而这样的函数被称为高级函数(概念)



### 高级函数有以下几种(实例)

* 尾调用函数
* 尾递归函数  `斐波那契数列`
* 柯里化函数



### 数组中的高级函数方法

* **ECMAScript为数组定义了五个迭代方法，每个方法都接收两个参数:**

* 两个参数分别是:`回调函数`、`this的绑定的对象`
* 而`回调函数`内又有三个参数
  1. 数组项的值(value)
  2. 数组项的索引(index)
  3. 数组本身(array)

* 五个迭代方法分别是:

* **（1）forEach()  对数组的每一项运行给定函数，该方法没有返回值**                                                               

- ```javascript
  var arr = [1,2,3,4,5];
  
  arr.forEach(function(value, index, array) {
      console.log(index + '-' + value + '-' +arr);
  })
  ```

- ```js title="输出结果"
  0-1-1,2,3,4,5
  1-2-1,2,3,4,5
  2-3-1,2,3,4,5
  3-4-1,2,3,4,5
  4-5-1,2,3,4,5
  ```

<br />

* **（2）some（）对数组中的每一项运行给定函数，如果该函数有任一项返回true，则整个返回true**                                                                

- ```javascript
  var arr = [1,2,3,4,5];
  
  var b = arr.some(function (value) {
      return value > 3;
  })
  
  console.log(b);
  console.log(arr);
  ```

- ```js title="输出结果"
  true
  [ 1, 2, 3, 4, 5 ]
  ```

<br />

* **（3）every（）对数组中的每一项运行都给定函数，如果该函数对每一项都返回true，则返回true**                                               

- ```javascript
  var arr = [1,2,3,4,5];
  
  var b = arr.every(function (value) {
      return value > 3;
  })
  
  console.log(b);
  console.log(arr);
  ```

- ```js title="输出结果"
  false
  [ 1, 2, 3, 4, 5 ]
  ```

<br />

* **（4）filter（）对数组的每一项运行给定函数，返回该函数会返回true的项组成的数组**                                                     

- ```javascript
  var arr = [1,2,3,4,5];
  
  var b = arr.filter(function (value) {
      return value > 3;
  })
  
  console.log(b);
  console.log(arr);
  ```

- ```js title="输出结果"
  [ 4, 5 ]
  [ 1, 2, 3, 4, 5 ]
  ```

<br />

* **（5）map（）对数组的每一项运行给定函数，返回每次函数调用结果所组成的数组**                                                                   

- ```javascript
  var arr = [1,2,3,4,5];
  
  var b = arr.map(function (value) {
      return value * 3;
  })
  
  console.log(b);
  console.log(arr);
  ```

- ```js title="输出结果"
  [ 3, 6, 9, 12, 15 ]
  [ 1, 2, 3, 4, 5 ]
  ```

<br />

### ES5新增了两个归并数组的方法,这两个方法都接收两个参数

* 两个个归并方法分别是:`reduce（）`、`reduceRight()`

  

* **reduce（）会迭代数组所有的项，然后构建一个最终的值返回**                                                                          

- ```javascript
  var arr = [1,2,3,4,5];
  
  var sum = arr.reduce(function (pre, cur, index, arr) {
      return pre + cur;
  })
  
  console.log(sum);
  ```

- ```js title="输出结果"
  15
  ```

  * 打印出15,是数组所有值之和

  * 这个函数返回的任何值都会作为第一个参数(pre)自动传给下一次迭代。

  * 第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数是数组的第二项。

  * **但是如果传入了第二个参数:**

  * ```javascript
    var arr = [1,2,3,4,5];
    
    var sum = arr.reduce(function (pre, cur, index, arr) {
        return pre + cur;
    },0)
    
    console.log(sum);
    ```

    * 那么第一次迭代发生在数组的第一项上，那么函数上的第一个形参(pre)是reduce方法的第二个参数，函数上的第二个形参(cur)是数组的第一项。
    * 虽然最后打印的结果都一样,但这里引入了reduce方法的第二个形参,是的第一次迭代发生在数组的不同位置



* **reduceRight（）与reduce（）使用一样，只不过是从后往前遍历**