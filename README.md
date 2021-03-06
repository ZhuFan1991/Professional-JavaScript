# 《JavaScript高级程序设计(第三版)》学习笔记

**作者简介：**

  Nicholas C. Zakas（尼古拉斯•泽卡斯）世界顶级Web技术专家，现为雅虎公司界面呈现架构师，负责My Yahoo!和雅虎首页等大访问量站点的设计。

  尼古拉斯拥有丰富的Web开发和界面设计经验，曾经参与许多世界级大公司的Web解决方案开发。他还是High Performance JavaScript一书的作者，并与他人合作撰写了Professional Ajax和Even Faster Web Sites。

  尼古拉斯拥有梅里马克学院计算机科学学士学位和埃迪柯特学院的MBA学位。他的个人网站是：<u>[www.nczonline.net](www.nczonline.net)</u>，他的Twitter别名是 @slicknet 。

<br>

**前言：**

记录自己在阅读本书过程中的收获以及思考。
<br>
<br>
<br>
<br>
<br>

## 第三章 基本概念

### 一、语法

`ECMAScript` 的语法大量借鉴了 `C` 及其他类 `C` 语言（如 `Java` ）的语法。

<br>


***1. 区分大小写***

- `ECMAScript` 中的一切（变量、函数名和操作符）都要区分大小写。

<br>

***2. 标识符***

- **标识符**：指变量、函数、属性的名字，或者函数的参数。

按照惯例，`ECMAScript` 标识符采用驼峰大小写格式，也就是第一个字母小写，剩下的每个单词的首字母大写。比如：
```
firstSecond
myCar
doSomethingImportant
```
虽然没有谁强制要求必须采用这种格式，但为了与 `ECMAScript` 内置的函数和对象命名格式保持一致，可以将其当作一种最佳实践。

<br>

***3. 语句***

`ECMAScript` 中的语句以一个分号结尾；如果省略分号，则由解析器确定语句的结尾。虽然语句结尾的分号不是必需的，但我们建议任何时候都不要省略它,因为:
```
  1、可以避免 很多错误（例如不完整的输入）；
  2、开发人员可以放心地通过删除多余的空格来压缩 `ECMAScript` 代码（代码行结尾处没有分号会导致压缩错误）；
  3、加上分号也会在某些情况下增进代码的性能，因为解析器不必再花时间推测应该在哪里插入分号了；
```

<br>

### 二、变量

`ECMAScript` 的变量是松散类型的，可以用来保存任何类型的数据。

- 用 `var` 操作符定义的变量将成为定义该变量的作用域中的局部变量;

- 省略 `var` 操作符可以定义全局变量，但局部作用域中定义的全局变量很难维护;

<br>

### 三、数据类型

`ECMAScript` 可以用 `typeof` 操作符来检测给定变量的数据类型。

**注意：**`typeof` 是一个操作符而不是函数，因此例子中的圆括号尽管可以使用，但不是必需的。比如：
```JavaScript
var message = "some string";
alert(typeof message);   // "string"
alert(typeof(message));  // "string"
alert(typeof 95);        // "number"
```

<br>

***1. Undefined类型***

Undefined类型只有一个值，即特殊的 `undefined`。

- 对未初始化的变量执行 `typeof` 操作符将返回 `undefined` 值；

- 对未声明的变量执行 `typeof` 操作符将返回 `undefined` 值；

**注意**：显式地初始化变量依然是明智的选择，因为当 `typeof` 操作符返回"undefined"值时， 我们就知道被检测的变量还没有被声明，而不是尚未初始化。

<br>

***2. Null类型***

Null类型只有一个值，这个特殊的值是 `null`。**从逻辑角度来看，`null` 值表示一个空对象指针，而这也正是使用 `typeof` 操作符检测 `null` 值时会返回"object"的原因。** 比如：

```JavaScript
var car = null;
alert(typeof car);   // "object"
```

如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 `null` 而不是其他值。这样一来，只要直接检查 `null` 值就可以知道相应的变量是否已经保存了一个对象的引用，比如：

```JavaScript
var car = null;
if (car != null) {
  // 对 car 对象执行某些操作
}
```

