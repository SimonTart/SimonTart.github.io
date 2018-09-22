title: HTTP请求的GET，DELETE请求能有body吗？
author: Trisolar
tags:
  - http
  - code
  - get
  - delete
  - 请求实体
categories:
  - code
date: 2018-09-22 11:44:00
---
昨天上班的时候后端有一个`delete`请求，要求我把参数放在`url`的`query string`上面。于是我说其实可以放在请求的实体中，但是后端说`delete`和`get`是没有请求实体的。这和我的记忆不太一样。那么到底`delete`和`post`请求能有实体吗？

## 先看网上的资料
先通过简单的搜索在stack overflow找到一个类似的问题, [Is an entity body allowed for an HTTP DELETE request?](https://stackoverflow.com/questions/299628/is-an-entity-body-allowed-for-an-http-delete-request)。回答中说到并没有禁止和不推荐在`get`和`delete`方法中使用实体。意思就是说其实是可以使用的。那么再看回答中给出的规范的资料，确实http规范中并没有说明有任何方法不能使用
实体。

## 继续探究
但是有时候现实生活往往和规范不一致。所以我们还要试验一下，首先是我们先用`node`搭建一个服务器，打印收到的实体：
```js
const http = require('http');

http.createServer((req, res) => {
    const { method, url } = req;
    console.log(`METHOD: ${method}`);
    console.log(`URL：${url}`);
    const body = [];

    req
    .on('data', (chunk) => {
        body.push(chunk)
    })
    .on('end', () => {
        let bodyString = Buffer.concat(body).toString();
        console.log(`BODY: ${body}`)
        res.end(bodyString || 'body is empty');
    });
}).listen(3000);
```
### 在浏览器中使用ajax测试

#### get请求
在浏览器中打开`localhost:300`页面,并且在控制台执行以下代码：
```js
(function() {
    const ajax = new XMLHttpRequest();
    ajax.open('get', 'http://localhost:3000?qs=qs');
    ajax.send('name=bob');
})()
```
服务器打印：
```
METHOD: GET
URL：/?qs=qs
BODY:
```
没有接收到body

#### delete请求
`delete`请求：
在浏览器中打开`localhost:300`页面,并且在控制台执行以下代码：
```js
(function() {
    const ajax = new XMLHttpRequest();
    ajax.open('delete', 'http://localhost:3000?qs=qs');
    ajax.send('name=bob');
})()
```
服务器打印：
```
METHOD: DELETE
URL：/?qs=qs
BODY: name=bob
```
接收到body
### 使用curl命令测试
#### get请求
命令行中执行下面的命令：
```bash
curl -X GET -d 'name=bob'  localhost:3000
```
服务器打印：
```
METHOD: GET
URL：/
BODY: name=bob
```
接收到body

#### delete请求
```bash
curl -X DELETE -d 'name=bob'  localhost:3000
```
服务器打印：
```
METHOD: DELETE
URL：/
BODY: name=bob
```
接收到body

发现`curl`命令的body都是可以正常发送，但是浏览器中的ajax却不行这是为什么呢？查看`XMLHttpRequest`规范发现：
> The send(body) method must run these steps:
> 	1. If state is not opened, then throw an "InvalidStateError" DOMException.
	2. If the send() flag is set, then throw an "InvalidStateError" DOMException.
	3. If the request method is GET or HEAD, set body to null.
	4. If body is not null, then:
	5.  ....

原来`XMLHttpRequest`规范定义method时get或者head的时候，XMLHttpRequest会忽略body。所以会产生前面实验的现象。

#### 那么这些请求到底有什么不一样呢
其实从http报文的角度看，他们完全都是一样的。没有任何区别，大家能发送的信息都是一样的。你能做的我也能做，不一样的在于`method`所代表的这个请求的语义。

#### 结论
最后总结一下，在http规范任何方法都能发送请求实体。他们报文时没有任何区别的，但是在浏览器中，因为`XMLHttpRequest`规范的限制，浏览器中ajax发送的http请求，`get`，`delete`请求不能携带实体。