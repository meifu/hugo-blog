+++
categories = ["javascript"]
date = "2017-07-06T09:13:08+08:00"
tags = []
title = "javascript reduce"

+++

javascript reduce 是一個很神奇的function，也讓我有點難懂...

最近剛好在看[這篇文章](https://stripe.com/blog/connect-front-end-experience)，裡面有一段用到了reduce瞬間讓我滿滿????????

需要研究一下


reduce主要是用在array上，他的第一個parameter是一個function()，第二個parameter是optional的，是一個index值，代表初始值

reduce是一個累加的感覺，它將array中的每個值，依序帶入第一個parameter的function中，再將這個function return的結果累加起來。

而這個function可以帶入四個parameters：accumulator、currentValue、currentIndex、array，可以看[Mozilla文件](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)的說明
accumulator就是把array的上一個值帶入function的結果(return value)。


它可以用來將一些多層的array平面化，像是：


```
const flatten = arr => arr.reduce(
  (acc, val) => acc.concat(
    Array.isArray(val) ? flatten(val) : val
  ),
  []
);
```


可以用來計算不同名字出現的次數：

```
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```




若沒有提供initial value，會用array的第一個元素值當作accumulator，會用array的第二個元素值當currentValue，也就是不會從array的第一個值開始跑喔。建議是要提供initial value，不然可能會有不同結果。







PS. IE9以上支援

