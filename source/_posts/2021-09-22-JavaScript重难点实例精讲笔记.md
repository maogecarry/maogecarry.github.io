---
title: JavaScript重难点实例精讲笔记
date: 2021-09-22 17:38:09
tags:
	 		- 'JavaScript'
---

在 JavaScript 中，每种数据类型都会使用 3bit 表示。

- 000 表示 Object 类型的数据。
- 001 表示 Int 类型的数据。
- 010 表示 Double 类型的数据。
- 100 表示 String 类型的数据。
- 110 表示 Boolean 类型的数据。
<!--more-->

由于 null 代表的是空指针，大多数平台中值为 0x00，因此 null 的类型标签就成了 0，所以使用 typeof 运算符时会判断为 object 类型，返回‘object’

## 逗号运算符

1. ### 在 for 循环中批量执行表达式

2. ### 用于交换变量，无须额外变量

   一般情况下

   ```js
   let a = 'a';
   let b = 'b';
   let c;
   c = a;
   a = b;
   b = c;
   ```

   使用逗号运算符

   ```js
   let a = 'a';
   let b = 'b';
   // 方案1
   a = [b, (b = a)][0];
   // 方案2
   a = [b][((b = a), 0)];
   ```

   在方案 1 中，前一部分[b, b = a]是一个一维数组，数组第二项值是 b = a，实际会将 a 值赋给 b，然后返回“'a'”，因此数组最终的值为['b', 'a']，然后取索引 0 的值为'b'，赋给变量 a，最终实现 a = 'b', b = 'a'。
   在方案 2 中，前一部分[b]是一个一维数组，后一部分[b = a, 0]，实际会先执行 b = a，将 a 值赋给 b，然后返回“0”，因此后一部分实际是修改了 b 的值并返回索引“0”，最终是 a = [b][0]，即 a = b，实现了 a 与 b 的交换。

3. ### 用于简化代码

   因为逗号运算符可以使多个表达式先后执行，并且返回最后一个表达式的值

   ```js
   if (x) {
     foo();
     return bar();
   } else {
     return 1;
   }
   // 使用逗号运算符简写后
   x ? (foo(), bar()) : 1;
   ```

4. ### 用小括号保证逗号运算符的优先级

   在所有的运算符中，逗号运算符的优先级是最低的，因此对于某些涉及优先级的问题，我们需要使用到小括号，将含有逗号运算符的表达式括起来。

## toString()函数与 valueOf()函数

1. ### toString()函数

   toString()函数的作用是把一个逻辑值转换为字符串，并返回结果。

2. ### valueOf()函数

   valueOf()函数的作用是返回最适合引用类型的原始值，如果没有原始值，则会返回引用类型自身。

## JavaScript 常用的判空方法

对变量进行取反，然后判断是否为 true

```js
if (!x) {
  return true;
}
```

### 判断变量为空对象

1. #### 判断变量为 null 或者 undefined

   判断一个变量是否为空时，可以直接将变量与 null 或者 undefined 相比较

   ```js
   if (obj == null) {
   } // 可以判断null或者undeﬁned

   if (obj === undeﬁned) {
   } // 只能判断undeﬁned
   ```

2. #### 判断变量是否为空对象{}

   判断一个变量是否为空对象时，可以通过 for...in 语句遍历变量的属性，然后调用 hasOwnProperty()函数，判断是否有自身存在的属性，如果存在则不为空对象，如果不存在自身的属性（不包括继承的属性），那么变量为空对象。

   ```js
   function isEmpty(obj) {
     for (let key in obj) {
       if (obj.hasOwnProperty(key)) {
         return false;
       }
     }
     return true;
   }
   // 定义空的对象字面量
   var o = {};

   function Person() {}
   Person.prototype.name = 'kingx';
   // 通过new操作符获取对象
   var p = new Person();

   console.log(isEmpty(o)); // true
   console.log(isEmpty(p)); // true
   ```

3. #### 判断变量为空数组

   ```js
   arr instanceof Array && arr.length === 0;
   ```

4. #### 判断变量为空字符串

   判断变量是否为空字符串时，可以直接将其与空字符串相比较，或者调用 trim()函数去掉前后的空格，然后判断字符串的长度。

   ```js
   str === '' || str.trim().length === 0;
   ```

5. #### 判断变量为 0 或者 NaN

   ```js
   !(Number(num) && num) == true;
   ```

## 引用数据类型

在 JavaScript 中，如果函数没有 return 值，则默认 return this。

创建一个对象实例，new 操作符做了 3 件事情。

```js
function Cat(name, age) {
  var Cat = {};
  Cat.name = name;
  Cat.age = age;
  return Cat;
}

let cat = new Cat();
```

```js
// 相当于
let cat = {};
cat.__proto__ = Cat.prototype;
Cat.call(cat);
```

第一行：创建一个空对象。

第二行：将空对象的**proto**属性指向 Cat 对象的 prototype 属性。

第三行：将 Cat()函数中的 this 指向 cat 变量。

