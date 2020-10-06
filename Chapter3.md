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



### 3.4.6 String 类型

String（字符串）数据类型表示零或多个16位Unicode字符序列。字符串可以使用双引号（"）、单引号（'）或反引号（`）标示。

```js
let firstName = "John"
let lastName = 'Jacob'
let lastName2 = `Dead By Daylight`
let wrong = 'error"   // 这是错误的，必须以同种引号作为字符串结尾。
```

#### 1. 字符字面量

字符串数据类型包含一些字符字面量，用于表示非打印字符或其他用途的字符，如下表所示：

| 字面量 | 含义                                |
| :----- | :---------------------------------- |
| \n     | 换行                                |
| \t     | 制表                                |
| \b     | 退格                                |
| \r     | 回车                                |
| \f     | 换页                                |
| \\\    | 反斜杠（\）                         |
| \\'    | 单引号（'）                         |
| \\"    | 双引号（"）                         |
| \\`    | 反引号（`）                         |
| \xnn   | 以十六进制编码nn表示的字符          |
| \unnnn | 以十六进制编码nnnn表示的Unicode字符 |

#### 2. 字符串的特点

ECMAScript中的字符串是不可变的（immutable），意思是一旦创建，它们的值就不能变了。要修改某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量。

#### 3. 转换为字符串

**几乎所有值都有toString()方法**。这个方法唯一的用途就是返回当前值的字符串等价物。比如：

```js
let age = 11
let ageAsString = age.toString()     // 字符串“11”
let found = true
let foundAsString = found.toString() // 字符串“true”
```

toString()方法可见于数值、布尔值、对象和字符串值。null和undefind值没有toString()方法。

在对数值调用这个方法时，toString()可以接收一个底数参数，即以什么底数来输出数值的字符串表示。

```js
let num = 10
console.log(num.toString())     // "10"
console.log(num.toString(2))    // "1010"
console.log(num.toString(8))    // "12"
console.log(num.toString(10))   // "10"
console.log(num.toString(16))   // "a"
```

#### 4. 模板字面量

模板字面量可以跨行定义字符串：

```js
let myNultiLineTemplateLiteral = `fisrt line
second line`
console.log(myNultiLineTemplateLiteral)
// first line
// second line
```

顾名思义，模板字面量在定义模板时特别有用，比如下面这个HTML模板：

```js
let pageHTML = `
<div>
  <a href="#">
	<span>Jake</span>
  </a>
</div>`
```

**模板字面量会保持反引号内部的空格**，因此在使用时要格外注意。格式正确的模板字符串看起来可能会缩进不当。

```js
// 这个模板字面量在换行符之后有25个空格符
let myTemplateLiteral = `first line
						 second line`
console.log(myTemplateLiteral.length)    // 47

let secondTemplateLiteral = `
first line
second line`
console.log(secondTemplateLiteral[0] === '\n')  // true
```



#### 5. 字符串插值

模板字面量最常用的一个特性是支持字符串插值，也就是可以在一个连续定义中插入一个或多个值。

字符串插值通过在${}中使用一个JavaScript表达式实现：

```js
let value = 5
let exponent = 'second'
let interpolatedTemplateLiteral = `${ value } to thr ${ exponent } power is ${ value * value }`
console.log(interpolatedTemplateLiteral) // 5 to the second power is 25
```

**所有插入的值都会使用toString()强制转型为字符串**。任何JavaScript表达式都可以用于插值。

#### 6. 模板字面量标签函数

模板字面量也支持定义**标签函数（tag function）**，而通过标签函数可以自定义插值行为。

标签函数会接收被插值记号分隔后的模板和对每个表达式求值的结果。

通过一个例子来理解：

```js
let a = 6
let b = 9

function simpleTag(strings, aValExpression, bValExpression, sumExpression){
    console.log(strings)
    console.log(aValExpression)
    console.log(bValExpression)
    console.log(sumExpression)
    return 'foobar'
}

let untaggedResult = `${ a } + ${ b } = ${ a + b }`
let taggedRsult = simpleTag`${ a } + ${ b } = ${ a + b }`
// ["", " + ", " = ", ""]
// 6
// 9
// 15

console.log(untaggedResult) // "6 + 9 = 15"
console.log(taggedRsult)    // "foobar"
```

