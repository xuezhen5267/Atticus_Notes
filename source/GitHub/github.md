# GitHub
## GitHub Create a New Repository
New Repository ->repository name -> public -> add a README file -> Create repository

## 配置 SSH
可以使用 SSH（安全外壳协议）访问和写入 GitHub.com 上的存储库中的数据。 通过 SSH 进行连接时，使用本地计算机上的私钥文件进行身份验证。   
使用 SSH 链接，需要依次执行以下几个步骤：
### Step 0：检查本地 PC 上是否有 SSH 秘钥
```shell
cd ~/.ssh
ls
```
默认情况下，GitHub 的一个支持的公钥的文件名是以下之一，如果没有，则需要在 PC 上生成一个新的 SSH 秘钥。
id_rsa.pub, id_ecdsa.pub, id_ed25519.pub

### Step 1：在本地 PC 上生成新的 SSH 秘钥
官方文档 <https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>

```shell
$ ssh-keygen -t ed25519 -C "xuezhen5267@163.com"
```
当系统提示您“Enter a file in which to save the key（输入要保存密钥的文件）”时，可以按 Enter 键接受默认文件位置。  
当系统提示您"Enter passphrase (empty for no passphrase): [Type a passphrase]" 可以输入安全码或按 Enter 键。
要求输入时，按回车即可，成功后，会输出累死以下的提示：
```
The key's randomart image is:
+--[ED25519 256]--+
| .*..  . . oo    |
| o X... . ..     |
|  * B.o o .      |
|   + * + . o     |
|.o..+ + .        |
|++*..* .         |
|&==*  .          |
+----[SHA256]-----+
```
查看 SSH 秘钥
```shell
$ ls ~/.ssh  # 查看是否成功生成秘钥
id_ed25519  id_ed25519.pub  known_hosts # .pub 是公钥，另一个秘钥
```
### Step 2：将 SSH 秘钥添加到 SSH 代理
启动 SSH 代理
```shell
$ eval "$(ssh-agent -s)"
Agent pid 5462
```
将 SSH 秘钥添加到 SSH 代理中
```shell
ssh-add ~/.ssh/id_ed25519  # 以实际生成的秘钥名称为准
```
### Step 3：将 SSH 秘钥添加到 GitHub 上
官方文档 <https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account>

### Step 4：测试 SSH 链接
```shell
$ ssh -T git@github.com
Hi xuezhen5267! You've successfully authenticated, but GitHub does not provide shell access.
```
## 使用 Git 操作 GitHub 仓库
### 在 Git 上设置 GitHub 仓库
```shell
cd ~/Atticus_Notes # 这里的 Atticus_Notes 是当前工作目录，该目录下有 .git 隐藏文件
git remote add Atticus_Notes git@github.com:xuezhen5267/Atticus_Notes.git # 添加一个新的远程仓库，名称为 Atticus_Notes, 地址为 git@github.com:xuezhen5267/Atticus_Notes.git ，使用 https 地址也可以。
```
### 使用 Git 将本地仓库推送到 GitHub 上
```shell
git push -u Atticus_Notes master
```