自定义一个类似 new 功能的函数

```js
function Cat(name, age) {
  this.name = name;
  this.age = age;
}

Cat.prototype.sayHi = function () {
  console.log('hi');
};

function New() {
  let obj = {};
  obj.__proto__ = Cat.prototype; // 核心代码，用于继承
  let res = Cat.apply(obj, arguments);
  return typeof res === 'object' ? res : obj;
}
console.log(New('mimi', 18).sayHi()); //hi
```

## Object 类型的静态函数

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
    conﬁgurable: true,
  },
  age: {
    value: 22,
  },
});
console.log(obj.name); // tom
console.log(obj.age); // 22
obj.age = 28;
console.log(obj.age); // 22 ：age属性的writable默认为false，此属性为只读
for (var p in obj) {
  console.log(p); // name ：只输出name属性；age属性的enumerable默认为false，不能通过for...in 枚举
}
```

### Array 类型

#### 判断一个变量是数组还是对象

1. ##### instanceof 运算符

   instanceof 运算符用于通过查找原型链来检测某个变量是否为某个类型数据的实例，使用 instanceof 运算符可以判断一个变量是数组还是对象。

   ```js
   let a = [1, 2, 3];
   console.log(a instanceof Array); // true
   console.log(a instanceof Object); // true
   let b = { name: 'kingx' };
   console.log(b instanceof Array); // false
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

   判断一个变量是否是数组或者对象，从另一个层面讲，就是判断变量的构造函数是 Array 类型还是 Object 类型。因为一个对象的实例都是通过构造函数生成的，所以，我们可以直接判断一个变量的 constructor 属性。

```js
let a = [1, 2, 3];
console.log(a.constructor === Array); // true
console.log(a.constructor === Object); // false

let b = { name: 'kingx' };
console.log(a.constructor === Array); // false
console.log(a.constructor === Object); // true
```

每个变量都会有一个**proto**属性，表示的是隐式原型。一个对象的隐式原型指向的是构造该对象的构造函数的原型，用数组举例。

```js
[].__proto__ === [].constructor.prototype // true
[].__proto__ === Array.prototype; // true
```

上面直接通过 constructor 属性判断的语句也可以改写成下面的形式

```js
let a = [1, 2, 3];
console.log(a.__proto__.constructor === Array); // true
console.log(a.__proto__.constructor === Object); // false
```

同时，也可以将代码进行封装，得到一个判断变量是数组还是对象的通用函数。

```js
// 判断变量是数组还是对象
function getDataType(o) {
   // 获取构造函数
   var constructor = o._ _proto_ _.constructor || o.constructor;
   if (constructor === Array) {
       return 'Array';
   } else if (constructor === Object) {
       return 'Object';
   } else {
       return 'param is not object type';
   }
}
```

4. ##### Array.isArray()函数

   Array.isArray()函数只能判断变量是否为数组。

### reduce()函数累加器处理数组元素

reduce()函数的语法如下所示

```js
arr.reduce(callback[, initialValue]);
```

initialValue 用作 callback 的第一个参数值，如果没有设置，则会使用数组的第一个元素值。

1. #### 求数组每个元素相加的和

   ```js
   let arr = [1, 2, 3, 4, 5];
   let sum = arr.reduce((accumulator, currentValue) => {
     return accumulator + currentValue;
   }, 0);
   console.log(sum);
   ```

2. #### 统计数组中每个元素出现的次数

   ```js
   let countOccurrences = (arr) => {
     return arr.reduce((accumulator, currentValue) => {
       accumulator[currentValue]
         ? accumulator[currentValue]++
         : (accumulator[currentValue] = 1);
       return accumulator;
     }, {});
   };
   // 测试代码
   countOccurrences([1, 2, 2, 3, 4, 4]);
   ```

## 函数的定义与调用

函数的定义大体可以分为3种，分别是函数声明，函数表达式和Function构造函数。

### 函数声明

函数声明式直接使用function关键字接一个函数名，函数名后是接收函数的形参。

```js
// 函数声明式
function sum(num1,num2){
	return num1 + num2
}
```

### 函数表达式

函数表达式的形式类似普通变量的初始化，只不过这个变量初始化的值是一个函数。

foo是函数名称，它实际是函数内部的一个局部变量，在函数外部是无法直接调用的。

```js
// 函数表达式 属于匿名函数表达式
let sum = function (num1,num2){
	return num1 + num2
}
console.log(sum(1, 3)) // 4
// 具有函数名的函数表达式
let sum = function foo(num, num2){
    return num1 + num2
}
```

### Function()构造函数

使用new操作符，调用Function()构造函数，传入对应的参数，也可以定义一个函数。

```js
let add = new Function('a', 'b', 'return a + b')
```

但是这种使用比较少，主要有以下两个原因。

第一个原因是Function()构造函数每次执行时，都会解析函数主体，并创建一个新的函数对象，所以当在一个循环或者频繁执行的函数中调用Function()构造函数时，效率是非常低的。