**注意**：`undefined` 值是派生自 `null` 值的，因此 `ECMA-262` 规定对它们的相等性测试要返回 `true`，比如：

```JavaScript
alert(null == undefined);   //true
```

尽管 `null` 和 `undefined` 有这样的关系，但它们的用途完全不同。无论在什么情况下，都没有必要把一个变量的值显式地设置为 `undefined`，可是同样的规则对 `null` 却不适用。

换句话说，只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存 `null` 值。这样做不仅可以体现 `null` 作为空对象指针的惯例，而且也有助于进一步区分 `null` 和 `undefined`。

<br>

***3. Boolean类型***

Boolean类型只有两个字面值：`true` 和 `false`。下表给出了各种数据类型及其对应的转换规则：

| 数据类型 | 转换为true的值 | 转换为false的值 |
| :------: | :------: | :------: |
| Boolean | true | false |
| String | 任何非空字符串 | ""（空字符串） |
| Number | 任何非零数字值（包括无穷大） | 0和NaN |
| Object | 任何对象 | null |
| Undefined | N/A | undefined |

这些转换规则对理解流控制语句（如 `if` 语句）自动执行相应的 `Boolean` 转换非常重要，因为错误地使用一个对象而不是一个 `Boolean` 值，就有可能彻底改变应用程序的流程。

<br>

***4. Number类型***

Number类型使用 `IEEE754` 格式来表示整数和浮点数值（浮点数值在某些语言中也被称为双精度数值）。为支持各种数值类型，`ECMA-262` 定义了不同的数值字面量格式。比如：

```js
var intNum = 55;        // 整数
var octalNum1 = 070;    // 八进制的 56
var octalNum2 = 079;    // 无效的八进制数值——解析为 79
var octalNum3 = 08;     // 无效的八进制数值——解析为 8
var hexNum1 = 0xA;      // 十六进制的 10
var hexNum2 = 0x1f;     // 十六进制的 31
```

**注意**：在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。

<br>

- **浮点数值：**

由于保存浮点数值需要的内存空间是保存整数值的两倍，因此 `ECMAScript` 会不失时机地将浮点数值 转换为整数值。比如：

```js
var floatNum1 = 1.;     // 小数点后面没有数字——解析为 1
var floatNum2 = 10.0;   // 整数——解析为 10
```

- **数值范围：**

由于内存的限制，`ECMAScript` 并不能保存世界上所有的数值。如果某次计算的 结果得到了一个超出 JavaScript 数值范围的值，那么这个数值将被自动转换成特殊的 `Infinity` 值。

- **NaN：**

NaN，即非数值（Not a Number），是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不会抛出错误了）。它有两个特点：

```
  a、任何涉及 NaN 的操作（例如 NaN/10）都会返回 NaN；
  b、NaN 与任何值都不相等，包括 NaN 本身；
```

- **数值转换：**

有 3 个函数可以把非数值转换为数值：`Number()`、`parseInt()` 和 `parseFloat()`。

**Number()函数的转换规则如下:**

```
  如果是 Boolean 值，true 和 false 将分别被转换为 1 和 0。
  如果是数字值，只是简单的传入和返回。
  如果是 null 值，返回 0。
  如果是 undefined，返回 NaN。
  如果是字符串，遵循下列规则：
     如果字符串中只包含数字（包括前面带正号或负号的情况），则将其转换为十进制数值，即 "1" 会变成 1，"123"会变成 123，而"011"会变成 11（注意：前导的零被忽略了）；
     如果字符串中包含有效的浮点格式，如"1.1"，则将其转换为对应的浮点数值（同样，也会忽略前导零）；
     如果字符串中包含有效的十六进制格式， 例如 "0xf" ， 则将其转换为相同大小的十进制整数值；
     如果字符串是空的（不包含任何字符），则将其转换为 0；
     如果字符串中包含除上述格式之外的字符，则将其转换为 NaN。
  如果是对象，则调用对象的 valueOf() 方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的 toString() 方法，然后再次依照前面的规则转换返回的字符串值。
 ```

 由于 `Number()` 函数在转换字符串时比较复杂而且不够合理，因此在处理整数的时候更常用的是 `parseInt()` 函数。

 <br>

 **parseInt()函数的转换规则如下:**

 ```
   parseInt()函数在转换字符串时，更多的是看其是否符合数值模式。它会忽略字符串前面的空格，直至找到第一个非空格字符；
   如果第一个字符不是数字字符或者负号，parseInt() 就会返回 NaN；
   用 parseInt() 转换空字符串会返回 NaN（ Number()对空字符返回 0）；
   如果第一个字符是数字字符，parseInt()会继续解析第二个字符，直到解析完所有后续字符或者遇  
  到了一个非数字字符；
 ```

