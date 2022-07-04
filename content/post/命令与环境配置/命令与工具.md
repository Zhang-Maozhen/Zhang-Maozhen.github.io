---
title: 命令与环境配置
subtitle: Conda、Git、Linux命令语法

# Summary for listings and search engines
summary: 记录一些用到的命令与法和操作上的问题.

# Link this post with a project
projects: []

# Date published
date: '2022-06-27T00:00:00Z'

# Date updated
lastmod: '2022-06-27T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin
#  - 吳恩達

tags:
  - 命令
  - 开源

categories:
   - Conda
   - Git 
   - Linux 
#  - Demo
#  - 教程
---

命令记录，持续更新

<!--more-->

# Conda

## 命令

### 对于conda的一些理解

作为一个虚拟环境配置管理软件，可以在电脑自身环境外创造虚拟环境(base是自身创造的虚拟环境)

在每个虚拟环境中就会相当于一个电脑（如虚拟机）

Pycharm等IDE开发工具可以选择conda中的某一个虚拟环境使用，进行的操作（更新、安装、删除）都会直接对虚拟环境进行操作

可再Pycharm中的setting中查看虚拟环境的包，同`conda list`

### shell Conda配置

#### 环境

查看当前系统环境

```shell
conda info -e
conda env list
```

创建新的环境

```shell
# 指定python版本为2.7，注意至少需要指定python版本或者要安装的包
# 自动安装python2.7最新版本
conda create -n env_name python=2.7
# 同时安装必要的包
conda create -n env_name numpy matplotlib python=2.7
```

删除环境

```shell
conda remove -n envs_name --all
```



克隆环境

```
conda create -n new_env1 new_env
```

环境切换

```Shell
# 切换到新环境
# linux/Mac下需要使用source activate env_name
conda activate env_name
#退出环境，也可以使用`activate root`切回root环境
conda deactivate env_name
```

查看当前环境的所有包

```
conda list
```

#### 镜像

镜像源

```shell
conda config --show
```

查看配置项`channels`，如果显示带有`tsinghua`,则说明已经安装过清华镜像

```shell
channels:
- https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/
- https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
- https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
- https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
- https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
```

使用`conda config --removecjannels url 地址`删除清华镜像，如：

```shell
conda config --removecjannels url https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/
```

添加中科大镜像

```shell
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
```

设置搜索时显示通道地址

```shell
conda config --set show_channel_urls yes
```

#### 配置虚拟环境的jupyter

```shell
pip install ipykernel
#设置jupyter的名称，jupyterName是显示在jupyter的名称
python -m ipykernel install --name jupyterName
#执行打开jupyter notebook
jupyter notebook
#如果发现jupyter notebook not found，则需要进行安装
pip install jupyter notebook
```



### 升级命令

#### pip升级

```shell
python -m pip install --upgrade pip
```

#### 包升级

```shell
pip install --upgrade 包名
#如
pip install --upgrade matplotlib
```

### 问题

##### 代码运行不报错，直接结束，排查发现在导入库名处终止

异常原因：

> 导入的包或许需要升级，兼容性存在问题

解决方法，升级/降级库版本

```shell
pip install --upgrade matplotlib
```

##### 报错源路径下找不到`current_repodata.json`

```shell
# 报错信息：
PS C:\Users\Mz> conda create -n env_python2 python=2.7
Collecting package metadata (current_repodata.json): failed

CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/win-64/current_repodata.json>
Elapsed: -

An HTTP error occurred when trying to retrieve this URL.
HTTP errors are often intermittent, and a simple retry will get you on your way.
'https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/win-64'
```

更改方法：

> 在进行常规操作的时候，比如创建一个虚拟环境，会报找不到`current_repodata.json`这个文件，这个时候，把路径`Anaconda3/Library/bin`目录下的`libcrypto-1_1-x64.dll`和`libssl-1_1-x64.dll`拷贝到`Anaconda3/DLLs`目录下即可。

# Git

## Git与github连接

