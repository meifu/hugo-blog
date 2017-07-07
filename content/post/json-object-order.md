+++
categories = ["javascript"]
date = "2017-06-30T16:44:49+08:00"
tags = []
title = "json object order"

+++

> "An object is an unordered set of name/value pairs"

亂翻譯：object是一堆『沒有順序』的「名稱/值」

重點是『沒有順序』，而我發現用 for (xxx in object) 

去取出來用的時候，他會自動用某個順序去取，不會用你製造這個object的時候的順序。


為什麼會突然體認到這件事呢？是這樣的：

在做商品頁的時候，我們要先判斷各個顏色的商品圖是否存在，判斷完再把存在的圖放進dom裡面。

我判斷圖片存在與否後，就先把存在的圖片跟對應顏色重新整理成一個新的object，再用這個新的object去產生出對應的html。

由於每一個顏色會對應到一組圖片，在某個專案中被發現是對應錯誤的（bug!!）。

去檢查之後發現，是在用 for (xx in Object) 取出資料 render html 的時候，順序跟原本的不同，結果就對應錯了。

簡單的測試：

```
testobj['B'] = ['B1','B2','B3'];

testobj['A'] = ['A2','A3','A1'];

testobj['101'] = ['1011', '1012'];

testobj['75'] = ['751', '752'];

for (key in testobj) {
	console.log('test key: ' + key);
}

```

會印出

```
test key: 75
VM695:2 test key: 101
VM695:2 test key: B
VM695:2 test key: A
```

也就是如果key值是數字，他取出的時候會以數字由小到大的順序！
但如果key值是文字，則似乎不會被改變順序？

根據Mozilla上面的教學文件：

	> The for...in statement iterates over the enumerable properties of an object, in original insertion order.

好像的確是這樣喔～因為數字是enumerable的。

總之，要小心。

在這個有bug的專案中，我把它改成用array來存取並取出了，因為用arrayr就會有固定順序性了。










