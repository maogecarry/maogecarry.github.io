---
title: JavaScript重难点实例精讲笔记
date: 2021-09-22 17:38:09
tags:
	 		- 'JavaScript'
---

在JavaScript中，每种数据类型都会使用3bit表示。

-  000表示Object类型的数据。
-  001表示Int类型的数据。
-  010表示Double类型的数据。
-  100表示String类型的数据。
-  110表示Boolean类型的数据。
<!--more-->

由于null代表的是空指针，大多数平台中值为0x00，因此null的类型标签就成了0，所以使用typeof运算符时会判断为object类型，返回‘object’

## 逗号运算符

1. ###  在for循环中批量执行表达式

2. ### 用于交换变量，无须额外变量

   一般情况下

   ```js
   let a = 'a'
   let b = 'b'
   let c
   c = a 
   a = b
   b = c
   ```

   使用逗号运算符

   ```js
   let a = 'a'
   let b = 'b'
   // 方案1
   a = [b,b = a][0];
   // 方案2
   a = [b][b = a, 0];
   ```

   在方案1中，前一部分[b, b = a]是一个一维数组，数组第二项值是b = a，实际会将a值赋给b，然后返回“'a'”，因此数组最终的值为['b', 'a']，然后取索引0的值为'b'，赋给变量a，最终实现a = 'b', b = 'a'。
   在方案2中，前一部分[b]是一个一维数组，后一部分[b = a, 0]，实际会先执行b = a，将a值赋给b，然后返回“0”，因此后一部分实际是修改了b的值并返回索引“0”，最终是a = [b][0]，即a = b，实现了a与b的交换。

3. ### 用于简化代码

   因为逗号运算符可以使多个表达式先后执行，并且返回最后一个表达式的值

   ```js
   if(x) {
   	foo()
   	return bar()
   }else {
   	return 1
   }
   // 使用逗号运算符简写后
   x ? (foo(),bar()) : 1
   ```

4. ### 用小括号保证逗号运算符的优先级

   在所有的运算符中，逗号运算符的优先级是最低的，因此对于某些涉及优先级的问题，我们需要使用到小括号，将含有逗号运算符的表达式括起来。

## toString()函数与valueOf()函数

1. ### toString()函数

   toString()函数的作用是把一个逻辑值转换为字符串，并返回结果。

2. ### valueOf()函数

   valueOf()函数的作用是返回最适合引用类型的原始值，如果没有原始值，则会返回引用类型自身。

## JavaScript常用的判空方法

对变量进行取反，然后判断是否为true

```js
if(!x){
	return true
}
```

### 判断变量为空对象

1. #### 判断变量为null或者undefined

   判断一个变量是否为空时，可以直接将变量与null或者undefined相比较

   ```js
   if(obj == null) {} // 可以判断null或者undeﬁned
   
   if(obj === undeﬁned) {} // 只能判断undeﬁned
   ```

2. #### 判断变量是否为空对象{}

   判断一个变量是否为空对象时，可以通过for...in语句遍历变量的属性，然后调用hasOwnProperty()函数，判断是否有自身存在的属性，如果存在则不为空对象，如果不存在自身的属性（不包括继承的属性），那么变量为空对象。

   ```js
   function isEmpty(obj){
       for(let key in obj){
           if(obj.hasOwnProperty(key)){
               return false
           }
       }
       return true
   }
   // 定义空的对象字面量
   var o = {};
   
   function Person() {}
   Person.prototype.name = 'kingx';
   // 通过new操作符获取对象
   var p = new Person();
   
   console.log(isEmpty(o));  // true
   console.log(isEmpty(p));  // true
   ```

3. #### 判断变量为空数组

   ```js
   arr instanceof Array && arr.length === 0
   ```

4. #### 判断变量为空字符串

   判断变量是否为空字符串时，可以直接将其与空字符串相比较，或者调用trim()函数去掉前后的空格，然后判断字符串的长度。

   ```js
   str === '' || str.trim().length === 0
   ```

5. #### 判断变量为0或者NaN

   ```js
   !(Number(num) && num) == true; 
   ```

## 引用数据类型

在JavaScript中，如果函数没有return值，则默认return this。

创建一个对象实例，new操作符做了3件事情。

```js
function Cat(name, age) {
   var Cat = {};
   Cat.name = name;
   Cat.age = age;
   return Cat;
}

let cat = new Cat()
```

```js
// 相当于
let cat = {} 
cat.__proto__ = Cat.prototype
Cat.call(cat)
```

第一行：创建一个空对象。

第二行：将空对象的__proto__属性指向Cat对象的prototype属性。

第三行：将Cat()函数中的this指向cat变量。

自定义一个类似new功能的函数

```js
function Cat(name, age){
    this.name = name;
    this.age = age;
}

Cat.prototype.sayHi = function(){
    console.log('hi')
}

function New(){
    let obj = {}
    obj.__proto__ = Cat.prototype // 核心代码，用于继承
    let res = Cat.apply(obj,arguments)
    return typeof res === 'object' ? res : obj
}
console.log(New('mimi', 18).sayHi());  //hi
```

## Object类型的静态函数

### object.create()函数

属性描述符的格式如下所示

```js
propertyName: {
   value: '', // 设置此属性的值
   writable: true, // 设置此属性是否可写入；默认为false：只读
   enumerable: true, // 设置此属性是否可枚举；默认为false：不可枚举
   conﬁgurable: true // 设置此属性是否可配置，如是否可以修改属性的特性及是否可以删除属性；默认为false
}
```

```js
// 建立一个自定义对象，设置name和age属性
var obj = Object.create(null, {
   name: {
       value: 'tom',
       writable: true,
       enumerable: true,
       conﬁgurable: true
   },
   age: {
       value: 22
   }
});
console.log(obj.name); // tom
console.log(obj.age); // 22
obj.age = 28;
console.log(obj.age); // 22 ：age属性的writable默认为false，此属性为只读
for (var p in obj) {
   console.log(p); // name ：只输出name属性；age属性的enumerable默认为false，不能通过for...in 枚举
}
```

### Array类型

#### 判断一个变量是数组还是对象

1. ##### instanceof运算符

   instanceof运算符用于通过查找原型链来检测某个变量是否为某个类型数据的实例，使用instanceof运算符可以判断一个变量是数组还是对象。

   ```js
   let a= [1, 2, 3]
   console.log(a instanceof Array) // true
   console.log(a instanceof Object) // true 
   let b = {name: 'kingx'};
   console.log(b instanceof Array);  // false
   console.log(b instanceof Object); // true
   ```

   封装函数判断变量是数组还是对象

   ```js
   // 判断变量是数组还是对象
   function getDataType(o){
       if(o instance of Array){
           return 'Array'
       }else if(o instanceof Object){
           return 'Object'
       }else{
           return 'param is not object type'
       }
   }
   ```

2. #### 判断构造函数

   判断一个变量是否是数组或者对象，从另一个层面讲，就是判断变量的构造函数是Array类型还是Object类型。因为一个对象的实例都是通过构造函数生成的，所以，我们可以直接判断一个变量的constructor属性。

```js
let a = [1, 2, 3]
console.log(a.constructor === Array) // true
console.log(a.constructor === Object) // false

let b = {name: 'kingx'}
console.log(a.constructor === Array) // false
console.log(a.constructor === Object) // true
```

