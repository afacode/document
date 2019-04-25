# flow
[Flow](https://flow.org)是Facebook开源的一个JavaScript静态类型检测器<br>
vue用ts写比较恶心, react可以

## 集成到webpack
` npm i -D flow-bin ` <br>
` ηpmi -D babel-preset-flow ` <br>
```js
// package.json
"srcipts": {
	"flow": "flow"
}

// .babelrc
"presets": [
	...[],
	"flow"
]


```
# 反复