#### 7. 原始字符串

使用模板字面量也可直接获取原始的模板字面量内容（如换行符或Unicode字符），而不是被转换后的字符表示。

为此，可以使用默认的**String.raw**标签函数：

```js
console.log(`\u00A9`)  			 //	©
console.log(String.raw`\u00A9`)  // \u00A9
```

另外，也可以通过标签函数的第一个参数，即字符串数组的.raw属性取得每个字符串的原始内容：

```js
function printRaw(strings){
    console.log('Actual characters:')
    for(const string of strings){
        console.log(string)
    }
    
    console.log('Escapled characters:')
    for(const rawString of strings.raw){
        console.log(rawString)
    }
}

printRaw`\u00A9${'and'}\n`
// Actual characters:
// ©
// (换行符)
// Escaped characters:
// \u00A9
// \n
```



### 3.4.7 Symbol类型

**符号（Symbol）是原始值，且符号实例是唯一、不可变的。**符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。

#### 1. 符号的基本用法

符号需要使用Symbol()函数初始化。

```js
let sym = Symbol()
console.log(typeof sym) // symbol
```

调用Symbol函数时，也可以传入一个字符串参数作为对符号的描述（description）。但是，这个字符串参数与符号定义或标识完全无关：

```js
let genericSymbol = Symbol()
let otherGenericSymbol = Symbol()

console.log(genericSymbol == otherGenericSymbol)  // false

let fooSymbol = Symbol('foo')
let otherFooSymbol = Symbol('foo')

console.log(fooSymbol == otherFooSymbol)  // false
```



#### 2. 使用全局符号注册表

如果运行时的不同部分需要共享和重用符号实例，那么可以用一个字符串作为键，在全局符号注册表中创建并重用符号。

为此，需要使用Symbol.for()方法。**这个方法对每个字符串键都执行幂等操作**。第一次使用某个字符串调用时，它会检查全局运行时注册表，发现不存在对应的符号，于是会生成一个新的符号实例并添加至注册表中。后续使用相同字符串的调用会同样检查注册表，发现存在与该字符串对应的符号，就会返回该符号实例。

```js
let fooGlobalSymbol = Symbol.for('foo')
let otherFooGlobalSymbol = Symbol.for('foo')

console.log(fooGlobalSymbol == otherFooGlobalSymbol)  // true
```

**全局注册表中的符号必须使用字符串键来创建，因此作为参数传给Symbol.for()的任何值都会被转换为字符串**。

还可以使用Symbol.keyFor()来查询全局注册表，这个方法接收符号，返回该全局符号对应的字符串键，如果查询的不是全局符号，则返回undefind。

```js
// 创建全局符号
let s = Symbol.for('foo')
console.log(Symbol.keyFor(s))  // foo

// 创建普通符号
let s2 = Symbol.for('bar')
console.log(Symbol.keyFor(s2)) // undefind

Symbol.keyFor(123)  // TypeErrr: 123 is not a symbol
```



#### 3. 使用符号作为属性

凡是可以使用字符串或数值作为属性的地方，都可以使用符号。

```js
let s1 = Symbol('foo')
let s2 = Symbol('bar')

let o = {
    [s1]: 'foo val'
}
// 这样也可以： o[s1] = 'foo val'

Object.defineProperty(0, s2, {value: 'bar val'})
```



#### 4. 常用内置符号

ECMAScript6也引入了一批常用内置符号（well-known symbol），用于暴露语言内部行为，开发者可以直接访问、重写或模拟这些行为。

这些内置符号最重要的用途之一是重新定义它们，从而改变原生结构的行为。比如，我们知道for-of循环会在相关对象上使用Symbol.iterator属性，那么就可以通过在自定义对象上重新定义Symbol.iterator的值来改变for-of在迭代该对象时的行为。

这些内置符号只是全局函数Symbol的普通字符串属性，指向一个符号的实例。所有这些内置符号属性都是不可写、不可枚举、不可配置的。

> 在提到ECMAScript规范时，经常会引用符号在规范中的名称，前缀为@@。比如，@@iterator指的就是Symbol.iterator。