第二个原因是使用Function()构造函数创建的函数，并不遵循典型的作用域，它将一直作为顶级函数执行。所以在一个函数A内部调用Function()构造函数时，其中的函数体并不能访问到函数A中的局部变量，而只能访问到全局变量。

### 函数表达式的应用场景

主要针对函数递归与代码模块化这两个应用场景进行讲解

#### 函数递归

通过函数表达式可以定义具有名称的函数，它作为函数内部的一个局部变量，指向函数自声。

```js
// 斐波那契数列问题
function fibonacci(num){
	if(num === 1 || num === 2){
	return 1
    }
    return fibonacci(num-2) + fibonacci(num-1)
}
```

通过函数表达式实现代码

```js
// 斐波那契数列问题
let fibonacci= function(num) {
	if(num === 1 || num === 2){
	return 1
    }
    return fibonacci(num-2) + fibonacci(num-1)
}
```

#### 代码模块化

在ES6以前，JavaScript中是没有块级作用域的，但是我们可以通过函数表达式来间接地实现模块化，将特定的模块化代码封装在一个函数中，只对外暴露接口。

```js
let person = (function name() {
  let _name = '';
  return {
    getName: function () {
      return _name;
    },
    setName: function (newName) {
      _name = newName;
    },
  };
})();

person.setName('king');
console.log(person.getName());
```

#### 函数提升

对于函数声明，存在函数提升，所以即使函数的调用在函数的声明之前，仍然可以正常执行。

```js
console.log(add(1,2)) // 3
console.log(sub(5,3)) // Uncaught ReferenceError: sub is not defined
// 函数声明
function add(a1,a2){
	return a1 + a2
}
// 函数表达式
let sub = function(a1, a2){
	return a1 -a2
}
```

### 函数调用

函数调用存在5种模式，函数调用模式，方法调用模式，构造器调用模式，call()函数、apply()函数调用模式，匿名函数调用模式。

#### 函数调用模式

函数调用模式是通过函数声明或者函数表达式的方法定义函数，然后直接通过函数名调用的模式

```js
// 函数声明
function add(a1, a2){
	return a1 + a2
}
// 函数表达式
let sub = function (a1, a2){
	return a1 - a2
}
add(1 ,3)
sub(4 ,1)
```

#### 方法调用模式

方法调用模式会优先定义一个对象obj，然后在对象内部定义值为函数的属性property，通过对象obj.property()来进行函数的调用。

```js
// 定义对象
let obj ={
	name: 'kingx',
	// 定义getName属性，值为一个函数
	getName: function(){
		return this.name
	}
}
obj.getName() // 通过对象进行调用
```

函数还可以通过中括号来调用，即对象名['函数名']，那么上面的实例代码可以改写为

```js
let obj ={
	name:'kingx',
	getName: function(){
        console.log(this.name)
	},
	setName:function(name){
        this.name=name;
        return this //在函数内部返回函数对象本身
	}
}
obj.setName('kingx2').getName() //链式函数调用
```

#### 构造器调用模式

构造器调用模式会定义一个函数，在函数中定义实例属性，在原型上定义函数，然后通过new操作符生成函数的实例，再通过实例调用原型上定义的函数。

```js
// 定义函数对象
function Person(name){
	this.name = name
}
// 原型上定义函数
Person.prototype.getName = function (){
	return this.name;
}
// 通过new操作符生成实例
let p = new Person('kingx')
// 通过实例进行函数的调用
p.getName()
```

#### call()函数，apply()函数调用模式

通过call函数或者apply函数可以改变函数执行的主体，使得某些不具有特定函数的对象可以直接调用该特定函数。

```js
// 定义一个函数
function sum(num1, num2){
    return num1 + num2;
}
// 定义一个对象
let person = {}
// 通过call()函数与apply()函数调用sum()函数
sum.call(person,1, 2)
sum.apply(person,[1, 2])
```

通过call()函数与apply()函数，使得没有sum()函数的person对象也可以直接调用sum()函数。

#### 匿名函数调用模式

匿名函数，就是没用函数名称的函数。匿名函数的调用有两种方式，一种是通过函数表达式定义函数，并赋给变量，通过变量进行调用。另一种是使用小括号()将匿名函数括起来，然后在后面使用小括号，传递对应的参数，进行调用。

```js
// 通过函数表达式定义匿名函数，并赋给变量sum
let sum = function(num1, num2) {
    return num1 + num2
}
// 通过sum()函数进行匿名函数调用
sum(1, 2)
// 小括号括起来
(function(num1, num2){
    return num1 + num2
})(1, 2) //3
```

### 函数参数

#### 形参实参

函数的参数分为两种，分别是形参和实参。

形参全称为形式参数。是在定义函数名称与函数体使用的参数，目的是用来接收调用该函数时传入的参数。

实参全称为实际参数，是在调用时传递给函数的参数，实参可以使常量、变量、表达式、函数等类型。

##### 形参和实参的区别

