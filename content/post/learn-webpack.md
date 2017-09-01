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



### code spliting(basic)

一開始講師解釋了一下為何要做code spliting，舉例來說，通常登入頁面跟dashboard頁面所需要的javascript會差很多，dashboard需要用到較多的javascript，但login只需要很少的javascript。

我們當然都希望user可以越快看到畫面越好，也就是可以load越少的javascript越好，所以希望在login的時候，就不要跑全部的javascript，只跑基本的部分就好。這時候就需要將javascript整理成不同部分了。


在一開始load的index.js裡面，安排額外的js進入的時機（例如點一個按鈕），而進入的寫法是用`System.import`:

```
System.import('./image_viewer').then(module => {
	console.log(module);
	module.default();
})
```
這樣網頁一開始就只會load index.js，點了按鈕才會再load image_viewer.js。主要就是靠`System.import`來另外載入js


### multiple bundle (bundle is to big! -> vendor caching)

再來是提到cache的功能，簡單來說，為了讓網站變快，所以有些已經載入過，而且沒有發生改變的js，我們希望他們是被cache住的。但是有些js是經常被改變的，我們不希望被cache住，因次就要將這兩種js分開bundle，而用webpack是可以輕易做到的：

```
const VENDOR_LIBS = [
	'react', 'faker', 'lodash', 'redux', 'react-redux', 'react-dom', 'react-input-range', 'redux-form', 'redux-thunk'
];
...
...
entry: {
	bundle: './src/index.js',
	vendor: VENDOR_LIBS
},
output: {
	path: path.join(__dirname, 'dist'),
	filename: '[name].js'
},
```
`entry`的值改成一個object，裡面的key值就會是bundle出來的js的名稱；裡面的value就會是要bundle的內容，像`VENDOR_LIBS`就是通常是別人做好的tool或plugin。

`output`的值，在`filename`那邊就用`[name]`來表示，要output出來的js名稱就依照entry那邊的key值來決定。

再跑一次`npm run build`雖然成功地變成兩個檔案，但是更大了！！？

原因：在index.js裡面，也有import vendor的js，所以vendor js是被bundle了兩次！

這時候要使用webpack的plugin: `CommonsChunkPlugin`，它會判斷哪些是重複到用的js，並告訴webpack這些js就只出現在name那個js裏就好。

```
webpack.optimize.CommonsChunkPlugin({name: 'vendor'})
```

上面就是把index.js裡面有重複到vendor的部分去掉。神奇～


### Html Webpack Plugin

