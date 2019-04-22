## webpack

[官网](https://webpack.js.org/) <br>
[中文](https://webpack.docschina.org/) <br>
[github](https://github.com/webpack/webpack)


> demo 运行不起来, 只是一个学习的记录

```js
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const CleanWebpackPlugin = require('clean-webpack-plugin')
const extractTextPlugin = `require`("extract-text-webpack-plugin")

var website ={
    publicPath:"/"
}

module.exports = {
	entry: {
		
	},
	output: {
		path: path.resolve(__dirname, 'dist'),
		filename: '[name].js',
		publicPath: website.publicPath
	},
	// 模块：loader 例如解读CSS,图片如何转换，压缩
	module: {
		rules: [
			{
				test: /\.css$/,
				// use: ['style-loader', 'css-loader']
				use: extractTextPlugin.extract({
					fallback: "style-loader",
					use: "css-loader"
				})
			},
			{
				test: /\.scss$/,
				use: ['style-loader', 'css-loader', 'sass-loader']
			},
			{
				test: /\.less$/,
				use: [
					{
						loader: 'style-loader'
					},
					{
						loader: 'css-loader'
					},
					{
						loader: 'less-loader'
					}
				]
			},
			{
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000, // 是把小于10000B的文件打成Base64的格式，写入JS
					outputPath:'images/',
        }
      },
			{
				test: /\.(htm|html)$/i,
				use: ['html-withimg-loader'] 
			},
			{
				loader: "postcss-loader"
			}
		]
	},
	/**
   * 插件，用于生产模版和各项功能
   * 插件的范围包括：打包优化、资源管理和注入环境变量
   * 可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例
   */
	plugins: [
		// new CleanWebpackPlugin(['dist']),
    // new HtmlWebpackPlugin({
    //   title: 'Hot Module Replacement'
    // }),
    // new webpack.HotModuleReplacementPlugin()
    new HtmlWebpackPlugin({
      minify:{
        removeAttributeQuotes:true // 是对html文件进行压缩，removeAttrubuteQuotes是却掉属性的双引号
      },
      hash:true, // 有效避免缓存
      template: './src/index.html' // 打包的html模版路径和文件名称
    }),
		new extractTextPlugin("/css/index.css")
	],
	// 配置webpack开发服务功能
	devServer: {
		contentBase: path.resolve(__dirname, '../dist'), // 设置基本目录结构 用于找到程序打包地址
    host: 'localhost',
    compress: true, //服务端压缩是否开启
    port: 8000
	},
	devtool: 'inline-source-map', // 源码映射
}
```
---
根目录 .postcssrc.js
```js
module.exports = {
  "plugins": {
    "postcss-import": {},
    "autoprefixer": {}
  }
}
```
---
* webpack
* webpack-cli
* webpack-dev-server
---
* style-loader  将模块的导出作为样式添加到 DOM 中
* css-loader  解析 CSS 文件后
* less-loader 加载和转译 LESS 文件
* sass-loader 加载和转译 SASS/SCSS 文件 sass-loader依赖于 npm i -D node-sass
* postcss-loader  PostCSS是一个CSS的处理平台 前缀的功能 npm install -D postcss-loader postcss-import autoprefixer 
* stylus-loader   加载和转译 Stylus  
* url-loader  file loader 一样工作，将文件的内容经过base64编码后注入JavaScript或者css中 但如果文件小于限制，可以返回 data URL
* file-loader 将文件发送到输出文件夹，并返回（相对）URL
* html-withimg-loader mtl文件中引入标签问题
---
* raw-loader  加载文件原始内容（utf-8）
* svg-inline-loader 与raw-loader非常相似 同之处在于svg-inline-loader会分析SVG的内容，去除其中不必要的部分代码，以减小SVG的文件大小
* xml-loader  
* csv-loader
---
一起的
* babel-loader 加载 ES2015+ 代码，然后使用 Babel 转译为 ES5
* babel-preset-env 
* babel-preset-es2015
---
插件 
* clean-webpack-plugin  删除
* html-webpack-plugin		
* uglifyjs-webpack-glugin JS压缩插件
* extract-text-webpack-plugin CSS分离
* purifycss-webpack  消除未使用的CSS npm  i -D purifycss-webpack purify-css
* imagemin-webpack-plugin 压缩图片
* webpack-spritesmith 制作雪碧图
* 加载图片的方法同样适用于其他二进制类型的资源，例如PDF,SWF等


### 比较好的连接
[http://webpack.wuhaolin.cn](http://webpack.wuhaolin.cn)



## 优化
### 优化开发体验 提升开发效率
* 优化构建速度
* 优化使用体验 通过自动化手段完成一些重复的工作，让我们专注于解决问题本身

> 缩小文件的搜索范围

* 优化module Loader配置
尽可能少地让文件被Loader处理，可以通过include 去命中只有哪些文件需要被处理 通过include缩小命中的范围
```js
module: {
	rules: [
		{
			test: /\.js$/,
			// babel-loader支持缓存转换出的结果，通过cacheDirectory选项开启
			use: ['babel-loader?cacheDirectory'],
			include: path.resolve(__dirname, 'src')
			
		}
	]
}
```
* 优化module.noParse配置
* 优化resolve.modules配置
* 优化resolve.mainFields配置
* 优化resolve.alias配置
* 优化resolve.extensions配置














### 优化输出质量
* 减少用户能感知到的加载时间，也就是首屏加载时间
* 提升流畅度，也就是提升代码性能



