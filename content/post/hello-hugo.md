+++
date = "2017-07-06T15:25:24+08:00"
draft = false
title = "Hello Hugo!"

+++

I decide to try Hugo for my blog. I will write some learning notes and some murmur.
I use this theme: 

<!--more-->
	[https://github.com/digitalcraftsman/hugo-strata-theme]

and ...

Amazing!

this is sooooo cool!!!!

（以上是一開始玩覺得很棒的反應）

## push to github page

先是參考hugo document的做法，失敗。再看了github page官方的做法，了解他的意思但是好像少了什麼，因為我並不需要把整包檔案都push到git（因為會包含development的東西）

其實大概知道有個很單純的做法，就是移掉原本整個專案的git，只在hugo compile出來的public檔案夾那邊做git，然後把這包按照github page官方網站的做法push上去。但是，這樣真正應該要git的部分，也就是開發環境，反而沒有version control；又或者是要手動將要deploy的資料夾搬出去另外git，這樣感覺也不對。

後來又去google，試來試去終於研究到一個用git submodule的方式，成功deploy到github page了。參考的是這篇：

[Deploy a hug site to github page](https://github.com/whipperstacker/blog/blob/master/content/post/deploying-a-hugo-site-to-github-pages.md)

首先把public資料夾從原本的git移除 `git rm -r public`

接著用原本的repo把public資料夾做成一個submodule `git submodule add git@github.com:<username>/<username>.github.io.git public`

然後再把全部public裡面的東西push上去

```
$ git add .
$ git push -u origin master
```

接下來才是真的Deploy

首先再run一次hugo，然後進入public資料夾

```
$ git add .
$ git commit -m "Generate site"
$ git push origin master
```

簡單來說就是在public這個submodule裡面再做一次git push

好像就可以以囉！

然後我就發現每次有新增或修改，跑`git status`的時候，顯示的訊息看起來怪怪的，

在public裡面有改變的檔案，就不會被詳細列出，而是直接show

`modified:    public (new commits, modified content, untracked content)`

再進入public資料夾裡面run `git status` 則就會是一般的git訊息了，在這裡面就是一個自己的git環境，也就是一個submodule囉。










