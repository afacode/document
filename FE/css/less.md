## 安装
```
npm install less -g
```
## 使用
```
lessc main.less
OR 
lessc main.less main.css
```

## 变量(Variables)
```
@classname: main;
@color: red;

.@classname{
    background-color: @color;
}
```
## 混合(Minins)
> 将公共属性抽取出来作为一个公共类
* 混合也是减少代码书写量的一个方法；
* 混合的类名在定义的时候加上小括弧 ()，那么在转译成 css 文件时就不会出现；
* 混合的类名在被调用的时候加上小括弧 ()和不加上小括弧 ()是一样的效果，看个人习惯

```
.bordered {
    border-top: dotted 1px black;
    border-bottom: solid 2px black;
}

#menu a {
    color: #111;
    .bordered;
}

#menu span {
    height: 16px;
    .bordered;
}

#menu p {
    color: red;
    .bordered();
}
```
```
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu a {
  color: #111;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu span {
  height: 16px;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#menu p {
  color: red;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

## 嵌套(Nesting)
```
.container {
    padding: 0;
    .article {
        background-color: red;
    }
}
```
```
#header {
  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```

## 运算(Operations)
* 加减法时 以第一个数据的单位为基准
* 乘除法时 注意单位一定要统一
```
/* Less */
@width:300px;
@color:#222;
#wrap{
  width:@width-20;
  height:@width-20*5;
  margin:(@width-20)*5;
  color:@color*2;
  background-color:@color + #111;
}

/* 生成的 CSS */
#wrap{
  width:280px;
  height:200px;
  margin:1400px;
  color:#444;
  background-color:#333;
}
```

## Escaping

## 函数(Functions)
```
.border-radius(@radius) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}

#header {
  .border-radius(4px);
}
.button {
  .border-radius(6px);
}
```
```
// 函数的参数设置默认值：
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
  border-radius: @radius;
}

// 函数有多个参数时用分号隔开
.mixin(@color; @padding:2) {
  color-2: @color;
  padding-2: @padding;
}

// 函数如果没有参数，在转译成 css 时就不会被打印出来，详见上面混合中的示例
.wrap() {
  text-wrap: wrap;
}

// 函数参数如果有默认，调用时就是通过变量名称，而不是位置
.mixin(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}
.class1 {
  .mixin(@margin: 20px; @color: #33acfe);
}

// 函数参数有个内置变量 @arguments，相当于 js 中的 arguments
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000) {
  -webkit-box-shadow: @arguments;
     -moz-box-shadow: @arguments;
          box-shadow: @arguments;
}

// 函数名允许相同，但参数不同，类似于 java 中多态的概念
.mixin(@color: black) {      
.mixin(@color: black; @margin: 10px) { 

```

## Namespaces and Accessors

## Maps

## (Scope)
```
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```
```
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

## (Comments)
```
/* 一个块注释
 * style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;
```

## (Importing)
```
@import "demo.css"
@import "demo.less" OR @import "demo"
// 如果导入的文件是 .less 扩展名，则可以将扩展名省略掉：

```