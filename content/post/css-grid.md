+++
categories = ["css"]
date = "2017-06-23T09:02:41+08:00"
tags = []
title = "學習 CSS grid"

+++

先筆記一下喔

### for parent element:
`display: grid;`

`grid-template-columns: 20% 20% 20% 20% 20%`

也可以用 `repeat(5, 20%);`
新的單位 `fr = fraction`


`grid-template-rows` 同上

`grid-template:` shorthand for grid-template-columns and grid-template-rows

ex: `grid-template: 1fr 50px / 20% 80%;`



### for child element:
`grid-column-start`, `grid-row-start`

可以放正數、負數、或 span 數字

`grid-column-end`, `grid-row-end`

可以放正數、負數、或 span 數字，end 的那格是不包含的

**奇怪的地方

如果共有五格，要填2,3,4，可以：(end值小於start值)

	grid-column-start: 5;
	grid-column-end: 2;


**用負數

如果共有五格，要填1,2,3,4，可以：

	grid-column-start: 1;
	grid-column-end: -2;

如果是負值，-1好像沒東西？-2開始跑到右邊...


`grid-template-start` 跟 `grid-template-end` 到回去數會不一樣，差1

`grid-column`: shorthand for grid-column-start and grid-column-end

ex: 3 / 5

`grid-row`: shorthand for grid-row-start and grid-row-end

ex: 1 / 5

`grid-area`: shorthand for above

grid-row-start / grid-column-start / grid-row-end / grid-column-end

`order`:類似z-index


這篇文章不錯唷
[https://stripe.com/blog/connect-front-end-experience]


references:

*	[http://cssgridgarden.com/]

*	[https://stripe.com/blog/connect-front-end-experience]

