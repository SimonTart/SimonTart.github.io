---
title: 第一章 JavaScript简介
date: 2016-12-25 12:04:12
tags:
- JavaScript
- ECMAScript
---

#### JavaScript历史
   JavaScript是在1995年出现的，当时最开始的目的是为了在浏览器端做一些用户输入的验证。因为当时网速太慢，用户输入表单提交到后端，等待了半天发现浏览器返回的信息是某个必填的选项没填。这个时候就想要把这些检查放在浏览器端，这个时候Netscape希望通过一门在能在浏览器运行的脚本语言解决这个问题。于是当时在Netscape工作的Brendan Eich负责设计这个语言。最开始叫做LiveScript，后来为了想搭上当时Java的顺风车在发布时改成了JavaScript。并且Netscape在 Netscape Navigator 3 中发布了JavaScript，随后微软也在自家的浏览器Internet Explorer中也弄了一个叫做JScript的JavaScript的实现。这个时候市场上出现了两个不同的JavaScript的实现，开发者也越来越担心兼容性这个问题。所以1997年JavaScript1.1被提交到欧洲计算机制造商协会(ECMA，European Computer Manufacturers Association)，协会指定TC39号技术委员会负责“标准化一种通用、跨平台、供应商中立的脚本语言的语法和语义”，TC39 由来自 Netscape、Sun、微软、Borland 及其他关注脚本语言 发展的公司的程序员组成，他们经过数月的努力完成了 ECMA-262——定义一种名为 ECMAScript的新脚本语言的标准。
<!-- more -->
#### JavaScript实现
JavaScript由三个部分组成：
- 核心 ECMAScript
- 文档对象模型 (DOM Document Object Model)
- 浏览器对象模型 (BOM Browser Object Model)

  ##### ECMAScript
  ECMAScript主要定义了语言的以下部分：
  - 语法（Syntax）
  - 类型（Types）
  - 语句（Statements）
  - 关键字（Keywords）
  - 保留字（Reserved Words）
  - 操作符（Operators）
  - 对象（Objects）

  ECMAScript并不和浏览器绑定，浏览器只是ECMAScript实现的宿主环境之一。ECMAScript在其他地方也有实现，比如：Node,ActionScript。
  ##### ECMAScript版本
  - ECMAScript1 主要是将提交的javascript1.1中和浏览器相关的部分移除，并且支持了Unicode标准以及让对象保持平台独立性。
  - ECMAScript2 主要是进行了大量的编辑工作，并没有添加，改变删除任何特性
  - ECMAScript3 这一版是第一次真正的更新，添加了字符串操作，错误的定义以及数字的输出，也支持正则表达式，新的控制语句和try-catch异常。为了更好的国际化也做了一些小的相关改变。
  - ECMAScript4 这一版是一次大的改变。包括了强类型变量，新的语句，新的数据结构，真正的类和类继承，新的数据交互方法。TC39委员会的一个子委员会（因为他们认为第四版的改动实在是太大了）提交了一个替补的3.1版本的草案，3.1版本相对来说是改变更小，能够在现有的JavaScript引擎上实现。最终3.1版本赢得了大多数的支持，第4版在正式发布之前就被废弃了。
  - ECMAScript5 因为第4版被废弃，所以最终3.1版本成为了第5版的ECMAScript。
  - ECMAScript6 ECMAScript6是2015年发布的，添加了新的语法，新的数据类型以及新的数据交互方式。极大地提高了JavaScript的生产效率，不过目前ECMAScript6在浏览器端的支持并不理想。需要通过`Babel`转为ECMAScript5的语法才能下浏览器中兼容。

  ##### 文档对象模型（DOM）
  文档对象模型是针对XML但是经过扩展用于HTML的API。文档对象模型把页面映射为一个有多层节点的结构，让用户可以用API对节点进行增删改查，这样的好处就是不用刷新页面就该修改页面的外观和内容。文档对象模型提供了访问和操作网页内容的方法和接口。例如我们通过DOM可以遍历节点以及操控DOM的样式以及监听DOM事件。

  ##### 浏览器对象模型（BOM）
  浏览器对象模型能让开发者操控浏览器的窗口，让开发者可以和页面以外的部分进行交互。浏览器对象提供了和浏览器窗口交互的方法和接口。例如可以通过BOM可以跳转页面，控制浏览器窗口的大小以及打开新的窗口等。
