# How to install Git
```shell
sudo apt install git
```
# Git 理论
## Git 的四个区域：  
工作区（workspace）：存放代码的文件夹。

暂存区（index/stage）：工作区中隐藏文件夹 .git 中的的一个文件，不重要。

本地仓库（local repository）： 用于存放历史版本数据，不重要。

远程仓库（eg. GitHub）： 用于托管代码的服务器。  
## Git 中文件的四种状态
查看仓库中的文件状态
```shell
git status
```
untracked：未跟踪，不参与版本控制。通过 `git add` 可以将文件状态改为 staged状态。

unmodified：文件入库暂且未被修改。使用`git rm`可以将文件还原为 untracked。

modified：文件被入库暂存区且**被**修改。使用 `git add`可以跟踪当前修改，将当前状态变为 staged；使用`git checkout`可以回溯到之前跟踪的状态（覆盖当前状态）。

staged：暂存状态。使用`git commit`将该文件入库，入库后文件状态变为 unmodified 状态；使用`git reset HEAD filename` 取消暂存，文件状态变为 Modified状态。

# How to use Git
## 配置 Git
在使用 Git 前，需要配置 Git 的用户名和邮箱
```shell
git config --global user.name "Atticus"
git config --global user.email xuezhen5267@163.com
```
## 创建仓库
```shell
git init                    # 把当前目录当做仓库项目根目录
git clone xxx               # 在当前目录中创建一个文件夹，该文件夹（而非当前目录）为仓库根目录
```
## Git 基本操作
正向操作
```shell
git add .                   # 将工作区内所有文件添加到暂存区
git add filename            # 将工作区内的 filename 文件添加到暂存区
git commit -m “message”     # 将暂存区文件提交到本地仓库 
```
恢复文件
```shell
git checkout HEAD filename  # 用仓库中的 filename 文件覆盖工作区中的 filename 文件
```
## 配置 .gitignore 文件
.gitignore 文件只能忽略那些没有被 track 的文件，如果这些文件已经被纳入到版本管理中，则修改 .gitingore 是无效的。 解决方案是吧本地缓存删除，然后再提交。
```shell
git rm -r --cached .
``` 

无任特殊符号，既表示文件也表示文件夹
```shell 
folder              # 与 “folder” 同名的文件和文件夹都会被忽略
```
/ 放在字符串后，表示该字符串仅表示文件夹名称
```shell
folder/             # 只有与 “folder” 同名的文件夹都会被忽略
```
！ 放在字符串前，表示取反，不忽略，在忽略的前提下除外
```shell
folder              # 先忽略与 “folder” 同名的文件和文件夹
!folder/            # 再明确与 “folder” 同名的文件夹除外，只忽略文件
```
\* 表示统配字符
```shell
doc/*.txt           # 忽略的文件需要满足两个条件：
                    # 第一，在doc文件夹下一级
                    # 第二，文件类型为 .txt
                    # 所以，doc/abc.txt 会被忽略，doc/xyz/abc.txt 不会被忽略
```
## 正规开源项目的 Git 使用方法
参考 <https://www.bilibili.com/video/BV19e4y1q7JJ/?spm_id_from=333.337.search-card.all.click&vd_source=45a2f0bac62c02182ba7aa5ba31b556e>

在某个 tag 的基础上创建新的 branch, tag 名称可以区 GitHub 上查看
```shell
git checkout -b branch_name tag_name
```

## Git 其他问题
### Git 克隆项目太大，克隆超时
参考 <https://blog.csdn.net/wsmrzx/article/details/115793236>

报错如下：
```shell
fetch-pack: unexpected disconnect while reading sideband packet
```
方案1 增加 http 缓存
```shell
git config --global http.postBuffer 1048576000 # post Buffer 单位是 Byte， 1048576000 为1G
```
方案2 放宽 http 网络传输限制
```shell
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999
```