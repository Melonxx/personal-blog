## 配置 npm 淘宝源

```
npm config set registry https://registry.npm.taobao.org/
```

如果后悔了，想撤销淘宝源就运行 `npm config delete registry`

## 安装

```
npm install typescript@2.9.2 -g
npm install ts-node@7.0.0 -g
```

注意记下 ts-node 安装后的可执行文件路径，后面要用的。

![](./images/ts-install/ts-install_(1).png)



## 调试

1.  下载 vscode
    1.  按 ctrl+K ctrl+S
    2.  将格式化文件的快捷键绑定到自己喜欢的按键（我用的是 ctrl+L）
2.  创建文件夹 tsdemo
3.  用 vscode 打开 tsdemo 目录
4.  创建 tsdemo/1.ts 作为我们的第一个 TS 文件
5.  在文件里写一句 `console.log(1)` 保存
6.  Windows 用户注意了，这里需要单独运行一些命令（Linux 用户和 macOS 用户不用执行）

    ```
     npm init -y
     npm i -D ts-node typescript

    ```

7.  创建 tsdemo/.vscode/launch.json 文件，内容如下

    ```
     {
         "configurations": [
             {
             "name": "ts-node",
             "type": "node",
             "request": "launch",
             "program": "注意看这里，要写成ts-node对应的可执行文件，Windows 用户注意了，你应该写成 ${workspaceRoot}/node_modules/ts-node/dist/bin.js",
             "args": ["${relativeFile}"],
             "cwd": "${workspaceRoot}",
             "protocol": "inspector"
             }
         ]
     }

    ```

8.  打开 tsdemo/1.js，找到调试选项，选择 ts-node，然后点击调试

![](./images/ts-install/ts-install_(2).png)

9.  然后你就可以看到 console.log(1) 的输入结果了（请确保你选中的是 tsdemo/1.ts）

![](./images/ts-install/ts-install_(3).png)


参考文章：[https://segmentfault.com/a/1190000011935122](https://segmentfault.com/a/1190000011935122 "null")

---ps：使用命令行打包TS。`tsc xx.ts`，持续监听并打包`tsc -w xx.ts`