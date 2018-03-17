---
title: 第三章 JavaScript基本概念
date: 2017-01-02 21:34:39
tags: 
- JavaScript
---

## 一 语法
JavaScript的语法大量借鉴C语言和其他的类C语言（例如：Java和Perl）。
##### 大小写敏感
JavaScript里面所有的东西都是大小写敏感的。变量，函数名和操作符等都是大小写敏感的。
<!-- more -->
##### 标识符
标识符是变量，函数，属性，函数参数的名字。标识符按照下列格式由一个和多个字符组成的。
- 第一个字符必须是字母、下划线（_）或者一个美元符号（$）
- 其他的字符必须是字母、下划线、美元符号或者数字
##### 注释

单行注释由两个`/`组成

```javascript
// single line comment
```

多行注释由`/*`开始，`*/`结尾

```javascript
/*
 * this is multi-line
 * comment
 */
```
##### 严格模式
ECMAScript5中介绍了严格模式的概念（strict mode）,严格模式对JavaScript来说一种不同的解析和执行方式。严格模式中，ECMAScript3中的一些不确定行为会得到处理，同时对一些不安全的行为也会抛出异常。
你可以在整个脚本的顶部写下下面的代码使整个脚本进入脚本模式。
```javascript
"use strict"
```
你也可以通过把`"use strict"`放在函数的顶部让函数在严格模式下执行，像下面这样：
```javascript
function doSomething(){
    "use strict"
    // function body
}
```
`"use stirct";`看上去就是一个普通的字符串。选用这种方式是为了不破坏ECMAScript3语法的同时让支持严格模式的JavaScript引擎使用严格模式。

##### 语句
ECMAScript的语句以分号（;）结束，分号也是可以省略的。如果省略，解释器会自动确定分号的位置。
```javascript
var sum = a + b
var diff = a - b;
```
尽管分号可以省略，但是还是建议加上。这样可以避免很多一些因为省略分号导致的错误，也可以方便后面压缩代码。在某些情况下加上分号也能提高性能。
多行代码可以组合到代码块中，代码块以`{`开始，`}`结束。
```javascript
if(test){
    test = false;
    alert(test);
}
```
一般来说，只有当条件控制语句（例如if）执行多行代码的时候才需要代码块，但是最佳实践是条件控制语只需要执行一行代码也最好加上代码块。这样可以让代码结构更加清晰。

### 二 关键字和保留字
ECMA-262描述一些用于特殊用途的关键字。例如可以表示控制语句的开始和结束，以及执行一些特殊的操作。例如：`var`、`break`、`if`等就是关键字。

ECMA-262描述了一些现在没有特殊用途，但是先保留起来，以便将来当做关键字使用的保留字。例如`int`、`abstract`等，虽然现在不是关键字但是将来有可能是关键字。

