# 第三章：语言基础

## 3.1 语法

### 3.1.1 区分大小写

ECMAScript中一切区分大小写。

### 3.1.2 标识符

标识符就是变量、函数、属性或函数参数的名称。标识符可以由一或多个下列字符组成：

- 第一个字符必须是一个字母、下划线(_)或美元符号($)；
- 剩下的其他字符可以是字母、下划线、美元符号或数字。

按照惯例，ECMAScript标识符使用驼峰大小写形式，即第一个单词的首字母小写，后面每个单词的首字母大写，如：

- firstSecond
- myCar

### 3.1.3 注释

```js
//  单行注释
/*
	多行注释
*/
```

### 3.1.4 严格模式

ES5增加了严格模式(strict mode)的概念。**严格模式是一种不同的JavaScript解析和执行模型**，ES3的一些不规范写法在这种模式下会被处理，对于不安全的活动将抛出错误。

要启动严格模式，有两种方式：

```js
"use strict"  // 全局严格模式
----------------------------------------
function doSomething(){
    "use strict"   // 局部严格模式
    
}
```

### 3.1.5 语句

ECMAScript中的语句以分号结尾，**但不是必需的**：

```js
let sum = a + b  // 没有分号也有效
let diff = a - b;  // 加分号有效
```

## 3.2 关键字与保留字

略过

## 3.3 变量

ECMAScript是松散类型的，意思是变量可以用于保存任何类型的数据。每个变量不过是一个用于保存在任意值的命名占位符。

有3个关键字可以声明变量：

- var
- let
- const

var在所有版本中可以使用，但let与const只能在ES6及更晚的版本中使用。

### 3.3.1 var关键字

```js
var message     // 定义一个名为message的变量

/* ----------------------------------------- */
var message = "hi"      // 定义并赋值，这个变量可以改变保存的值或类型
message = 100
```

#### 1.var声明作用域

使用var操作符定义的变量会成为包含它的函数的局部变量。

```js
function test(){
    var message = "hi"  // 局部变量
}
test()
console.log(message)  // 出错！
```

在函数内定义变量时省略var操作符，可以创建全局变量：

```js
function test(){
    message = "hi" // 全局变量
}
test()
console.log(message) // "hi"
```

不建议这样写

#### 2.var声明提升

使用var时，下面代码不会报错。这是因为使用这个关键字声明的变量会自动提升到函数作用域顶部：

```js
function foo(){
    console.log(age)
    var age = 26
}
foo() // undefined
```

等价于如下代码：

```js
function foo(){
    var age
    console.log(age)
    age = 26
}
foo() // undefined
```

这就是所谓的“提升” (hoist)， 也就是把所有变量声明都拉到函数作用域的顶部。

反复多次使用var声明同一个变量也没问题：

```js
function foo(){
    var age = 16
    var age = 26
    var age = 36
    console.log(age)
}
foo() // 36
```

### 3.3.2 let声明

let 跟 var的作用差不多，但有重要的区别。最明显的区别是，**let声明的范围是块作用域，而var声明的范围是函数作用域**。

```js
if(true){
    var name = 'Matt'
    console.log(name) // Matt
}
console.log(name) // Matt
```

```js
if(true){
    let age = 26
    console.log(age)  // 26
}
console.log(age) // ReferenceError: age没有定义
```

**let不允许同一个块作用域中出现冗余申明**。这样会导致报错：

```js
let age
let age // SyntaxError: 标识符age已经声明过了
```

#### 暂时性死区

let 与 var的另一个重要的区别，就是let声明的变量不会在作用域中被提升。

```js
console.log(age) // ReferenceError: age没有定义
let age = 26 
```

在let声明之前的执行瞬间被称为“暂时性死区”(temporal dead zone), 在此阶段引用任何后面才声明的变量都会抛出ReferenceError。

#### for循环中的let声明

在let出现之前，for循环定义的迭代变量会渗透到循环体外部(var是函数作用域)

```js
for(var i = 0; i < 5; i ++){
    // 循环逻辑
}
console.log(i)  // 5
```

改成let 后，这个问题就消失了，因为let作用域仅限于循环块内部

```js
for(var i = 0; i < 5; i ++){
    // 循环逻辑
}
console.log(i)  // ReferenceError: i 没有定义
```

### 3.3.3 const声明

const 的行为与let基本相同，唯一一个重要的区别是用它声明变量时必须同时初始化变量，且尝试修改const声明的变量会导致运行时错误。

```js
const age = 26
age = 36 // TypeError: 给常量赋值
```

const 声明的限制只适用于它指向的变量的引用。换句话说，如果const变量引用的是一个对象，那么**修改这个对象内部的属性并不违反const的限制**。

```js
const person = {}
person.name = 'Matt' // ok
```

### 3.3.4 声明风格及最佳实践

#### 1. 不使用var

#### 2. const优先， let次之

使用const可以让浏览器运行时强制保持变量不变，也可以让静态代码分析工具提前发现不合法的赋值操作。只有再提前知道未来会有修改操作时，才使用let。

## 3.4 数据类型

ECMAScript有7种简单数据类型(也称为**原始类型**):

- Undefined
- Null
- Boolean
- Number
- String
- Symbol
- BigInt（原书没有写这个，因为只介绍到了ES2019，BigInt是ES2020新增的原始类型）

还有一种复杂数据类型叫Object。Object是一种无序名值对的集合。

### 3.4.1 typeof 操作符

对一个值使用typeof操作符会返回下列字符串之一：

- “undefined”表示值未定义
- "boolean"表示值为布尔值
- “string”表示值为字符串
- “number”表示值为数值
- “object”表示值为对象(而不是函数)或null
- “function"表示值为函数
- "symbol"表示值为符号
- “bigint”表示值为大整数（原书没有提到）

**注意typeof 是一个操作符而不是函数，所以不需要参数**（但可以使用参数）。

例子：

```js
let message = "some string"
console.log(typeof message)  // "string"
console.log(typeof(message)) // "string"
console.log(typeof 95) // "number"
```

typeof null会返回“object”。这是因为特殊值null被认为是一个对空对象的引用。

### 3.4.2 Undefind 类型

Undefind类型只有一个值，就是特殊值undefined。当使用var或let声明了变量但没有初始化时，相当于给变量赋予了undefind值。

```js
let message
console.log(message == undefind) // true
```

undefind是一个假值

### 3.4.3 Null 类型

Null类型同样只有一个值，即特殊值null。逻辑上讲，null值表示一个空对象指针。

在定义将来要保存对象值的变量时，建议使用null来初始化，不要使用其他值。

```js
if(car != null){
    // car是一个对象的引用
}
```

undefind值是有null值派生而来的，他们表面上相等

```js
console.log(null == undefind)
```

null同样是一个假值

### 3.4.4 Boolean类型				

Boolean（布尔值）类型是ECMAScript中使用最频繁的类型之一，有两个字面量值：true和false。

要将其他值转换为布尔值，可以调用特定的Boolean()转型函数：

```js
let message = "Hello world!" 
let messageAsBoolean = Boolean(message)  // true
```

什么值能转换为true或false取决于数据类型和实际的值。下表总结了不同类型与布尔值之间的转换规则。

| 数据类型 |     转换为true的值     |   转换为false的值    |
| :------: | :--------------------: | :------------------: |
| Boolean  |          true          |        false         |
|  String  |       非空字符串       |   “” （空字符串）    |
|  Number  | 非零数值（包括无穷值） | 0、NaN（后面会介绍） |
|  Object  |        任意对象        |         null         |
| Undefind |     N/A（不存在）      |       undefind       |





