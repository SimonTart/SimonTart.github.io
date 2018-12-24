---
title: 第二章 在HTML中使用JavaScript
date: 2017-01-02 17:25:36
tags: JavaScript
---

## &lt;script&gt;元素
向HTML页面中插入JavaScript的主要方式是通过&lt;script&gt;元素。
&lt;script&gt;元素中有以下6个属性：
- `async` 可选，表示应该立即下载，下载完后立即执行。并且下载的过程不会阻塞页面中其他资源和脚本的下载，但是设置了该属性的脚本执行时仍然会阻塞页面的渲染和其他脚本的执行。并且只对外部脚本文件有效。
- `charset` 可选，表示通过src指定的代码的字符集。一般很少使用，因为浏览器通常会忽视它。
- `defer` 可选，表示脚本的执行可以安全的延迟到文档完全解析和展示之后。只对外部脚本文件有效，IE7及以下对内联脚本也有效。
<!-- more -->
- `language` 已废弃。最初用来表示使用的是哪种脚本语言（例如：JavaScript,VBScript）。大部分浏览器会忽视这个属性，这个属性不应该再使用。
- `src` 可选，表明将要执行代码的外部文件。
- `type` 可选，用来替代`language`，用来表示脚本内容的类型（通常也叫做MIME type），一般来说这个值通常是`text/javascript`,尽管`text/javascript`和`text/ecmascript`已经都被废弃了。服务器在传输的时候MIME类型通常是`application/x-javascript`（实际上似乎浏览器传输的MIME type是`application/javascript`），但是在type中设置这个值可能导致脚本被浏览器忽略。所以考虑到约定俗成和浏览器之间最大的兼容性，通常设置为`text/javascript`。但是这个值也是可以忽略的，当忽略的时候`text/javascript`会被设置为默认值。

##### &lt;script&gt;元素的使用
&lt;script&gt;主要有两种使用方式。
1.嵌入
像下面这样直接将JavaScript代码嵌入&lt;script&gt;元素中。
```javascript
<script type="text/javascript">
    function(){
      alert("Hello World!");
    }
</script>
```
这种使用方式中，JavaScript代码中任何地方都不能出现&lt;/script&gt;字符串，因为浏览器在解析嵌入的JavaScript代码的时候会把字符串&lt;/script&gt;当做闭合标签&lt;/script&gt;。但是可以通过转移符使用。如下：
```HTML
<script type="text/javascript">
    function(){
      alert("<\/script>");
    }
</script>
```
2.引入外部JavaScript文件
这种方法需要指定src为外部需要引入JavaScript文件的路径，像下面这样。
```HTML
<script type="text/javascript" src="example.js"></script>
```
使用这种方式需要注意的是在HTML中是不能省略闭合标签`</script>`的，但是在XML中是可以的。

注意：
当你引入了外部JavaScript文件时，如果你又在&lt;script&gt;中嵌入了内联JavaScript代码。引入的外部JavaScript文件会正常加载并执行，嵌入的代码会被忽略。&lt;script&gt;元素中的src可以指定为外部域名的JavaScript文件。所以加载外部域名的文件的时候要特别小心，以免引入了恶意的JavaScript文件。

##### 标签的位置
过去的方式是把&lt;script&gt;放在页面的&lt;head&gt;元素内部，向下面这样：
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Example Page</title>
        <script type="text/javascript" src="example1.js"></script>
        <script type="text/javascript" src="example2.js"></script>
    </head>
    <body>
    <!-- content -->
    </body>
</html>
```
这种写法的目的是为了让外部引用（css和JavaScript文件）都在相同的区域，但是这样也产生了一个问题。&lt;head&gt;内部所有的JavaScript下载，解析，执行完成之后，页面才会开始渲染（页面的展现是从遇到body标签开始的）。这种情况下会产生能被人注意到的渲染延迟，在这段期间页面完全是一片空白。所以在现代网页中中，JavaScript的外部引用都被放在body标签中，并且在页面内容的后面。

#### 延迟脚本
HTML4.01为&lt;script&gt;定义了一个`defer`属性，表明脚本执行时不会改变页面的结构，脚本可以再整个页面都解析之后在执行。设置了`defer`的脚本的表明应该立即下载，但是执行会延后，延后到浏览器解析到&lt;/html&gt;标签之后执行。HTML5中规定了，设置了`defer`属性的脚本需要按照他们出现的顺序执行，并且是在`DOMContentLoaded`事件之前执行。但实际上，设置了`defer`的脚本并不一定按照顺序执行，也不一定在`DOMContentLoaded`事件之前执行。在HTML5中明确规定了`defer`只能用于外部脚本文件，因此会对那些内联的脚本忽略`defer`属性。但是在一些对HTML5不完全支持的浏览器中还是会支持内嵌的设置了`defer`的脚本（例如IE7及以下）。

##### 异步脚本
HTML5中为&lt;script&gt;元素定义了`async`属性，和`defer`相似，这个属性告诉浏览应立即下载脚本，下载完成后立即执行。和`defer`不同的是这个属性不会确保脚本的执行顺序。这个属性的目的是为了让页面不必等待的设置了`async`的脚本的下载和执行，同时这些脚本也不需要等待其他脚本下载和执行。设置了`async`的脚本会在`load`事件之前执行，会在`DOMContentLoaded`事件之前或者之后执行。

##### 引入外部文件的脚本，延迟脚本，异步脚本
ps:下面的脚本都是指引入外部JavaScript文件的脚本
脚本的下载和执行一般来说是会阻塞文档继续解析，只有等到脚本下载和执行完了之后才会继续解析文档。由于现代浏览器会为了加快页面呈现的速度，现代浏览器遇到了脚本之后仍然会解析文档后面的部分，但是文档的渲染会阻塞。所以浏览器出现了并行加载，顺序执行脚本的现象。所以一般来说普通脚本，延迟脚本，异步脚本基本都是立即下载（不考虑域名连接数限制），只是执行的时机不同，不过执行的时候都会阻页面的渲染和其他脚本（主要是因为单线程）。这三种情况的下载和执行可以用下图总结：

{% img /images/Chapter2-JavaScript-in-HTML/asyncdefer.svg 840 140 %}
##### 在XHTML中的用法
ps:HTML5已经被前端大量的使用了，感觉前端都没人用XHTML了，所以本小段关于XHTML的可以跳过。
可扩展超文本标记语言，即 XHTML(Extensible HyperText Markup Language)，是将 HTML 作为 XML 的应用而重新定义的一个标准。
在XHTML中，&lt;script&gt;元素内嵌代码有着更加严格的标准（比如<会被解释为一个标签的开始），所以通常将JavaScript代码包裹在一个CDATA的区域，这样可以使用宽松格式的代码，在这里面可以使用任意字符且不会导致错误。就像下面这样：

```html
<script type="text/javascript"><![CDATA[
    function compare(a, b) {
        if (a < b) {
            alert("A is less than B");
        } else if (a > b) {
            alert("A is greater than B");
        } else {
            alert("A is equal to B");
        }
    }
]]></script>
```
以上代码在兼容XHTML中的浏览其中可以正常运行，但是不兼容的怎么办呢？可以使用JavaScript的注释把CDATA标记给注释掉。向下面这样：
```html
<script type="text/javascript">
//<![CDATA[
    function compare(a, b) {
        if (a < b) {
            alert("A is less than B");
        } else if (a > b) {
            alert("A is greater than B");
        } else {
          alert("A is equal to B");
        }
    }
