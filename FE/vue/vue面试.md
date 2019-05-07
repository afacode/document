[vue核心知识点](https://zhuanlan.zhihu.com/p/64786939)
### v-if 和 v-show 有什么区别

```
v-show 仅仅控制元素的显示方式，将 display 属性在 block 和 none 来回切换；而v-if会控制这个 DOM 节点的存在与否。当我们需要经常切换某个元素的显示/隐藏时，使用v-show会更加节省性能上的开销；当只需要一次显示或隐藏时，使用v-if更加合理

共同点：
v-if和v-show都是通过判断绑定数据的true\false来展示的
不同点：
v-if只有在判断为true的时候才会对数据进行渲染，false的时候把包含的代码进行删除。除非再次进行数据渲染，v-if才会重新判断。可以说是用法比较倾向于对数据一次操作。
v-show是无论判断是什么都会先对数据进行渲染，只是false的时候对节点进行display:none;的操作。所以再不重新渲染数据的情况下，改变数据的值可以使数据展示或隐藏。
用法推荐：
v-if更适合于带有权限的操作，渲染时判断权限数据，有则展示该功能，没有则删除。
v-show更适合于日常使用，可以减少数据的渲染，减少不必要的操作。
综上，v-if 有更高的切换消耗，而 v-show 有更高的初始渲染消耗。
因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变，更倾向功能权限性的话 v-if 较好。
```
### vue常用的修饰符
```
1.事件修饰符
vue为v-on提供了事件修饰符，通过点(.)表示的指令后缀来调用修饰符。

.stop
阻止点击事件冒泡。等同于JavaScript中的event.stopPropagation()
例如：

<a v-on:click.stop="doThis"></a>
<a @click.stop="doThis"></a>

实例1，防止冒泡：

<div id="app"> 
　　<div class="outeer" @click.stop="outer"> 
　　　　<div class="middle" @click.stop="middle"> 
　　　　　　<button @click.stop="inner">点击我(^_^)</button>
 　　　　</div>
 　　</div> 
</div>

使用了.stop后，点击子节点不会捕获到父节点的事件

作者：杨芳霞
链接：https://zhuanlan.zhihu.com/p/64786939
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

2.prevent
防止执行预设的行为（如果事件可取消，则取消该事件，而不停止事件的进一步传播），等同于JavaScript中的event.preventDefault()，prevent等同于JavaScript的event.preventDefault()，用于取消默认事件。比如我们页面的标签，当用户点击时，通常在浏览器的网址列出#：
例如：

<a v-on:submit.prevent="doThis"></a>

.capture
与事件冒泡的方向相反，事件捕获由外到内,捕获事件：嵌套两三层父子关系，然后所有都有点击事件，点击子节点，就会触发从外至内 父节点-》子节点的点击事件

<a v-on:click.capture="doThis"></a>

<div id="app"> 
　　<div class="outeer" @click.capture="outer"> 
　　　　<div class="middle" @click.capture="middle"> 
　　　　　　<button @click.capture="inner">点击我(^_^)</button>
 　　　　</div>
 　　</div> 
</div>

.self
作者：杨芳霞
链接：https://zhuanlan.zhihu.com/p/64786939
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

只会触发自己范围内的事件，不包含子元素

<a v-on:click.self="doThat"></a>

<div id="app"> 
　　<div class="outeer" @click.self="outer"> 
　　　　<div class="middle" @click.self="middle"> 
　　　　　　<button @click.stop="inner">点击我(^_^)</button>
 　　　　</div>
 　　</div> 
</div>
.once
只执行一次，如果我们在@click事件上添加.once修饰符，只要点击按钮只会执行一次。

<a @click.once="doThis"></a>
1
.passive
Vue 还对应 addEventListener 中的 passive 选项提供了 .passive 修饰符

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>

这个 .passive 修饰符尤其能够提升移动端的性能。不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，.passive 会告诉浏览器你不想阻止事件的默认行为。

事件修饰符还可以串联
作者：杨芳霞
链接：https://zhuanlan.zhihu.com/p/64786939
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

例如：

<a v-on:click.stop.prevent="doThis"></a>

注：使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

2.键盘修饰符
在JavaScript事件中除了前面所说的事件，还有键盘事件，也经常需要监测常见的键值。在Vue中允许v-on在监听键盘事件时添加关键修饰符。记住所有的keyCode比较困难，所以Vue为最常用的键盘事件提供了别名：
.enter：回车键
.tab：制表键
.delete：含delete和backspace键
.esc：返回键
.space: 空格键
.up：向上键
.down：向下键
.left：向左键
.right：向右键

例如：

<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">

记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：

<!-- 同上 -->
作者：杨芳霞
链接：https://zhuanlan.zhihu.com/p/64786939
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">

可以通过全局 config.keyCodes 对象自定义按键修饰符别名：

// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112

3.系统修饰键
可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。
.ctrl
.alt
.shift
.meta

注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

例如：

<!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>

注意：
作者：杨芳霞
链接：https://zhuanlan.zhihu.com/p/64786939
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

请注意修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。而单单释放 ctrl 也不会触发事件。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17。

.exact修饰符
.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。

<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>

鼠标按钮修饰符
鼠标修饰符用来限制处理程序监听特定的滑鼠按键。常见的有：
.left
.right
.middle
这些修饰符会限制处理函数仅响应特定的鼠标按钮。

自定义按键修饰符别名
在Vue中可以通过config.keyCodes自定义按键修饰符别名。例如，由于预先定义了keycode 116（即F5）的别名为f5，因此在文字输入框中按下F5，会触发prompt方法，出现alert。

<template>
  <div class="main">
      <input type="text" @keyup.f5="prompt()" />
  </div>
</template>
<script>
export default {
  data() {
    return {
    };
  },
  methods:{
      prompt(){
          alert("aaaaa")
      }
  }

};
</script>

当点击f5时立马调用prompt方法。

4.修饰符
(1).lazy
在改变后才触发（也就是说只有光标离开input输入框的时候值才会改变）

<input v-model.lazy="msg" />
1
(2).number
将输出字符串转为Number类型·（虽然type类型定义了是number类型，但是如果输入字符串，输出的是string）

<input v-model.number="msg" />
1
(3).trim
自动过滤用户输入的首尾空格

<input v-model.trim="msg" />
```

### v-on可以监听多个方法吗
```
v-on可以监听多个方法，例如：
 <input type=“text” :value=“name” @input=“onInput” @focus=“onFocus” @blur=“onBlur” />
 但是同一种事件类型的方法，vue-cli工程会报错，例如：
 <a href=“javascript:;” @click=“methodsOne” @click=“methodsTwo”></a>
```
### vue中 key 值的作用
```
key值：用于 管理可复用的元素。因为Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做使 Vue 变得非常快，但是这样也不总是符合实际需求。
    2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。
    例如，如果你允许用户在不同的登录方式之间切换：
    <template v-if=“loginType === 'username‘”>
    <label>Username</label>
    <input placeholder=“Enter your username”>
    </template>
    <template v-else>
    <label>Email</label>
    <input placeholder=“Enter your email address”>
    </template>
    那么在上面的代码中切换loginTypeloginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，</span>不会被替换掉，仅仅是替换了它的 placeholder`.
    这样也不总是符合实际需求，所以Vue为你提供了一种方式来表达这两个元素是完全独立的，不要复用它们。只需添加一个具有唯一值的 key 属性即可：
    <template v-if=“loginType === 'username’”>
    <label>Username</label>
    <input placeholder=“Enter your username” key=“username-input”>
    </template>
    <template v-else>
    <label>Email</label>
    <input placeholder=“Enter your email address” key=“email-input”>
    </template>
```

### $nextTick的使用
```
Vue.js 通常鼓励开发人员沿着“数据驱动”的方式思考，避免直接接触 DOM。this.$nextTick()官方介绍：将回调延迟到下次 DOM 更新循环之后执行。
在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。
 new Vue({
        el: "#app",
        data: {
            message: 'Hello Vue.js',
            showMe: false
        },
        methods: {
            getMyWidth() {
                this.showMe = true;
                //this.message = this.$refs.myWidth.offsetWidth;//报错 TypeError: this.$refs.myWidth is undefined
                this.$nextTick(()=>{
                    //dom元素更新后执行，此时能拿到p元素的属性
                    this.message = this.$refs.myWidth.offsetWidth;
                })
            }
        }
    })
```