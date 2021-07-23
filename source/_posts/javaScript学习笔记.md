---
title: javaScript学习笔记
date: 2021-02-06 16:50
desecription: javaScript学习笔记
tags:
  - 'JavaScript'
---

# javascript

## 对象引用和复制

与原始类型相比，对象的根本区别之一是对象是“通过引用”被存储和复制的，与原始类型值相反：字符串，数字，布尔值等 —— 始终是以“整体值”的形式被复制的。

<!--more-->

### 克隆与合并，Object.assign

Object.assign(dest, [src1, src2, src3...])

- 第一个参数 dest 是指目标对象。
- 更后面的参数 src1, ..., srcN（可按需传递多个参数）是源对象。
- 该方法将所有源对象的属性拷贝到目标对象 dest 中。换句话说，从第二个开始的所有参数的属性都被拷贝到第一个参数的对象中。
- 调用结果返回 dest。
- 可以用它来合并多个对象

​ 对象方法,"this"

- 对象方法的调用时基于“引用类型”的调用

### Symbol 类型

      1. 防止自己在对象中写的属性被其他人篡改，隐藏该属性
      2. 用for..in.., Object.keys(obj)访问不到，但是使用Object.assign是可以复制过来的

## 对象

      1. 对象是具有一些特殊特性的关联数组。
      2. 它们存储属性（键值对），其中：
           1. 属性的键必须是字符串或者 symbol（通常是字符串）。
           2. 值可以是任何类型。
      3. 下面的方法访问属性
           1. 点符号: obj.property。
           2. 方括号 obj["property"]，方括号允许从变量中获取键，例如 obj[varWithKey]。
      4. 其他操作：
           1. 删除属性：delete obj.prop。
           2. 检查是否存在给定键的属性："key" in obj。
           3. 遍历对象：for(let key in obj) 循环。

### 原始类型的方法

- 基本类型不是对象。
- 基本类型不能存储数据。
- 所有的属性/方法操作都是在临时对象的帮助下执行的。

### 数字类型

