# 第七章：迭代器与生成器

## 7.1 理解迭代

在JavaScript中，计数循环就是一种最简单的迭代：

```js
for(let i = 1; i <= 10; i ++){
    console.log(i)
}
```

**循环**是迭代机制的基础，这是因为它可以指定迭代的次数，以及要执行的操作，迭代的顺序是预先定义好的。

迭代会在一个有序集合上进行（比如数组）：

```js
let collection = ['foo', 'bar', 'baz']

for(let index = 0; index < collection.length; index ++){
    console.log(collection[index])
}
```

然而这种循环并不理想

- **迭代之前需要事先知道如何使用数据结构**。比如数组必须使用[]操作符获取数组中的一个元素。
- **遍历顺序并不是数据结构固有的**。通过递增索引来访问数据是特定于数组类型的方式，并不适用于其它数据结构。

随着代码量的增加，代码会变得愈发混乱。JavaScript在ECMAScript 6以后支持了**迭代器模式**，作为这些问题的解决方案。



## 7.2 迭代器模式

**迭代器模式**描述了一个方案，即可以把有些结构称为“可迭代对象”，因为它们实现了Iterable接口，而且可以通过Iterable接口生成的**迭代器**消费。

实现Iterable接口要求具备两种能力

- **支持迭代的自我识别能力**：对象必须暴露Symbol.iterator属性，将这个属性作为“默认迭代器”
- **创建具有Iterator接口的对象的能力**：即默认迭代器（Symbol.iterator）必须返回一个新的迭代器。



看到这里可能会有点懵，我尽量用简单的语言描述。

数组就是一个**可迭代对象**：

```js
let arr = [1, 2, 3]
```

它实现了Iterable接口：

```js
let arr = [1, 2, 3]
console.log(arr[Symbol.iterator])   // values() { [native code] }
```

Iterable接口可以生成一个具有Iterator接口的对象（我们叫它**迭代器**）

```js
let arr = [1, 2, 3]
console.log(arr[Symbol.iterator])   // values() { [native code] }
console.log(arr[Symbol.iterator]()) // Array Iterator {}
```

接下来讲什么是具有Iterator接口的对象（迭代器）。

迭代器是一种一次性使用的对象，迭代器使用**next**()方法在可迭代对象中遍历数据。每次成功调用**next**()，都会返回一个IteratorResult对象，这个对象包含两个属性：done和value，done是一个布尔值，表示是否可以再次调用**next**()，value包含可迭代对象的下一个值。done为true时，value为undefined，意为“耗尽”。

```js
let arr = ['foo', 'bar']

let iter = arr[Symbol.iterator]()

console.log(iter) // Array Iterator{}

// 执行迭代
console.log(iter.next()) // { done: false, value: 'foo' }
console.log(iter.next()) // { done: false, value: 'foo' }
console.log(iter.next()) // { done: true, value: undefined }
```

为了更方便地使用迭代器，我们可以使用**接收可迭代对象的原生语言特性**

如 **for-of** 循环：

```js
let arr = [1, 2, 3, 4]
for(let num of arr){
    console.log(arr)
}
// 1
// 2
// 3
// 4
```

其他接收可迭代对象的原生语言特性：

- for-of 循环
- 数组解构
- 扩展操作符
- Array.from()
- 创建集合（Set）
- 创建映射（Map）
- Promise.all()
- Promise.race()
- yield*操作符

我在这里实现一个简单的可迭代对象，帮助大家更好地理解迭代器模式。

```js
// 实现了Iterable接口的对象，并且这个接口返回一个迭代器
let iterable = {
    [Symbol.iterator](){			
        let count = 0
        // 返回了一个具有Iterator接口的对象（迭代器）
        return {
            next(){
                /*
                  每次调用next，count的值会加一，当count等于5的时候停止。
                */
                if( count ++ < 5){
                    return {
                        done: false,
                        value: count,
                    }
                }else{
                    return {
                        done: true,
                        value: undefined
                    }
                }
            }
        }         
    }
}

for(let num of iterable){
    console.log(num)
}
// 1
// 2
// 3
// 4
// 5

let arr = [...iterable]
console.log(arr)   // [1, 2, 3, 4, 5]
```

**按照我自己的理解做一个总结：迭代器模式，就是可以让本不具备迭代能力的对象，具备可迭代的能力。最直观的体现就是能在for-of循环中直接遍历对象。**



## 7.3 生成器

生成器是ECMAScript6新增的一个极为灵活的结构，拥有一个函数块内暂停和恢复代码执行的能力。可以用作自定义迭代器。

