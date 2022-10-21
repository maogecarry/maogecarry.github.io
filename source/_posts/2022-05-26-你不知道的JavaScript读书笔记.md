---
title: 你不知道的JavaScript读书笔记
date: 2022-05-26 10:47:54
tags:
    - 'JavaScript'
---

# 你不知道的JavaScript读书笔记

# 你不知道的JavaScript（上卷）

1. 最小特权原则（最小授权或最小暴露原则）

   指在软件设计中，应该最小限度地暴露必要内容，而将其他内容都 “隐藏” 起来，比如某个模块或对象的API设计。
	 
<!--more-->
2. 立即执行函数

   ```js
   var a = 2;
   (function IIFE() {
       var a = 3;
       console.log( a ); // 3
   })();
   console.log( a ); // 2
   ```

   由于函数被包含在一对 ( ) 括号内部，因此成为了一个表达式，通过在末尾加上另外一个 ( ) 可以立即执行这个函数，比如 (function foo(){ .. })()。第一个 ( ) 将函数变成表 达式，第二个 ( ) 执行了这个函数。

## 作用域提升

1. ### 函数优先

   函数声明和变量声明都会提升。但是一个值得注意的细节是函数会首先提升，然后才是变量。

   ```js
   foo(); // 1
   var foo;
   function foo() {
       console.log( 1 );
   }
   foo = function() {
       console.log( 2 );
   };
   ```

   会输出1而不是2，这个代码片段会被引擎理解为如下形式：

   ```js
   function foo() {
       console.log( 1 );
   }
   foo(); // 1
   foo = function() {
       console.log( 2 );
   };
   ```

## 模块

```js
function CoolModule() {
    var something = "cool";
    var another = [1, 2, 3];
    function doSomething() {
    console.log( something );
    }
    function doAnother() {
    console.log( another.join( " ! " ) );
    }
    return {
    doSomething: doSomething,
    doAnother: doAnother
    };
}
var foo = CoolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

这个模式在 JavaScript 中被称为模块。最常见的实现模块模式的方法通常被称为模块暴露， 这里展示的是其变体。

模块模式需要具备两个必要条件：

1. 必须有外部的封闭函数，该函数必须至少被调用一次（每次调用都会创建一个新的模块实例）
2. 封闭函数必须返回至少一个内部函数，这样内部函数才能在私有作用域中形成闭包，并且可以访问或者修改私有的状态。

## 现代的模块机制

大多数模块依赖加载器 / 管理器本质上都是将这种模块定义封装进一个友好的API。

```js
var MyModules = (function Manager() {
    var modules = {};
    
    function define(name, deps, impl) {
        for (var i=0; i<deps.length; i++) {
            deps[i] = modules[deps[i]];
        }
    	modules[name] = impl.apply( impl, deps );
    }
    
    function get(name) {
    	return modules[name];
    }
    
    return {
    define: define,
    get: get
    };
})()
```

这段代码的核心是 `modules[name] = impl.apply(impl, deps)`。为了模块的定义引入了包装 函数（可以传入任何依赖），并且将返回值，也就是模块的 API，储存在一个根据名字来管理的模块列表中。

定义模块方式：

```js
MyModules.define("bar", [], function () {
  function hello(who) {
    return "Let me introduce: " + who;
  }
  return {
    hello: hello
  };
});

MyModules.define("foo", ["bar"], function (bar) {
  var hungry = "hippo";
  function awesome() {
    console.log(bar.hello(hungry).toUpperCase());
  }
  return {
    awesome: awesome
  };
});

var bar = MyModules.get("bar");
var foo = MyModules.get("foo");
console.log(bar.hello("hippo")); // Let me introduce: hippo
foo.awesome(); // LET ME INTRODUCE: HIPPO
```

## 使用new来调用函数

1. 创建（或者说构造）一个全新的对象。
2. 这个新对象会被执行[[ 原型 ]]连接。
3. 这个新对象会绑定到函数调用的this。
4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。

## this的指向问题

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
  foo: foo
};
var a = "oops, global";
setTimeout(obj.foo, 100);
```

这个题考了this的作用域和延迟执行函数中的this指向

> 备注：即使是在严格模式下，`setTimeout()`的回调函数里面的`this`仍然默认指向`window`对象， 并不是`undefined`。

## 对象

