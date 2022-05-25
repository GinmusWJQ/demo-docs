---
sidebar_position: 3
---

# 深入理解作用域

深入理解作用域的前提是:先理解`变量环境`和`记录`这两个词

## 其实上面写的都是基于早期ECMA的版本规范：

>Every execution context has associated with it a variable object.Variables and functions declared in the source text are added as properties of the variable object.For function codeparameters are added as properties of the variable object.
>
>摘自维基百科


---


##### 那这句话该怎么翻译才正确呢?

每一个执行上下文会被关联到一个变量环境(variableobjectVO)，在源代码中的变量和函数声明会被作为属性添加到VO中。对于函数来说，参数也会被添加到VO中。



## 在最新的ECMA的版本规范中，对于一些词汇进行了修改：

>Every execution context has an associated VariableEnvironmentVariables and functions declared in ECMAScript code evaluated in an execution context are added as bindings in that VariableEnvironment’s Environment Record.For function code,parameters are also added as bindings to that Environment Record
>
>摘自最新的维基百科

##### 翻译过来:

每一个执行上下文会关联到一个变量环境(VariableEnvironment)中，在执行代码中变量和函数的声明会作为环境记录(EnvironmentRecord)添加到变量环境中。对于函数来说，参数也会被作为环境记录添加到变量环境中。



## 以上两者的区别在于VO(Variable Object)变成了VE(VariableEnvironment)

之所以VO变成VE是因为现在不再必须使执行上下文关联到一个变量对象上,而是变成了变量环境,是因为可以关联到环境记录,而该记录可以用对象实现,还可以用Map或者Table实现