ECMAScript具体每个版本的关键字保留字可以参考[这里](https://mathiasbynens.be/notes/reserved-keywords)。

在非严格模式下有一些保留字（例如`extends`、`super`等）可以使用，但是最好不要使用关键字作为标识符和属性名。

在ECMAScript3中关键字和保留字都不能作为标识符和属性名，但是ECMAScript5中啥稍微改变了一下规则，关键字和保留字都不能作为标识符，但是用作属性名。

### 变量
ECMAScript的变量是松散类型的，松散类型意味着变量可以保存任何类型数据。每个变量仅仅是用于保存值得占位符而已。我们可以通过`var`操作符后面跟上一个变量的名字定义一个变量。
```javascript
var message;
```
上面的代码定义了一个message变量，对于未初始化的变量。他保存着一个特殊的值`undefined`（这个值会在后面详细介绍）。当变量初始化了一个值之后，后面也有也可以改变变量的值或者类型。像下面这样：
```javascript
var message = "hi"; //变量message初始化为字符串"hi"
message = 123; //变量message被重新赋值为数字123，改变了message存储的值得类型。
```
使用`var`定义的变量会成为定义该变量的作用域的局部变量。例如在函数内部利用`var`定义变量，函数执行完之后局部变量就会被销毁。
```javascript
function test(){
    var message = "hi"; //局部变量
}
test();
alert(message); //错误
```
当生命变量的时候省略`var`，那么这个变量就是全局变量。
```javascript
function test(){
    message = "hi"; //局部变量
}
test();
alert(message); //hi
```
用一条声明也可以声明多个变量。变量之间用`,`号隔开。
```javascript
var message = "hi",
    name = "zhangsan",
    age = 22;
```
### 数据类型
ECMAScript中定义了五种简单数据类型（也叫基本基本数据类型）：Undefined、Null、Boolean、Number、String。也有一种复杂的数据类型Object。Object是一组无序的名值对（键值对）组成。
因为ECMAScript中变量是松散类型，所以需要一种方法来判断变量存储的数据类型。`typeof`可以提供这方面的信息，`typeof`根据值返回一下字符串：
- undefined，如果值是未定义（undefined）
- boolean，如果值是布尔值(Boolean)
- string，如果值是字符串（String）
- number，如果值是数字（Number）
- object，如果值是对象或者Null
- function，如果值是函数

##### Undefined类型
Undefined类型只有一个值undefined，当变量没有初始化的时候undefined会自动赋值给变量，当然你也手动给一个变量赋值为undefined。
```javascript
var message;
alert(typeof message);//undefined
var name = undefined;
```
##### Null类型
Null是第二个只有一个值的数据类型。他只有一个值null。从逻辑角度看，null代表一个空对象指针。这也是为什么用typeof判断null的类型时返回的是object。
undefined是由null派生而来，所以null==undefined返回true。

##### Boolean类型
Boolean类型有两个值：true、flase。
虽然Boolean类型只有两个值，但是任何类型的值都可以通过Boolean()方法转化为Boolean值。下表列出了不同值得转换规律：

数据类型|转换为true的值|转换为false的值
-----|-----|-----
Boolean|true|false
String|任何非空字符串|空字符串（""）
Number|任何非0数字（包括无穷大）|0或者NaN
Object|任何非空对象|null
Undefined|没有值|undefined

##### Number类型
ECMAScript中的数字类型使用IEEE-754格式来表示整数和浮点数。
1.整数
整数你可以使用十进制，也可以使用八进制（以0开头），十六进制（0x开头，大小写不敏感）。十六进制是由0-9，A-F组成的，字母是不区分大小的的。
```javascript
var intNum = 55; //十进制整数55
var octalNum1 = 070; //八进制数，等于十进制56
var octalNum2 = 079; //不合法的八进制，所以被解释为十进制79
var hexNum1 = 0xA; //十六进制，等于十进制10
var hexNum1 = 0X1f; //十六进制，等于十进制31
````
2.浮点数
浮点数必须是十进制，为了定义浮点数你必须包含一个小数点。通常小数点前面的整数是0的话可以省略。但通常不推荐省略。
```javascript
var floatNum1 = 1.1;
var floatNum2 = 0.1;
var floatNum3 = .1;
```
如果小数点后没有跟任何数字，那么他会变成整数。同样的，如果浮点数本身的值表示的是一个整数，那么它也会变成整数。
```javascript
var floatNum1 = 1.;
alert(floatNum1); // 1
var floatNum2 = 1.0;
alert(floatNum2); // 1
var floatNum3 = 1.1;
alert(floatNum3); // 1.1
```

对于特别大和特别小的数，可以用科学计数发表示。
```javascript
var floatNum = 3.17e7; //等于31700000
```
上面的这种情况用科学计数法表示会更加简洁。上面的科学计数法本质上是表示3.17乘以10^7。
对于特别小的数，例如：0.00000000000000003，可以间接地用3e-17表示。
浮点数可以精确到小数第17位，但是浮点数之间的算数运算没有整数来得精确。例如，
0.1+0.2的值是0.30000000000000004。并不是所有的浮点数的运算都像0.1+0.2那样是不精确的。比如：0.25+0.05的结果是0.3。

3.数值的范围
ECMAScript并不能代表世上所有的数字，ECMAScript能表示的最小的值保存在`Number.MIN_VALUE`中，在大多数浏览器中是5e-324。最大的值存储在`Number.MAX_VALUE`中，在大多数浏览器中是1.7976931348623157e+308，如果计算的数值超过了这个范围，那么通常会自动获得一个特殊的值`Infinity`，正无穷用`Infinity`表示，负无穷用`-Infinity`表示。我们可以通过`isFinite`方法判断一个数是否在最大值和最小值之间，如果需要的话，传入的值会先转换为数字。例如：isFinite("1e20")这种情况。转换规则和Number()函数一样，后面会介绍。

4.NaN
这是一个特殊的数字值叫NaN，是Not a Number的简写。迎来表示操作数本来是要返回数字的但是失败了。例如"123"/123返回的就是NaN。任何涉及到NaN的操作都会返回NaN。NaN不和任何值相等，包括他自己。例如：NaN == NaN 返回false。所以ECMAScript提供了isNaN来判断一个值是不是NaN。isNaN在会尝试将传入的参数转换为数字，若转换结果为数字则返回true，若转化结果是NaN则返回true。这种转换规则和Number()函数一样，后面会介绍。

##### 未完待续

##备注
感觉这种总结的方式太低了，毕竟写文章需要写的详细。后面的总结还是放到私人的笔记里面去。这样可以简洁一些。