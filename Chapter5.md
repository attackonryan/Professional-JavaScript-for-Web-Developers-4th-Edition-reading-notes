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