举例如下：

```JavaScript
var num1 = parseInt("1234blue");    // 1234
var num2 = parseInt("");            // NaN
var num3 = parseInt("0xA");         // 10（十六进制数）
var num4 = parseInt(22.5);          // 22
var num5 = parseInt("070");         // 56（八进制数）
var num6 = parseInt("70");          // 70（十进制数）
var num7 = parseInt("0xf");         // 15（十六进制数）
```

在 `ECMAScript 5` JavaScript 引擎中，`parseInt()` 已经不具有解析八进制值的能力，因此前导的 0 会被认为无效，为了消除在使用 `parseInt()` 函数时可能导致的上述困惑，可以为这个函数提供第二个参数：

```js
var num = parseInt("0xAF", 16);   //175
var num1 = parseInt("AF", 16);    //175
var num2 = parseInt("AF");        //NaN
```

**注意**：指定基数会影响到转换的输出结果。比如：

```js
var num1 = parseInt("10", 2);     //2 （按二进制解析）
var num2 = parseInt("10", 8);     //8 （按八进制解析）
var num3 = parseInt("10", 10);    //10 （按十进制解析）
var num4 = parseInt("10", 16);    //16 （按十六进制解析）
```

不指定基数意味着让 `parseInt()` 决定如何解析输入的字符串，因此为了避免错误的解析，我们建议无论在什么情况下都明确指定基数。

<br>

**parseFloat()函数的转换规则如下:**

```
  与parseInt()函数类似，parseFloat() 也是从第一个字符（位置 0）开始解析每个字符；
  parseFloat()会一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止；
  字符串中的第一个小数点是有效的，而第二个小数点就是无效的；
  parseFloat()始终都会忽略前导的 0 ；
  parseFloat()可以识别前面讨论过的所有浮点数值格式，也包括十进制整数格式。但十六进制格式的字符串则始终会被转换成 0；
  parseFloat()只解析十进制值，因此没有第二个参数指定基数的用法；
  如果字符串包含的是一个可解析为整数的数（没有小数点，或者小数点后 都是零），  
  parseFloat()会返回整数；
```

举例如下：
```js
var num1 = parseFloat("1234blue");    //1234 （整数）
var num2 = parseFloat("0xA");         //0
var num3 = parseFloat("22.5");        //22.5
var num4 = parseFloat("22.34.5");     //22.34
var num5 = parseFloat("0908.5");      //908.5
var num6 = parseFloat("3.125e7");     //31250000
```

<br>

***5. String类型***

`ECMAScript` 中用双引号表示的字符串和用单引号表示的字符串完全相同。

- **字符串的特点：**

`ECMAScript` 中的字符串是不可变的，一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量；

<br>

- **转换为字符串：**

把一个值转换为一个字符串有两种方式：

**toString()方法**

```js
var num = 10;
alert(num.toString());      // "10"（默认情况）
alert(num.toString(2));     // "1010" （二进制）
alert(num.toString(8));     // "12" （八进制）
alert(num.toString(10));    // "10" （十进制）
alert(num.toString(16));    // "a" （十六进制）
```

<br>

**String()方法**
```
  如果值有 toString()方法，则调用该方法（没有参数）并返回相应的结果；
  如果值是 null，则返回"null"；
  如果值是 undefined，则返回"undefined"。
```

`null` 和 `undefined` 没有 `toString()` 方法，所以 `String()` 函数就返回了这两个值的字面量。

举例如下：

