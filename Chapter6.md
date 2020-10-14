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

为解决这个问题，ECMAScript提供了**Array.isArray()**方法。这个方法的目的是确定一个值是否为数组，而不用管它在哪个全局执行上下文中创建的。

```js
if(Array.isArray(value)){
    // 操作数组
}
```



### 6.2.5 迭代器方法

ES6中，Array的原型上暴露了3个用于检索数组内容的方法：**keys**()、**values**()和**entries**()。

**keys()**返回数组索引的迭代器，**values()**返回数组元素的迭代器、而**entries()**返回索引/值对的迭代器。

```js
const a = ["foo", "bar", "baz", "qux"]

// 因为这些方法都返回迭代器，所以可以将它们的内容通过Array.from()直接转换为数组实例
const aKeys = Array.from(a.keys())
const aValues = Array.from(a.balues())
const aEntries = Array.from(a.entries())

console.log(aKeys)      // [0, 1, 2, 3]
console.log(aValues)    // ["foo", "bar", "baz", "qux"]
console.log(aEntries)   // [[0, "foo"], [1, "bar"], [2, "baz"], [3, "qux"]]
```



### 6.2.6 复制和填充方法

ES6新增了两个方法：批量赋值方法**fill()**，以及填充数组方法**copyWithin()**。

fill()的第一个参数是填充的值，第二个可选参数是索引的开始。第三个可选参数的索引的结束。

```js
const zeroes = [0, 0, 0, 0, 0]

zeroes.fill(7, 1, 3) 

console.log(zeroes)  // [0, 7, 7, 0, 0]
```

copyWithin()会按照指定范围浅复制数组中的部分内容，然后插入到指定索引开始位置。第一个参数是索引开始位置，第二，第三个参数是指定范围的开始索引和结束索引。

```js
const numbers = [1,2,3,4,5,6]

numbers.copyWithin(2, 4, 6)

console.log(numbers)  // [1, 2, 5, 6, 5, 6]
```



### 6.2.7 转换方法

所有对象都有**toLocaleString()**、**toString()**和**valueOf()**方法。

其中**valueOf()**返回的还是数组本身。

**toString()**返回由数组中每个值调用**toString()**返回的字符串拼接而成的一个逗号分隔的字符串。

**toLocaleString()**方法类似**toString()**，取数组每个元素值的时候会调用**toLocaleString()**方法，返回字符串拼接而成的一个逗号分隔的字符串。

```js
let colors = ["red", "blue", "green"]
console.log(colors.toString())   // "red,blue,green"
```

调用数组上的join方法也可以获取字符串，不传参的情况下与toString()的返回值相同，如果传了参数，则分隔符变为参数。

```js
let colors = ["red", "blue", "green"]
console.log(colors.join(","))  // "red,green,blue"
console.log(colors.join("||")) // "red||green||blue"
```



### 6.2.8 栈方法

ECMAScript数组提供了**push()**和**pop()**方法，以实现类似栈的行为。

**push()**方法接收任意数量的参数，并将它们添加到数组末尾，返回数组的最新长度。

**pop()**方法则用于删除数组的最后一项，同时减少数组的length值，返回被删除的项。



### 6.2.9 队列方法

使用**shift()**和**push()**方法，可以把数组当成队列来使用。

**shift()**方法会删除数组的第一项并返回它。

ECMAScript也提供了**unshift()**方法，这个方法执行**shift()**相反的操作：在数组开头添加任意多个值，然后返回新的数组长度。



### 6.2.10 排序方法

数组有两种方法可以用来对元素重新排序：**reverse()**和**sort()**。

__reverse()__方法将数组元素反向 排列。

```js
let values = [1, 2, 3, 4, 5]
values.reverse()
console.log(values)  // [5, 4, 3, 2, 1]
```

__sort()__方法用于排序数组，默认情况下会按照升序排序，最小的值在前面，最大的值在后面。**为此sort()会在每一项上调用String()转型函数，然后来比较字符串决定顺序**。

```js
let values = [0, 1, 5, 10, 15]
values.sort()
console.log(values) // [0, 1, 10, 15 ,5]     // "5" > "15"
```