1. 形参出现在函数的定义中，只能在函数体内使用，一旦离开该函数则不能使用；实参出现在主调函数中，进入被调函数后，实参也将不能被访问。
2. 在强类型语言中，定义的形参和实参在数量、数据类型和顺序上要保持严格一致，否则会抛出“类型不匹配”的异常
3. 在函数调用过程中，数据传输是单向的，即只能把实参的值传递给形参，而不能把形参的值反向传递给实参。
4. 当实参是基本数据类型的值时，实际是将实参的值复制一份传递给形参，在函数运行结束时形参被释放，而是实参中的值不会变化。

### arguments对象的性质

arguments是所有函数都具有的一个内置局部变量，表示的是函数实际接收的参数，是一个类数组结构。它除了具有length属性外，不具有数组的一些常有方法。

#### 函数内部无法访问

arguments对象只能在函数内部使用，无法在函数外部访问到arguments对象。

```js
function foo(){
	console.log(arguments.length) // 3 
    function foo2(){
        console.log(arguments.length) // 0
    }
    foo2()
}

foo(1,2,3)
```

#### 可通过索引访问

arguments对象是一个类数组结构，可以通过索引访问，每一项表示对应传递的实参值，如果该项索引值不存在，则会返回'undefined'

```js
function sum(num1, num2){
    console.log(arguments[0]) // 3
    console.log(arguments[1]) // 4
    console.log(arguments[2]) // undefined
}

sum(3, 4)
```

#### 由实参决定

arguments对象的值由实参决定，而不是由定义的形参决定，形参与arguments对象占有独立的内存空间。

1.  arguments对象的length属性在函数调用的时候就已经确定，不会随着函数的处理而改变。
2.  指定的形参在传递实参的情况下，arguments对象与形参值相同，并且可以相互改变。
3.  指定的形参在未传递实参的情况下，arguments对象对应索引值返回“undefined”。
4. 指定的形参在未传递实参的情况下，arguments对象与形参值不能相互改变。

```js
function foo(a,b,c){
    console.log('arguments的长度',arguments.length); //2
    arguments[0] = 11;
    console.log('a的值', a); // 11
    b = 12;
    console.log('arguments索引为1的值',arguments[1]); //12
    arguments[2] = 3;
    console.log('c的值', c); // undefined
    c = 13;
    console.log('arguments索引为2的值', arguments[2]); // 3
  
    console.log('arguments的长度', arguments.length); // 2
}
foo(1, 2)
```

#### 特殊的arguments.callee属性

arguments对象有一个很特殊的属性callee，表示的是当前正在执行的函数，在比较时是严格相等的,但是使用arguments.callee属性会改变函数内部的this值。

```js
function foo(){
	console.log(arguments.callee === foo); //true
}
foo()
```

用法：

```js
function create(){
	return function(n) {
		if(n <= 1) return 1;
        return n * arguments.callee(n - 1)
	}
}
let result = create()(5)
```

### arguments对象的应用

#### 实参的个数判断

```js
function f(x, y, z){
    // 检查传递的参数个数是否正确
    if(arguments.length !== 3){
        throw new Error('期望传递的参数个数为3，实际传递的个数为：'+ arguments.length)
    }
    // ...do something
}
f(1, 2) //期望传递的参数个数为3，实际传递的个数为：2
```

#### 任意个数的参数处理

定义一个函数，该函数只会特定处理传递的前几个参数，对于后面的参数不论传递多少个都会统一处理，这种场景下我们可以使用arguments对象。

```js
function joinStr(sepreator){
    // arguments对象是一个类数组结构，可以通过call()函数间接调用slice()函数，得到一个数组
    let strArr = Array.prototype.slice.call(arguments, 1)
    // strArr数组直接调用join()函数
    return strArr.join(sepreator)
}
joinStr('-','orange','apple','banana')
joinStr(',','orange','apple','banana')
```

#### 模拟函数重载

函数重载表示的是在函数名相同的情况下，通过函数形参的不同参数类型或者不同参数个数来定义不同的函数。JavaScript是一门弱类型语言，没有函数重载。

```js
// 通用求和函数
function sum(){
    // 通过call()函数间接调用数组的slice()函数得到函数参数的数组
    let arr = Array.prototype.slice.call(arguments)
    // 调用数组的reduce函数进行多个值的求和
    return arr.reduce((pre,cur)=>{
        return pre + cur
    }, 0)
}
sum(1,2,3,4)
```

### 变量提升和函数提升

```js
var a = true;
foo();

function foo(){
    if(a){
        var a =10
    }
    console.log(a); // undefined
}
```

首先在全局作用域中定义一个变量a，值为true，然后调用foo()函数。foo()函数时通过函数声明定义的，会进行函数提升，因此foo()函数会正常调用。在foo()函数内部，首先判断变量a的值，由于变量a在函数内部重新通过var关键字声明了一次，因此a会出现变量提升，a会提升至foo()函数的顶部，此时a的值为undefined。那么通过if语句进行判断时，返回'false'，并未执行a=10 的赋值语句，因此最后输出'undefined'。
过程可以改为一下代码段。