```js
var value1 = 10;    alert(String(value1));     // "10"
var value2 = true;  alert(String(value2));     // "true"
var value3 = null;  alert(String(value3));     // "null"
var value4;         alert(String(value4));     // "undefined"
```

<br>

***6. Object类型***

`ECMAScript` 中的对象其实就是一组数据和功能的集合。对象可以通过执行 `new` 操作符后跟要创建的对象类型的名称来创建。

**Object 的每个实例都具有下列属性和方法：**

```
  constructor：保存着用于创建当前对象的函数，构造函数（constructor）就是 Object()；
  hasOwnProperty(propertyName)：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定（例如：o.hasOwnProperty("name")）；
  isPrototypeOf(object)：用于检查传入的对象是否是传入对象的原型；
  propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用 for-in 语句来枚举。与 hasOwnProperty()方法一样，作为参数的属性名必须以字符串形式指定；
  toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应；
  toString()：返回对象的字符串表示；
  valueOf()：返回对象的字符串、数值或布尔值表示。通常与 toString() 方法的返回值相同；
```

<br>

### 四、函数

`ECMAScript` 中的函数使用 `function` 关键字来声明，通过函数可以封装任意多条语句，而且可以在 **<font color="#282828">任何地方</font>** 、 **<font color="#282828">任何时候</font>** 调用执行。

<br>

***1. 参数***

`ECMAScript` 函数不限制参数数量，也不限制参数的数据类型。即便定义的函数只接收两个参数，但在调用这个函数时也未必一定要传递两个参数。

也就是说，我们可以向 `ECMAScript` 函数传递任意数量的参数，并且可以通过 `arguments` 对象来访问这些参数。

**<u><font color="#282828">`ECMAScript` 中的参数在内部是用一个数组来表示的，函数接收到的始终都是这个数组，而不关心数组中包含哪些参数（如果有参数的话）。</font></u>**

<br>

**`ECMAScript` 函数的两个重要特点：**

```
a、命名的参数只提供便利，但不是必需的；
b、解析器不会验证命名参数；
```

举例如下：

```js
function howManyArgs() {
  alert(arguments.length);
}

howManyArgs("string", 45);    //2
howManyArgs();                //0
howManyArgs(12);              //1
```

<br>

***2. 返回值***

`ECMAScript` 函数无须指定函数的返回值，因为任何函数都可以在任何时候返回任何值。 **<u><font color="#282828">实际上，未指定返回值的函数返回的是一个特殊的 `undefined` 值</font></u>** 。

<br>

***3. ECMAScript没有重载***

如前所述，`ECMAScirpt` 函数没有签名，因为其参数是由包含零或多个值的数组来表示的。所以 **<font color="#282828">真正的重载</font>** 是不可能做到的。但是，我们可以通过 **<u><font color="red">检查传入函数中参数的类型和数量并作出不同的反应，来模仿方法的重载。</font></u>**

举例如下：

```js
function doAdd (){
    if(arguments.length === 1){
        alert(arguments[0] + 10);
    }           // 检查参数的长度
    else if (arguments.length === 2){
        alert(arguments[0] + arguments[1]);
    }           // 检查参数的长度
}
doAdd(10);      //20
doAdd(10,20);   //30
```

将上面的代码进行修改完善，如下：

```js
// addMethod - By John Resig (MIT Licensed)
function addMethod(object, name, fn){
    var old = object[ name ];
    object[ name ] = function(){
        if ( fn.length == arguments.length )
            return fn.apply( this, arguments );
        else if ( typeof old == 'function' )
            return old.apply( this, arguments );
    };
}
```

所谓 `addMethod` 函数，简单的理解，就是给某个object，添加一个指定name的函数fn。它利用了闭包，可以通过old变量将先后绑定的函数链接起来。

<br>

**一个巧妙的办法实现重载：**

这个方法来自：JQuery之父John Resig写的《secrets of the JavaScript ninja》这本书。文章地址如下：

