#### 文件 `.git/HEAD`

>查看 cat .git/HEAD
 ref：refs/heads/master

此文件是一个引用，记录当前分支

#### 文件 `.git/config`

>查看 cat .git/config

此文件记录当前git配置（用户名及邮箱等）
<hr>

查看 git 文件类型
```
git cat-file -t 编码
```
查看 git 文件内容
```
git cat-file -p 编码
```
<hr>

#### 文件夹 `.git/refs`
里面有 `heads` 和 `tags` 目录
head 有一个最近的master
输入 cat master 查看文件内容为 `git` 文件
输入 git cat-file -t 编码 查看该文件类型为 commit
输入 git cat-file -p 编码 发现内容为最近的一次 commit 的具体内容

#### 文件夹 `.git/objects`
里面有很多两个字符的文件夹和 `info` 和 `pack`
>`pack` 的作用是 `git` 的自我梳理过程，如果里面的松散文件过多，会打包到这里

另外以两个字符的文件夹机制是这样的：
![](./images/git-file/git-file_(1).png)

组合查看具体文件类型及内容
