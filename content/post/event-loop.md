+++
categories = ["javascript"]
date = "2017-09-01T08:56:54+08:00"
tags = ["javascript"]
title = "event loop"

+++

主要是看了Philip Robert的文章：[https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html](https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html)，獲益良多～

他把event發生之後 javascript 跟 browser怎麼運作，用圖示跟動畫解釋得很清楚，甚至還做了一個demo網站[Loupe](http://latentflip.com/loupe/)，非常厲害！

這裡還有一篇大陸大神寫的文章也是參考過Philip Robert：http://www.ruanyifeng.com/blog/2013/10/event_loop.html

主要筆記開始:

javascript 是 single-thread，所以正常情況下，一次只能處理一件事情。也就是一個function在跑的時候，會擋住其他的事情，很有可能這時候使用者無法在browser上做任何事。

browser內會有一個叫"stack"的東西，用來存放要執行的javascript，這邊的確是single-thread，那麼當我們跑 setTimeout, ajax 這種 asyncronous的動作的時候，又是怎麼運作呢？

這時候就要說到 callback queue了，這類asyncronous都會讓我們可以寫一個callback，這個callback就會被丟到callback queue去排隊，等到stack那邊沒事要做了，callback queue裡面的東西就會被拿到stack執行。假如說現在用setTimeout的話，執行後就會由web api接手，等到setTimeout設定的時間到的時候，就把callback function丟到callback queue去；當stack這邊已經沒有事情要做，event loop的作用就是去callback queue找排第一個的事件，把它拉到stack來執行，當stack又空了，event loop再次作用去callback queue拿事情來做。

所以！這時候有個驚人的事實，就是：用setTimeout的時候，那個時間參數只是最少會等待的時間，而不是一定會依照那個時間執行！因為他也會只少等stack清空之後才可以跑喔！～～我終於恍然大悟以前有時候某些plugin不知為何不動作時，要加下面這樣才會跑。
```
setTimeout(function() {
    // 做某些事情
}, 0);
```
可能那個function在當下被卡住，要讓他移動一下，才會動。（好隨便的解釋XD）
依據阮一峰大師的文章，node js裡面也有一個process.nextTick()，感覺是同樣的功能。


## 額外資訊


## web workers
之前就知道web workers可以同時在背景進行一些事情，但實在是不懂，如果javascript是single thread怎麼能夠同時進行呢？

文件中是說，他是在“背景執行緒”中執行，不干擾使用者。他可以做的事情除了操作DOM的事情是不被允許外，其他幾乎都可以。自己還沒有實際用過所以只能這樣很抽象地去了解一下。關於service worker應該要另外研究寫筆記了。

## event bubbling and capture
後來又看到這篇[https://javascript.info/bubbling-and-capturing](https://javascript.info/bubbling-and-capturing)，也覺得很清楚喔，也獲益良多喔。
通常我在用event的時候（像onclick），都會有一個event target，但註冊的function裡面也會可以用this，原來event target跟this不是一樣的喔！因為event是會bubbling的，就是雖然你現在點的是一個`<a>`但是，如果他的母層也都有註冊click事件，他們會由裡到外的去跑一遍。

然後又學到一個不知道的事情，就是其實還有一個capture的階段！詳細一點說，當一個event發生，第一個階段是capture，就是從最基本的doom root開始往下跑，跑到event target（事件觸發的位置）；再來是target階段，最後才是bubbling階段。而且如果在addEventListener的第三個參數（完全不知道有這個參數）設定為true，這個function就會用capture的方式來跑，如果沒設第三個參數，default是用bubbling的方式。

如果我們要避免bubbling，通常會加 e.stopPropagation()，但除非必要，並不建議使用；文中意思似乎是說bubbling通常是會需要的，把它拿掉可能會發生意想不到的錯吧。