[原文链接](http://www.cnblogs.com/fundebug/p/9956850.html)

**<u><font color="red">addMethod 函数的秘诀之一在于 fn.length</font></u>** 。所有函数都有一个length属性，它的值等于定义函数时的参数个数。比如，当你定义的函数只有1个参数时，其length属性为1。

<br>
<br>
<br>
<br>
<br>

## 第四章 变量、作用域和内存问题

JavaScript 的变量与其他语言的变量有很大区别。变量松散类型的本质，决定了它只是在特定时间用于保存特定值的一个名字而已。

由于不存在定义某个变量必须要保存何种数据类型值的规则，变量的值及其数据类型可以在脚本的生命周期内改变。尽管 从某种角度看，这可能是一个既有趣又强大，同时又容易出问题的特性，但 JavaScript 变量实际的复杂 程度还远不止如此。

<br>

### 一、基本类型和引用类型的值

`ECMAScript` 变量包含两种不同数据类型的值：

- **基本类型值** (简单的数据段)：Undefined、Null、Boolean、Number 和 String；
- **引用类型值** (由多个值构成的对象)：Object；

**<u><font color="#282828">在将一个值赋给变量时，解析器必须确定这个值是基本类型值还是引用类型值</font></u>** 。基本数据类型是按值访问的，因为可以操作保存在变量中的实际的值，因此被保存在 **栈内存** 中。

引用类型的值是保存在内存中的对象，保存在 **堆内存** 中。包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针。与其他语言不同，**<u><font color="#282828">JavaScript 不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间</font></u>**。在操作对象时，实际上是在操作对象的引用而不是实际的对象。为此，引用类型的值是按引用访问的。

> 注意：在很多语言中， 字符串以对象的形式来表示， 因此被认为是引用类型的。 ECMAScript 放弃了这一传统。

<br>

***1. 传递参数***

`ECMAScript` 中所有函数的参数都是按值传递的。也就是说，把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。举例如下：

```js
function setName(obj) {
  obj.name = "Nicholas";
  obj = new Object();
  obj.name = "Greg";
  return obj;
}

var person1 = new Object();
var person2 = setName(person1);

alert(person1.name); //"Nicholas"
alert(person2.name); //"Greg"
```

> 可以把 ECMAScript 函数的参数想象成局部变量。

<br>

***2. 检测类型***

- 基本类型检测：typeof 操作符；
- 引用类型检测：instanceof 操作符；

```js
alert(person instanceof Object);    // 变量 person 是 Object 吗？
alert(colors instanceof Array);     // 变量 colors 是 Array 吗？
alert(pattern instanceof RegExp);   // 变量 pattern 是 RegExp 吗？
```

instanceof 操作符的返回值：`true` 和 `false` ；在检测一个引用类型值和 Object 构造函数时，instanceof 操作符始终会返回 true。如果使用 instanceof 操作符检测基本类型的值，则该操作符始终会返回 false。

<br>

### 二、执行环境及作用域

<br>

参考学习：[理解 JavaScript 作用域和作用域链](http://www.cnblogs.com/lhb25/archive/2011/09/06/javascript-scope-chain.html)

<br>

**<u><font color="red">每个执行环境都有一个与之关联的“变量对象”（variable object），环境中定义的所有变量和函数都保存在这个对象中</font></u>**。全局执行环境是最外围的一个执行环境。

在 Web 浏览器中，全局执行环境被认为是 `window` 对象，因此所有全局变量和函数都是作为 `window` 对象的属性和方法创建的。某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁。

<br>

***1. 作用域链***

当代码在一个环境中执行时，会创建变量对象的一个 **作用域链**（scope chain）。

- **作用域链的用途**：**<u><font color="#282828">保证对执行环境有权访问的所有变量和函数的有序访问</font></u>**。内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。

函数参数也被当作变量来对待，因此其访问规则与执行环境中的其他变量相同。

<br>

***2. 作用域链的延长***

当执行流进入下列任何一个语句时，作用域链就会得到加长：

- try-catch 语句的 catch 块；
- with 语句；

这两个语句都会在作用域链的前端添加一个变量对象。对 `with` 语句来说，会将指定的对象添加到作用域链中。对 `catch` 语句来说，会创建一个新的变量对象，其中包含的是被抛出的错误对象的声明。

<br>

***3. 没有块级作用域***

在其他类 C 的语言中，由花括号封闭的代码块都有自己的作用域，JavaScript 没有块级作用域，举例如下：

```js
// 例一：if语句中定义的变量color，在if语句执行完后不会被销毁
if (true) {
  var color = "blue";
}
alert(color);   //"blue"
```

```js
// 例二：for语句中定义的变量i，在for语句执行完后不会被销毁
for (var i=0; i < 10; i++){
  doSomething(i);
}
alert(i);       //10
```

对于有块级作用域的语言来说，`for` 语句初始化变量的表达式所定义的变量，只会存在于循环的环境之中。而对于 JavaScript 来说，由 `for` 语句创建的变量 i 即使在 `for` 循环执行结束后，也依旧会存在于循环外部的执行环境中。

<br>

- **声明变量：**

使用 `var` 声明的变量会自动被添加到最接近的环境中。如果初始化变量时没有使用 `var` 声明，该变量会自动被添加到全局环境。

> 在编写 JavaScript 代码的过程中，不声明而直接初始化变量是一个常见的错误做 法，因为这样可能会导致意外。

<br>

***4. 垃圾收集***

JavaScript 具有自动垃圾收集机制，执行环境会负责管理代码执行过程中使用的内存。

**原理**：找出那些不再继续使用的变量，然后释放其占用的内存。垃圾收集器会按照固定的时间间隔（或代码执行中预定的收集时间），周期性地执行这一操作。

<br>

- **标记清除：**

JavaScript 中最常用的垃圾收集方式是 **标记清除**（mark-and-sweep）。当变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”，而当变量离开环境时，则将其标记为“离开环境”。

<br>

- **引用计数：**

引用计数的含义是跟踪记录每个值被引用的次数，当代码中存在循环引用现象时，“引用计数”算法就会导致问题。这是一种不太常见的垃圾收集策略，JavaScript 引擎目前都不再使用这种算法。

<br>

- **性能问题：**

垃圾收集器是周期性运行的，如果为变量分配的内存数量很可观，那么回收工作量也是相当大。在这种情况下，确定垃圾收集的时间间隔是一个非常重要的问题。

<br>

- **管理内存：**

通常来说分配给 Web 浏览器的可用内存数量通常要比分配给桌面应用程序的少。这样做的目的主要是出于安全方面的考虑，目的是防止运行 JavaScript 的网页耗尽全部系统内存而导致系统崩溃。

**解除引用**（dereferencing）：为确保占用最少的内存获得更好的性能，最佳方式就是为执行中的代码只保存必要的数据，一旦数据不再有用，最好通过将其值设置为 `null` 来释放其引用。这一做法适用于大多数全局变量和全局对象的属性。

这一做法称为解除引用，举例如下

```js
function createPerson(name){
  var localPerson = new Object();
  localPerson.name = name;
  return localPerson;
}
var globalPerson = createPerson("Nicholas");

// 手工解除 globalPerson 的引用
globalPerson = null;
```

解除一个值的引用并不意味着自动回收该值所占用的内存。**<u><font color="#282828">解除引用的真正作用是让值脱离执行环境，以便垃圾收集器下次运行时将其回收</font></u>**。

<br>
<br>
<br>
<br>
<br>

## 第五章 引用类型

引用类型的值（对象）是引用类型的一个实例。**<u><font color="red">在 ECMAScript 中，引用类型是一种数据结构，用于将数据和功能组织在一起</font></u>**。

<br>

### 一、Object 类型

Object 也是 `ECMAScript` 中使用最多的一个类型。Object 是一个基础类型，其他所有类型都从 Object 继承了基本的行为。

创建 Object 实例的方法有两种：

- 使用 new 操作符后跟 Object 构造函数;
- 使用 **对象字面量** 表示法；

对象字面量是对象定义的一种简写形式，目的在于简化创建包含大量属性的对象的过程。举例如下：

```js
var person = {
  "name" : "Nicholas",
  "age" : 29,
  5 : true
};
```

<br>

### 二、Array 类型

Array 类型是一组值的有序列表，同时还提供了操作和转换这些值的功能。`ECMAScript` 中的数组与其他多数语言中的数组有着相当大的区别。

```
  ECMAScript 数组的每一项可以保存任何类型的数据。 也就是说，可以用数组的第一个位置来保存字符串，用第二位置来保存数值，用第三个位置来保存对象， 以此类推。
  ECMAScript 数组的大小是可以动态调整的，即可以随着数据的添加自动增长以容纳新增数据。
```

创建 Array 数组的基本方式有两种：

- 是使用 Array 构造函数（ var colors = new Array(); )；
- 是使用数组字面量表示法（ var colors = ["red", "blue", "green"]; ）；

<br>

***1. 检测数组***

自从 `ECMAScript 3` 做出规定以后，就出现了确定某个对象是不是数组的经典问题。

- instanceof 操作符（适用于全局执行环境）；
- `Array.isArray()` 方法，目的是最终确定某个值到底是不是数组，而不管它是在哪个全局执行环境中创建的；

<br>

### 三、Date 类型

`ECMAScript` 中的 `Date` 类型是在早期 Java 中的 java.util.Date 类基础上构建的。为此，`Date` 类型使用自 UTC（Coordinated Universal Time，国际协调时间）1970 年 1 月 1 日午夜（零时）开始经过的毫秒数来保存日期。

`Date` 类型提供了有关日期和时间的信息，包括当前日期和时间以及相关的计算功能。

<br>

### 四、RegExp 类型

`ECMAScript` 通过 `RegExp` 类型来支持正则表达式。RegExp 类型是 ECMAScript 支持正则表达式的一个接口，提供了最基本的和一些高级的正则表达式功能。

创建 RegExp 类型的两种方式：

```js
/*
 *  1、以字面量形式来定义的正则表达式
 */
var pattern1 = /[bc]at/i;

/*
 *  2、使用构造函数创建的正则表达式
 */
var pattern2 = new RegExp("[bc]at", "i");
```

<br>

### 五、Function 类型

`ECMAScript` 中函数实际上是对象。每个函数都是 `Function` 类型的实例，而且都与其他引用类型一样具有属性和方法。

由于函数是对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。

<br>

***1. 函数声明与函数表达式***

解析器在向执行环境中加载数据时，对函数声明和函数表达式并非一视同仁。

解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。请看下面的例子：

```js
/*
 *  1、函数声明语法：正常运行
 */
 alert(sum(10,10));
 function sum(num1, num2){
    return num1 + num2;
  }

/*
 *  2、函数表达式：运行报错
 */
alert(sum(10,10));
var sum = function(num1, num2){
   return num1 + num2;
 };
```

<br>

***2. 作为值的函数***

因为 `ECMAScript` 中的函数名本身就是变量，所以函数也可以作为值来使用。也就是说，不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。

<br>

 ***3. 函数内部属性***

函数内部，有两个特殊的对象：`arguments` 和 `this`。

<br>

 ***4. 函数属性和方法***

`ECMAScript` 中的函数是对象，因此函数也有属性和方法。



### 五、基本包装类型

三种基本包装类 型分别是：Boolean、Number 和 String。以下是它们共同的特征：

```
  每个包装类型都映射到同名的基本类型；
  在读取模式下访问基本类型值时，就会创建对应的基本包装类型的一个对象，从而方便了数据 操作；
  操作基本类型值的语句一经执行完毕，就会立即销毁新创建的包装对象。
 ```

<br>
<br>
<br>
<br>
<br>

## 第六章 面向对象的程序设计

面向对象（Object-Oriented，OO）的语言有一个标志，那就是它们都有类的概念，而通过类可以创建任意多个具有相同属性和方法的对象。`ECMAScript` 中没有类的概念，因此它的对象也与基于类的语言中的对象有所不同。

每个对象都是基于一个引用类型创建的，这个引用类型可以是第 5 章讨论的原生类型，也可以是开发人员定义的类型。

<br>

### 一、理解对象

<br>

***1. 属性类型***

`ECMAScript` 中有两种属性：**数据属性** 和 **访问器属性** 。












**<u><font color="red">样例红</font></u>**
**<u><font color="#282828">样例黑</font></u>**