```js
var a;
a = true;
function foo(){
    var a;
    if(a){
        a = 10;
    }
    console.log(a)
}
```

### 变量提升和函数提升优先级

```js
function fn() {
   console.log(typeof foo); // function
   // 变量提升
   var foo = 'variable';
   // 函数提升
   function foo() {
      return 'function';
   }
   console.log(typeof foo); // string
}
fn();  
```

变量提升的优先级要比函数提升的优先级高，因此执行过程可以改写为：

```js
funciton fn(){
    // 变量提升至函数顶部
    var foo;
    // 函数提升，但是优先级低，出现在变量声明后面，则foo是一个函数
    funciton foo(){
        return 'function'
    }
    console.log(typeof foo)
    
    foo = 'variable' // 变量赋值
    console.log(typeof foo) // string
}
fn()
```

### 变量提升和函数提升整体应用

```js
function foo(){
	var a = 1;
	function b(){
		a = 10;
		return;
		function a(){}
	}
	b();
	console.log(a) // 1
}
foo()
```

代码中首先定义一个变量a并赋值为1，然后声明一个b()函数，并在后面直接进行调用，最后输出变量a的值。

在b()函数中，赋值的变量a并未使用var关键字，因此变量a是一个全局环境的变量，而在return语句之后出现了一个变量a的函数声明，则会进行提升，执行return语句后，变量a的值仍然为1，可改写代码为：

```js
function foo(){
    // 变量a的提升
    var a;
    // 函数声明b的提升
    function b(){
        // 内部的函数声明a的提升
        function a() {}
        a = 10
        return
    }
    a = 1;
    b();
    console.log(a) // 1
}
foo()
```

### 闭包

一个拥有许多变量和绑定了这些变量执行上下文环境的表达，通常是一个函数。

闭包有两个特点

- 函数拥有的外部变量的引用，在函数返回时，该变量仍然处于活跃状态。
- 闭包作为一个函数返回时，其执行上下文环境不会被销毁，仍处于执行上下文环境中。

在JavaScript中存在一种内部函数，即函数声明和函数表达式可以位于另一个函数的函数体内，在内部函数中可以访问外部函数声明的变量，当这个内部函数在包含它们的外部函数之外被调用时，就会形成闭包。

```js
function fn() {
  var max = 10
  return function bar(x) {
    if (x > max) {
      console.log(x)
    }
  }
}

var f1 = fn()
f1(11) // 11
```

#### 闭包的优点

- 保护函数内变量的安全，实现封装，防止变量流失其他环境发生命名冲突，造成环境污染。
- 在适当的时候，可以在内存中维护变量并缓存，提高执行效率。

#### 闭包的缺点

- 消耗内存：通常来说，函数的活动对象会随着执行上下文环境一起被销毁，但是，由于闭包引用的是外部函数的活动对象，因此这个活动对象无法被销毁，这意味着，闭包比一般的函数需要消耗更多的内存。
- 泄露内存：在IE9之前，如果闭包的作用于链中存在DOM对象，则意味着该DOM对象无法被销毁，造成内存泄露。

## this使用

### this指向全局对象

当函数没有所属对象而直接调用时，this指向的是全局对象。

#### this指向所属对象

谁调用它，它就指向谁。

#### this指向对象实例

当通过new操作符调用构造函数生成对象的实例时，this指向该实例。

```js
//全局变量
let number = 10
function Person(){
    // 复写全局变量
    number = 20;
    // 实例变量
    this.number =30
}
// 原型函数
Person.prototype.getNumber = function () {
    return this.number
}
// 通过new操作符获取对象的实例
let p = new Person()
console.log(p.getNumber()) // 30
```

#### this指向call()函数、apply()函数、bind()函数调用后重新绑定的对象

```js
// 全局变量
let value = 10;
let obj = {
    value: 20
}
// 全局函数
let method = function() {
    console.log(this.value)
}
method(); // 10
method.call(obj); // 20
method.apply(obj); // 20
let newMethod = method.bind(obj)
newMethod(); // 20
```

#### 闭包中的this

函数的this变量只能被自身访问，其内部函数无法访问。因此在遇到闭包时，闭包内部的this关键字无法访问到外部函数的this变量。

## call()函数、apply()函数、bind()函数的使用与区别

### call()函数的基本使用

call()函数调用一个函数时，会将该函数的执行对象上下文改变为另一个对象。其语法为：

```js
function.call(thisAra, arg1, arg2)
```

- function为需要调用的函数。
- thisArg表示的是新的对象上下文，函数中的this将指向thisArg，如果thisArg为null或者undefined，则this会指向全局对象。
- arg1,arg2，...表示的是函数所接收的参数列表。

call函数用法实例

```js
// 定义一个add()函数
function add(x, y){
    return x + y
}
// 通过call()函数进行add()函数的调用
function myAddCall(x, y){
    // 调用add()函数的call()函数
    return add.call(this, x, y)
}
console.log(myAddCall(10, 20)) // 输出'30'
```

