用 macOS 的同学按照如下命令做就行了，比 Windows 的 Git Bash 方便很多。

## 安装命令行工具

首先你要让命令行翻墙：

1.  如果你有 VPN，直接开启 VPN 即可
2.  如果你的是 Shadowsocks，那么你需要按照 [这篇帖子](https://jscode.me/t/mac-linux-proxychains-ng/1141 "null") 让命令行翻墙

    1.  安装 homebrew

        ```
        /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
        ```

    2.  安装 proxychains-ng
        ```
        brew install proxychains-ng
        ```

    3.  配置proxychains-ng

        2.  下载配置文件

            ```
             curl -L https://raw.githubusercontent.com/FrankFang/dot-files/master/proxychains.conf > ~/.proxychains.conf
            ```

        3.  添加 bash alias，运行

            ```
             touch ~/.bashrc; echo 'alias pc="proxychains4 -f ~/.proxychains.conf"' >> ~/.bashrc
            ```

        4.  `source ~/.bashrc`
        5.  pc git clone xxx 或者 pc brew install xxx ，那么这个命令行就是翻墙的。

然后就可以安装工具了

```
xcode-select --install
如果你是 vpn 就运行：brew install coreutils vim node git wget 
如果你是 SS 就运行：pc brew install coreutils vim node git wget 
npm i -g fanyi
```

你的 git、node、npm、fanyi 等命令就都有了 :)

## 安装 iTerm2

[https://www.iterm2.com/](https://www.iterm2.com/ "null")

iTerm2 比 macOS 自带的 Terminal 好用很多。

配置方法 [见此](http://yijiebuyi.com/blog/9c6419897949a7935d0fdec74cb7c61b.html "null")
