## macOS 设置
### 给MacOS终端CLI添加颜色 Terminal
* ` vim ~/.bash_profile `

```
export CLICOLOR=1
export LSCOLORS=ExGxFxdaCxDaDahbadeche
# 修改苹果终端的样式与默认提示符的颜色
# 修改终端提示符（用户名颜色，主机名颜色、路径颜色）颜色
export PS1='\[\033[01;36m\]\u@\h\[\033[01;32m\] \W\$\[\033[00m\]'
```
* 更新刚配置的环境变量 ` source .bash_profile `

### ll = ls -al 设置别名 
* `vim ~/.bash_profile alias ll='ls -al' `

```
pwd

cd ~ 回到home路径
cd - 返回上一步

mkdir 创建目录
rmdir 删除空路径
touch
cp 复制文件和目录
mv 移动、重命名文件或目录
less 查看文件内容
cat 查看文件内容
exit 退出命令行

清除屏幕： ctrl + l or clear
清除当前行： ctrl + u
将光标移动到行尾：ctrl + e
将光标移动到行首：ctrl + a
清除至当前行尾： ctrl + k

```