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
```
## How to use Sphinx?
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
    'sphinx_markdown_tables'
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
## Upload Sphinx to Github
TBD
