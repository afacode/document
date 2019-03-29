## git
```
添加远程仓库 git remote add
git remote add origin git@github.com:afacode/mymd.git

git push -u origin master -u 可以在推送的同时，将origin仓库的master分支设置为本地仓库当前分支的upstream（上游）。添加了这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin的master分支获取内容，省去了另外添加参数的麻烦。执行该操作后，当前本地仓库master分支的内容将会被推送到GitHub的远程仓库中。在GitHub上也可以确认远程master分支的内容
-u 线上与本地分支相同 省去参数


git add .
git commit -m "aaaa"
git commit -am "aaaa"

查看分支 git branch 
新建分支 git checkout -b feature-A
切换分支 git checkout master
合并feature-A分支 git merge --no-ff feature-A
为了在在历史记录中明确记录下本次分支合并，我们需要创建合并提交 --no-ff 

以图表形式查看分支 git log --graph
只显示提交信息的第一行 git log --pretty=short
git diff

回溯历史版本 git reset
完全恢复至该时间点的状态 git reset --hard fd0cbf0d4a25f747230694d95cac1be72d33441d

修改提交信息 git commit --amend 或 git commit --amend -m

压缩历史 git rebase -i
git rebase -i HEAD~2







```
