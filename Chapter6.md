# 第六章：集合引用类型

## 6.1 Object

Object是ECMAScript中最常用的类型之一。

创建Object有两种方式，一种是使用new操作符和Object构造函数，另一种方式是使用**对象字面量**表示法（object literal）

```js
let person = new Object()
person.name = 'Ryan'

// 对象字面量表示
let person2 = {
    name: 'Titan'
}         
```

字符串的属性名可以是字符串或数值，数值属性会自动转换为字符串

```js
let person = {
    "name": "Ryan",
    age: 20,
    5: true
}
```

使用中括号可以通过变量访问属性，或设置属性。

```js
let option = 'name'

let person = {
    [option]: 'Ryan'
}

person[option] = 'AttackonRyan'
```

通常，点语法是首选的属性存取方式，除非访问属性时必须使用变量。



## 6.2 Array

ECMAScript中的数组是一组有序的数据，且每个槽位可以存储任意类型的数据。

ECMAScript中的数组也是动态大小的，会随着数据的添加而自动增长。

### 6.2.1 创建数组

有几种基本方式创建数组。一种是使用Array构造函数，给构造函数传入一个数值，然后length属性就会被自动创建并设置为这个值。

```js
let colors = new Array(20)    // 创建一个初始length为20的数组
```

也可以给Array构造函数传入要保存的元素。比如下面的代码会创建一个包含3个字符串值的数组。

```js
let colors = new Array("red", "blue", "green")
```

**有时候想创建一个只含一个数字的数组，直接使用Array构造函数是办不到的，因为他会创建一个长度为传入的数值的数组**。这时候可以使用Array.of()方法（ES6新增），这个方法用于将一组参数转换为数组实例。

```js
let colors = new Array(20)    // [empty × 20] 创建了一个初始length为20的数组
let nums = Array.of(20)       // [20]
```

另一种创建数组的方式是使用**数组字面量**（array literal）表示法。

```js
let colors = ["red", "blue", "green"]
```

Array.from()也可以创建数组，用于将类数组结构转换为数组实例。

Array.from()的第一个参数是一个类数组对象，即任何可迭代的结构，或者有一个length属性和可索引元素的结构。

```js
// 字符串会被拆分成单字符串数组
console.log(Array.from("Ryan"))  // ["R", "y", "a", "n"]

// Array.from()对现有数组执行浅复制
const a1 = [1, 2, 3, 4]
const a2 = Array.from(a1)

console.log(a1)      // [1, 2, 3, 4]
console.log(a1 === a2)  // false

// arguments对象可以被轻松地转换为数组
function getArgsArray(){
    return Array.from(arguments)
}
console.log(getArgsArray(1, 2, 3))   // [1, 2, 3]

// from()也能转换带有必要属性的自定义对象
const arrayLikeObject = {
    0: 1,
    1: 2,
    2: 3,
    3: 4,
    length: 4
}
console.log(Array.from(arrayLikeObject))   // [1, 2, 3, 4]
```

Array.from()还接收第二个可选的映射函数参数。这个函数可以改写新数组的值。还可以接收第三个可选参数，用于指定映射函数中this的值。

```js
const a1 = [1, 2, 3, 4]
const a2 = Array.from(a1, x => x**2)
const a3 = Array.from(a1, function(x){ 
    return x **this.exponent
}, {
    exponent: 2
})

console.log(a2)   // [1, 4, 9, 16]
console.log(a3)   // [1, 4, 9, 16]
```



### 6.2.2 数组空位

使用数组字面量初始化数组时，可以使用一串逗号来创建空位（hole）。ECMAScript会将逗号之间相应索引位置的值当成空位，ES6规范重新定义了该如何处理这些空位。

```js
const options = [,,,,,]    //创建包含5个元素的数组
console.log(options.length)   // 5
console.log(options)    // [,,,,,]
```

for-of循环会将空位当成存在的元素，只不过值为undefined。

ES6之前的方法则会忽略这个空位，但具体的行为也会因方法而异。

> **注意**：由于行为不一致和存在性能隐患，实践中要避免使用数组空位。如确实需要空位，则可以显示地用undefined代替



### 6.2.3 数组索引

要取得或设置数组的值，需要使用中括号并提供相应值的数字索引：

```js
let colors = ["red", "blue", "green"]
colors[2] = "black"   // 修改第三项
```

如果把一个值设置给超过数组最大索引的索引，则数组长度会自动扩展到该索引值加1

```js
let colors = ["red", "blue", "green"]
colors[5] = "black"   // 设置第6项为"black"
console.log(colors.length)   // 6
```

通过修改length属性，可以截断或增长数组。增长数组后多出来的空位由undefined填充。

```js
let colors = ["red", "blue", "green"]
colors.length = 1
console.log(colors)   // ["red"]

colors.length = 4
console.log(colors[3]) // undefined 
```



### 6.2.4 检测数组

 instanceof 操作符可以检测一个对象是不是数组。

```js
if(value instanceof Array){
   // 操作数组 
}
```

但是如果网页里存在多个框架，可能会涉及两个不同的全局执行上下文，因此会有两个不同版本的Array构造函数，这个时候instanceof不一定返回正确的结果。

为解决这个问题，ECMAScript提供了Array.isArray()方法。这个方法的目的是确定一个值是否为数组，而不用管它在哪个全局执行上下文中创建的。

```js
if(Array.isArray(value)){
    // 操作数组
}
```

