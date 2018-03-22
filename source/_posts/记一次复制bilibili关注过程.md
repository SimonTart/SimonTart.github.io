---
title: 记一次复制bilibili已关注用户到新账号的过程
date: 2018-03-22 20:40:02
tags: Bilibili
---
　　起因是朋友的Bilibili账号没有绑定手机号导致没办法发弹幕，但是因为注册邮箱用的是别人所以没有办法绑定手机。如果注册新的账号的话老账号需要重新一个一个的关注之前已经关注的用户实在是麻烦了，于是救助于我。好在朋友的关注列表没有分类，只需要简单的重新关注一遍就可以了，不需要分类。作为程序员，这种事儿当然义不容辞了。
　　思路其实比较简单，就是把原账号的关注的用户全部获取到然后重新关注一遍。于是关键在于找到获取关注列表的接口和关注其他用户的接口。审查请求之后很容易就能找到如下信息：

获取关注列表接口：
```
url: https://api.bilibili.com/x/relation/followings
method: get
参数：vmid 用户id
    pn pageNumber
    ps pageSize
    order 顺序
    jsonp jsonp跨域
```

关注其他用户的接口：
```
url: https://api.bilibili.com/x/relation/modify
method: post
参数： fid: 要关注的用户id,
    act: 动作，关注or取消关注，1代表关注
    re_src: 不知道是什么，像是request source,					
    csrf: 用于防止跨站请求伪造的Token
```

通过打断点可知csrf是从cookie中获取。于是通过如下代码就可以复制另一个账号的关注列表到自己账号上：
```javascript
// 可以直接使用$.ajax是因为B站引入了jQuery
(function(){
csrf = document.cookie.split('bili_jct')[1].split(';')[0].slice(1); // 获取token
$.ajax('https://api.bilibili.com/x/relation/followings', {
    dataType: 'jsonp',
    data: 'vmid=xxxxx&pn=1&ps=10000&order=desc&jsonp=jsonp' // xxxxx代表要复制账号的id,通过进入这个人的主页，查看url上的数字获知。例如 https://space.bilibili.com/2572936/#/ 中2572936就是用户的id
})
.then((data) => {
    for(let u of data.data.list) {
        $.ajax('https://api.bilibili.com/x/relation/modify', {
            type: 'post',
                xhrFields: {
                withCredentials: true // 这里设置了withCredentials
            },
            data: {
                fid: u.mid,
                act: 1,
                re_src: 11,					
                csrf: csrf
            }
        }).then((res) => console.log(res))   
    }
})
})()
```
至此把代码复制给我同学，然后再控制台里面执行一下就可以了。