JavaScript中有许多特殊的对象子类型，可以称之为复杂基本类型。函数就是对象的一个子类型(从技术角度来说就是"可调用的对象")。JavaScript中的函数是“一等公民”,因为它们本质上和普通的对象一样(只是可以调用),所以可以像操作其他对象一样操作函数(比如当做另一个函数的参数)

## 类

一个类就是一张蓝图。为了获得真正可以交互的对象，我们必须按照类来建造一个东西，这个东西通常被称为实例，有需要的话，我们可以直接在实例上调用方法并访问其所有公有数据属性。

JavaScript才是真正应该被称为 “面向对象” 的语言，因为它是少有的可以不通过类，直接创建对象的语言。

### 构造函数

类实例是由一个特殊的类方法构造的，这个方法名通常和类名相同，被称为构造函数。

# 你不知道的JavaScript（中卷）

## 类型

类型是值的内部特征，它定义了值的行为，以使其区别于其他值。

函数是 “ 可调用对象 ”，它有一个内部属性[[Call]]，该属性使其可以被调用。

### 值和类型

JavaScript中的变量是没有类型的，只有值才有。变量可以随时持有任何类型的值。类型定义了值的行为特征。

JavaScript 中的数字包括“整 数”和“浮点型”。

#### 字符串

字符串 “借用” 数组的非变更方法来处理字符串

```js
const a = 'foo'
const c = Array.prototype.join.call( a, "-" );
const d = Array.prototype.map.call( a, function(v){
 return v.toUpperCase() + ".";
} ).join( "" );
c; // "f-o-o"
d; // "F.O.O.
```

###  值和引用

简单值（即标量基本类型值，scalar primitive）总是通过值复制的方式来赋值 / 传递，包括null、undefined、字符串、数字、布尔和ES6中的symbol。

复合值（compound value）——对象（包括数组和封装对象）和函数，则总 是通过引用复制的方式来赋值 / 传递。

## 第四章：强制类型转换

### 隐式类型转换

1. `ToString`：基本类型值的字符串化规则为：null转换为 "null"，undefined转换为"undefined"，true转换为"true"。数字的字符串化则遵循通用规则。

2. `JSON.toStringify()`

   ```js
   const a = {
     b: 42,
     c: "42",
     d: [1, 2, 3]
   };
   
   console.log(JSON.stringify(a, ["b", "c"]));
   
   console.log(
     JSON.stringify(a, function (k, v) {
       if (k !== "c") return v;
     })
   );
   ```

   - 字符串、数字、布尔值和null的`JSON.stringify()`规则与`ToString`基本相同
   - 如果传递给`JSON.stringify()`的对象中定义了`toJSON（）`方法，那么该方法会在字符串化前调用，以便将对象转换为安全的JSON值。

3. `ToNumber`

   `ToNumber`对字符串的处理基本遵循数字常量的相关规则/语法。处理失败时返回`NaN`。

   对象(包括数组)会首先被转化为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

   如果`valueOf()`和`toString()`均不返回基本类型值，会产生`TypeError`错误。

4. `ToBoolean`

   - 假值

     `JavaScript`中的值可以分为以下两类：

     - 可以被强制类型转换为false的值
     - 其他(被强制类型转换为true的值)

     假值：

     - undefined
     - null
     - false
     - +0, -0 和` NaN`
     - ""

     假值的布尔强制类型转换结果为false，你不知道的JavaScript中理解为规定所有的对象都是正值，假值列表以外的值都是真值。

   - 假值对象(falsy object)

     假值对象看起来和普通对象并无二致(都有属性，等等)，但将它们强制类型转换为布尔值时结果为false。

   - 真值(truthy value)

     真值就是假值列表之外的值。

### 显示强制类型转换

1. 字符串和数字之间的显示转换

   ```js
   const a= 42
   const b = String(a)
   
   const c = '3.14'
   const d = Number(c)
   
   b; // '42'
   d; // 3.14
   ```

   `String()`遵循`ToString`规则，将值转换为字符串基本类型。`Number()`遵循`ToNumber`规则，将值转换为数字基本类型。

## 生成器

一个函数 一旦开始执行，就会运行到结束，期间不会有其他代码能够打断它并插入其间。ES6 引入了一个新的函数类型，它并不符合这种运行到结束的特性。这类新的函数被称为生成器。