### apply()函数的基本使用

apply()函数的作用域与call()函数时一致的，只是在传递参数的形式上存在差别。

```js
funciton.apply(thisArg, [argsArray])
```

- function与thisArg参数与call()函数中的解释一样
- [argsArray]表示的是参数会通过数组的形式进行传递，如果argsArray不是一个有效的数组或者arguments对象，则会抛出一个TypeError异常

```js
// 定义一个add()函数
function add(x, y){
    return x + y
}
// 通过apply()函数进行add()函数的调用
function myAddApply(x, y){
    // 调用add()函数的call()函数
    return add.apply(this, [x, y])
}
console.log(myAddApply(10, 20)) // 输出'30'
```

### bind()函数的基本使用

bind()函数创建一个新的函数，在调用时设置this关键字为提供的值，在执行新函数时，将给定的参数列表作为原函数的参数序列，从前往后匹配。

```js
function.bind(this.Arg, arg1, arg2)
```

bind()函数与call()函数接收的参数是一样的，其返回值是原函数的副本，并拥有指定的this值和初始参数。

```js
// 定义一个add()函数
function add(x, y){
    return x + y
}
// 通过bind()函数进行add()函数的调用
function myAddBind(x, y){
    // 调用bind()函数得到一个新的函数
    let bindAddFn = add.bind(this, x, y)
    return bindAddFn()
}
console.log(myAddBind(10, 20)) // 输出'30'
```

###  call()函数、apply()函数、bind()函数的比较

三者的相同之处都是：都会改变函数调用的执行主体，修改this的指向。

不同之处在以下两点

1. call()函数与apply()函数在执行后会理解立即调用前面的函数，而bind()函数不会立即调用，它会返回一个新的函数，可以在任何时候进行调用。
2. call()接收从第二个到最后一个参数，apply()接收一个数组。

## 对象的属性

### 访问器属性

访问器属性同样包含4个特性，分别是[[Configurable]]、[[Enumerable]]、[[Get]]和[[Set]]。

- [[Configurable]]：表示属性能否删除而重新定义，或者是否可以修改为访问器属性，默认值为true
- [[Enumerable]]：表示属性是否可枚举，可枚举的属性能够通过for...in循环返回，默认值为true。 
- [[Get]]：在读取属性值时调用的函数（一般称为getter()函数），负责返回有效的值，默认值为undefined。
- [[Set]]：在写入属性值时调用的函数（一般称为setter()函数），负责处理数据，默认值为undefined。

如果需要修改访问器属性默认的特性，则必须使用Object.defineProperty()函数

getter()函数和setter()函数的存在在一定程度上可以实现对象的私有属性，私有属性不对外暴露。如果想要读取和写入私有属性的值，则需要通过设置额外属性的getter()函数和setter()函数来实现。

```js
let person ={
	_age: 10
}
Object.defineProperty(person, 'age', {
  get: function () {
    return this._age;
  },
  set: function (newValue) {
    if (newValue > 10) {
      this._age = newValue;
      console.log('设置成功');
    }
  },
});
```

### 对象深拷贝

JSON.parse(JSON.stringify(Obj))

```js
/* 使用JSON.stringify()函数将原始对象序列化为字符串，再使用JSON.parse()函数将字符串反序列化为一个对象。 */
let origin = {
  a: 1,
  b: 2,
  C: {
    d: 'name',
  },
};
let result = JSON.parse(JSON.stringify(origin));
console.log(result);
```

该方法有以下几个问题：

1. 无法实现对函数、RegExp等特殊对象的克隆。
2. 对象的constructor会被抛弃，所有的构造函数会指向Object，原型链关系断裂。
3. 对象中如果存在循环引用，会抛出异常。

```js
function Animal(name) {
  this.name = name;
}
let animal = new Animal('tom');
let origin = {
  // 属性为函数
  a: function () {
    return 'a';
  },
  // 属性为正则表达式对象
  b: new RegExp('d', 'g'),
  // 属性为某个对象的实例
  c: animal,
};
let result = JSON.parse(JSON.stringify(origin));

console.log(origin, '打印origin');
console.log(result, '打印result');
console.log(origin.c.constructor);
console.log(result.c.constructor);
```

### 原型对象、构造函数、实例之间的关系

每一个函数在创建时都会被赋予一个prototype属性。在默认情况下，所有的原型对象都会增加一个constructor属性，指向prototype属性所在的函数，即构造函数。

当我们通过new操作符调用构造函数创建一个实例时，实例具有一个__proto__属性，指向构造函数的原型对象，因此__proto__属性可以看做是一个连续实例与构造函数的原型对象的桥梁。

```js
function Person(){}
Person.prototype.name = 'Tom'
Person.prototype.age = 29
Person.prototype.job = 'Software Enginner'
Person.prototype.sayName = function(){
    console.log(name)
}
let person1 = new Person()
let person2 = new Person()
```

