# Sphinx
## What is Sphinx？
TBD
## How to install Sphinx?
安装依赖
```shell
sudo apt install git
sudo apt install make
sudo apt install python3
sudo apt install python3-pip
```
安装 Sphinx
```shell
pip3 install -U Sphinx
```
安装 Sphinx 可能需要的软件包
```shell
pip3 install sphinx-autobuild
pip3 install sphinx_rtd_theme
pip3 install recommonmark
pip3 install sphinx_markdown_tables
pip3 install sphinx_copybutton
```
## 本地编译 Sphinx 项目
### 使用 sphinx-quickstart
```shell
mkdir Atticus_Notes
cd Atticus_Notes
sphinx-quickstart
make html
```
Note1 询问是否创建独立目录，输入 y 表示创建独立目录。  
Note2 询问语言，输入 zh_CN 表示中文。  
Note3 执行完 sphinx-quickstart 只是帮忙创建好了文件夹，但是并未编译，需要执行 make html 后， build 文件夹中才有文件。  
### 使用 sphinx-autobuild
```shell
cd Atticus_Notes
sphinx-autobuild source build/html
```
浏览器地址栏输入 <http://127.0.0.1:8000> , 即可看到预览画面。
### 设置 sphinx 主题
打开 source/conf.py 文件，修改如下
```python
html_theme = 'sphinx_rtd_theme'
```
### 设置 sphinx 支持 markdown
打开 source/conf.py 文件，修改如下
```python
extensions = [
   'recommonmark',
   'sphinx_markdown_tables',
   'sphinx_copybutton'
]
```
### 创建内容
在 source 文件夹下建立各种独立的文件夹 Chapter_X,用于存放每个章节的内容，每个文件夹内都需要包含一个 index.rst 文件，内容如下所示。
```rst
TITLE OF CHAPTER X
==================

.. toctree::
   :maxdepth: 2
   :caption: Conents:
   
   content_1.md
   content_2.rst
   sub_chapter/content_3.md
   sub_chapter/content_4.rst
```
其中 content_1.md, content_2.rst 等是单独的文档，用以存储小章节的内容，这些文档级别可以使用 markdown 书写。

在 source 目录下，也有一个 index.rst 文件，需要将每个子文件夹(eg. Chapter_1)下的 index.rst 文件路径添加进来，如下。
```rst
.. Atticus' Notes documentation master file, created by
   sphinx-quickstart on Tue Jan  2 21:13:36 2024.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Atticus Notes's documentation!
==========================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   Chapter_1/index.rst

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```
# 使用 Read the Docs 远程编译/托管 Sphinx 项目
## 添加新文件
如果要使用 Read the Docs 远程编译/托管 Sphinx 项目，则需要在项目根目录中添加三个文件，分别是 .readthedocs.yaml， requirements.txt，和 .gitignore。
### 添加 requirements.txt 文件
添加 requirements.txt 的目的是为了在 Read the Docs 服务器上安装与本地相同的 python 插件。  
requirements.txt 内容如下所示：
```
recommonmark==0.7.1
sphinx-autobuild==2021.3.14
sphinx-markdown-tables==0.0.17
sphinx-rtd-theme==2.0.0
sphinx-copybutton==0.5.2
```
### 添加 .readthedocs.yaml 文件
官方文档 <https://docs.readthedocs.io/en/stable/config-file/>  
.readthedocs.yaml 内容如下所示：
```
# Read the Docs configuration file for Sphinx projects
# See https://docs.readthedocs.io/en/stable/config-file/v2.html for details

# Required
version: 2

# Set the OS, Python version and other tools you might need
build:
  os: ubuntu-22.04
  tools:
    python: "3.12"
    # You can also specify other tool versions:
    # nodejs: "20"
    # rust: "1.70"
    # golang: "1.20"

# Build documentation in the "docs/" directory with Sphinx
sphinx:
  configuration: conf.py
  # You can configure Sphinx to use a different builder, for instance use the dirhtml builder for simpler URLs
  # builder: "dirhtml"
  # Fail on all warnings to avoid broken references
  # fail_on_warning: true

# Optionally build your docs in additional formats such as PDF and ePub
# formats:
#   - pdf
#   - epub

# Optional but recommended, declare the Python requirements required
# to build your documentation
# See https://docs.readthedocs.io/en/stable/guides/reproducible-builds.html
python:
  install:
    - requirements: requirements.txt
```
### 添加 .gitignore 文件
因为 Read the Docs 会重新编译文件，所以不需要 build 文件夹中的内容，.gitignore 内容如下：
```
build/
```

## 建立本地仓库
```shell
cd ~/Atticus_Notes # 进入本地 Sphinx 项目工作目录
git init 
git add . # 添加到缓存区
git commit -m "initial commit" # 提交到本地仓库
```
## 设置 SSH 秘钥
详见 GitHub 章节

## GitHub 仓库
设置 GitHub 仓库
```shell
cd ~/Atticus_Notes # 这里的 Atticus_Notes 是当前工作目录，该目录下有 .git 隐藏文件
git remote add Atticus_Notes git@github.com:xuezhen5267/Atticus_Notes.git # 添加一个新的远程仓库，名称为 Atticus_Notes, 地址为 git@github.com:xuezhen5267/Atticus_Notes.git ，使用 https 地址也可以。
```
使用 Git 将本地仓库推送到 GitHub 上
```shell
git push -u Atticus_Notes master
```
# 将 Sphinx 项目 托管到 Read the docs 网站
参考 <https://zhuanlan.zhihu.com/p/618886468>
官网 <https://readthedocs.org/dashboard/> 注册病登录

Import a Project -> xuezhen5267/Atticus_Notes -> default branch -> master -> Next -> Finish