​ parseInt(str, radix)` 的第二个参数

- parseInt() 函数具有可选的第二个参数。它指定了数字系统的基数，因此 parseInt 还可以解析十六进制数字、二进制数字等的字符串：
- alert( parseInt('0xff', 16) ); // 255
- alert( parseInt('ff', 16) ); // 255，没有 0x 仍然有效
- alert( parseInt('2n9c', 36) ); // 123456

​ prompt

- 注意在 prompt 前面的一元加号 +。它将立即把值转换成数字。
- 否则，a 和 b 将会是字符串，它们的总和将是它们的连接，即："1" + "2" = "12"。

## 字符串

### str

### 1. str.indexOf(substr, pos)。

​ 1. 它从给定位置 pos 开始，在 str 中查找 substr，如果没有找到，则返回 -1，否则返回匹配成功的位置。
​ 2. 还有一个类似的方法 str.lastIndexOf(substr, position)，它从字符串的末尾开始搜索到开头。

### 2. 按位（bitwise）NOT 技巧

​ 1. 它将数字转换为 32-bit 整数（如果存在小数部分，则删除小数部分），然后对其二进制表示形式中的所有位均取反。

### 3. 获取子字符串

     1.  JavaScript 中有三种获取字符串的方法：substring、substr 和 slice。
                 1.  str.slice(start [, end])，返回字符串从 start 到（但不包括）end 的部分。
                 2.  如果没有第二个参数，slice 会一直运行到字符串末尾：
                        1.  let str = "stringify";
                                   alert( str.slice(0, 5) ); // 'strin'，从 0 到 5 的子字符串（不包括 5）
                                   alert( str.slice(0, 1) ); // 's'，从 0 到 1，但不包括 1，所以只有在 0 处的字符
                                   let str = "stringify";
                                   alert( str.slice(2) ); // 从第二个位置直到结束
                           2.  str.substring(start [, end])返回字符串在 start 和 end 之间 的部分。这与 slice 几乎相同，但它允许 start 大于 end。
                                   et str = "stringify";
                                          // 这些对于 substring 是相同的
                                          alert( str.substring(2, 6) ); // "ring"
                                          alert( str.substring(6, 2) ); // "ring"
                                          // ……但对 slice 是不同的：
                                          alert( str.slice(2, 6) ); // "ring"（一样）
                                          alert( str.slice(6, 2) ); // ""（空字符串）
               3.  str.substr(start [, length]) 返回字符串从 start 开始的给定 length 的部分。与以前的方法相比，这个允许我们指定 length 而不是结束位置：
                       . let str = "stringify";
                              alert( str.substr(2, 4) ); // 'ring'，从位置 2 开始，获取 4 个字符
                              2. 第一个参数可能是负数，从结尾算起：
                                     let str = "stringify";
                                     alert( str.substr(-4, 2) ); // 'gi'，从第 4 位获取 2 个字符

​ 方法 选择方式…… 负值参数  
​ - slice(start, end) 从 start 到 end（不含 end） 允许
​ - substring(start, end) start 与 end 之间（包括 start，但不包括 end） 负值代表 0
​ - substr(start, length) 从 start 开始获取长为 length 的字符串 允许 start 为负数

​

## 数组

- 数组, 声明, pop/push, shift/unshift 方法, 内部, 性能, 循环, 关于 “length”, new Array(), 多维数组, toString, 不要使用 == 比较数组

​ Iterable object（可迭代对象）

- 可迭代（Iterable） 对象是数组的泛化。这个概念是说任何对象都可以被定制为可在 for..of 循环中使用的对象。

## Map and Set（映射和集合）

### Map

- Map 是一个带键的数据项的集合，就像一个 Object 一样。 但是它们最大的差别是 Map 允许任何类型的键（key）。

它的方法和属性如下：

- new Map() —— 创建 map。
- map.set(key, value) —— 根据键存储值。
- map.get(key) —— 根据键来返回值，如果 map 中不存在对应的 key，则返回 undefined。
- map.has(key) —— 如果 key 存在则返回 true，否则返回 false。
- map.delete(key) —— 删除指定键的值。
- map.clear() —— 清空 map。
- map.size —— 返回当前元素个数。

### Map 迭代

如果要在 map 里使用循环，可以使用以下三个方法：

- map.keys() —— 遍历并返回所有的键（returns an iterable for keys），
- map.values() —— 遍历并返回所有的值（returns an iterable for values），
- map.entries() —— 遍历并返回所有的实体（returns an iterable for entries）[key, value]，for..of 在默认情况下使用的就是这个。

### Object.entries：从对象创建 Map

如果我们想从一个已有的普通对象（plain object）来创建一个 Map，那么我们可以使用内建方法 Object.entries(obj)，该返回对象的键/值对数组，该数组格式完全按照 Map 所需的格式。

### Object.fromEntries：从 Map 创建对象

Object.fromEntries 方法的作用是相反的：给定一个具有 [key, value] 键值对的数组，它会根据给定数组创建一个对象：

## Set

Set 是一个特殊的类型集合 —— “值的集合”（没有键），它的每一个值只能出现一次。

它的主要方法如下：

- new Set(iterable) —— 创建一个 set，如果提供了一个 iterable 对象（通常是数组），将会从数组里面复制值到 set 中。
- set.add(value) —— 添加一个值，返回 set 本身
- set.delete(value) —— 删除值，如果 value 在这个方法调用的时候存在则返回 true ，否则返回 false。
- set.has(value) —— 如果 value 在 set 中，返回 true，否则返回 false。
- set.clear() —— 清空 set。
- set.size —— 返回元素个数。

Map 中用于迭代的方法在 Set 中也同样支持：

- set.keys() —— 遍历并返回所有的值（returns an iterable object for values），
- set.values() —— 与 set.keys() 作用相同，这是为了兼容 Map，
- set.entries() —— 遍历并返回所有的实体（returns an iterable object for entries）[value, value]，它的存在也是为了兼容 Map。

## 解构赋值

解构赋值 是一种特殊的语法，它使我们可以将数组或对象“拆包”为到一系列变量中，因为有时候使用变量更加方便。解构操作对那些具有很多参数和默认值等的函数也很奏效。
解构赋值, 数组解构, 对象解构, 嵌套解构, 智能函数参数

- 解构赋值可以立即将一个对象或数组映射到多个变量上。
- 解构对象的完整语法：
  - let {prop : varName = default, ...rest} = object
  - 这表示属性 prop 会被赋值给变量 varName，如果没有这个属性的话，就会使用默认值 default。
  - 没有对应映射的对象属性会被复制到 rest 对象。
- 解构数组的完整语法：
  - let [item1 = default, item2, ...rest] = array
  - 数组的第一个元素被赋值给 item1，第二个元素被赋值给 item2，剩下的所有元素被复制到另一个数组 rest。
- 从嵌套数组/对象中提取数据也是可以的，此时等号左侧必须和等号右侧有相同的结构。

## 日期和时间

该对象存储日期和时间，并提供了日期/时间的管理方法。
例如，我们可以使用它来存储创建/修改时间，或者用来测量时间，再或者仅用来打印当前时间。

### 创建

创建一个新的 Date 对象，只需要调用 new Date()，在调用时可以带有下面这些参数之一：

- new Date()

  - 不带参数 —— 创建一个表示当前日期和时间的 Date 对象：
    - let now = new Date();

- new Date(milliseconds)
  - 创建一个 Date 对象，其时间等于 1970-01-01 00:00:00 UTC+0 再过一毫秒（1/1000 秒）。
  - 传入的整数参数代表的是自 1970-01-01 00:00:00 以来经过的毫秒数，该整数被称为 时间戳。
  - 这是一种日期的轻量级数字表示形式。我们通常使用 new Date(timestamp) 通过时间戳来创建日期，并可以使用 date.getTime() 将现有的 Date 对象转化为时间戳。
- new Date(datestring)
  - 如果只有一个参数，并且是字符串，那么它会被自动解析。该算法与 Date.parse 所使用的算法相同
- new Date(year, month, date, hours, minutes, seconds, ms)
  - 使用当前时区中的给定组件创建日期。只有前两个参数是必须的。
  - 自动校准（Autocorrection）

自动校准 是 Date 对象的一个非常方便的特性。我们可以设置超范围的数值，它会自动校准。
超出范围的日期组件将会被自动分配。

## JSON 方法，toJSON

假设我们有一个复杂的对象，我们希望将其转换为字符串，以通过网络发送，或者只是为了在日志中输出它。

当然，这样的字符串应该包含所有重要的属性。

### JSON.stringify

JSON（JavaScript Object Notation）是表示值和对象的通用格式。在 RFC 4627 标准中有对其的描述。最初它是为 JavaScript 而创建的，但许多其他编程语言也有用于处理它的库。因此，当客户端使用 JavaScript 而服务器端是使用 Ruby/PHP/Java 等语言编写的时，使用 JSON 可以很容易地进行数据交换。

JavaScript 提供了如下方法：

- JSON.stringify 将对象转换为 JSON。
- JSON.parse 将 JSON 转换回对象。

方法 JSON.stringify(student) 接收对象并将其转换为字符串。
得到的 json 字符串是一个被称为 JSON 编码（JSON-encoded） 或 序列化（serialized） 或 字符串化（stringified） 或 编组化（marshalled） 的对象。

请注意，JSON 编码的对象与对象字面量有几个重要的区别：:

- 字符串使用双引号。JSON 中没有单引号或反引号。所以 'John' 被转换为 "John"。
- 对象属性名称也是双引号的。这是强制性的。所以 age:30 被转换成 "age":30。

JSON.stringify 也可以应用于原始（primitive）数据类型。
JSON 支持以下数据类型：

- Objects { ... }
- Arrays [ ... ]
- Primitives：
  - strings，
  - numbers，
  - boolean values true/false，
  - null。

JSON 是语言无关的纯数据规范，因此一些特定于 JavaScript 的对象属性会被 JSON.stringify 跳过。
即：

- 函数属性（方法）。
- Symbol 类型的属性。
- 存储 undefined 的属性。

​ 排除和转换：replacer

JSON.stringify 的完整语法是：
let json = JSON.stringify(value, [replacer, space])  
 value
要编码的值。
replacer
要编码的属性数组或映射函数 function(key, value)。
space
用于格式化的空格数量

## Rest 参数与 Spread 语法

在 JavaScript 中，很多内建函数都支持传入任意数量的参数。
例如：

- Math.max(arg1, arg2, ..., argN) —— 返回入参中的最大值。
- Object.assign(dest, src1, ..., srcN) —— 依次将属性从 src1..N 复制到 dest。

### Rest 参数 ...

在 JavaScript 中，无论函数是如何定义的，你都可以使用任意数量的参数调用函数。
Rest 参数可以通过使用三个点 ... 并在后面跟着包含剩余参数的数组名称，来将它们包含在函数定义中。这些点的字面意思是“将剩余参数收集到一个数组中”。
Rest 参数必须放到参数列表的末尾
Rest 参数会收集剩余的所有参数，因此下面这种用法没有意义，并且会导致错误：
function f(arg1, ...rest, arg2) { // arg2 在 ...rest 后面？！
// error
}
...rest 必须处在最后。

​ “arguments” 变量

有一个名为 arguments 的特殊的类数组对象，该对象按参数索引包含所有参数。

在过去，JavaScript 中没有 rest 参数，而使用 arguments 是获取函数所有参数的唯一方法。现在它仍然有效，我们可以在一些老代码里找到它。

但缺点是，尽管 arguments 是一个类数组，也是可迭代对象，但它终究不是数组。它不支持数组方法，因此我们不能调用 arguments.map(...) 等方法。

此外，它始终包含所有参数，我们不能像使用 rest 参数那样只截取入参的一部分。

因此，当我们需要这些功能时，最好使用 rest 参数。

### Spread 语法

在脚本执行时，可能参数数组中有很多个元素，也可能一个都没有。并且这样设置的代码也很丑。
Spread 语法 来帮助你了！它看起来和 rest 参数很像，也使用 ...，但是二者的用途完全相反。
当在函数调用中使用 ...arr 时，它会把可迭代对象 arr “展开”到参数列表中。

以 Math.max 为例：

      1. - let arr = [3, 5, 1];
      2. - alert( Math.max(...arr) ); // 5（spread 语法把数组转换为参数列表）

我们还可以通过这种方式传递多个可迭代对象：

      1. let arr1 = [1, -2, 3, 4];
      2. let arr2 = [8, 3, -8, 1];
      3.
      4. alert( Math.max(...arr1, ...arr2) ); // 8

我们甚至还可以将 spread 语法与常规值结合使用：

      1. let arr1 = [1, -2, 3, 4];
      2. let arr2 = [8, 3, -8, 1];
      3.
      4. alert( Math.max(1, ...arr1, 2, ...arr2, 25) ); // 25

并且，我们还可以使用 spread 语法来合并数组：

      1. let arr = [3, 5, 1];
      2. let arr2 = [8, 9, 15];
      3.
      4. let merged = [0, ...arr, 2, ...arr2];
      5.
      6. alert(merged); // 0,3,5,1,2,8,9,15（0，然后是 arr，然后是 2，然后是 arr2）

在上面的示例中，我们使用数组展示了 spread 语法，其实任何可迭代对象都可以。

例如，在这儿我们使用 spread 语法将字符串转换为字符数组：

- let str = "Hello";
- alert( [...str] ); // H,e,l,l,o
  Spread 语法内部使用了迭代器来收集元素，与 for..of 的方式相同

不过 Array.from(obj) 和 [...obj] 存在一个细微的差别：

- Array.from 适用于类数组对象也适用于可迭代对象。
- Spread 语法只适用于可迭代对象。
  因此，对于将一些“东西”转换为数组的任务，Array.from 往往更通用。

​ 总结  
 当我们在代码中看到 "..." 时，它要么是 rest 参数，要么就是 spread 语法。

有一个简单的方法可以区分它们：

- 若 ... 出现在函数参数列表的最后，那么它就是 rest 参数，它会把参数列表中剩余的参数收集到一个数组中。
- 若 ... 出现在函数调用或类似的表达式中，那它就是 spread 语法，它会把一个数组展开为列表。
- 使用场景：

- Rest 参数用于创建可接受任意数量参数的函数。
- Spread 语法用于将数组传递给通常需要含有许多参数的列表的函数。
  它们俩的出现帮助我们轻松地在列表和参数数组之间来回转换。

“旧式”的 arguments（类数组且可迭代的对象）也依然能够帮助我们获取函数调用中的所有参数。

## 变量作用域，闭包

### Step 1. 变量

在 JavaScript 中，每个运行的函数，代码块 {...} 以及整个脚本，都有一个被称为 词法环境（Lexical Environment） 的内部（隐藏）的关联对象。

词法环境对象由两部分组成：

      1. 环境记录（Environment Record） —— 一个存储所有局部变量作为其属性（包括一些其他信息，例如 this 的值）的对象。
      2. 对 外部词法环境 的引用，与外部代码相关联。

一个“变量”只是 环境记录 这个特殊的内部对象的一个属性。“获取或修改变量”意味着“获取或修改词法环境的一个属性”。

- 变量是特殊内部对象的属性，与当前正在执行的（代码）块/函数/脚本有关。
- 操作变量实际上是操作该对象的属性。

### Step 2. 函数声明

一个函数其实也是一个值，就像变量一样。

**\* \*不同之处在于函数声明的初始化会被立即完成。 \*\***

当创建了一个词法环境（Lexical Environment）时，函数声明会立即变为即用型函数（不像 let 那样直到声明处才可用）。

### Step 3. 内部和外部的词法环境

在一个函数运行时，在调用刚开始时，会自动创建一个新的词法环境以存储这个调用的局部变量和参数。

**\* \*当代码要访问一个变量时 —— 首先会搜索内部词法环境，然后搜索外部环境，然后搜索更外部的环境，以此类推，直到全局词法环境。 \*\***

如果在任何地方都找不到这个变量，那么在严格模式下就会报错（在非严格模式下，为了向下兼容，给未定义的变量赋值会创建一个全局变量）。

### Step 4. 返回函数

所有的函数在“诞生”时都会记住创建它们的词法环境。从技术上讲，这里没有什么魔法：所有函数都有名为 [[Environment]] 的隐藏属性，该属性保存了对创建该函数的词法环境的引用。

### **闭包**

开发者通常应该都知道“闭包”这个通用的编程术语。

[闭包](<https://en.wikipedia.org/wiki/Closure_(computer_programming)>) 是指内部函数总是可以访问其所在的外部函数中声明的变量和参数，即使在其外部函数被返回（寿命终结）了之后。

也就是说：JavaScript 中的函数会自动通过隐藏的 `[[Environment]]` 属性记住创建它们的位置，所以它们都可以访问外部变量。

在面试时，前端开发者通常会被问到“什么是闭包？”，正确的回答应该是闭包的定义，并解释清楚为什么 JavaScript 中的所有函数都是闭包的，以及可能的关于 `[[Environment]]` 属性和词法环境原理的技术细节。

### **垃圾收集**

通常，函数调用完成后，会将词法环境和其中的所有变量从内存中删除。因为现在没有任何对它们的引用了。与 JavaScript 中的任何其他对象一样，词法环境仅在可达时才会被保留在内存中。

但是，如果有一个嵌套的函数在函数结束后仍可达，则它将具有引用词法环境的 `[[Environment]]` 属性。

### 实际开发中的优化

正如我们所看到的，理论上当函数可达时，它外部的所有变量也都将存在。

但在实际中，JavaScript 引擎会试图优化它。它们会分析变量的使用情况，如果从代码中可以明显看出有未使用的外部变量，那么就会将其删除。

**在 V8（Chrome，Edge，Opera）中的一个重要的副作用是，此类变量在调试中将不可用。**

# 函数对象, NFE

在 JavaScript 中，函数就是对象。

## 属性 “name”

函数对象包含一些便于使用的属性。

1. 一个函数的名字可以通过属性 “name” 来访问。

2. 名称赋值的逻辑很智能。即使函数被创建时没有名字，名称赋值的逻辑也能给它赋予一个正确的名字，然后进行赋值。

3. 以默认值的方式完成了赋值时，它也有效。

4. 如果函数自己没有提供，那么在赋值中，会根据上下文来推测一个。

   对象方法也有名字，

## 属性 “length”

内置属性 “length”，它返回函数入参的个数

属性 `length` 有时在操作其它函数的函数中用于做 [内省/运行时检查（introspection）](<https://zh.wikipedia.org/wiki/内省_(计算机科学)>)。

## 自定义属性

**属性不是变量**

被赋值给函数的属性，比如 `sayHi.counter = 0`，**不会** 在函数内定义一个局部变量 `counter`。换句话说，属性 `counter` 和变量 `let counter` 是毫不相关的两个东西。

我们可以把函数当作对象，在它里面存储属性，但是这对它的执行没有任何影响。变量不是函数属性，反之亦然。它们之间是平行的。

## 命名函数表达式

命名函数表达式（NFE，Named Function Expression），指带有名字的函数表达式的术语。

```
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};

