+++
categories = ["javascript"]
date = "2017-07-18T16:21:32+08:00"
tags = []
title = "Intersection Observer API"

+++

初步了解，這東西是會默默地在後方，不干擾你的，偵測某元素是否已可見。

再詳細一點說，是針對某個目標元素，你可以寫一個callback。當這個目標進入viewport或者是另一個元素時，就會呼叫這個callback。這個API沒有辦法告訴我們目標出現位置的實際pixel，但可以告訴我們百分比。

### 基本設定：

```
var options = {
	root: document.querySelector('#scrollArea'),
	rootMargin: '0px',
	threshold: 1.0	
}

var observer = new IntersectionObserver(callback, options);
```

- root: 要被當作viewport的元素，如果寫null就是指document viewport

- rootMargin: 預設是0，跟css的margin同樣寫法。用margin去調整root的範圍。

- threshold: 可以是一個數字或是一個數字array。決定目標出現超過viewport多少百分比要呼叫callback。例如`[0, 0.25, 0.5, 0.75, 1]`就代表超過這幾個百分比的時候，都要呼叫callback


### 開始observe：

```
var target = document.querySelector('#listItem');
observer.observe(target);
```

### callback：

```
var callback = function(entries, observer) {
	entries.forEach(entry => {
		//每一個entry是表示每一個intersection
	});
}
```

在每一個entry物件中，都有以下物件可用：

- intersectionRatio: 某個threshold值

- target: 目標元素 

- boundingClientReact: 目標元素的區塊範圍

- intersectionRect: 目標元素可見的範圍

- isIntersecting: boolean，目標是否與viewport有相交集

- rootBounds: viewport元素

- time: intersect發生的時間，但他是"relative to the IntersectionObserver's time origin"，不清楚是什麼



使用時機範例：

* 當使用者滾到有圖片的地方時才顯示圖片，lazy-load image

* 在"infinite scrolling"的設計中

* 偵測廣告區域的出現以計算價格

* 根據使用者看到的部分決定是否要做某些動作


目前除了IE以外都支援喔！！！


Reference:

[Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

[IntersectionObserverEntry](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry)

[Connect: behind th front-end experience](https://stripe.com/blog/connect-front-end-experience)