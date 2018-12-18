## babel 更目录新建 .babelrc 文件
```js
// .babelrc
{
	"presets": [
		{
			"es2015", 
			{
				"modules": false
			}
		}
		"stage-2"
	],
	"plugins": [
		{
			"transform-runtime",
			{
				"polyfill": false
			}
		}
	]
}

```
> presets 告诉Babel要转换的源码使用了哪些新的语法特性
* ES2015 
* ES2016 
* ES2017 
* Env 所有的最新标准
>被社区提出来的但还未被写入ECMAScript标准里的特性
* stage0 只是一个美好激进的想法，一些Babel插件实现了对这些特性的支持，但是不确定是否会被定为标准
* stage1 值得被纳入标准的特性
* stage2 该特性规范己经被起草，将会被纳入标准里
* stage3 该特性规范已经定稿，各大浏览器厂商和Node.js社区己开始着手实现
* stage4 在接下来的一年里将会加入标准里
> 用于支持一些特定应用场景下的语法的特性,跟ECMAScript没关系 如babel-preset-react用于支持React开发里的JSX语法



## loader
* ` babel-core babel-loader `
> 选择定义的级别, 比如
* ` babel-preset-env `
* ` babel-preset-stage-2 `
* ` @types/react @types/react-dom ` ts支持react