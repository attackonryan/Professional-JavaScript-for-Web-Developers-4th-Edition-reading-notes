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