每當我們更新js的bundle方式時，或是換bundle的名字時，因為產生出來的js名稱改變過了，所以正常來說我們就要跑去html檔案裡面，手動更新`<script>`的src等等。但是...又有一個神奇的plugin：[Html Webpack Plugin](https://github.com/jantimon/html-webpack-plugin)，只要在webpack.config裡面加上

```
var HtmlWebpackPlugin = require('html-webpack-plugin');

...
...

plugins: [
	new HtmlWebpackPlugin({
      template: 'src/index.html'
    })
]
```
在`template: 'src/index.html'`這邊表示的是，請自動將bundle好的js加到template這裡。

從此以後，就不用手動更新了！它會自動幫你在html裡面插入bundle好的js file。Amazing again~



為了更精準的判斷js是否有更動過，需要再跟server拿一次，可以在webpack.config裡面，將output的部分再做一點修改，也就是在[name]的後面再加上`.[chunkhash]`，這樣的話，只要其中的js有做過修改，webpack就會給這個js後面帶一串不同的數字，讓browser知道要再去拿新的檔案。

```
output: {
	path: path.join(__dirname, 'dist'),
	filename: '[name].[chunkhash].js'
}
```
這時候還有一個地方要調整，假設我們的bundle.js有了修改，vendor並沒有修改，但webpack在bundle的時候，會認為vendor也被修改過(跟`CommonsChunkPlugin`有關)。所以要修改一下plugins的部分，把`CommonsChunkPlugin`的`name`改成`names`，裡面放的是一個array：

```
plugins: [
	new webpack.optimize.CommonsChunkPlugin({
		names: ['vendor', 'manifest']
	}),
	new HtmlWebpackPlugin({
      template: 'src/index.html'
    })
]
```
這樣做之後，就會產生另一個js：manifest，好像是可以幫助判斷`vendor`到底有沒有改變的（不大懂）。


加了chunkhash之後，因為每次build之後，只要js有修改，檔名都會不一樣，dist資料夾裡面的js不會被覆寫，會一直增加，這時候可以使用`rimraf`這個plugin，

package.json

scripts
	clean: rimraf dist
	build: npm run clean && webpack




### webpack dev server

當你修改專案中的檔案時，他就會自動refresh。但是如果修改的是 webpack.config ，就必須要停掉server重啟。

run webpack-dev-server的時候，webpack會run但是並沒有儲存任何檔案，講師說他是存在memory裡面（!?）。

因此如果我們要弄一個portable的版本（包括index.html, css files, js files），就必須要跑webpack才行。也就是說，webpack-dev-server是給development用的，而不是production。


### code splitting with React-Router

接下來要利用React-Router來讓網頁一開始進入首頁的時候，只load首頁需要的js，等使用者去到下一頁，才去load下一頁需要的js。

原本的 React-Router是這樣：

```
import Home from './components/Home';
import ArtistMain from './components/artists/ArtistMain';
import ArtistDetail from './components/artists/ArtistDetail';
import ArtistCreate from './components/artists/ArtistCreate';
import ArtistEdit from './components/artists/ArtistEdit';

<Router history={hashHistory}>
	<Route path="/" component={Home}>
	<IndexRoute component={ArtistMain} />
	<Route path="artists/new" component={ArtistCreate} />
	<Route path="artists/:id" component={ArtistDetail} />
	<Route path="artists/:id/edit" component={ArtistEdit} />
	</Route>
</Router>
```

把它改成這樣

```
import Home from './components/Home';
import ArtistMain from './components/artists/ArtistMain';

const componentRoutes = {
  component: Home,
  path: '/',
  indexRoute: { component: ArtistMain },
  childRoutes: [
    { 
      path: 'artists/new',
      getComponent(location, cb) {
        System.import('./components/artists/ArtistCreate').then(module => cb(null, module.default));
      }
    },
    { 
      path: 'artists/:id',
      getComponent(location, cb) {
        System.import('./components/artists/ArtistDetail').then(module => cb(null, module.default));
      }
    },
    { 
      path: 'artists/:id/edit',
      getComponent(location, cb) {
        System.import('./components/artists/ArtistEdit').then(module => cb(null, module.default));
      }
    }
  ]
};

const Routes = () => {
  return (
    <Router history={hashHistory} routes = {componentRoutes} />
  );
};
```

### sass

因為我要用sass，所以是一定要裝sass轉css的東西的。google一下是看到[sass-loader](https://github.com/webpack-contrib/sass-loader)，本人是過的用法是這樣：
```
module: {
	rules: [
		...
		{
			test: /\.css$/,
			use: [
				{loader: 'style-loader'},
				{loader: 'css-loader', options: {modules: true}}
			]
		},
		{
			test: /\.scss$/,
			exclude: /node_modules/,
			use: extractSass.extract({
				use: [{
					loader: 'css-loader'
				}, {
					loader: 'sass-loader'
				}],
				fallback: 'style-loader'
			})
		},
		...
```

### post css

未完待續

### html template

本來是想要找看看有沒有像gulp 的file include這種plugin的東西，讓我切版的時候，共用的部分就引用同一隻檔案，這樣修改的話只要修改一個地方就好。但目前研究出來的方式是用已經裝過的[HtmlWebpackPlugin](https://webpack.js.org/guides/output-management/#setting-up-htmlwebpackplugin)，分成多個檔案在plugins的地方去做，像這樣：

```
	,plugins: [
		extractSass,
		new HtmlWebpackPlugin({
			template: 'src/index.html'
		}),
		new HtmlWebpackPlugin({
			// inject: false,
			template: 'src/pdlist.html',
			filename: 'pdlist.html'
		})
	]
```
是不是我瞭解的還不夠多呢？