1. 创建SSH Key

> 本地Git仓库和GitHub仓库之间通过SSH加密传输的，GitHub需要识别是否是你的推送，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送，所以需要配置SSh Key
>
> 产生的密码在用户主目录下，id_rsa和id_rsa.pub两个目录

```git
ssh-keygen -t rsa -C"1209173759@qq.com"
```

2. 登录github上，在``SSH and GPG keys``页面Add SSH Key粘贴id_rsa.pub文件里的全部内容
3. 检验是否添加成功，在git bash里面输入

```git
ssh -T git@github.com
```

4. 设置username和email，github每次commit都会记录该信息（分别是哪个用户进行的上传）

```git
git config --global user.name "name"//github登录名，上传信息的名字
git config --global user.email "email"//你的github注册邮箱
```

5. 把本地仓库传到github上/把远程仓库下载到本地

```
//本地仓库传到github上
git remote add origin git@github.com:Zhang-Maozen/仓库名.git
git push -u origin master

//远程仓库下载到本地
git clone HTTPS //HTTPS是github上的连接地址 //会将连接下载到本地仓库，作为一个项目文件
cd projectName
```



## Git操作

### Git上传文件到远程仓库

```git
//起名为origin
git remote add origin https://gitee.com/Danerszz/my-note.git
```



### 查看状态

```
git remote 
git remote -v
```

可以查看到别名， 一般默认为origin

```
git remote show origin 显示详细信息
git remote rm origin 删除此别名的长裤设置
git remote add 仓库别名 Url
```

### 删除连接

```
git remote rm origin
```

### 添加连接

```
//本地仓库传到github上
git remote add origin git@github.com:Zhang-Maozen/仓库名.git
```

### 分支

```shell
#查看分支
git branch
#创建分支
git branch newBranchName
#切换分支
git checkout branchName
#推送分支到远程
git push origin branchName

#查看远程分支
git branch -a
```

### 获取远程库与本地同步合并

如果远程库不为空必须做这一步，否则后面的提交会失败

//不加这句可能报错，原因是 github 中的 README.md 文件不在本地仓库中
//可以通过该命令进行代码合并***

```shell
git pull --rebase origin master
```



### 问题

#### 撤销commit，同时保留git add

```java
git reset --soft HEAD^
```

#### 删除工作空间改动的代码，撤销commit，并且撤销git add

```
git reset --hard HEAD^
```

#### 修改git commit注释内容

```
git commit --amend
```

#### 还没git  commit ,只撤销git add,此时会保留本地修改（绿字变红字）

```
git reset HEAD filename
```

## 错误

#### fatal: refusing to merge unrelated histories

本地仓库在想做同步远程仓库到本地为之后本地仓库推送到远程仓库做准备时报错了，错误如下：

`fatal: refusing to merge unrelated histories`

解决：出现这个问题的最主要原因还是在于本地仓库和远程仓库实际上是独立的两个仓库。假如我之前是直接clone的方式在本地建立起远程github仓库的克隆本地仓库就不会有这问题了。

查阅了一下资料，发现可以在pull命令后紧接着使用`--allow-unrelated-history`选项来解决问题（该选项可以合并两个独立启动仓库的历史）。

```
$git pull origin master --allow-unrelated-histories
```

#### OpenSSL SSL_connect: Connection was reset in connection to github.com:443

查看全局配置：

```
git config --global -l
```

已有设置情况修改代理项：

```
git config --global --unset http.proxy 
```

如果使用过VPN或者代理，关机时未关，通过cmd下命令行ipconfig/flushdns清理dns缓存

## 个人主页与GIt操作

### 发布到master

仓库中创建`\docs`，将生成的静态网页放入，在仓库`setting`页面选择`pages`，选择生成原路径`docs`

### 发布到gh-pages branch

如果希望单独控制源文档和生成的网页文档的版本历史，可以使用单独建立一个`gh-pages branch`的方法托管到`Github Pages`——先将`/public`子目录添加到`.gitignore`中，让`master branch`忽略其更新，然后在本地和Github端添加一个名为`gh-pages`的branch：

