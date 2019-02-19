## electron
[github](https://github.com/electron/electron) <br>
[官网文档](https://electronjs.org/docs) <br>
[官网示例](https://github.com/electron/electron-quick-start) <br>

[react模板](https://github.com/electron-react-boilerplate/electron-react-boilerplate) <br>
[vue模板](https://github.com/SimulatedGREG/electron-vue) <br>

---
electron-vue <br>
[文档](https://simulatedgreg.gitbooks.io/electron-vue/content/cn/getting_started.html) <br>
[https://electron.org.cn/](https://electron.org.cn/)<br>
[https://newsn.net](https://newsn.net)<br>
[github](https://github.com/SimulatedGREG/electron-vue) <br>
```bash
npm install electron --save-dev 
npm install electron-packager --save-dev // 打包electron应用的工具
vue init simulatedgreg/electron-vue electron-demo
```

> 隐藏 electron 窗体的菜单
删除顶部菜单的方法，只在windows和linux版本上有效。在mac系统下无效，mac系统下还是会存在顶部的菜单栏。但是mac版本下，本来主窗体和菜单也是分开的

```js 
const Menu = electron.Menu or const {Menu} = require('electron')
Menu.setApplicationMenu(null)
```
> 创建无边框窗体
```js
设置frame属性为false即可, window下自动隐藏菜单
mainWindow = new BrowserWindow({width: 800, height: 600, frame:false})
```
> 窗体是禁止拖动和改变大小
```js 
resizable:false
```
> mac系统，electron创建无边框窗体的几种特殊方式
```js
frame:false 交通灯区域完全隐藏
titleBarStyle: 'hidden'
titleBarStyle: 'hiddenInset'
hiddenInset的交通灯按钮位置比hidden的交通灯位置靠下

titleBarStyle: 'customButtonsOnHover', frame: false
customButtonsOnHover必须配合frame:false使用 frame:true或者未设置，那么就是展示最普通的有普通边框的情况

交通灯按钮是等鼠标移动上去之后，才会出现的
```
> 实现 electron 窗体透明 transparent 效果
BrowserWindow
transparent: true

> electron 如何禁用本地缓存
app.commandLine.appendSwitch("--disable-http-cache")
需要放置在ready事件之前

> electron 如何执行任务栏闪烁功能
```js
在 createWindow 里面

mainWindow.on("blur",function(){
  if(!mainWindow.isFocused()){
    mainWindow.showInactive();
    mainWindow.flashFrame(true);
  }
})

关键代码：mainWindow.flashFrame(true);
```

> 读取框架（electron、node、chrome）版本号
``` process.versions ```
> 读取软件自身的版本信息
```js
var package = require("./package.json");
console.log(package)
```

> electron 搭配 sqlite 数据库之数据交互

> electron 如何禁用页面缩放事件？如何禁用双指缩放事件
```js
document.addEventListener('mousewheel', (ev) => {
  if(ev.ctrlKey){
    ev.preventDefault();
    return false;
  }
});
```
> electron 的窗口设置最大化 最小化
```js
// 渲染进程
class WindowUtil{
	// 窗口最小化
	static minWindow() {
			remote.getCurrentWindow().minimize();
	}
	// 窗口最大化
	static maxWindow(isMaxed) {
			const browserWindow = remote.getCurrentWindow();
			if(!isMaxed) {
					browserWindow.unmaximize();
			} else {
					browserWindow.maximize();
			}
	}
	// 设置窗口是否能改变大小，参数true/false
	static setResizable(resizable) {
			remote.getCurrentWindow().setResizable(resizable);
	}
	// 下载文件
	static download(url){
			remote.getCurrentWebContents().downloadURL(url);
	}

	// 隐藏窗口
	static hide(){
			const browserWindow = remote.getCurrentWindow();
			browserWindow.hide();
	}

	// 显示窗口
	static show(){
			const browserWindow = remote.getCurrentWindow();
			browserWindow.show();
	}
	// 窗口闪烁
	static flashFrame(){
			const browserWindow = remote.getCurrentWindow();
	 //   if(browserWindow.isFocused() || browserWindow.isVisible())
			if(!browserWindow.isFocused()) {
					browserWindow.showInactive();
					browserWindow.flashFrame(true);
			}
	}
	// 设置窗口最前端显示
	static setAlwaysOnTop(top){
			const browserWindow = remote.getCurrentWindow();
			browserWindow.setAlwaysOnTop(top);
	}

	// 设置开机启动
	static enableAutoStart(callback) {
			let key = new WinReg({hive: WinReg.HKCU, key: RUN_LOCATION});
			key.set(‘EUC‘, WinReg.REG_SZ, file, (err)=> {
					console.log(‘设置自动启动‘+err);
					callback(err);
			});
	}
	// 取消开机启动
	static disableAutoStart(callback) {
			let key = new WinReg({hive: WinReg.HKCU, key: RUN_LOCATION});
			key.remove(‘EUC‘,  (err)=>{
					console.log(‘取消自动启动‘+err);
					callback(err);
			});
	}
	// 获取是否开机启动
	static getAutoStartValue(callback) {
			let key = new WinReg({hive: WinReg.HKCU, key: RUN_LOCATION});
			key.get(‘EUC‘, function (error, result) {
					console.log("查询自动启动:"+JSON.stringify(result));
					console.log("file:"+file);
					if (result) {
							callback(true);
					}
					else {
							callback(false);
					}
			});
	}
	 /**
	 * 托盘图标闪烁
	 * @param flash true：闪烁；false：停止
	 */
	static flashTray(flash) {
			let hasIcon = false;
			const tayIcon = ‘./imgs/logo.ico‘;
			const tayIcon1 = ‘./imgs/empty.png‘;
			if (flash) {
					if (flashTrayTimer) {
							return;
					}
					flashTrayTimer = window.setInterval(() => {
							ipcRenderer.send(‘ChangeTrayIcon‘, hasIcon ? tayIcon : tayIcon1);
							hasIcon = !hasIcon;
					}, 500);
			} else {
					if(flashTrayTimer) {
							window.clearInterval(flashTrayTimer);
							flashTrayTimer = null;
					}
					ipcRenderer.send(‘ChangeTrayIcon‘, tayIcon);
			}
	}
}

> electron窗口大小控制
```js
主进程
ipcMain.on('login-window', () => {
	mainWindow.setSize(1440, 900)
})
渲染进程
mounted: function () {
	ipcRenderer.send('login-window') 
}
```









































































> electron-packager
全局安装yarn global add electron-packager
最简单一键打包命令 electron-packager ./ --all
会打包生成所有你需要的各种platform各种arch的包，
当然如果需要安装wine之类的库的话，
请参见苏南大叔的以前的相关文章。
注意命令里面的 ./ 表示的是当前目录，也就是你的package.json文件所在的目录
electron-packager ./ --platform=darwin
electron-packager ./ --platform=linux
electron-packager ./ --platform=mas
electron-packager ./ --platform=win32
不写arch的情况下，arch的取值就是打包的电脑的arch值。
一般可以取值有：ia32,x64,armv7l,arm64
electron-packager ./ --platform=darwin --arch=ia32
electron-packager ./ --platform=darwin --arch=x64
electron-packager ./ --platform=win32 --arch=ia32
electron-packager ./ --platform=win32 --arch=x64

> electron-builder
构建 npm run build

对于vue的packager方式，由于默认是一次性构建四个平台的。
此过程中，容易发生一些错误。并生成了你一些不需要的东西。
所以，你也可以分平台进行构建

npm run build:win32
npm run build:darwin
npm run build:linux
npm run build:mas






