{
	"compilerOptions": {
		"module": "commonjs", // 编译采用的模块规范
		"target": "es5", // 输出版本
		"sourceMap": true, // 输出SourceMap以方便调试
		"importHelpers": true // 减少代码冗余
	},
	"exclude": [ // 不编译这些目录里的文件
		"node_module"
	]
}