构造函数的Person有个prototype属性，指向的是Person的原型对象。在原型对象中有constructor属性和另外4个原型对象上的属性，其中constructor属性指向构造函数本身。

通过new操作符创建的两个实例person1和person2，都具有一个__proto__属性，指向的是Person的原型对象。

### 实例的属性读取顺序

当我们通过对象的实例读取某个属性时，它会先在实例本身去找指定的属性，如果找到了，则直接返回该属性的值；如果没找到则会继续沿着原型对象寻找，如果在原型对象中找到了该属性，则返回该属性的值。

改造上方代码

```js
function Person(){
    this.name = 'kingx'
}
Person.prototype.name = 'Tom'
Person.prototype.age = 29
Person.prototype.job = 'Software Enginner'
Person.prototype.sayName = function(){
    console.log(name)
}
let person1 = new Person() 
console.log(person1.name) // kingx
```

### 重写原型对象

```js
function Person(){}
Person.prototype = {
    constructor: Person, // 重要
    name: 'Nicholas',
    age: 29,
    job: 'Software Engineer',
    sayName: function(){
        console.log(name)
    }
}
```

将一个对象字面量赋给prototype属性的方式实际是重写了原型对象，等同于切断了构造函数和最初原型之间的关系，因此有一点需要的是，如果仍然想使用constructor属性做后续处理，则应该在对象字面量增加一个constructor属性，指向构造函数本身，否则原型的constructor属性会指向Object类型的构造函数，从而导致constructor属性与构造函数的脱离，而且必须要按照顺序执行，在最后进行新的对象的创建。

### 原型链

在JS中几乎所有的对象都具有__proto__属性，由__proto__属性连接而成的链路构成了JS的原型链，原型链的顶端是Object.prototype，它的__proto__属性为null。

```js
function Person(){}
let person = new Person 
// person实例沿着原型链第一次追溯，__proto__属性指向Person()构造函数的原型对象
person.__proto__ === Person.prototype
// person实例沿着原型链第二次追溯，Person原型对象的__protos__属性指向Object类型的原型对象。
person.__proto__.__proto__ === Person.prototype.__proto__ === Object.prototype
// person实例沿着原型链第三次追溯，Object类型的原型对象的__proto__属性为null
person.__proto__.__proto__.__proto__ === Object.prototype.__proto__ === null
```

#### 原型链的特点

1. 由于原型链的存在，属性查找的过程不再是只查找自身的原型对象，而是会沿着整个原型链一直向上，直到追溯到Object.prototype。如果Object.prototype上，直到追溯到Object.prototype。如果Object.prootype上也找不到该属性，则返回'undefined'。如果期间在实例本身或者某个原型对象上找到了该属性，则会直接返回结果，因此会存在属性覆盖的问题。

   在生成自定义对象的实例时，也可以调用到某些未在自定义构造函数上的函数，例如toString()函数

   ```js
   function Person(){}
   let p = new Person()
   p.toString() // [object Object] 实际调用的是Object.prototype.toString()函数
   ```

2. 由于属性查找会经历整个原型链，因此查找的链路越长，对性能的影响越大。

#### 属性区分

对象属性的寻找往往会设计整个原型链，Object()构造函数的原型对象中提供了一个hasOwnProperty()函数，用于判断属性是否为自身拥有的。

```js
function Person(name){
    // 实例属性name
    this.name = name
}
// 原型对象上的属性age
Person.prototype.age = 12
let person = new Person('kingx')
console.log(person.hasOwnProperty('name')) // true
console.log(person.hasOwnProperty('age')) // false
```

在使用for...in运算符遍历对象的属性时，一般可以配合hasOwnProperty()函数一起使用，检测是否是对象自身的属性，然后做后续处理。

```js
for (let prop in person){
    if(person.hasOwnproperty(prop)){
 		...       
    }
}
```

#### 内置构造函数

JavaScript中有一些特定的内置构造函数，如String()构造函数、Number()构造函数，Array()构造函数、Object()构造函数等。

它们本身的__proto__属性都统一指向Function.prototype。

```js
String.__proto__ === Function.prototype // true
Number.__proto__ === Function.prototype // true
Array.__proto__ === Function.prototype // true
Date.__proto__ === Function.prototype // true
Object.__proto__ === Function.prototype // true
Function.__proto__ === Function.prototype // true
```

#### __proto__属性

```js
Function.prototype.a = 'a'
Object.prototype.b = 'b'

function Person(){}

let p = new Person();

console.log('p.a', p.a)
console.log('p.b', p.b)
```

实例p的原型链

```js
// 实例p直接原型
p.__proto__ = Person.prototype
// Person原型对象的原型
Person.prototype.__proto__ = Object.prototype
```

#### 继承

##### 原型链继承

原型链继承的主要思想是：重写子类的prototype属性，将其指向父类的实例

```js
// 子类Cat
function Cat(name) {
    this.name = name
}
// 原型继承
Cat.prototype = new Animal();
// 很关键的一句，将Cat的构造函数指向自身
Cat.prototype.constructor = Cat;

let cat = new Cat('coffee')
```

