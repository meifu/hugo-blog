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

css-loader: teach webpack how to import and parse css files（讓webpack打開css然後讀裡面的內容，但還沒做任何事）

style-loader: takes css import and adds them to the HTML document（告訴webpack要把css放在哪，怎麼用）

(murmur: webpack真的很神奇誒)


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

在view裡面(ex. image_viewr.js)要用

```
import '../styles/xxxx.css'
```
這樣做之後，webpack就會把css的內容直接放到那個html頁面裡面。而如果要把所有css生成到另外一支css的話就要用到這個plugin:

[ExtractTextPlugin](https://github.com/webpack-contrib/extract-text-webpack-plugin)變成

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

PS. 這裡的code跟課程教的不一樣是因為 ... webpack 更新了～～@@



### handling img file

[image-webpack-loader](https://github.com/tcoopman/image-webpack-loader): 自動reduce file size, automatically compress image

file-loader: image-webpack-loader的官方文件說要跟這個loader一起用，不加這個會有錯

url-loader: 根據上面的結果，判斷圖片大小。如果圖片小，就會把圖片用raw data放在html，圖片大就放在image資料夾

```
module: {
	rules: [
		{
			test: /\.(jpe?g|png|gif|svg)$/,
			use: [
				{
					loader: 'url-loader',
					options: {limit: 40000}
				},
				'image-webpack-loader'
			]
		}
	]
}

```

在`url-loader`的部分可以給一個object，裡面就可以加options給他一個值來判斷圖片是大還是小

注意：use裡面跑的順序是後面那個先喔～所以就是`image-webpack-loader`先。

這樣的話，在view裡面用img:

```
import big from '../assets/big.jpg';
import small from '../assets/small.jpg';
```
webpack就會幫你自動壓縮並判斷圖片大小然後做不同處理。

但是在這樣的情況下，使用img的方式如果是這樣
```
var img = document.creatElement('img')
img.src = big
```
實際run的時候，它output出來的圖片路徑是錯的，而我們又不可能手動地在src前面增加路徑，因為像是small圖片就不需要了。怎麼解決呢？只要增加一個設定即可！（怎麼那麼神奇）如下：
```
entry: './src/index.js',
output: {
	path: path.resolve(__dirname, 'build'),
	filename: 'bundle.js',
	publicPath: 'build/'
}

```
而publicPath的作用可以看[文件](https://github.com/webpack/docs/wiki/configuration)。有用到loaders去轉換link path的(包括images, href, link)，webpack就會自動在path上更新。



### code spliting

一開始講師解釋了一下為何要做code spliting，舉例來說，通常登入頁面跟dashboard頁面所需要的javascript會差很多，dashboard需要用到較多的javascript，但login只需要很少的javascript。

我們當然都希望user可以越快看到畫面越好，也就是可以load越少的javascript越好，所以希望在login的時候，就不要跑全部的javascript，只跑基本的部分就好。

這時候就需要將javascript整理成不同部分了。


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


### webpack dev server


## 以下是自己要做的 待續

### sass

### html template
[HtmlWebpackPlugin](https://webpack.js.org/guides/output-management/#setting-up-htmlwebpackplugin)





