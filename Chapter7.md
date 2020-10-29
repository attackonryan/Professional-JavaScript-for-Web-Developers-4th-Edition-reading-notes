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