sayHi("John"); // Hello, John
```

关于名字 `func` 有两个特殊的地方，这就是添加它的原因：

1. 它允许函数在内部引用自己。
2. 它在函数外是不可见的。

下面的函数 `sayHi` 会在没有入参 `who` 时，以 `"Guest"` 为入参调用自己：

```javascript
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`)
  } else {
    func('Guest') // 使用 func 再次调用函数自身
  }
}
sayHi() // Hello, Guest
// 但这不工作：
func() // Error, func is not defined（在函数外不可见）
```

## 问题（待解决）

### [任意数量的括号求和](https://zh.javascript.info/function-object#ren-yi-shu-liang-de-kuo-hao-qiu-he)

重要程度: 2

写一个函数 `sum`，它有这样的功能：

```javascript
sum(1)(2) == 3 // 1 + 2
sum(1)(2)(3) == 6 // 1 + 2 + 3
sum(5)(-1)(2) == 6
sum(6)(-1)(-2)(-3) == 0
sum(0)(1)(2)(3)(4)(5) == 15
```

P.S. 提示：你可能需要创建自定义对象来为你的函数提供基本类型转换。

## [总结](https://zh.javascript.info/function-object#zong-jie)

函数就是对象。

它们的一些属性：

- `name` —— 函数的名字。通常取自函数定义，但如果函数定义时没设定函数名，JavaScript 会尝试通过函数的上下文猜一个函数名（例如把赋值的变量名取为函数名）。
- `length` —— 函数定义时的入参的个数。Rest 参数不参与计数。

