+++
categories = ["javascript"]
date = "2017-08-02T12:57:11+08:00"
tags = []
title = "Learn Webpack"

+++

很久以前買了一個Udemy上面的[webpack](https://www.udemy.com/webpack-2-the-complete-developers-guide/learn/v4/content)課程，一直沒看完，來做一下筆記好了，只記要點。

<!--more-->

webpack主要是在處理專案中的js，包括合併、分類合併，也幫忙處理其他像是 babel, css/sass, 判斷圖片大小，等等的事情，是很多很多功能的。

基本設定通常是在`webpack.config.js`，是一個object結構，寫法如下

```
{
	entry: './src/index.js',
	output: {
		path: path.join(__dirname, 'dist'),
    	filename: 'bundle.js'
	},
	module: {
		rules: [
			{
				use: 'babel-loader',
				test: /\.js$/,
				exclude: /node_modules/
			}
		]
	},
	plugins: [
	]
}
```

`entry`: 程式進入點

`output`: output出來變成什麼

`rules`:  rules for modules (configure loaders, parser options, etc.)

`test`: 用來判斷哪些程式需要用這個rule/loader處理，是一個regex

`exclude`: 排除哪些程式不要處理

`plugins`: 就是用來裝webpack的plugins..吧


啟動的方式通常是寫在`package.json`：

```
{
	"name": "xxx",
	"scripts": {
		"build": "webpack"
	}
}
```

這樣的話就在command line輸入 `npm run build`就會跑webpack

再來是介紹一些常用的webpack功能：

### babel

安裝包括 babel-core(babel主要核心)、babel-loader()、babel-preset-env(基本的轉碼規則?自動根據環境判斷需要用的plugins)、babel-preset-react(轉碼規則for react)


好像只需要這樣寫：
```
module: {
	rules: [
		{
			use: 'babel-loader',
			test: /\.js$/,
			exclude: /node_modules/
		}
	]
},
```
然後再加 `.babelrc`

```
{
	"presets": ["babel-preset-env", "react"]
}
```


### css

css-loader & style-loader
extract text webpack plugin

```
module: {
	rules: [
		{
			use: ['style-loader', 'css-loader'],
			test: /\.css$/
		}
	]
}
```

加上[ExtractTextPlugin](https://github.com/webpack-contrib/extract-text-webpack-plugin)變成

```
module: {
	rules: [
		{
			use: ExtractTextPlugin.extract({
				fallback: 'style-loader',
				use: 'css-loader'
			}),
			test: /\.css$/
		}
	]
},
plugins: [
	new ExtractTextPlugin('style.css')
]
```
這樣css就會output到 style.css那，不然會是inline的



### code spliting



```
entry: {
	bundle: './src/index.js',
	vendor: VENDOR_LIBS
},
output: {
	path: path.join(__dirname, 'dist'),
	filename: '[name].js'
},
```

未完待續

## 以下是自己要做的 待續

### sass

### html template
[HtmlWebpackPlugin](https://webpack.js.org/guides/output-management/#setting-up-htmlwebpackplugin)





