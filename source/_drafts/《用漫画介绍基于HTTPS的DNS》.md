title: 《用漫画介绍基于HTTPS的DNS》
author: Trisolar
tags: []
categories:
  - TLDR
date: 2019-01-15 20:42:00
---
原文链接：[A cartoon intro to DNS over HTTPS](https://hacks.mozilla.org/2018/05/a-cartoon-intro-to-dns-over-https/)

## TLDR
隐私问题越来越被大家关注，但是目前的DNS因为以下原因还是会泄漏用户的信息：
1. 基于http协议的请求，在使用DNS的时候无法防止中间人攻击
2. 在使用DNS的时候，会带有一些不必要的信息导致用户信息泄漏，例如用户IP等
3. 联网时推荐的DNS解析服务者可能是不值得信任的

火狐通过以下方式解决上面的问题
1. 在使用基于https的DNS服务
2. 在DNS查询时，只发送必要的信息
3. 使用可靠的DNS解析服务。目前默认是Cloudflare，Cloudflare承诺不会泄露给其他人并且只保留24个小时，用户也可设置为其他服务商。

火狐仍然没有解决的问题：
1. 在和web服务连接时，初始化的请求仍然会暴露信息，因为这是未初始请求时未加密的