### 7.3.1 生成器基础

生成器是一个函数，在函数名前面加一个星号（*）表示它是一个生成器：

```js
function * generator(){
    
}
```

> 箭头函数不能用来定义生成器

调用生成器函数会产生一个**生成器对象**，这个对象也实现了Iterator接口。

生成器对象一开始处于**暂停执行**（suspended）的状态，调用**next**()方法，调用这个方法会让生成器开始或恢复执行。

next()方法的返回值类似于迭代器，有一个done属性和一个value属性。函数体为空的生成器函数中间不会停留，调用一次**next**()就会到达**done: true**状态。

```js
function * generator(){
    
}

let generatorObj = generator()

console.log(generatorObj)           // generator {<suspended>}
console.log(generatorObj.next())    // {value: undefined, done: true}
```

value属性是生成器的返回值，默认为undefined，可以通过生成器函数的返回值指定：

```js
function * generator(){
    return 'foo'
}

let generatorObj = generator()

console.log(generatorObj)           // generator {<suspended>}
console.log(generatorObj.next())    // {value: 'foo', done: true}
```

生成器只会在初次调用next()方法后开始执行：

```js
function * generator(){
    console.log(1)
}

let generatorObj = generator()

console.log(generatorObj)           // generator {<suspended>}
console.log(generatorObj.next())    // 1
```

生成器对象实现了Iterable接口，它们默认的迭代器是自引用的：

```js
function * generator(){
    console.log(1)
}

let generatorObj = generator()

console.log(generatorObj === generatorObj[Symbol.iterator]())   // true
```



### 7.3.2 通过yield中断执行

生成器函数在遇到yield关键字之前 会正常执行，但遇到这个关键字后，执行会停止，函数作用域的状态会被保留。必须使用**next**()方法恢复执行。

yield后跟的值会出现在**next**()方法返回的对象里（value属性）。

通过yield关键字暂停的生成器函数会处在**done: false**状态。

```js
function * generator(){
    yield 1
    yield 2
    yield 3
}

let generatorObj = generator()

console.log(generatorObj.next())    // {value: 1, done: false}
console.log(generatorObj.next())    // {value: 2, done: false}
console.log(generatorObj.next())    // {value: 3, done: false}
console.log(generatorObj.next())    // {value: undefined, done: true}
```

生成器函数生成的生成器对象不会干扰其他的生成器对象：

```js
function * generator(){
    yield 1
    yield 2
    yield 3
}

let generatorObj1 = generator()
let generatorObj2 = generator()

console.log(generatorObj1.next())    // {value: 1, done: false}
console.log(generatorObj2.next())    // {value: 1, done: false}
```

yield只能在生成器函数内使用，在其他地方会抛出错误。



#### 1. 生成器对象作为可迭代对象

可以将生成器对象作为可迭代对象使用（因为它实现了iterable接口）：

```js
function * generator(){
    yield 1
    yield 2
    yield 3
}

for(let num of generator()){
    console.log(num)
}
// 1
// 2
// 3
```



#### 2. 使用yield实现输入和输出

yield 关键字还可以作为函数的中间参数使用。并且在next()函数中传递的参数会成为yield的返回值。

第一次调用next()传入的值不会被使用，因为这次调用是为了开始执行生成器函数：

```js
function * generator(){
    console.log('start')
    let a = yield 1
    console.log(a)
    let b = yield 2
    console.log(b)
}

let generatorObj = generator()

console.log(generatorObj.next().value)  // 'start'
console.log(generatorObj.next('a').value)  // 'a'
console.log(generatorObj.next('b').value)  // 'b'
```



#### 3. 产生可迭代对象

可以使用星号增强yield的行为，让它能够迭代一个可迭代对象。

```js
function * generator(){
    yield* [1, 2, 3]
}

for(let num of generator()){
    console.log(num)
}

// 1
// 2
// 3
```



### 7.3.3 生成器作为默认迭代器

生成器很适合用作实现对象的iterable接口。

改写我们先前的例子：

```js
// 实现了Iterable接口的对象，并且这个接口返回一个迭代器
let iterable = {
    // 修改为生成器函数
    *[Symbol.iterator](){			
        let count = 0
        // 循环，并且使用yield输出值。
        while(count ++ < 5){
            yield count
        }
    }
}

for(let num of iterable){
    console.log(num)
}
// 1
// 2
// 3
// 4
// 5

let arr = [...iterable]
console.log(arr)   // [1, 2, 3, 4, 5]
```

代码变得简洁许多。