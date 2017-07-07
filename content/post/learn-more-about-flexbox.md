+++
categories = ["css"]
date = "2017-06-21T12:35:43+08:00"
tags = []
title = "learn more about flexbox"

+++

好像真的看完就忘記了，所以就試著打一篇筆記看看吧～

主要是有關flexbox在使用的時候，好像不是那麼會控制裡面的東西，看了這篇文章：
[https://hackernoon.com/11-things-i-learned-reading-the-flexbox-spec-5f0c799c776b]

覺得很值得學習！

## 1.Margins 有特殊能力

看了他的第一個範例，我就發現我很少用到flex這個屬性，應該要常用XD

至於 margin 的功用，就是讓某個child element 不延伸寬度而自然而然地待在右邊或左邊（可以這樣說嗎？）

## 2.min-width 是重要的

這個主要應該是說 child element 的 min-width 屬性 default 是 auto

碰到像他範例中有一邊的文字過長時，可以將 min-width 設成 0，來保持另外一邊的 child element 不被擠出去

## 3.flex 縮寫

flex: initial 等於

>If I want an item to squish in a bit if there isn’t enough room, but not to stretch any wider than it needs to: flex: 0 1 auto

flex: auto

>If my flex item should stretch to fill the available space and squish in a bit if there’s not enough room: flex: 1 1 auto

flex: none

>If my item should not flex at all: flex: 0 0 auto


## 4.inline-flex

就是 inline 的 flexbox?

## 5.在flex-item上 vertical-align 沒有作用


## 6.不要用 % margins 或 padding


## 7.鄰近的flex item 的 margins 不會 collapse


## 8.z-index works


## 9.flex-basis 很微妙

<img src="https://cdn-images-1.medium.com/max/800/1*eiAn12jGzun4F7U3mfqUtQ.png">


## 10.align-items: baseline

可以用喔～


文末還提供一個連結 是用來 紀錄目前flexbox 的bugs的！
[https://github.com/philipwalton/flexbugs]


趕快多用flexbox吧！