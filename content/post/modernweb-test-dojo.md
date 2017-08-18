+++
categories = ["test", "javascript"]
date = "2017-08-18T08:44:58+08:00"
tags = []
title = "modernweb test dojo"

+++

參加Modern Web 2017時，抓機會去參加了 Test Dojo 的活動，因為一直想要了解別人到底怎麼做前端測試的。

這個活動很好的是，他是一對一的教學，而且提供電腦。教學的github在這：[https://github.com/trunk-studio/modern-web-2017](https://github.com/trunk-studio/modern-web-2017)

裡面用到的工具包括：

- [WEBDRIVER I/O](http://webdriver.io/)，他是node.js上的，可以讓你輕鬆控制browser上的動作，也就是可以做前端的測試了。

- WEBDRIVER I/O裡面包含一個 [Selenium Server](http://www.seleniumhq.org/)，他就是主要在控制browser的東西。

- [Docker](https://www.docker.com/)，介紹請看https://yeasy.gitbooks.io/docker_practice/content/introduction/what.html

- [Jenkins](https://jenkins.io/)

- [Mocha.js](https://mochajs.org/)，常見的前端測試程式



第一步驟（課程指示[https://github.com/trunk-studio/modern-web-2017/tree/master/e2eSample/wd-ex01](https://github.com/trunk-studio/modern-web-2017/tree/master/e2eSample/wd-ex01)）

用Mocha.js寫好測試程式，像是這樣的：

```
var assert = require('assert');
describe('這是一個測試範例', function() {

    it('登入失敗', function() {
        browser.url('http://demo.keystone.js.com/keystone/siginin');
        browser.element('[name=email]').setValue('demo@keystonejs.com');
        ....
        const message = browser.getText('[data-alert-type=danger]');
        assert.equal(message, 'The email and password you entered are not valid.');
    });
    
});
```
上面這段中的`browser.url`、`browser.element`這些語法是來自WEBDRIVER I/O，它好像是讓browser執行你要他做的事情，用了這個之後，跑test的時候就會看到browser真的在動作了，好像有一個人在那邊操作，很有趣。做完以後就會在 terminal 看到測試結果。

至於跑test就是下`npm run test`在local跑，或是下`npm run test-docker`會用docker開container來做測試。但我不清楚他們是怎樣開docker的，docker的環境跟設定都已經事先準備好就是。

只記得要開不只一個container，有一個要跑selenium server，有一個是網頁本身的環境，似乎還有一個？另外docker設定似乎是在一個.yml檔裡面？




第二三步驟 就是結合 git, jenkins讓流程整個自動化，也就是說工作變成是這樣：

修改程式 --> git add & commit & push --> Jenkins偵測到push自動開docker跑測試 --> 測試結果在Jenkins server上的介面顯示

只能說好神奇啊～～什麼時候才能在真實工作中實做看看呢？