//]]>
</script>
```
##### 废弃的语法
在最开始的时候，并不是所有的浏览器都支持&lt;script&gt;元素，所以不支持的浏览器会把内嵌的代码显示出来，解决方案就是将内嵌的脚本用HTML注释包裹起来。不支持的浏览器就会忽视内嵌的代码，想下面这样使用：
```javascript
<script type="text/javascript"><!--
    function(){
        alert("Hello World!");
    }
--></script>
```

## 内嵌代码和外部文件对比
虽然内嵌JavaScript代码没有什么问题，但是尽量使用外部文件通常被认为是最佳实践。因为使用外部脚本文件有以下优点：
- 可维护性 在一个文件夹下面维护JavaScript文件里的代码总是会比在页面的不同地方去维护方便的许多。
- 缓存 通过一些配置，浏览器可以缓存所有的外部JavaScript文件，意味着不同的页面使用相同的地址的JavaScript文件，该文件只会下载一次。最终页面会加载得更快。
- 未来的趋势 这样写不需要考虑XHTML的兼容性，因为外部文件引用的格式同样适用于XHTML。

## 文档模式
Internet Explorer 5.5 第一次使用了文档模型的概念，并且可以通过文档类型（doctype）切换。

- 怪癖模式/混杂模式（quirks mode）,该模式会让浏览器的的行为与IE 5和Navigator 4行为相同（包括那些非标准的特性）,这种模式主要是为了兼容性的考虑。主要是因为在最开始的时候网页有两个版本，一个是Microsoft Internet Explorer一个是Netscape Navigator，当W3C标准化了网页标准的时候，为了兼容以前的页面，就诞生了这种模式。
- 标准模式（full standards mode）,老版本IE的行为会更加接近标准，现代浏览器则是会按照CSS和HTML规范渲染。
- 标准准模式（almost standards mode），IE的一种模式，接近标准，但是仍然会有一些怪异的表现。

doctype除了可以切换文档模式，同时也是为了告诉浏览器使用了HTML的哪个版本书写了文档。如果没有申明文档类型，浏览器会默认使用怪癖模式。目前来说都是使用标准模式，因为怪癖模式会发生一些奇怪的现象，并且不同浏览器下的怪癖模式都不一样，需要花费大量时间去兼容。
标准模式可以通过一下方式开启:
```html
<!-- HTML 4.01 严格型 -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<!-- XHTML 1.0 严格型 -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<!-- HTML 5 -->
<!DOCTYPE html>
```
而对于准标准模式，则可以通过使用过渡型(transitional)或框架集型(frameset)文档类型来触发，如下所示:
```html
<!-- HTML 4.01 过渡型 -->
<!DOCTYPE HTML PUBLIC
"-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<!-- HTML 4.01 框架集型 -->
<!DOCTYPE HTML PUBLIC 9 "-//W3C//DTD HTML 4.01 Frameset//EN"
"http://www.w3.org/TR/html4/frameset.dtd">
<!-- XHTML 1.0 过渡型 -->
<!DOCTYPE html PUBLIC
"-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<!-- XHTML 1.0 框架集型 -->
<!DOCTYPE html PUBLIC
"-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

#### &lt;noscript&gt;元素
早期的页面面临着一个问题，对不支持JavaScript的浏览器如何让页面平稳退化。解决方案就是&lt;noscript&gt;元素，对于不支持JavaScript的浏览器&lt;noscript&gt;内部的内容就会显示出来。&lt;noscript&gt;的内容会在以下两种情况下显示：
- 浏览器不支持JavaScript
- 浏览器支持JavaScript，但是JavaScript被禁用了