# 调度：setTimeout 和 setInterval

有时我们并不想立即执行一个函数，而是等待特定一段时间之后再执行。这就是所谓的“计划调用（scheduling a call）”。

目前有两种方式可以实现：

- `setTimeout` 允许我们将函数推迟到一段时间间隔之后再执行。
- `setInterval` 允许我们重复运行一个函数，从一段时间间隔之后开始运行，之后以该时间间隔连续重复运行该函数。

这两个方法并不在 JavaScript 的规范中。但是大多数运行环境都有内建的调度程序，并且提供了这些方法。目前来讲，所有浏览器以及 Node.js 都支持这两个方法。

## setTimeout

语法：

```javascript
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
```

参数说明：

- `func|code`

  想要执行的函数或代码字符串。 一般传入的都是函数。由于某些历史原因，支持传入代码字符串，但是不建议这样做。

- `delay`

  执行前的延时，以毫秒为单位（1000 毫秒 = 1 秒），默认值是 0；

- `arg1`，`arg2`…

  要传入被执行函数（或代码字符串）的参数列表（IE9 以下不支持）

## [嵌套的 setTimeout](https://zh.javascript.info/settimeout-setinterval#qian-tao-de-settimeout)

周期性调度有两种方式。

一种是使用 `setInterval`，另外一种就是嵌套的 `setTimeout`，就像这样：

```javascript
/** instead of:
let timerId = setInterval(() => alert('tick'), 2000);
*/

let timerId = setTimeout(function tick() {
  alert('tick')
  timerId = setTimeout(tick, 2000) // (*)
}, 2000)
```

上面这个 `setTimeout` 在当前这一次函数执行完时 `(*)` 立即调度下一次调用。

嵌套的 `setTimeout` 要比 `setInterval` 灵活得多。采用这种方式可以根据当前执行结果来调度下一次调用，因此下一次调用可以与当前这一次不同。

**传入一个函数，但不要执行它**

新手开发者有时候会误将一对括号 `()` 加在函数后面：

```javascript
// 错的！
setTimeout(sayHi(), 1000)
```

这样不行，因为 `setTimeout` 期望得到一个对函数的引用。而这里的 `sayHi()` 很明显是在执行函数，所以实际上传入 `setTimeout` 的是 **函数的执行结果**。在这个例子中，`sayHi()` 的执行结果是 `undefined`（也就是说函数没有返回任何结果），所以实际上什么也没有调度。

### [用 clearTimeout 来取消调度](https://zh.javascript.info/settimeout-setinterval#yong-cleartimeout-lai-qu-xiao-tiao-du)

`setTimeout` 在调用时会返回一个“定时器标识符（timer identifier）”，在我们的例子中是 `timerId`，我们可以使用它来取消执行。

取消调度的语法：

```javascript
let timerId = setTimeout(...);
clearTimeout(timerId);
```

## [setInterval](https://zh.javascript.info/settimeout-setinterval#setinterval)

`setInterval` 方法和 `setTimeout` 的语法相同：

```javascript
let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...)
```

所有参数的意义也是相同的。不过与 `setTimeout` 只执行一次不同，`setInterval` 是每间隔给定的时间周期性执行。

想要阻止后续调用，我们需要调用 `clearInterval(timerId)`。

**alert 弹窗显示的时候计时器依然在进行计时**

在大多数浏览器中，包括 Chrome 和 Firefox，在显示 `alert/confirm/prompt` 弹窗时，内部的定时器仍旧会继续“嘀嗒”。

所以，在运行上面的代码时，如果在一定时间内没有关掉 `alert` 弹窗，那么在你关闭弹窗后，下一个 `alert` 会立即显示。两次 `alert` 之间的时间间隔将小于 2 秒。

**嵌套的 `setTimeout` 能够精确地设置两次执行之间的延时，而 `setInterval` 却不能。**

## [零延时的 setTimeout](https://zh.javascript.info/settimeout-setinterval#ling-yan-shi-de-settimeout)

这儿有一种特殊的用法：`setTimeout(func, 0)`，或者仅仅是 `setTimeout(func)`。

这样调度可以让 `func` 尽快执行。但是只有在当前正在执行的脚本执行完成后，调度程序才会调用它。

也就是说，该函数被调度在当前脚本执行完成“之后”立即执行。

例如，下面这段代码会先输出 “Hello”，然后立即输出 “World”：

```javascript
setTimeout(() => alert('World'))

alert('Hello')
```

第一行代码“将调用安排到日程（calendar）0 毫秒处”。但是调度程序只有在当前脚本执行完毕时才会去“检查日程”，所以先输出 `"Hello"`，然后才输出 `"World"`。

**零延时实际上不为零（在浏览器中）**

## [总结](https://zh.javascript.info/settimeout-setinterval#zong-jie)

- `setTimeout(func, delay, ...args)` 和 `setInterval(func, delay, ...args)` 方法允许我们在 `delay` 毫秒之后运行 `func` 一次或以 `delay` 毫秒为时间间隔周期性运行 `func`。
- 要取消函数的执行，我们应该调用 `clearInterval/clearTimeout`，并将 `setInterval/setTimeout` 返回的值作为入参传入。
- 嵌套的 `setTimeout` 比 `setInterval` 用起来更加灵活，允许我们更精确地设置两次执行之间的时间。
- 零延时调度 `setTimeout(func, 0)`（与 `setTimeout(func)` 相同）用来调度需要尽快执行的调用，但是会在当前脚本执行完成后进行调用。
- 浏览器会将 `setTimeout` 或 `setInterval` 的五层或更多层嵌套调用（调用五次之后）的最小延时限制在 4ms。这是历史遗留问题。

# 函数绑定

当将对象方法作为回调进行传递，例如传递给 `setTimeout`，这儿会存在一个常见的问题：“丢失 `this`”。

## [丢失 “this”](https://zh.javascript.info/bind#diu-shi-this)

下面是使用 `setTimeout` 时 `this` 是如何丢失的：

```javascript
let user = {
  firstName: 'John',
  sayHi() {
    alert(`Hello, ${this.firstName}!`)
  },
}

setTimeout(user.sayHi, 1000) // Hello, undefined!
```

## [解决方案 1：包装器](https://zh.javascript.info/bind#jie-jue-fang-an-1-bao-zhuang-qi)

最简单的解决方案是使用一个包装函数：

```javascript
let user = {
  firstName: 'John',
  sayHi() {
    alert(`Hello, ${this.firstName}!`)
  },
}

setTimeout(function () {
  user.sayHi() // Hello, John!
}, 1000)
```

现在它可以正常工作了，因为它从外部词法环境中获取到了 `user`，就可以正常地调用方法了。

## [解决方案 2：bind](https://zh.javascript.info/bind#jie-jue-fang-an-2-bind)

函数提供了一个内建方法 [bind](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)，它可以绑定 `this`。

基本的语法是：

```javascript
// 稍后将会有更复杂的语法
let boundFunc = func.bind(context)
```

`func.bind(context)` 的结果是一个特殊的类似于函数的“外来对象（exotic object）”，它可以像函数一样被调用，并且透明地（transparently）将调用传递给 `func` 并设定 `this=context`。

`boundFunc` 调用就像绑定了 `this` 的 `func`。

## [在没有上下文情况下的 partial](https://zh.javascript.info/bind#zai-mei-you-shang-xia-wen-qing-kuang-xia-de-partial)

当我们想绑定一些参数（arguments），但是这里没有上下文 `this`，应该怎么办？例如，对于一个对象方法。

原生的 `bind` 不允许这种情况。我们不可以省略上下文直接跳到参数（arguments）。

