+++
categories = ["css"]
date = "2017-07-06T14:15:13+08:00"
tags = []
title = "Vertical padding to control aspect ratios"

+++

之前在找怎樣讓youtube的iframe自動滿版、responsive的時候，看到的解法是：


- html

```
<div class="wrap">
  <iframe />
</div>
```
- css(for 16:9)

```
.wrap {
	position: relative;
	padding-bottom: 56.25%;
	padding-top: 25px;
	height: 0
}
iframe {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
```

但我都不懂是為什麼，後來看到這篇文章[Aspect Ratios in CSS are a Hack](https://www.bram.us/2017/06/16/aspect-ratios-in-css-are-a-hack/)

終於.....

還是不太懂XD，為了寫這篇文章，再認真的看了一下...

.

.

.

我懂了！！！！！（其實也沒什麼）

其實就是用外層的padding-top或是padding-bottom來撐出高度，而高度又是根據width的百分比來訂。

that's it

收工



Reference: 

[css-trick](https://css-tricks.com/NetMag/FluidWidthVideo/Article-FluidWidthVideo.php)