**关于内置符号，我推荐大家看一看阮一峰老师在[<ECMAScript 6 入门>](https://es6.ruanyifeng.com/#docs/symbol#%E5%86%85%E7%BD%AE%E7%9A%84-Symbol-%E5%80%BC)中的讲解**。



### 3.4.8 Object 类型

ECMAScript中的对象其实就是一组数据和功能的集合。对象通过new操作符后跟对象类型的名称来创建。开发者可以通过创建Object类型的实例来创建自己的对象，然后再给对象添加属性和方法。

Object实例本身并不是很有用，但理解与它相关的概念非常重要。类似Java中的Java.lang.Object，ECMAscript中的Object也是派生其他对象的基类。Object类型的所有属性和方法在派生的对象上同样存在。

每个Object实例都有如下属性和方法。

- constructor: 用于创建当前对象的函数。在前面的例子中，这个属性的值就是Object()函数。
- hasOwnProperty(propertyName): 用于判断当前对象实例（不是原型）上是否存在给点的属性。
- isPrototypeOf(object): 用于判断当前对象是否为另一个对象的原型。
- propertyIsEnumerable(protertyName): 用于判断给点的属性是否可以使用for-in语句枚举。
- toLocaleString(): 返回对象的字符串表示。该字符串反映对象所在的本地化执行环境。
- toString(): 返回对象的字符串表示。
- valueOf(): 返回对象对应的字符串、数值或布尔值表示。通常与toString()的返回值相同。



## 3.5 操作符

### 3.5.1 一元操作符

只操作一个值的操作符叫**一元操作符**（unary operator）。

#### 1. 递增/递减操作符

递增和递减操作符直接照搬自C语言，有两个版本：前缀版和后缀版。

前缀操作，变量的值都会在语句被求值前改变。

```js
let age = 29
++age   // 等于 age = age + 1

let age = 29
let anotherAge = --age + 2

console.log(age)         // 28
console.log(anotherAge)  // 30
```

后缀操作与前缀操作的主要区别在于，后缀版递增和递减在语句被求值后才发生。。

```js
let num1 = 2
let num2 = 20
let num3 = num1-- + num2
let num4 = num1 + num2

console.log(num3)      // 22
console.log(num4)      // 21
```

这4个操作符可以作用于任何值，意思是不限于整数，字符串、布尔值、浮点值，甚至对象都可以。递增和递减操作符遵循如下规则。

- 对于字符串，如果是有效的数值形式，则转换为数值再应用改变。变量类型从字符串变为数值。
- 对于字符串，如果不是有效的数值形式，则将变量的值设置为NaN。变量类型从字符串变为数值。
- 对于布尔值，如果是false则转换为0再应用改变。变量类型从布尔值变为数值。
- 对于布尔值，如果是true则转换为1再应用改变。变量类型从布尔值变为数值。
- 对于浮点值，加1或减1。
- 如果是对象，则调用valueOf()方法取得可以操作的值。对得到的值应用上述规则。如果是NaN，则调用toString()并再次应用其他规则。变量类型从对象变为数值。



#### 2. 一元加和减

一元加由一个加号（+）表示。对数值没有任何影响。

如果将一元操作符应用到非数值，则会执行与使用Number()转型函数一样的类型转换。

```js
let s1 = '01'
let s2 = '1.1'
let s3 = 'z'
let b = false
let f = 1.1
let o = {
    valueOf(){
        return -1
    }
}

s1 = +s1        // 值变成数值1
s2 = +s2        // 值变成数值1.1
s3 = +s3        // 值变成NaN
b = +b	        // 值变成数值0
f = +f          // 不变，还是1.1
o = +o          // 值变成数值-1 
```



### 3.5.2 位操作符

#### 1. 按位非

按位非操作符用波浪符（~）表示，它的作用是返回数值的一补数。

```js
let num1 = 25				 // 二进制00000000000000000000000000011001
let num2 = ~num1             // 二进制11111111111111111111111111100110
console.log(num2)            // -26
```

**按位非的最终效果是对数值取反并减1**。



#### 2. 按位与

按位与操作符用和号（＆）表示。



#### 3. 按位或

按位或操作符用管道符（|）表示。



#### 4.按位异或

按位异或操作符用脱字符（^）表示。



#### 5. 有符号左移右移

有符号左移：<<

有符号右移：>>



#### 6. 无符号左移右移

无符号左移：<<<

无符号右移：>>>



### 3.5.3 布尔操作符

布尔操作由3个： 逻辑非、逻辑与和逻辑或。

#### 1. 逻辑非

逻辑非操作符由一个叹号（!）表示，可用于任何值。这个操作符始终返回布尔值。**逻辑非操作符首先将操作数转换为布尔值，然后取反**。逻辑非操作符遵循以下规则：

- 如果操作数是对象，则返回false
- 如果操作数是空字符串，则返回true
- 如果操作数是非空字符串，则返回false
- 如果操作数是数值0，则返回true
- 如果操作数是非0数值(包括Infinity)，则返回false
- 如果操作数是null，则返回true
- 如果操作数是NaN，则返回true
- 如果操作数是undefind，则返回true



#### 2. 逻辑与

逻辑与操作符由两个和号（&&）表示。

逻辑与操作符遵循 **两真为真，否则为假** 的规律。

逻辑与操作符可用于任何类型的操作数，不限于布尔值。如果有操作数不是布尔值，则逻辑与并不一定会返回布尔值，而是遵循如下规则。

- 如果第一个操作数是对象，则返回第二个操作数。
- 如果第二个操作数是对象，则只有第一个操作数求值为true时才会返回该对象。
- 如果两个操作数都是对象，则返回第二个操作数。
- 如果有一个操作数是null/NaN/undefind，则返回null/NaN/undefind

逻辑与操作符是一种短路操作符，意思就是如果第一个操作数决定了结果，那么永远不会对第二个操作数求值。对逻辑与来说，如果第一个操作数是false，那么无论第二个操作数是什么值，结果也不可能等于true。

``` javascript
let found = true
let result = (found && someUndeclaredVariable)   // 这里会出错
console.log(result)      // 不会执行这一行
```

```js
let found = false
let result = (found && someUndeclaredVariable)   // 这里不会出错
console.log(result)      // 会执行这一行
```



#### 3. 逻辑或

逻辑或操作符由两个管道符（||）表示。

逻辑或操作符遵循 **两假为假，否则为真** 的规律。

与逻辑与类似，如果有一个操作数不是布尔值，那么逻辑或操作符也不一定返回布尔值。

- 如果第一个操作数是对象，则返回第一个操作数。
- 如果第一个操作数求值为false，则返回第二个操作数。
- 如果两个操作数都是对象，则返回第一个操作数。
- 如果两个操作数都是null/NaN/undefind，则返回null/NaN/undefind

逻辑或同样也具有短路的特性。

利用逻辑或的特性，可以避免给变量赋值null或undefind

```js
let myObject = preferredObject || backupObject
```

### 3.5.4 乘性操作符

略



### 3.5.5 指数操作符

略



### 3.5.6 加性操作符

#### 1. 加法操作符

加法操作符（+）用于求两个数的和。

如果两个操作数都是数值，加法操作符执行加法运算并根据如下规则返回结果。

- 如果有任一操作数是NaN，则返回NaN
- 如果是Infinity加Infinity，返回Infinity
- 如果是-Infinity加-Infinity，返回-Infinity
- 如果是Infinity加-Infinity，返回NaN
- 如果是+0 加 +0，返回+0
- 如果是-0 加 +0，返回+0
- 如果是-0 加 -0，返回-0

不过，如果有一个操作数是字符串，则要应用如下规则：

- 如果两个操作数都是字符串，则拼接
- 如果只有一个是字符串，则将另一个操作数转换为字符串，再拼接。

如果有任一操作数是对象、数值或布尔值，则调用它们的toString()方法以获取字符串，对于undefind和null则调用String()函数，分别获取“undefind"和”null“

#### 2. 减法操作符

简单记忆，减法操作符会尽力将两个操作数转成数值。会优先调用对象的valueOf()方法，如果没有valueOf()则调用toString()方法，再将获得的值变为数值。如果操作数存在NaN，返回NaN

```js
let result1 = 5 - true    // true被转换成1， 结果是4
let result2 = NaN - 1     // NaN
let result3 = 5 - ''      // ''被转换成0， 结果是5
let result4 = 5 - null    // null被转换成0，结果是5
```



