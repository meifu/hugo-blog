+++
categories = ["css"]
date = "2017-06-22T10:12:52+08:00"
tags = []
title = "hello css variables"

+++

最近好像有很多文章在講css variables，應該真的可以開始用了吧？
今天再找了一些文章才發現，他早就出來啦...我又lag...趕快來試試

	:root {
		--font-size-1: 1em;
		--font-size-2: 1.2em;
		--font-size-3: 1.44rem;
		--font-size-4: 1.728rem;
		--font-size-5: 2.074rem;
		--font-size-6: 2.488rem;
	}

	@media screen and (min-width: 800px) {
		:root {
			--font-size-1: 1rem;
			--font-size-2: 1.333rem;
			--font-size-3: 1.777rem;
			....
		}
	}

	h1 {
		font-size: var(--font-size-6);
	}
	h2 {
		font-size: var(--font-size-5);
	}
	...


也可以

	:root {
		--bg-color: #fff;
	}
	p {
		--bg-color: #333;
	}
	h3 {
		--bg-color: #aaa;
	}


這樣有點像是有scope的變數，跟javascript 的 let 一樣



## Seperate logic from design

	.this-is-a-variable-declaration {
	  --my-var: red;
	}

	.this-is-a-property-declaration {
	  background: var(--my-var)
	}

## 簡化css
	
	/* Default values */
	:root {
	  --font-size: 1.2em;
	  --background-color: #fff;
	  --text-color: #222;
	}
	/* Values in aside */
	aside {
	  --font-size: 1em;
	  --background-color: #222;
	  --text-color: #FAFAFA;
	}
	 
	/* Same property declarations */
	main,
	aside {
	  font-size: var(--font-size);
	  color: var(--text-color);
	  background-color: var(--background-color);
	}

這個寫法挺有趣的，就是可以把兩個不同的東西寫在一起，但又可以長得不一樣！


## Fallback (給default value)

可以加fallback 像是 color: var(--bg-color, black);
這樣如果沒有--bg-color 就會用 black
也可以像下面這樣在裡面用var
 
	background: var(--bgColor, var(--colorPrimary));



## Javascript 可以直接操弄 css variable

getPropertyValue()

	var styles = getComputedStyle(document.documentElement);
	var value = String(styles.getPropertyValue('--bg-primary-color')).trim();


setProperty()

改變 :root 的 variables可以這樣

	document.documentElement.style.setProperty('--bg-primary-color', '#369');


記一下這個 loading progress 的範例

	function calculateLoadProgress() {
		let loadProgress = 0;

		// codes to update loadProgress here

		return loadProgress;
	}

	// Set width of progress bar
	document.documentElement.style.setProperty('--progressBarWidth', calculateLoadProgress());




## 不會改變的東西：用sass variable

Use preprocessors for static variables


## @supports

好佛心啊～可以用這個來判斷瀏覽器是否支援...被推薦用在grid






reference:

[https://madebymike.com.au/writing/using-css-variables/]

[https://una.im/local-css-vars/]

[http://muki.tw/tech/native-css-variables/]