`partial(func[, arg1, arg2...])` 调用的结果是一个包装器 `(*)`，它调用 `func` 并具有以下内容：

- 与它获得的函数具有相同的 `this`（对于 `user.sayNow` 调用来说，它是 `user`）
- 然后给它 `...argsBound` —— 来自于 `partial` 调用的参数（`"10:00"`）
- 然后给它 `...args` —— 给包装器的参数（`"Hello"`）

## [总结](https://zh.javascript.info/bind#zong-jie)

方法 `func.bind(context, ...args)` 返回函数 `func` 的“绑定的（bound）变体”，它绑定了上下文 `this` 和第一个参数（如果给定了）。

通常我们应用 `bind` 来绑定对象方法的 `this`，这样我们就可以把它们传递到其他地方使用。例如，传递给 `setTimeout`。

当我们绑定一个现有的函数的某些参数时，绑定后的（不太通用的）函数被称为 **partially applied** 或 **partial**。

当我们不想一遍又一遍地重复相同的参数时，partial 非常有用。就像我们有一个 `send(from, to)` 函数，并且对于我们的任务来说，`from` 应该总是一样的，那么我们就可以搞一个 partial 并使用它。

# 深入理解箭头函数

让我们深入研究一下箭头函数。

箭头函数不仅仅是编写简洁代码的“捷径”。它还具有非常特殊且有用的特性。

JavaScript 充满了我们需要编写在其他地方执行的小函数的情况。

```javascript
let sum = (a, b) => a + b

/* 这个箭头函数是下面这个函数的更短的版本：

let sum = function(a, b) {
  return a + b;
};
*/

alert(sum(1, 2)) // 3
```

- 如果我们只有一个参数，还可以省略掉参数外的圆括号，使代码更短。
- 如果没有参数，括号将是空的（但括号应该保留）：

## [箭头函数没有 “this”](https://zh.javascript.info/arrow-functions#jian-tou-han-shu-mei-you-this)

