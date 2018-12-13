## Ubuntu16.04 
---
### NGINX
### NODE
1. 安装
目前node 长期支持版 10.14.1 当前发布版 11.3.0 npm 6.4.1

我的源码放在 /root
```sh
	wget http://nodejs.org/dist/v10.14.1/node-v10.14.1-linux-x64.tar.xz  # 下载
	ls
	tar -zxvf node-v10.14.1-linux-x64.tar.xz # 解压
	cd node-v10.14.1-linux-x64
	主要看看 /bin
```
2. 设置node和npm为全局变量 
```sh
	ln -s /root/node-v10.14.1-linux-x64/bin/node /usr/local/bin/node # 软链
	ln -s /home/node-v10.14.1-linux-x64/bin/npm /usr/local/bin/npm
	node -v 
	npm -v
```
3. 测试
> 写个最小的node http 测试 和正常一样
4. 捎带安装个pm2
```sh
	npm config get prefix 
	npm i pm2 -g
	pm2 -v
```
5. pm2命令(这点不是我写的)
```sh
$ pm2 start app.js -i 4  # 后台运行pm2，启动4个app.js 
                         # 也可以把'max' 参数传递给 start
                         # 正确的进程数目依赖于Cpu的核心数目
$ pm2 start app.js --name my-api # 命名进程
$ pm2 list               # 显示所有进程状态
$ pm2 monit              # 监视所有进程
$ pm2 logs               # 显示所有进程日志
$ pm2 stop all           # 停止所有进程
$ pm2 restart all        # 重启所有进程
$ pm2 reload all         # 0 秒停机重载进程 (用于 NETWORKED 进程)
$ pm2 stop 0             # 停止指定的进程
$ pm2 restart 0          # 重启指定的进程
$ pm2 startup            # 产生 init 脚本 保持进程活着
$ pm2 web                # 运行健壮的 computer API endpoint (http://localhost:9615)
$ pm2 delete 0           # 杀死指定的进程
$ pm2 delete all         # 杀死全部进程
$ pm2 update             # 更新pm2    
```
