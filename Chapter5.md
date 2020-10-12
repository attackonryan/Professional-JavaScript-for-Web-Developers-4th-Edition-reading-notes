# 第五章：基本引用类型

Javascript中的对象称为引用值。有几种内置的引用类型可用于创建特定类型的对象。

## 5.1 Date

要创建日期对象，使用new操作符来调用Date构造函数：

```js
let now = new Date()
```

Date类型将日期保存为自1970年1月1日午夜（零时）至今所经过的毫秒数。

在不给Date构造函数传参的情况下，创建的对象将保存当前的日期和时间。

```js
let now = new Date()
now.getTime()          // 1602464952410(相当于Date.now())
```

### 5.1.1 继承的方法

Date类型重写了toLocaleString()，toString()和valueOf()。

toLocaleString()方法返回与浏览器运行的本地环境一致的日期和时间。

toString()方法通常返回带时区信息的日期和时间。

valueOf()返回日期的毫秒表示。

```js
let now = new Date()
now.toLocaleString()   // "2020/10/12 上午9:14:52"
now.toString()         // Mon Oct 12 2020 09:14:52 GMT+0800 (中国标准时间)"
now.valueOf()          // 1602465292914
```

常用方法

```js
let now = new Date()
now.getDate()          // 12 (返回日期中的日(1~31))
now.getDay()           // 1  (表示周几，0表示周日，6表示周六)
now.getMonth()         // 9  (返回日期中的月，0表示一月，11表示十二月)
now.getHours()         // 9  (返回日期中的时(0~23))
```



## 5.2 RegExp

RegExp类型是ECMAScript支持正则表达式的接口，提供了大多数基础的和部分高级的正则表达式功能。

正则表达式使用类似Perl的简洁语法来创建：

```txt
let expression = /pattern/flags
```

pattern（模式）可以是任何简单或复杂的正则表达式，包括字符类、限定符、分组、向前查找和反向引用。

每个正则表达式可以带零个或多个flags（标记），用于控制正则表达式的行为：

- g: 全局模式，表示查找字符串的全部内容，而不是找到第一个就结束。
- i: 不区分大小写，表示在查找匹配时忽略pattern的字符串大小写。
- m: 多行模式，表示查找到一行文本末尾时会继续查找。
- y: 粘附模式，表示只查找从lastIndex开始及之后的字符串。

使用不同模式和标记可以创建出各种表达式：

```js
// 匹配字符串中的所有"at"
let pattern1 = /at/g

// 匹配第一个"bat"或"cat"，忽略大小写
let pattern2 = /[bc]at/i

// 匹配所有以"at"结尾的三字符组合，忽略大小写
let pattern3 = /.at/gi
```

如果要匹配元字符 （ [ { \ ^ $ | ] } ? * + . ，需要转义：

```js
// 匹配第一个"[bc]at",忽略大小写
let pattern = /\[bc\at]/i

// 匹配所有".at"，忽略大小写
let pattern4 = /\.at/gi
```

RegExp实例的主要方法是exec()。主要用于配合捕获组的使用。这个方法只接收一个参数，即要应用模式的字符串。

```js
let text = "attack on ryan"
let pattern = /at(t)ack/
let matches = pattern.exec(text)     // ["attack", "t", index: 0, input: "attack on ryan", groups: undefined]
```

另一个常用方法是test()。接收一个字符串参数，如果能够匹配则返回true否则返回false。

```js
let text = "attack on ryan"
let pattern = /attack/

pattern.test(text)     // true
```



## 5.3 原始值包装类型

为了方便操作原始值，ECMAScript提供了3中特殊的引用类型：Boolean、Number和 String。

**每当用到某个原始值的方法或属性时，后台都会创建一个相应原始包装类型的对象，从而暴露出操作原始值的各种方法**。

```js
let s1 = "some text"
let s2 = s1.substring(2)
```

s1为基本字符串类型，本不应该存在substring方法，但是后台会进行如下3个步骤：

1. 创建一个String类型的实例
2. 调用实例上的特定方法
3. 销毁实例

可以想象成如下代码

```js
let s1 = new String("some text")
let s2 = s1.substring(2)
s1 = null
```

因此给原始值添加属性或方法时无效的

```js
let s1 = "some text"
s1.color = "cyan"       // （临时创建的实例会在语句结束后销毁）
console.log(s1.color)   // undefined
```



## 5.4 单例内置对象

ECMA-262对内置对象的定义是“任何由ECMAScript实现提供、与宿主环境无关，并在ECMAScript程序开始执行时就存在的对象”。这意味着开发者不用显式实例化内置对象，因为它们已经实例化好了。

### 5.4.1 Global

Global对象时ECMAScript中最特别的对象，因为代码不会显式访问它。ECMA-262规定Global对象为兜底对象，在全局作用域中定义的变量和函数都会变成Global对象的属性。

#### window对象

浏览器将window对象实现为Global对象的代理，因此所以全局作用域中声明的变量和函数都会变成window的属性。

```js
var color = "red"

window.color = "red"
```



### 5.4.2 Math

ECMAScript提供了Math对象作为保存数学公式、信息和计算的地方。

常用属性和方法

```js
Math.E          // 自然对数的基数e的值
Math.PI         // Π的值

Math.min(2，5，1，10)      // 返回一组数值中的最小值
Math.max(2，5，1，10)      // 返回一组数值中的最大值
Math.ceil()               // 向上舍入为最接近的整数
Math.floor()              // 向下舍入为最接近的整数
Math.round()              // 四舍五入返回整数
Math.random()             // 返回[0, 1)之间的随机数
```