正如我们在 [对象方法，"this"](https://zh.javascript.info/object-methods) 一章中所学到的，箭头函数没有 `this`。如果访问 `this`，则会从外部获取。

**不能对箭头函数进行 `new` 操作**

不具有 `this` 自然也就意味着另一个限制：箭头函数不能用作构造器（constructor）。不能用 `new` 调用它们。

## [箭头函数没有 “arguments”](https://zh.javascript.info/arrow-functions#jian-tou-han-shu-mei-you-arguments)

箭头函数也没有 `arguments` 变量。

当我们需要使用当前的 `this` 和 `arguments` 转发一个调用时，这对装饰器（decorators）来说非常有用。

## [总结](https://zh.javascript.info/arrow-functions#zong-jie)

箭头函数：

- 没有 `this`
- 没有 `arguments`
- 不能使用 `new` 进行调用
- 它们也没有 `super`

# Mixin 模式

[mixin](https://en.wikipedia.org/wiki/Mixin) 是一个包含可被其他类使用而无需继承的方法的类。

## [EventMixin](https://zh.javascript.info/mixins#eventmixin)

现在让我们为实际运用构造一个 mixin。

例如，许多浏览器对象的一个重要功能是它们可以生成事件。事件是向任何有需要的人“广播信息”的好方法。因此，让我们构造一个 mixin，使我们能够轻松地将与事件相关的函数添加到任意 class/object 中。

- Mixin 将提供 `.trigger(name, [...data])` 方法，以在发生重要的事情时“生成一个事件”。`name` 参数（arguments）是事件的名称，`[...data]` 是可选的带有事件数据的其他参数（arguments）。
- 此外还有 `.on(name, handler)` 方法，它为具有给定名称的事件添加了 `handler` 函数作为监听器（listener）。当具有给定 `name` 的事件触发时将调用该方法，并从 `.trigger` 调用中获取参数（arguments）。
- ……还有 `.off(name, handler)` 方法，它会删除 `handler` 监听器（listener）。

## [总结](https://zh.javascript.info/mixins#zong-jie)

_Mixin_ — 是一个通用的面向对象编程术语：一个包含其他类的方法的类。

一些其它编程语言允许多重继承。JavaScript 不支持多重继承，但是可以通过将方法拷贝到原型中来实现 mixin。

我们可以使用 mixin 作为一种通过添加多种行为（例如上文中所提到的事件处理）来扩充类的方法。

**受保护的属性通常以下划线 `_` 作为前缀。**

# 简介：回调

JavaScript 主机（host）环境提供了许多函数，这些函数允许我们计划 **异步** 行为（action）。

脚本是“异步”调用的，因为它从现在开始加载，但是在这个加载函数执行完成后才运行。

# Promise

Promise 对象的构造器（constructor）语法如下：

```javascript
let promise = new Promise(function (resolve, reject) {
  // executor（生产者代码，“歌手”）
})
```

传递给 `new Promise` 的函数被称为 **executor**。当 `new Promise` 被创建，executor 会自动运行。

当 executor 获得了结果，无论是早还是晚都没关系，它应该调用以下回调之一：

- `resolve(value)` — 如果任务成功完成并带有结果 `value`。
- `reject(error)` — 如果出现了 error，`error` 即为 error 对象。

executor 会自动运行并尝试执行一项工作。尝试结束后，如果成功则调用 `resolve`，如果出现 error 则调用 `reject`。

由 `new Promise` 构造器返回的 `promise` 对象具有以下内部属性：

- `state` — 最初是 `"pending"`，然后在 `resolve` 被调用时变为 `"fulfilled"`，或者在 `reject` 被调用时变为 `"rejected"`。
- `result` — 最初是 `undefined`，然后在 `resolve(value)` 被调用时变为 `value`，或者在 `reject(error)` 被调用时变为 `error`。

| Promises                                                                                                                                                                                                           | Callbacks                                                                                                                                                            |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Promises 允许我们按照自然顺序进行编码。首先，我们运行 `loadScript` 和 `.then` 来处理结果。                                                                                                                         | 在调用 `loadScript(script, callback)` 时，在我们处理的地方（disposal）必须有一个 `callback` 函数。换句话说，在调用 `loadScript` **之前**，我们必须知道如何处理结果。 |
| 我们可以根据需要，在 promise 上多次调用 `.then`。每次调用，我们都会在“订阅列表”中添加一个新的“分析”，一个新的订阅函数。在下一章将对此内容进行详细介绍：[Promise 链](https://zh.javascript.info/promise-chaining)。 | 只能有一个回调。                                                                                                                                                     |

因此，promise 为我们提供了更好的代码流和灵活性。

一个 promise 构造器和一个简单的 executor 函数，该 executor 函数具有包含时间（即 `setTimeout`）的“生产者代码”：

```javascript
let promise = new Promise(function (resolve, reject) {
  // 当 promise 被构造完成时，自动执行此函数

  // 1 秒后发出工作已经被完成的信号，并带有结果 "done"
  setTimeout(() => resolve('done'), 1000)
})
```

在 new Promise()的时候，Promise 的执行器就会立马执行，但是**调用 resolve()会触发异步操作，传入的 then()方法的函数会被添加到任务队列并异步执行**

`promise` 对象代表异步执行结果，通过构造函数 `new Promise` 创建，其参数是 executor function 可能有一些异步代码，对象其后的方法 `.then`、`.catch`、`.finally` 相当于回调函数。

一开始创建的 `promise` 对象状态是 `pending`（表示初始状态）然后在 executor function 中会有条件地执行 `resolve` 或 `reject` 之一，这样就会改变 `promise` 对象的状态为 `settled`（表示状态确定，可以触发对象后的方法 `.finally`）

`promise` 对象确定的状态其实有两种，如果在 executor function 调用的是 `resolve` 状态就变成 `resolved` 相应地会调用对象后的方法 `.then`；而如果在 executor function 调用的是 `reject` 状态就变成 `rejected` 相应地会调用对象后的方法 `.catch`（其实这是 `.then(null, rejectFunc)` 缩写形式）

# Promise 链

它的理念是将 result 通过 `.then` 处理程序（handler）链进行传递。

运行流程如下：

1. 初始 promise 在 1 秒后进行 resolve `(*)`，
2. 然后 `.then` 处理程序（handler）被调用 `(**)`。
3. 它返回的值被传入下一个 `.then` 处理程序（handler）`(***)`
4. ……依此类推。

**新手常犯的一个经典错误：从技术上讲，我们也可以将多个 `.then` 添加到一个 promise 上。但这并不是 promise 链（chaining）。**

## [总结](https://zh.javascript.info/promise-chaining#zong-jie)

如果 `.then`（或 `catch/finally` 都可以）处理程序（handler）返回一个 promise，那么链的其余部分将会等待，直到它状态变为 settled。当它被 settled 后，其 result（或 error）将被进一步传递下去。

![image-20210128140346440](E:\Markdown图片\image-20210128140346440.png)

# 使用 promise 进行错误处理

Promise 链在错误（error）处理中十分强大。当一个 promise 被 reject 时，控制权将移交至最近的 rejection 处理程序（handler）。

## [隐式 try…catch](https://zh.javascript.info/promise-error-handling#yin-shi-trycatch)

Promise 的执行者（executor）和 promise 的处理程序（handler）周围有一个“隐式的 `try..catch`”。如果发生异常，它（译注：指异常）就会被捕获，并被视为 rejection 进行处理。

下面这段代码：

```javascript
new Promise((resolve, reject) => {
  throw new Error('Whoops!')
}).catch(alert) // Error: Whoops!
```

……与下面这段代码工作上完全相同：

```javascript
new Promise((resolve, reject) => {
  reject(new Error('Whoops!'))
}).catch(alert) // Error: Whoops!
```

在 executor 周围的“隐式 `try..catch`”自动捕获了 error，并将其变为 rejected promise。

这不仅仅发生在 executor 函数中，同样也发生在其 handler 中。如果我们在 `.then` 处理程序（handler）中 `throw`，这意味着 promise 被 rejected，因此控制权移交至最近的 error 处理程序（handler）。

一个例子：

```javascript
new Promise((resolve, reject) => {
  resolve('ok')
})
  .then((result) => {
    throw new Error('Whoops!') // reject 这个 promise
  })
  .catch(alert) // Error: Whoops!
```

## [再次抛出（Rethrowing）](https://zh.javascript.info/promise-error-handling#zai-ci-pao-chu-rethrowing)

链尾端的 `.catch` 的表现有点像 `try..catch`。我们可能有许多个 `.then` 处理程序（handler），然后在尾端使用一个 `.catch` 处理上面的所有 error。

## [总结](https://zh.javascript.info/promise-error-handling#zong-jie)

- `.catch` 处理 promise 中的各种 error：在 `reject()` 调用中的，或者在处理程序（handler）中抛出的（thrown）error。
- 我们应该将 `.catch` 准确地放到我们想要处理 error，并知道如何处理这些 error 的地方。处理程序应该分析 error（可以自定义 error 类来帮助分析）并再次抛出未知的 error（可能它们是编程错误）。
- 如果没有办法从 error 中恢复的话，不使用 `.catch` 也可以。
- 在任何情况下我们都应该有 `unhandledrejection` 事件处理程序（用于浏览器，以及其他环境的模拟），以跟踪未处理的 error 并告知用户（可能还有我们的服务器）有关信息，以使我们的应用程序永远不会“死掉”。

# Promise API

## [Promise.all](https://zh.javascript.info/promise-api#promiseall)

假设我们希望并行执行多个 promise，并等待所有 promise 都准备就绪。语法：

```javascript
let promise = Promise.all([...promises...]);
```

`Promise.all(iterable)` **允许在** `iterable` **中使用 non-promise 的“常规”值**

`Promise.all` 接受一个 promise 数组作为参数（从技术上讲，它可以是任何可迭代的，但通常是一个数组）并返回一个新的 promise。

**如果任意一个 promise 被 reject，由 `Promise.all` 返回的 promise 就会立即 reject，并且带有的就是这个 error。**

## [Promise.allSettled](https://zh.javascript.info/promise-api#promiseallsettled)

`Promise.allSettled` 等待所有的 promise 都被 settle，无论结果如何。结果数组具有：

- `{status:"fulfilled", value:result}` 对于成功的响应，
- `{status:"rejected", reason:error}` 对于 error。

## [Promise.race](https://zh.javascript.info/promise-api#promiserace)

与 `Promise.all` 类似，但只等待第一个 settled 的 promise 并获取其结果（或 error）。

`Promise.all` 可能是在实战中使用最多的。

# 微任务（Microtask）

Promise 的处理程序（handlers）`.then`、`.catch` 和 `.finally` 都是异步的。

即便一个 promise 立即被 resolve，`.then`、`.catch` 和 `.finally` **下面** 的代码也会在这些处理程序（handler）之前被执行。

## [微任务队列（Microtask queue）](https://zh.javascript.info/microtask-queue#wei-ren-wu-dui-lie-microtaskqueue)

异步任务需要适当的管理。为此，ECMA 标准规定了一个内部队列 `PromiseJobs`，通常被称为“微任务队列（microtask queue）”（ES8 术语）。

如 [规范](https://tc39.github.io/ecma262/#sec-jobs-and-job-queues) 中所述：

- 队列（queue）是先进先出的：首先进入队列的任务会首先运行。
- 只有在 JavaScript 引擎中没有其它任务在运行时，才开始执行任务队列中的任务。

当一个 promise 准备就绪时，它的 `.then/catch/finally` 处理程序（handler）就会被放入队列中：但是它们不会立即被执行。当 JavaScript 引擎执行完当前的代码，它会从队列中获取任务并执行它。

如果有一个包含多个 `.then/catch/finally` 的链，那么它们中的每一个都是异步执行的。也就是说，它会首先进入队列，然后在当前代码执行完成并且先前排队的处理程序（handler）都完成时才会被执行。

## [总结](https://zh.javascript.info/microtask-queue#zong-jie)

Promise 处理始终是异步的，因为所有 promise 行为都会通过内部的 “promise jobs” 队列，也被称为“微任务队列”（ES8 术语）。

因此，`.then/catch/finally` 处理程序（handler）总是在当前代码完成后才会被调用。

如果我们需要确保一段代码在 `.then/catch/finally` 之后被执行，我们可以将它添加到链式调用的 `.then` 中。

# Async/await

## [Async function](https://zh.javascript.info/async-await#asyncfunction)

Async/await 是以更舒适的方式使用 promise 的一种特殊语法，同时它也非常易于理解和使用。

```javascript
async function f() {
  return 1
}
```

在函数前面的 “async” 这个单词表达了一个简单的事情：即这个函数总是返回一个 promise。其他值将自动被包装在一个 resolved 的 promise 中。

## [Await](https://zh.javascript.info/async-await#await)

语法如下：

```javascript
// 只在 async 函数内工作
let value = await promise
```

关键字 `await` 让 JavaScript 引擎等待直到 promise 完成（settle）并返回结果。

# Generator

常规函数只会返回一个单一值（或者不返回任何值）。

而 Generator 可以按需一个接一个地返回（“yield”）多个值。它们可与 [iterable](https://zh.javascript.info/iterable) 完美配合使用，从而可以轻松地创建数据流。

## [Generator 函数](https://zh.javascript.info/generators#generator-han-shu)

要创建一个 generator，我们需要一个特殊的语法结构：`function*`，即所谓的 “generator function”。

像这样：

```javascript
function* generateSequence() {
  yield 1
  yield 2
  return 3
}
```

一个 generator 的主要方法就是 `next()`。当被调用时（译注：指 `next()` 方法），它会恢复上图所示的运行，执行直到最近的 `yield <value>` 语句（`value` 可以被省略，默认为 `undefined`）。然后函数执行暂停，并将产出的（yielded）值返回到外部代码。

`next()` 的结果始终是一个具有两个属性的对象：

- `value`: 产出的（yielded）的值。
- `done`: 如果 generator 函数已执行完成则为 `true`，否则为 `false`。

代码恢复执行并返回下一个 `yield` 的值：

```javascript
let two = generator.next()

alert(JSON.stringify(two)) // {value: 2, done: false}
```

**`function\* f(…)` 或 `function \*f(…)`？**

这两种语法都是对的。

但是通常更倾向于第一种语法，因为星号 `*` 表示它是一个 generator 函数，它描述的是函数种类而不是名称，因此 `*` 应该和 `function` 关键字紧贴一起。

## [“yield” 是一条双向路](https://zh.javascript.info/generators#yield-shi-yi-tiao-shuang-xiang-lu)

目前看来，generator 和可迭代对象类似，都具有用来生成值的特殊语法。但实际上，generator 更加强大且灵活。

这是因为 `yield` 是一条双向路（two-way street）：它不仅可以向外返回结果，而且还可以将外部的值传递到 generator 内。

调用 `generator.next(arg)`，我们就能将参数 `arg` 传递到 generator 内部。这个 `arg` 参数会变成 `yield` 的结果。

## [generator.throw](https://zh.javascript.info/generators#generatorthrow)

正如我们在上面的例子中观察到的那样，外部代码可能会将一个值传递到 generator，作为 `yield` 的结果。

……但是它也可以在那里发起（抛出）一个 error。这很自然，因为 error 本身也是一种结果。

要向 `yield` 传递一个 error，我们应该调用 `generator.throw(err)`。在这种情况下，`err` 将被抛到对应的 `yield` 所在的那一行。

## [总结](https://zh.javascript.info/generators#zong-jie)

- Generator 是通过 generator 函数 `function* f(…) {…}` 创建的。
- 在 generator（仅在）内部，存在 `yield` 操作。
- 外部代码和 generator 可能会通过 `next/yield` 调用交换结果。

# 异步迭代和 generator

异步迭代允许我们对按需通过异步请求而得到的数据进行迭代。

当值是以异步的形式出现时，例如在 `setTimeout` 或者另一种延迟之后，就需要异步迭代。

最常见的场景是，对象需要发送一个网络请求以传递下一个值，稍后我们将看到一个它的真实示例。

要使对象异步迭代：

1. 使用 `Symbol.asyncIterator` 取代 `Symbol.iterator`。
2. `next()` 方法应该返回一个 `promise`（带有下一个值，并且状态为 `fulfilled`）。
   - 关键字 `async` 可以实现这一点，我们可以简单地使用 `async next()`。
3. 我们应该使用 `for await (let item of iterable)` 循环来迭代这样的对象。
   - 注意关键字 `await`。

作为开始的示例，让我们创建一个可迭代的 `range` 对象，与前面的那个类似，不过现在它将异步地每秒返回一个值。

我们需要做的就是对上面代码中的部分代码进行替换：

```javascript
let range = {
  from: 1,
  to: 5,

  [Symbol.asyncIterator]() {
    // (1)
    return {
      current: this.from,
      last: this.to,

      async next() {
        // (2)

        // 注意：我们可以在 async next 内部使用 "await"
        await new Promise((resolve) => setTimeout(resolve, 1000)) // (3)

        if (this.current <= this.last) {
          return { done: false, value: this.current++ }
        } else {
          return { done: true }
        }
      },
    }
  },
}

;(async () => {
  for await (let value of range) {
    // (4)
    alert(value) // 1,2,3,4,5
  }
})()
```

正如我们所看到的，其结构与常规的 iterator 类似:

1. 为了使一个对象可以异步迭代，它必须具有方法 `Symbol.asyncIterator` `(1)`。
2. 这个方法必须返回一个带有 `next()` 方法的对象，`next()` 方法会返回一个 promise `(2)`。
3. 这个 `next()` 方法可以不是 `async` 的，它可以是一个返回值是一个 `promise` 的常规的方法，但是使用 `async` 关键字可以允许我们在方法内部使用 `await`，所以会更加方便。这里我们只是用于延迟 1 秒的操作 `(3)`。
4. 我们使用 `for await(let value of range)` `(4)` 来进行迭代，也就是在 `for` 后面添加 `await`。它会调用一次 `range[Symbol.asyncIterator]()` 方法一次，然后调用它的 `next()` 方法获取值。

这是一个对比 Iterator 和异步 iterator 之间差异的表格：

|                          | Iterator          | 异步 iterator          |
| :----------------------- | :---------------- | :--------------------- |
| 提供 iterator 的对象方法 | `Symbol.iterator` | `Symbol.asyncIterator` |
| `next()` 返回的值是      | 任意值            | `Promise`              |
| 要进行循环，使用         | `for..of`         | `for await..of`        |

# 模块 (Module) 简介

社区发明了许多种方法来将代码组织到模块中，使用特殊的库按需加载模块。

列举一些（出于历史原因）：

- [AMD](https://en.wikipedia.org/wiki/Asynchronous_module_definition) —— 最古老的模块系统之一，最初由 [require.js](http://requirejs.org/) 库实现。
- [CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1) —— 为 Node.js 服务器创建的模块系统。
- [UMD](https://github.com/umdjs/umd) —— 另外一个模块系统，建议作为通用的模块系统，它与 AMD 和 CommonJS 都兼容。

## [什么是模块？](https://zh.javascript.info/modules-intro#shi-mo-shi-mo-kuai)

一个模块（module）就是一个文件。一个脚本就是一个模块。就这么简单。

模块可以相互加载，并可以使用特殊的指令 `export` 和 `import` 来交换功能，从另一个模块调用一个模块的函数：

- `export` 关键字标记了可以从当前模块外部访问的变量和函数。
- `import` 关键字允许从其他模块导入功能。

## [模块核心功能](https://zh.javascript.info/modules-intro#mo-kuai-he-xin-gong-neng)

### [始终使用 “use strict”](https://zh.javascript.info/modules-intro#shi-zhong-shi-yong-usestrict)

模块始终默认使用 `use strict`，例如，对一个未声明的变量赋值将产生错误（译注：在浏览器控制台可以看到 error 信息）。

### [模块级作用域](https://zh.javascript.info/modules-intro#mo-kuai-ji-zuo-yong-yu)

每个模块都有自己的顶级作用域（top-level scope）。换句话说，一个模块中的顶级作用域变量和函数在其他脚本中是不可见的。

### [模块代码仅在第一次导入时被解析](https://zh.javascript.info/modules-intro#mo-kuai-dai-ma-jin-zai-di-yi-ci-dao-ru-shi-bei-jie-xi)

如果同一个模块被导入到多个其他位置，那么它的代码仅会在第一次导入时执行，然后将导出（export）的内容提供给所有的导入（importer）。

### [在一个模块中，“this” 是 undefined](https://zh.javascript.info/modules-intro#zai-yi-ge-mo-kuai-zhong-this-shi-undefined)

这是一个小功能，但为了完整性，我们应该提到它。

在一个模块中，顶级 `this` 是 undefined。

### [import.meta](https://zh.javascript.info/modules-intro#importmeta)

`import.meta` 对象包含关于当前模块的信息。

它的内容取决于其所在的环境。在浏览器环境中，它包含当前脚本的 URL，或者如果它是在 HTML 中的话，则包含当前页面的 URL。

### [模块脚本是延迟的](https://zh.javascript.info/modules-intro#mo-kuai-jiao-ben-shi-yan-chi-de)

模块脚本 **总是** 被延迟的，与 `defer` 特性（在 [脚本：async，defer](https://zh.javascript.info/script-async-defer) 一章中描述的）对外部脚本和内联脚本（inline script）的影响相同。

也就是说：

- 下载外部模块脚本 `<script type="module" src="...">` 不会阻塞 HTML 的处理，它们会与其他资源并行加载。
- 模块脚本会等到 HTML 文档完全准备就绪（即使它们很小并且比 HTML 加载速度更快），然后才会运行。
- 保持脚本的相对顺序：在文档中排在前面的脚本先执行。

它的一个副作用是，模块脚本总是会“看到”已完全加载的 HTML 页面，包括在它们下方的 HTML 元素。

### [不允许裸模块（“bare” module）](https://zh.javascript.info/modules-intro#bu-yun-xu-luo-mo-kuai-baremodule)

在浏览器中，`import` 必须给出相对或绝对的 URL 路径。没有任何路径的模块被称为“裸（bare）”模块。在 `import` 中不允许这种模块。

### [兼容性，“nomodule”](https://zh.javascript.info/modules-intro#jian-rong-xing-nomodule)

旧时的浏览器不理解 `type="module"`。未知类型的脚本会被忽略。

## [构建工具](https://zh.javascript.info/modules-intro#gou-jian-gong-ju)

在实际开发中，浏览器模块很少被以“原始”形式进行使用。通常，我们会使用一些特殊工具，例如 [Webpack](https://webpack.js.org/)，将它们打包在一起，然后部署到生产环境的服务器。

使用打包工具的一个好处是 —— 它们可以更好地控制模块的解析方式，允许我们使用裸模块和更多的功能，例如 CSS/HTML 模块等。

构建工具做以下这些事儿：

1. 从一个打算放在 HTML 中的 `<script type="module">` “主”模块开始。
2. 分析它的依赖：它的导入，以及它的导入的导入等。
3. 使用所有模块构建一个文件（或者多个文件，这是可调的），并用打包函数（bundler function）替代原生的 `import` 调用，以使其正常工作。还支持像 HTML/CSS 模块等“特殊”的模块类型。
4. 在处理过程中，可能会应用其他转换和优化：
   - 删除无法访问的代码。
   - 删除未使用的导出（“tree-shaking”）。
   - 删除特定于开发的像 `console` 和 `debugger` 这样的语句。
   - 可以使用 [Babel](https://babeljs.io/) 将前沿的现代的 JavaScript 语法转换为具有类似功能的旧的 JavaScript 语法。
   - 压缩生成的文件（删除空格，用短的名字替换变量等）。

## [总结](https://zh.javascript.info/modules-intro#zong-jie)

下面总结一下模块的核心概念：

1. 一个模块就是一个文件。浏览器需要使用<script type="module">以使 import/export 可以工作。模块（译注：相较于常规脚本）有几点差别：
   - 默认是延迟解析的（deferred）。
   - Async 可用于内联脚本。
   - 要从另一个源（域/协议/端口）加载外部脚本，需要 CORS header。
   - 重复的外部脚本会被忽略
2. 模块具有自己的本地顶级作用域，并可以通过 `import/export` 交换功能。
3. 模块始终使用 `use strict`。
4. 模块代码只执行一次。导出仅创建一次，然后会在导入之间共享。

# Proxy 和 Reflect

一个 `Proxy` 对象包装另一个对象并拦截诸如读取/写入属性和其他操作，可以选择自行处理它们，或者透明地允许该对象处理它们。

Proxy 被用于了许多库和某些浏览器框架。

## [Proxy](https://zh.javascript.info/proxy#proxy)

语法：

```javascript
let proxy = new Proxy(target, handler)
```

- `target` —— 是要包装的对象，可以是任何东西，包括函数。
- `handler` —— 代理配置：带有“捕捉器”（“traps”，即拦截操作的方法）的对象。比如 `get` 捕捉器用于读取 `target` 的属性，`set` 捕捉器用于写入 `target` 的属性，等等。

对 `proxy` 进行操作，如果在 `handler` 中存在相应的捕捉器，则它将运行，并且 Proxy 有机会对其进行处理，否则将直接对 target 进行处理。

由于没有捕捉器，所有对 `proxy` 的操作都直接转发给了 `target`。

1. 写入操作 `proxy.test=` 会将值写入 `target`。
2. 读取操作 `proxy.test` 会从 `target` 返回对应的值。
3. 迭代 `proxy` 会从 `target` 返回对应的值。

## [带有 “get” 捕捉器的默认值](https://zh.javascript.info/proxy#dai-you-get-bu-zhuo-qi-de-mo-ren-zhi)

最常见的捕捉器是用于读取/写入的属性。

要拦截读取操作，`handler` 应该有 `get(target, property, receiver)` 方法。

读取属性时触发该方法，参数如下：

- `target` —— 是目标对象，该对象被作为第一个参数传递给 `new Proxy`，
- `property` —— 目标属性名，
- `receiver` —— 如果目标属性是一个 getter 访问器属性，则 `receiver` 就是本次读取属性所在的 `this` 对象。通常，这就是 `proxy` 对象本身（或者，如果我们从 proxy 继承，则是从该 proxy 继承的对象）。现在我们不需要此参数，因此稍后我们将对其进行详细介绍。

## [使用 “set” 捕捉器进行验证](https://zh.javascript.info/proxy#shi-yong-set-bu-zhuo-qi-jin-hang-yan-zheng)

假设我们想要一个专门用于数字的数组。如果添加了其他类型的值，则应该抛出一个错误。

当写入属性时 `set` 捕捉器被触发。

`set(target, property, value, receiver)`：

- `target` —— 是目标对象，该对象被作为第一个参数传递给 `new Proxy`，
- `property` —— 目标属性名称，
- `value` —— 目标属性的值，
- `receiver` —— 与 `get` 捕捉器类似，仅与 setter 访问器属性相关。

如果写入操作（setting）成功，`set` 捕捉器应该返回 `true`，否则返回 `false`（触发 `TypeError`）。

## [使用 “ownKeys” 和 “getOwnPropertyDescriptor” 进行迭代](https://zh.javascript.info/proxy#shi-yong-ownkeys-he-getownpropertydescriptor-jin-hang-die-dai)

`Object.keys`，`for..in` 循环和大多数其他遍历对象属性的方法都使用内部方法 `[[OwnPropertyKeys]]`（由 `ownKeys` 捕捉器拦截) 来获取属性列表。

这些方法在细节上有所不同：

- `Object.getOwnPropertyNames(obj)` 返回非 Symbol 键。
- `Object.getOwnPropertySymbols(obj)` 返回 Symbol 键。
- `Object.keys/values()` 返回带有 `enumerable` 标志的非 Symbol 键/值（属性标志在 [属性标志和属性描述符](https://zh.javascript.info/property-descriptors) 一章有详细讲解)。
- `for..in` 循环遍历所有带有 `enumerable` 标志的非 Symbol 键，以及原型对象的键。
