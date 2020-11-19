# 第八章：对象、类与面向对象编程

ECMA-262将对象定义为**一组属性的无序集合**。可以把对象想象成一张散列表，内容就是一组键值对。

## 8.1 理解对象

### 8.1.1 属性的类型

ECMA-262使用一些内部特性来描述属性的特征。为了将某个特性标识为内部特性，规范会用两个中括号把特性的名称括起来，比如[[Enumerable]]。

属性分两种：数据属性和访问器属性。

#### 1. 数据属性

数据属性有4个特性描述它们的行为

- [[Configurable]]：表示是否可以通过delete删除并重新定义，是否可以修改它的特性，以及是否可以把它改为访问器属性。
- [[Enumerable]]：表示属性是否可以通过for-in循环返回。
- [[Writable]]：表示属性的值是否可以被修改。
- [[Value]]：表示属性实际的值。

当属性显示添加到对象后，[[Configurable]]，[[Enumerable]]，[[Writable]]都会被设置为true，[[Value]]属性会被设置为指定的值，如：

```js
let person = {
    name: "attackonryan"
}

console.log(Object.getOwnPropertyDescriptor(person, 'name'))
// {value: "attackonryan", writable: true, enumerable: true, configurable: true}
```

要修改属性的默认特性，就必须使用**Object.defineProperty**()方法。这个方法接收3个参数：要添加属性的对象、属性的名称和一个描述符对象。

描述符对象上的属性可以包含：**configurable**、**enumerable**、**writable**和**value**，与特性名一一对应。

```js
let person = {}
Object.defineProperty(person, 'name', {
    writable: false,
    value: "attackonryan"
})
console.log(person.name)  // "attackonryan"
person.name = "ryan"
console.log(person.name)  // "attackonryan"
```

**非严格模式下给只读的属性赋值会被忽略，但严格模式下会抛出错误**。

```js
let person = {}
Object.defineProperty(person, 'name', {
    configurable: false,
    value: "attackonryan"
})

// 抛出错误
Object.defineProperty(person, 'name', {
    configurable: false,
    value: "attackonryan"
})
```

调用**Object.defineProperty**()时，**configurable**、**enumerable**、**writable**的值如果不指定，则默认为false。



#### 2. 访问器属性

访问器属性有4个特性描述它们的行为

- [[Configurable]]：表示是否可以通过delete删除并重新定义，是否可以修改它的特性，以及是否可以把它改为访问器属性。
- [[Enumerable]]：表示属性是否可以通过for-in循环返回。
- [[Get]]：获取函数，在读取属性时调用。
- [[Set]]：设置函数，在写入属性时调用。

访问器属性不能直接定义，必须使用**Object.defineProperty**()。

```js
let person = {
    _name: "attackonryan"
}
Object.defineProperty(person, "name", {
    get(){
        return this._name
    },
    set(newVal){
        this._name = newVal
    }
})

console.log(person.name)  // "attackonryan"
person.name = "ryan"
console.log(person.name)  // "ryan"
```

获取函数和设置函数不一定都要定义，只定义获取函数意味着属性**只读**，尝试修改属性会被忽略，**严格模式下会抛出错误**。同样只有一个设置函数则意味着**不能读取**，非严格墨水下读取会返回undefined，**严格模式下会抛出错误**。



**注意：每个属性只能定义一种属性类型（数据属性 or 访问器属性）**

