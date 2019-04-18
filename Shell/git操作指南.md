查看配置
```
git config --global --list
// 可选参数 
--global(有所仓库)
--local(当前仓库) 
--system(当前系统登陆用户)
```
设置名字、邮箱
```
git config --global user.name 'yourName'
git config --blobal user.email 'email@domin.com'
```
给文件重命名的简便方法
```
git mv fileName fileName
```
查看日志
```
git log
// 只查看简单的log
git log --oneline
// 查看最近几次log
git log -n5 // 可组合 git log -n5 --online
// 查看所有记录包括其他分支
git log --all
// 图形化
git log --all praph
```
分支
```
// 创建分支
git branch <name>
// 创建分支并切换
git checkout -b <name>
// 查看所有本地分支
git branch
// 查看所有远程分支
git branch -r
// 查看有本地分支和远程分支
git branch -a （详细信息 -av）
// 删除分支
git brach -d <name> （强制-D）
```
变更记录
```
工作区和暂存区的不同
git diff (-- 文件名 [比较某几个文件差异，用空格分隔])（或者直输入分支，比较分支所有差异，后面也可以继续跟 -- fileName 查看某个文件）（或者某两次提交的git编码）
暂存区和 head 比较
git diff --cached
和上 `1` 次比较
git diff HEAD HEAD^1 （或 HEAD~1）
```
修改上一次的commit
```
git commiit --amend
// 修改旧的commit
git rebase -i 想修改的上一次commit编码
把pick改成reword，只修改commit message， 退出git就会自动提交。
```

恢复
```
恢复到 HEAD 的内容
git reset --head HEAD (加上<-- fileName>恢复单个文件，不加恢复所有)
让暂存区恢复成和HEAD的一样（取消git add ...）
git reset HEAD (加上<-- fileName>恢复单个文件，不加恢复所有)
让工作区恢复到 暂存区 的内容
git checkout -- 文件名
// 恢复到某次提交
git reset --hard 提交编码
```

临时仓库
```
停止目前的修改，恢复到上一次状态
git stash
恢复到临时仓库
git stash pop (恢复并在临时仓库中删除) git stash apply (恢复并在临时仓库中保留)
临时仓库列表 
git stash list
```

<hr>
ps：

linux命令复制一个文件
```
cp ./file ./file
ls -al 查看所有文件详细信息 
ll 同上，除隐藏文件外
```