###### 原型链继承的优点

1. 简单、易于实现

2. 继承关系纯粹

   生成的实例既是子类的实例，也是父类的实例。

3. 可通过子类直接访问父类原型链属性和函数

   通过原型链继承的子类，可以直接访问到父类原型链上新增的函数和属性。

###### 原型链继承的缺点

1. 子类的所有实例将共享父类的属性
2. 在创建子类实例时，无法向父类的构造函数传递参数
3. 无法实现多继承
4. 为子类增加原型对象上的属性和函数时，必须放在new Animal()函数之后

##### 构造继承

构造继承的主要思想是在子类的构造函数中通过call()函数改变this指向，调用父类的构造函数，从而能将父类的实例的属性和函数绑定到子类的this中

```js
// 父类
function Animal(age) {
  // 属性
  this.name = "Animal";
  this.age = age;
  // 实例函数
  this.sleep = function () {
    return this.name + "正在睡觉！";
  };
}
// 父类原型函数
Animal.prototype.eat = function (food) {
  return this.name + "正在吃：" + food;
};
// 子类
function Cat(name) {
  // 核心，通过call()函数实现Animal的实例的属性和函数的继承
  Animal.call(this);
  this.name = name || "tom";
}
// 生成子类的实例
let cat = new Cat("tony");
// 可以正常调用父类实例函数
console.log(cat.sleep());
// 不能调用父类原型函数
console.log(cat.eat());
```

###### 构造继承的优点

1. 可解决子类实例共享父类属性的问题

2. 创建子类的实例时，可以向父类传递参数

   在call()函数中，可以传递参数，将参数传递给父类。

3. 可以实现多继承

   在子类的构造函数中，可以通过多次调用call()函数来继承多个父对象，每调用一次call()函数就会将父类的实例的属性和函数

###### 构造继承的缺点

1. 实例只是子类的实例，并不是父类的实例
2. 只能继承父类实例的属性和函数，并不能继承原型对象上的属性和函数
3. 无法复用父类的实例函数

复制继承

复制继承的主要思想是首先生成父类的实例，然后通过for...in遍历父类实例的属性和函数，并将其依次设置为子类实例的属性和函数或者原型对象上的属性和函数。

```js
// 父类
function Animal(parentAge) {
  // 实例属性
  this.name = "Animal";
  this.age = parentAge;
  // 实例函数
  this.sleep = function () {
    return this.name + "正在睡觉";
  };
}
// 原型函数
Animal.prototype.eat = function (food) {
  return this.name + "正在吃：" + food;
};
// 子类
function Cat(name, age) {
  let animal = new Animal(age);
  // 父类的属性和函数，全部添加至子类中
  for (const key in animal) {
    if (Object.hasOwnProperty.call(key)) {
      this[key] = animal[key];
    } else {
      // 原型对象上的属性和函数
      Cat.prototype[key] = animal[key];
    }
  }
  // 子类自身的属性
  this.name = name;
}
// 子类自身原型函数
Cat.prototype.eat = function (food) {
  return this.name + "正在吃" + food;
};

let cat = new Cat("tony", 12);
console.log(cat.age);
console.log(cat.sleep());
console.log(cat.eat("猫粮"));
```

###### 复制继承的优点

1. 支持多继承
2. 能同时继承实例的属性和函数与原型对象上的属性和函数
3. 可以向父类构造函数中传递值

###### 复制继承的缺点

1. 父类的所有属性都需要复制，消耗内存
2. 实例只是子类的实例，并不是父类的实例

##### 组合继承

组合继承的主要思想是组合了构造函数和原型函数两种方法，一方面在子类的构造函数中通过call()函数调用父类的构造函数，将父类的实例的属性和函数绑定到子类的this中，另一方面，通过改变子类的prototype属性，继承父类的原型对象上的属性和函数

```js
// 父类
function Animal(parentAge) {
  // 实例属性
  this.name = "Animal";
  this.age = parentAge;
  // 实例函数
  this.sleep = function () {
    return this.name + "正在睡觉";
  };
  this.feature = ["fat", "thin", "tall"];
}
// 原型函数
Animal.prototype.eat = function (food) {
  return this.name + "正在吃：" + food;
};
// 子类
function Cat(name) {
  // 通过构造函数继承实例的属性和函数
  Animal.call(this);
  this.name = name;
}
// 通过原型继承原型对象上的属性和函数
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

let cat = new Cat("tony");
console.log(cat.name);
console.log(cat.sleep());
console.log(cat.eat("猫粮"));
```

###### 组合继承的优点

1. 既能继承父类实例的属性和函数，又能继承原型对象上的属性和函数
2. 既是子类的实例，又是父类的实例
```js
console.log(cat instanceof Cat);   // true
console.log(cat instanceof Animal);// true
```
3. 不存在引用属性共享的问题
4. 可以向父类的构造函数中传递参数
###### 组合继承的缺点

组合继承的缺点为父类的实例属性会绑定两次