__sort()__方法接收一个__比较函数__，用于判断哪个值应该排在前面。

比较函数接收两个参数，如果第一个参数应该排在第二个参数前面，则函数返回负值。如果第一个参数应该排在第二个参数后面，则返回正值。如果参数不用变化位置，则返回0。

```js
function compare(v1, v2){
    if(v1 < v2){
        return -1
    }else if(v1 > v2){
        return 1
    }else{
        return 0
    }
}
/*
	等价
	function compare(v1, v2){
		return v1 - v2
	}
*/

let values = [0, 1, 15, 10, 5]
values.sort(compare)
console.log(values)  // [0, 1, 5, 10, 15]
```



### 6.2.11 操作方法

对于数组中的元素，我们有很多操作方法。接下来讲三种方法：**concat()、slice()、splice()**

__concat()__方法可以在现有数组全部元素基础上创建一个新的数组。它首先会创建一个当前数组的副本，然后再把它的参数添加到副本末尾，最后返回这个新构建的数组。**如果参数是一个或多个数组，concat()会把数组的每一项添加到结果数组**。

```js
let colors = ["red", "green", "blue"]
let colors2 = colors.concat("yellow", ["black", "brown"])

console.log(colors2)  // ["red", "green", "blue", "yellow", "black", "brown"]
```

__slice()__方法用于创建一个包含原始数组中一个或多个元素的新数组。slice()方法可以接收一个或两个参数：返回元素的开始索引和结束索引**(不包含)**，不过不提供第二个参数，则默认从开始索引取到末尾。

```js
let colors = ["red", "green", "blue"]
let colors2 = colors.slice(1)
let colors3 = colors.slice(1,2)
console.log(colors2)   // ["green", "blue"]
console.log(colors3)   // ["green"]
```

__splice()__方法可以改变数组本身。它接收三个参数，第一个参数指定删除元素的开始位置，第二个参数指定删除元素的数量，第三个以及之后的参数指定在开始位置处要插入的元素。

```js
let colors = ["red", "green", "blue"]
let removed = colors.splice(0, 1)    // 在位置0处删除1个元素
console.log(removed)    // "red"
console.log(colors)     // ["green", "blue"]

colors.splice(1, 0 ,"a", "b")    // 在位置1处删除0个元素，并插入两个元素
console.log(colors)    // ["green", "a", "b", "blue"]
```



### 6.2.12 搜索和位置方法

ECMAScript提供两类搜索数组的方法：按严格相等搜索和按断言函数搜索。

#### 1. 严格相等

ECMAScript提供了3个严格相等的搜索方法：**indexOf()**、**lastIndexOf()**和**includes()**（ES7）。

这三个方法都接收两个参数：要查找的元素和一个可选的起始搜索位置。 

__indexOf()__和__includes()__方法从数组前头开始向后搜索，而__lastIndexOf()__方法相反。

__indexOf()__和__lastIndexOf()__返回查找的元素在数组中的位置，如果没找到则返回-1.

**includes()**返回布尔值，表示是否找到一个指定元素匹配的项。

**三个方法在比较时会使用全等（===）比较**。

```js
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]

console.log(numbers.indexOf(4))     // 3
console.log(numbers.lastIndexOf(4)) // 5
console.log(numbers.includes(4))    // true

console.log(numbers.indexOf(4, 4))     // 5
console.log(numbers.lastIndexOf(4, 4)) // 3
console.log(numbers.includes(4, 7))    // false
```

#### 2. 断言函数

ECMAScript也允许按照定义的断言函数搜索数组，每个索引都会调用这个函数。

断言函数接收3个参数：元素、索引和数组本身。元素指的是数组中当前搜索的元素。

__find()__和__findIndex()__方法使用了断言函数。__find()__返回第一个匹配的元素，__findIndex()__返回第一个匹配元素的索引。

```js
let nums = [5, 4, 2, 10]
let tenIndex = nums.findIndex(function(v, i, arr){
  return v === 10  
})

console.log(tenIndex)    // 3
```



### 6.2.13 迭代方法