1. #### 迭代消息传递

   除了能够接受参数并提供返回值之外，生成器甚至提供了更强大更引人注目的内建消息输入输出能力，通过yield和next()实现

   ```js
   function *foo(x){
   	let y = x * (yield)
        return y
   }
   
   let it = foo(6)
   
   // 启动foo()
   it.next()
   
   let res = it.next(7)
   
   res.value; // 42
   ```

   生成器消息是双向传递的——yield

   ```js
   function *foo(x) { 
    let y = x * (yield "Hello"); // <-- yield一个值！
    return y; 
   } 
   let it = foo( 6 ); 
   let res = it.next(); // 第一个next()，并不传入任何东西
   res.value; // "Hello" 
   res = it.next( 7 ); // 向等待的yield传入7
   res.value; // 42 
   
   ```

2. 多个迭代器

   同一个生成器的多个实例同事运行，它们甚至可以彼此交互：

   ```js
   function *foo() { 
    var x = yield 2; 
    z++; 
    var y = yield (x * z); 
    console.log( x, y, z ); 
   } 
   var z = 1; 
   var it1 = foo(); 
   var it2 = foo(); 
   var val1 = it1.next().value; // 2 <-- yield 2 
   var val2 = it2.next().value; // 2 <-- yield 2 
   val1 = it1.next( val2 * 10 ).value; // 40 <-- x:20, z:2 
   val2 = it2.next( val1 * 5 ).value; // 600 <-- x:200, z:3 
   it1.next( val2 / 2 ); // y:300 
    // 20 300 3 
   it2.next( val1 / 4 ); // y:10 
    // 200 10 3
   ```

   执行流程。 

   (1) *foo() 的两个实例同时启动，两个 next() 分别从 yield 2 语句得到值 2。 

   (2) val2 * 10 也就是 2 * 10，发送到第一个生成器实例 it1，因此 x 得到值 20。z 从 1 增 加到 2，然后 20 * 2 通过 yield 发出，将 val1 设置为 40。 

   (3) val1 * 5 也就是 40 * 5，发送到第二个生成器实例 it2，因此 x 得到值 200。z 再次从 2 递增到 3，然后 200 * 3 通过 yield 发出，将 val2 设置为 600。 

   (4) val2 / 2 也就是 600 / 2，发送到第一个生成器实例 it1，因此 y 得到值 300，然后打印 出 x y z 的值分别是 20 300 3。

    (5) val1 / 4 也就是 40 / 4，发送到第二个生成器实例 it2，因此 y 得到值 10，然后打印出 x y z 的值分别为 200 10 3。

# 你不知道的JavaScript（下卷）

## 作用域（词法作用域）

   每个函数都有自己的作用域。作用域基本上是变量的一个集合以及如何通过名称访问这些变量的规则。只有函数内部的代码才能访问这个函数作用域中的变量。

   ```js
function outer() { 
 var a = 1; 
 function inner() { 
 var b = 2; 
 // 这里我们既可以访问a，也可以访问b 
 console.log( a + b ); // 3 
 } 
 inner(); 
 // 这里我们只能访问a 
 console.log( a ); // 1 
} 
outer()
   ```

   JavaScript的值有类型，但变量无类型。

   ## 深入JavaScript

### 对象

对象类型是一个组合值，你可以为其设置属性（命名的位置），每个属性可以持有属于自己的任意类型的值。

1. #### 数组

   数组是一个持有（任意类型）值的对象，这些值不是通过命名属性 / 键值索引，而是通过数字索引位置。

2. #### 函数

   在JavaScript，函数也同样是对象的一个子类型，因为typeof返回 “ function ”，这意味着function是一个主类型，因此function可以拥有属性，但通常只在很少的情况下才会使用函数的对象属性。

#### 内置类型方法

举个栗子：

```js
var a = "hello world"; 
var b = 3.14159; 
a.length; // 11 
a.toUpperCase(); // "HELLO WORLD" 
b.toFixed(4); // "3.1416"
```

简单地说，存在一个String(S大写)对象封装形式，通常称为 “原生的”，与基本string类型相对应；在这个对象封装在其原型中定义了toUpperCase()方法。

像前面的代码片段中那样将" hello world "这样的原生值作为对象使用时，在引用其属性和方法时，JavaScript会（暗自）自动的将这个值 “封箱” 为其对应的对象封装。

字符串值可以封装为 String 对象，数字可以封装为 Number 对象，布尔型值可以封装为 Boolean 对象。


