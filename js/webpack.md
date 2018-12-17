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
` filename: '[name].js' `
其他
* id, Chunk的唯一标识，从0开始
* name, Chunk名称
* hash, Chunk的唯一标识的Hash值
* chunkhush Chunk内容的Hash值
hash和chunkhash的长度是可指定的，［hash:8]代表取8位Hash值，默认是20位




## module 配置处理模块的规则
### loader
` style-loader css-loader `


## plugins 配置扩展插件
` extract-text-webpack-plugin ` 将css文件单独提取出来
```js
const ExtractTextPlugin = require('extract-text-webpack-plugin')
plugins: [
	new ExtractTextPlugin({
		// 从.j s文件中提取出来的.css文件的名称
		filename: `[name][contenthash:B].css`
	})
]
```

## resolve 配置寻找模块的规则


## devServer 配置DevServer
` webpack-dev-server `
` webpack --config webpack.config.js `

### 启动参数
* --watch 开启监听模式
* --hot 开启热替换
* --devtool source map  支持Source Map,启动源码调试 

## 其他配置项 他零散的配置项