ECMAScript为数组定义了5个迭代方法。每个方法接收两个参数：以每一项为参数运行的函数、以及可选的作为函数运行上下文的作用域对象（影响函数中this的值）。传给每个方法的函数接收3个参数：数组元素、元素索引和数组本身。这5个迭代方法如下：

- __every()__：对数组每一项都运行传入的函数，如果对每一项都返回true，则这个方法返回true。
- __filter()__：对数组每一项都运行传入的函数，函数返回true的项会组成数组之后返回。
- __forEach()__：对数组每一项都运行传入的函数，无返回值。
- __map()__：对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。
- __some()__：对数组每一项都运行传入的函数，如果有一项函数返回true，则这个方法返回true，否则返回false

```js
let numbers = [1, 2, 3, 4]
let newNums = numbers.map(function(v, i, arr){
    return v ** 2
})
console.log(newNums)    // [2, 4, 6, 8]
```



### 6.2.14 归并方法

ECMAScript为数组提供了两个归并方法：__reduce()__和__reduceRight()__。

这两个方法接收两个参数：第一个参数为初始值，第二个为当前值。每轮循环的返回值都会作为下轮循环的初始值，最后一轮循环的返回值即最终的返回值。

```js
let values = [1, 2, 3, 4]
let sum = values.reduce(function(pre, cur){
    // 第一次循环，pre为1，cur为2
    return pre + cur
})
console.log(sum)   // 10
```

如果不提供第二个参数，则默认第一次初始值为第一个元素，当前值则为第二个元素。

如果提供第二个参数，则默认第一次循环的初始值为第二个参数，当前值为第一个元素。

```js
let values = [1, 2, 3, 4]
let sum = values.reduce(function(pre, cur){
    // 第一次循环，pre为0，cur为1
    return pre + cur
}, 0)
console.log(sum)   // 10
```

__reduceRight()__和__reduce()__方法的差别只是方向相反一下。



### 6.3 定型数组（typed array）

暂时跳过。



### 6.4 Map

作为ECMAScript的新增特性，Map()是一种行的集合类型，为这门语言带来了真正的键/值存储机制。Map的大多数特性都可以通过Object类型实现，但二者之间还是存在一些细微的差别。

### 6.4.1 基本API

使用new关键字和Map构造函数可以创建一个空映射：

```js
const m = new Map()
```

如果想在创建的同时初始化实例，可以给Map构造函数传入一个可迭代对象，需要包含键/值对数组。

```js
const m1 = new Map([
    ["key1", "val1"],
    ["key2", "val2"],
    ["key3", "val3"]
])

console.log(m1.size)  // 3
```

可以使用__set()__方法添加键/值对。可以使用__get()__和__has()__进行查询，可以通过**size**属性获取映射中的键/值对的数量。使用__delete()__和__clear()__删除值。

```js
const m = new Map()

console.log(m.has("name"))   // false
console.log(m,size)          // 0

m.set("name", "Ryan")

console.log(m.has("name"))   // true
console.log(m.get("name"))   // "Ryan"

m.delete("name")
console.log(m.size)          // 0 
```



> Map内部使用**SameValueZero**比较操作，NaN与NaN认为是同一个值，-0 与 +0也认为是一个值。



### 6.4.2 顺序与迭代

Map的默认迭代器返回**entries()**方法取得的值，也就是以插入顺序生成[key, value]形式的数组。

```js
const m1 = new Map([
    ["key1", "val1"],
    ["key2", "val2"],
    ["key3", "val3"]
])

for(let pair of m1){
    console.log(pair)
}

// ["key1", "val1"]
// ["key2", "val2"]
// ["key3", "val3"]
```

__keys()__和__values()__分别返回以插入顺序生成键和值的迭代器。

```js
const m1 = new Map([
    ["key1", "val1"],
    ["key2", "val2"],
    ["key3", "val3"]
])

for(let key of m1.keys()){
    console.log(key)
}

// "key1"
// "key2"
// "key3"

for(let value of m1.values()){
    console.log(value)
}

// "val1"
// "val2"
// "val3"
```



### 6.4.3 选择object还是Map

如果代码涉及大量删除操作，选择Map，其它情况，Map与object差距不大，一般来说Map内存占用更少。



