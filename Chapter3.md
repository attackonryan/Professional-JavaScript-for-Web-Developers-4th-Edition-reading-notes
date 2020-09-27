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
console.log(typeof 95)       // "number"
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

### 3.4.5 Number 类型

Number类型使用IEEE 754格式表示整数和浮点值（在某些语言中也叫双精度值）。

最基本的数字字面量格式是十进制：

```js
let intNum = 55   // 整数
```

可以用八进制表示，第一个数字必须为0，然后是相应的八进制数字（0 ~ 7）。如果字面量中包含的数字超过了有效范围，则忽略前缀的零，后面的数字序列会被当成十进制：

```js
let octalNum1 = 070 // 八进制的56
let octalNum2 = 079 // 无效的八进制，当成79处理
```

**八进制字面量在严格模式无效，必须使用0o前缀**

用十六进制表示，必须让真正的数值前缀0x（区分大小写）：

```js
let hexNum1 = 0xA  // 十六进制10
let hexNum2 = 0x1f // 十六进制31
```

#### 1.浮点值

定义浮点值，必须包含小数点：

```js
let floatNum1 = 1.1
let floatNum2 = 0.1
let floatNum3 = .1  // 有效但不推荐
```

ECMAScript总是想方设法把值转换为整数，如下两种情况：

```js
let floatNum1 = 1.   // 小数点后面没有数字，当成整数1处理
let floatNum2 = 10.0 // 小数点后面是零，当成整数10处理
```

浮点值的精确度最高可达17位小数，但在算数计算中远不如整数精确。例如0.1加0.2的值不是3而是0.30000000000000004。

**永远不要测试某个特定的浮点值**

#### 2.值的范围

ECMAScript可以表示的最小数值保存在**Number.MIN_VALUE**中。最大数值保存在**Number.MAX_VALUE**中。

如果超出了范围，数值会被自动转换为Infinity/-Infinity（无穷）值。

可以使用isFinite()函数判断是否是一个有穷值

正负Infinity不能再进一步用于任何计算

#### 3.NaN

有一个特殊的值叫NaN，意思是“不是数值”（Not a Number），用于表示本来要返回数值的操作失败了。

NaN有几个独特的属性，首先，任何涉及NaN的操作始终返回NaN。

其次，NaN不等于包括NaN在内的任何值：

```js
console.log(NaN == NaN) // false
```

ECMAScript提供了isNaN()函数。该函数会尝试把参数转换为数值，任何不能转换为数值的值都会导致这个函数返回true：

```js
console.log(isNaN(NaN))    // true
console.log(isNaN(10))     // false, 10是数值
console.log(isNaN("10"))   // false, 可以转换成数值10
console.log(isNaN("blue")) // true, 不可以转换成数值
console.log(isNaN(true))   // false, 可以转换成数值1
```

#### 4.数值转换

有3个函数可以将非数值转换为数值：**Number()**、**parseInt()**、**parseFloat()**

Number()函数基于如下规则执行转换。

- 布尔值，true转换为1，false转换为0
- 数值，直接返回
- null，返回0
- undefind，返回NaN
- 字符串，应用如下规则
- - 如果字符串包含数值字符，包括数值字符前面带加减号的情况，则转换为一个十进制数值。
  - 如果字符串包含有效的浮点值格式如“1.1”，则会转换为相应的浮点值（同样，忽略前面的零）。
  - 如果字符串包含有效的十六进制格式如“0xf”,则会转换为与该十六进制值对应的十进制整数值。
  - 如果是空字符串（不包含字符），则返回0。
  - 如果字符串包含上述情况之外的其他字符，则返回NaN。
- 对象，调用valueOf()方法，并按照上述规则转换返回的值。如果转换结果是NaN，则调用toString()方法，再按照转换字符串的规则转换。

下面是几个具体的例子：

```js
let num1 = Numer("Hello world") // NaN
let num2 = Numer("")            // 0
let num3 = Numer("000011")      // 11
let num4 = Numer(true)          // 1
```



**通常在需要得到整数时可以优先使用parseInt()函数**。

字符串最前面的空格会被忽略，从第一个非空格字符开始转换。如果第一个字符不是数值字符、加号或减号，parseInt()立即返回NaN。**这意味着空字符串也会返回NaN**（与Number()不一样）。

示例：

```js
let num1 = parseInt("1234blue")  // 1234
let num2 = parseInt("")          // NaN
let num3 = parseInt("0xA")       // 10，解释为十六进制
let num4 = parseInt(22.5)        // 22
let num5 = parseInt("70")        // 70，解释为十进制
let num6 = parseInt("0xf")       // 15，解释为十六进制
```

**parseInt()也接受第二个参数，用于指定底数（进制数）**。

```js
let num = parseInt("0xAF", 16) // 175
let num1 = parseInt("AF", 16)  // 175， 可以省掉0x前缀
let num2 = parseInt("AF")      // NaN
```



parseFloat()函数的工作方式与parseInt()类似。但是当parseInt()遇到第二个小数点时，剩余字符都会被忽略，因此，“22.34.5”将被转换为22.34

**parseFloat()始终忽略字符串开头的零**。

parseFloat()只解析十进制值。

如果字符串表示整数（没有小数点或者小数点后面只有零（原书说只有一个零，我测试后是多个零都可以）），则parseFloat()返回整数。

示例：

```js
let num1 = parseFloat("1234blue")  // 1234，按整数解析
let num2 = parseFloat("0xA")       // 0
let num3 = parseFloat("22.5")      // 22.5
let num4 = parseFloat("22.34.5")   // 22.34
let num5 = parseFloat("0908.5")    // 908.5
let num6 = parseFloat("3.125e7")   // 31250000
```





