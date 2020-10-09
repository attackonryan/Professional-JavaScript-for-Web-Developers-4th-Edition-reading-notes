# 第四章：变量、作用域与内存

## 4.1 原始值与引用值

ECMAScript包含两种不同类型的数据：原始值和引用值。原始值就是最简单的数据，引用值则是由多个值构成的对象。

**保存原始值的变量按值访问，操作的是储存在变量中的实际值**。

**保存引用值的变量是按引用访问的，操作的是对该对象的引用而不是对象本身**。

### 4.1.1 动态属性

略

### 4.1.2 复制值

在把值从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在位置。

引用值和原始值的区别在于，**引用值复制的值实际上是一个指针，指向堆内存中的对象**。

所以对同一个指针所指向的对象进行操作，变化会在两个变量中显示出来。

```js
let obj1 = new Object()
let obj2 = obj1         // 把指针赋给了obj2
let obj1.name = "ryan"
console.log(obj2.name)  // 因为指向同一个对象，name为"ryan"
```



### 4.1.3 传递参数

**ECMAScript中所有函数的参数都是按值传递的**。

```js
function addTen(num){
    num += 10
    return num
}

let count = 20
let result = addTen(count)

console.log(count)    // 20，没有变化
console.log(result)   // 30
```

num实际上是一个局部变量，这个num保存了实参的值，仅仅是一个副本，因此不会影响外部变量。

```js
function setName(obj){
    obj.name = "ryan"
}

let person = new Object()

setName(person)
console.log(person.name)    // "ryan"
```

这里的person是一个引用值，obj参数依然遵循按值传递的规则，但是obj是一个指针副本，**因此对指针所指向的对象进行修改，实际上是通过引用进行修改**，变化会表现在外部。

```js
function setName(obj){
    obj.name = "ryan"
    obj = new Object()
    obj.name = "Titan"
}

let person = new Object()

setName(person)
console.log(person.name)    // "ryan"
```

现在setName()函数内部会将obj变量重新赋值一个新的对象，但是这个变化不会反应在外部。原因就是，obj实际上只是一个指针副本，覆盖这个指针并不会影响这个指针所指向的对象。



### 4.1.4 确定类型

typeof无法区分null或对象。typeof虽然对原始值很有用，但它对引用值的作用不大。

为解决这个问题，ECMAScript提供了instanceof操作符，语法如下：

```js
result = variable instanceof constructor
```

如果变量时给定构造函数的实例，则instanceof操作符返回true。

> 所有引用值都是Object的实例，所以通过instanceof操作符检测任何引用值和Object构造函数都会返回true