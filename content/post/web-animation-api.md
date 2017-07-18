+++
categories = ["javascript"]
date = "2017-07-10T13:59:29+08:00"
tags = []
title = "web Animation API"

+++

Web animation api 是一個蠻新的東西，在今天(2017/07/10)還是有很多瀏覽器是不支援的。不過至少chrome還有！ya!


> we can move interactive animations from stylesheets to JavaScript, separating presentation from behavior


它運作的方式是跟CSS的動畫相同的，但又可以由javascript去控制。

主要就是一個 `.animate()`

而它的參數包括：

`keyframes`: 是一個arry, 每個值都是css屬性，由`,`分開，如果同一個值包含多個css屬性，可以用object的方式去呈現。與css的keyframes不同的是，它不包含時間的%，而是預設為平均分配在整段動畫中。最少要給兩個值，分別是起始跟結束，否則會出現 `NotSupportedError`。


`AnimationEffectTimingProperties`:是放`duration`、`direction`、`iterations`、`delay`、`easing`、`fill`、`delay`、`endDelay`、`iterationStart`這種時間相關參數的地方，用object的方式帶入。



`DocumentTimeline`: 還不支援。文件說是表示animation timeline。

`AnimationEffectTiming`: chrome還不支援。是第二個參數中的timing attribute...，not sure what it is.

`SharedKeyframeList`: 還不支援。可以跟其他keyframeEffect一起分享的keyframes。re-used可以減少parsing的時間。


`AnimationEffectTimingProperties`: 似乎還不支援。但真的看不太懂啊...


然後，可以在結束動畫的時候加callback，參考[這篇文章][Connect: Behind the Front-end Experience]加`addEventListener`：

	const animation = element.animate(keyframes, options);
	animation.addEventListener("finish", callback, {once: true});



最大的幫助是在performance上的，另外還有可以使用自訂的 `animation timimg function` 而不是只有基本的`ease-in`、`linear`那些。

以後要做動畫可採用的方法的優先順序：

	CSS transition > CSS animation > Web Animations API > requestAnimationFrame


大概就是這樣了，話說我對`requestAnimationFrame`其實不太了解，沒認真研究，要補一補。





Reference:

[Using Web Animation API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API/Using_the_Web_Animations_API)

[Connect: Behind the Front-end Experience](https://stripe.com/blog/connect-front-end-experience)