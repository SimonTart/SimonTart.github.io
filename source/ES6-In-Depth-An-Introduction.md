---
title: '深入ES6之迭代器和for-of循环'
date: 2017-01-04 21:04:32
tags:
---
深入ES6是一个系列介绍添加到ECMAScript第六版的标准中添加到JavaScript的新特性。ES6是ECMAScript第六版的一个简称。

你是如何遍历一个数组中的元素呢？在二十年前，JavaScript刚诞生的时候，你可能会像下面这样遍历数组：
```javascript
for(var index = 0; index < myArray.length; index++){
  console.log(myArray[index]);
}
```
从ES5开始，你可以使用内置的`forEach`方法：
```javascript
myArray.forEach(function (value) {
  console.log(value);
});
```
这样看比刚才简洁了，但是这里有一个小的缺陷，你不能通过`break`语句终止循环，也不能通过`return`语句从函数里返回到外面。
那么试一下`for-in`循环呢？
```javascript
for(var index in myArray){  //千万别真的这样做
  console.log(myArray[i]);
}
```
因为下面这些原因，这并不是一个好的选择。
- 这段代码中，赋值给`index`的值是字符串`"0"`，`"1"`，`"2"`等，并不是数字。后面你可能会产生你意料之外的字符串计算（`"2" + 1 = "21"`），这实在是太麻烦了。
- 这个循环不仅会遍历数组的元素，同样也会遍历其他任何添加在数组上的属性。例如：如果你的数组有一个可枚举的属性`myArray.name`，那么循环将额外执行一次，此时 `index == "name"`。即使属性在数组的原型链上，也是会被访问你的。
- 最让人吃惊的是，在某些情况下，这段代码以随机顺序遍历数组元素。
简单来说，`for-in`是设计用来遍历对象的字符串类型的键。对数组来说，并不是那么好用。

#### 强大的`for-of`循环
上周我曾许诺过，ES6不会破坏已经写好的代码。上百万的网站都使用了`for-in`,甚至用在了数组上。所以，从来没有想多通过修复`for-in`来支持数组。对ES6来说，唯一的改善问题的方法是添加一些新的循环语法。
就像这样：
```javascript
for(var value of myArray){
  console.log(value);
}
```
恩~，毕竟变化不大，看起来不是那么令人印象深刻。好吧，我们来看一下`for-of`的下面是否有什么小技巧。但是现在，只需要记住：
- 这是目前最简介和直接的方法来遍历数组的元素
- 它规避了`for-in`所有的的陷阱
- 不想`forEach`，它可以搭配`break`，`continue`和`return`使用。

#### 其它的集合也支持`for-of`
`for-of`不仅适用于数组，他同样也能用在大多数类数组的对象上。比如DOM NodeLists。
它同样也能在字符串上使用，它将字符串当做一组Unicode字符。
```javascript
for(var chr of "😺😲"){
  alert(chr);
}
```
它同样也能在`Map`和`Set`上使用。抱歉，你可能还没听说过`Map`和`Set`对象。他们是新加入ES6中的。我们会在某个时候专门写文章介绍他们。如果你已经在其他语言使用过maps和sets，那可能没有太多的区别。
例如，`Set`可以消除重复的值。
