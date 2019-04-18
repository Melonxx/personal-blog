安装
不需要安装，Git Bash 内置了 Git 命令，Git Bash 还内置了以下命令：

ls
mkdir
cp
mv
等等，大概有几十个命令，Git Bash 其实是一个 Bash，不是 Git。

Git Bash 给我们提供了一个虚拟的 Linux 环境，这样我们就不用忍受 Windows 里面垃圾一般的命令行体验了。

配置
请在命令行运行这五句话！！！一定要运行这五句话，不然 git 就不能用了

git config --global user.name 你的英文名字---------#方便产品经理找（怼）你
git config --global user.email 你的常用邮箱---------#方便产品经理找（怼）你
git config --global push.default simple----------------# 本来我写的是 matching，不过想了想可能 simple 更好
git config --global core.quotepath false--------------#防止文件名变成数字
git config --global core.editor "vim"-------------------# 使用vim编辑提交信息
另外很重要的一点！你自己运行 git 的时候注意一下：git remote add origin 后面的地址，不允许使用 https 开头的地址，见下图

![](./images/git-install/git-install_(1).png)

![](./images/git-install/git-install_(2).png)

>[来自转载](https://xiedaimala.com/tasks/ac98cafe-86d3-4842-81f6-d0eec4930e80/text_tutorials/354cf717-8e2d-41fe-9f6f-456935c12967)