```shell
//忽略public子目录
echo "public" >> .gitignore
//初始化gh-pages branch
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "Initializing gh-pages branch"
git push origin gh-pages
git checkout master
```



# Linux

### Linux的档案调用权限

三级：档案拥有者、群组、其他

1. u°表示该档案的拥有者
2. g°表示与该党案的拥有者属于同一个群体（group）者
3. o°表示其他以外的人
4. a°表示三者都是

### `tar` 压缩解压

压缩文件 非打包,将test.tar.gz压缩为a.c

`tar -czvf test.tar.gz a.c`

列出压缩文件内容

`tar -tzvf test.tar.gz`

将解压名.tar.gz解压到当前目录

`tar -xzvf 解压名.tar.gz`

### `cat` 链接文件并打印到标准输出设备上

语法格式：

`cat [-AbeEnstTuv] [--help] [--version] filename`

> - **-n 或 --number**：由 1 开始对所有输出的行数编号。
> - **-b 或 --number-nonblank**：和 -n 相似，只不过对于空白行不编号。
> - **-s 或 --squeeze-blank**：当遇到有连续两行以上的空白行，就代换为一行的空白行。
> - **-v 或 --show-nonprinting**：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
> - **-E 或 --show-ends** : 在每行结束处显示 $。
> - **-T 或 --show-tabs**: 将 TAB 字符显示为 ^I。
> - **-A, --show-all**：等价于 -vET。
> - **-e：**等价于"-vE"选项；
> - **-t：**等价于"-vT"选项；



把textfile1的文档内容加上行号后输入textfile2文档中

`cat -n textfile1 > textfile2`

把textfile1和testfile2的文档内容加上行号（空白行不加）之后将内容附加到textfile3文档里

`cat -b textfile1 textfile2 >> textfile3`

清空/etc/text.txt文档内容

`cat /dev/null /etc/test.txt`

### `chmod ` 控制用户对文件的权限的命令

chmoc [-cfvr] [--help] [--version] mode file ...

> - u 表示该文件的拥有者，g 表示与该文件的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表示这三者皆是。
> - \+ 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
> - r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。
>
> 
>
> - -c : 若该文件权限确实已经更改，才显示其更改动作
> - -f : 若该文件权限无法被更改也不要显示错误讯息
> - -v : 显示权限变更的详细资料
> - -R : 对目前目录下的所有文件与子目录进行相同的权限变更(即以递归的方式逐个变更)
> - --help : 显示辅助说明
> - --version : 显示版本



将文件file1.txt设为所有人皆可读：

`chmod ugo+r file1.txt`

将文件file1.txt设为所有人皆可读取：

`chmod a+r file1.txt`

将文件file1.txt与file2.txt设为该文件拥有者，与其所属一个群体着可写入，但其他以外的人则不可写入：

`chmod ug+w,o-w file1.txt file2.txt`

将ex1.py设定为只有该文件拥有者可执行：

`chmod u+x ex1.py`

将目前目录下的所有文件与子目录皆设为任何人可读取：

chmod -R a+r *

## ubuntu

### 安装桌面

更新apt

```shell
sudo apt update && sudo apt upgrade
```

两种方法，安装桌面环境

- apt安装

- tasksel的Debian工具

```shell
#tasksel
sudo tasksel install ubuntu-desktop
#apt
sudo apt install ubuntu-desktop
```

安装和配置显示管理器

默认GDM3，但过大，使用如下显示管理器

```shell
sudo apt install lightdm
```

用下面的命令启动显示管理器并加载 GUI：

```javascript
sudo service lightdm start
```

你可以使用下面的命令来检查当前的显示管理器：

```javascript
cat /etc/X11/default-display-manager
```

运行后得到的结果类似这样：如果你想关闭 GUI，那么打开一个终端并输入：

```javascript
sudo service lightdm stop
```
