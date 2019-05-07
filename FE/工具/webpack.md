## entry 配置模块的入口
context默认为执行启动Webpack时所在的当前工作目录<br>
改变context的默认配置, context必须是一个绝对路径的字符串<br>
```js
module.exports = {
	context: path.resolve(__dirname, 'app')
}
```
entry 可以是相对或绝对路径<br>



## output 配置如何输出最终想要的代码
` filename: '[name].js' ` <br>
` publicPath: 'qiniu.afacode.top/assets' `
publicPath配置发布到线上资源的URL前缀 <br>
其他
* id, Chunk的唯一标识，从0开始
* name, Chunk名称
* hash, Chunk的唯一标识的Hash值
* chunkhush Chunk内容的Hash值
hash和chunkhash的长度是可指定的，［hash:8]代表取8位Hash值，默认是20位




## module 配置处理模块的规则
### loader
* style-loader css-loader
* sass-loader less-loader postcss-loader postcss-cssnext
* vue-template-compiler 将vue-loader提取出的HTML模板编译成对应的可执行的JavaScript代码
* vue-loader 解析和转换.vue文件，提取出其中的逻辑代码script、样式代码style及HTML模板template，再分别将它们交给对应的Loader去处理
* ts-loader
* awesome-typescript-loader 比较好的ts loader

> 条件配置 test, include, exclude

数组里面是或关系
```js
modlue: {
	rules: [
		{
			test: /\.js$/,
			use: ['babel-loader'],
			options: {
				cacheDirectory:true,
			},
			exclude: /node_modules/,
			enforce: 'post', // post loader执行顺序最后 pre 最前
			parser: {
				amd: false, // 禁用AMD
				commonjs: false, // 禁用CommonJS
				system: false, // 禁用SystemJS
				harmony: false  // 禁用ES6import/export 
				requireinclude: false, // 禁用require.include
				requireEnsure: false  // 禁用require.ensure
				requireContext: false, // 禁用require.context
				browserify: false, // 禁用browserify
				requireJs: false, // 
			}
		}
	]
}
```

## plugins 配置扩展插件
` extract-text-webpack-plugin ` 将css文件单独提取出来 <br>
```js
const ExtractTextPlugin = require('extract-text-webpack-plugin')
plugins: [
	new ExtractTextPlugin({
		// 从.js文件中提取出来的.css文件的名称 
		filename: `[name][contenthash:B].css`
	})
]
```

## resolve 配置寻找模块的规则
```js
resolve: {
	// 导入语句没带文件后缀时, 尝试访问,从左到右, 优先访问
	extensions: ['.js', '.jsx', '.json'],
	// alias通过别名来将原导入路径映射成一个新的导入路径
	alias: { 
		'react$': '/path/to/react.min.js', // $ 以react结尾的 import 'react' =  import '/path/to/react.min.js' 
	},
	// 默认 false,  true所有导入语句都必须带文件后缀
	enforceExtension: false,
	// 与 enforceExtension 类似, 只对node_modules下的模块生效
	enforceModuleExtension: false
},
```

## devServer 配置DevServer
` webpack-dev-server ` <br>
` webpack --config webpack.config.js ` <br>
### 启动参数
* --watch 开启监听模式
* --hot 开启热替换
* --devtool source map  支持Source Map,启动源码调试 
```js
devServer: {
	host: 'localhost',
	port: 8888, // 默认8080
	proxy: {
		
	}
	// 白名单列表
	allowedHosts: [
		'afacode.top',
		'.afacode.cn' // 所有的子域名
	],
	/*
	用自己的证书
	https: {
		key: fs.readFileSync('pathltolserver.key'), 
		cert: fs.readFileSync('pathltolserver.crt'), 
		ca: fs.readFileSync('pathltolca.pem')
	}
	*/
  https: true, //自动为我们生成一份HTTPS证书
	clientLogLevel: error, // none、error、warning、info, 默认info, 所有类型, none, 不输出任何日志
	compress: true, //是否启用Gzip压缩 true, false
	autoOpenBrowser: true, // 第一次启动是否自动打开浏览器
}
```


## 其他配置项 他零散的配置项
### target 配置Webpack构建出针对不同运行环境的代码
> web               默认, 所有代码都集中在一个文件里
> node	            针对Node.js，使用require语句加载Chunk代码 
> async-node        针对Node.js，异步加载Chunk代码 
> webworker         针对webworker
> electron-main     针对electron
> electron-renderer 针对electron
### devtool
默认值 false, 
devtool: 'source-map'

