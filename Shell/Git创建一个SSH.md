## 一、Generating a new SSH key

1.  Open Git Bash.

2.  Paste the text below, substituting in your GitHub email address.

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" 
```
#### // 输入这句话，连按三个回车，直接跳过下面的步骤
 This creates a new ssh key, using the provided email as a label.

```
Generating public/private rsa key pair.
```

3.  When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

```
Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]
```

4.  At the prompt, type a secure passphrase. For more information, see ["Working with SSH key passphrases"](https://help.github.com/articles/working-with-ssh-key-passphrases).

```
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```
## 二、安装完成之后，输入命令 `ll ~/.ssh` 查看是否安装成功，出现 `id_rsa`（锁） 和 `id_rsa.pub`（钥匙） 成功

![](./images/git-ssh/git-ssh_(1).png)

## 三、出现锁🔒之后，我们输入命令`cat ~/.ssh/id_rsa.pub`查看内容并上传github

![](./images/git-ssh/git-ssh_(2).png)

![](./images/git-ssh/git-ssh_(3).png)

## 四、创建完成之后我们可以测试一下
*如果之前已经存在，输入命令`rm ~./ssh/known_hosts`删除之前的连接*
##### 输入命令测试连接
```
ssh -T git@github.com
```
![yes](./images/git-ssh/git-ssh_(3).png)









