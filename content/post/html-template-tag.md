+++
categories = ["html"]
date = "2017-07-05T10:13:12+08:00"
tags = []
title = "html template tag"

+++

最近才發現有`<template>`這個 html tag 可以用...

看了Mozilla的[template文件](https://developer.mozilla.org/zh-TW/docs/Web/HTML/Element/template) 記錄一下學到了什麼。

顧名思義它就是template，他裡面放的就是一般的html，取用裡面的物件跟設定內容的方式也跟其他html很像。

可以用 `var t = document.querySelector('#id')` 來取到某個template

我們可以用 `t.content` 來取得這個template的content

在t.content裡面設定物件內容 似乎跟一般的html一樣，像是用 `textContent`

主要是要把塞完內容的template放到dom裡面，是要用到`importNode`：

```
var clone = document.importNode(t.content, true);

tb.appendChild(clone);

```

有個好處是可以重複使用，像是範例中的 var td 是指 template裡面的td

這個td在第一次塞完值之後，還可以繼續使用來塞第二次的值，

畢竟，他就是一個樣板啊～


目前看起來只有IE沒有support?

但是...

我覺得vue.js的template厲害太多了，不如就用vue吧！XD




