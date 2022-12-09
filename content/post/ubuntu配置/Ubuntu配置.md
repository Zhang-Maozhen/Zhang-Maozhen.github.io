---
title: 服务器配置
subtitle: linux使用、ubuntu上的配置和一些软件操作

# Summary for listings and search engines
summary: ubuntu的配置问题

# Link this post with a project
projects: []

# Date published
date: '2022-12-09T00:00:00Z'

# Date updated
lastmod: '2022-**-**T00:00:00Z'

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

#links:
#  - icon: PDF
#    icon_pack: fab
#    name: PDF
#    url: 
#tags:
#  - 命令
#  - 开源

categories:
   - 服务器配置
#  - Demo
#  - 教程
---



# 	Ubuntu配置

## 文件夹设置

创建了一个公共文件夹`/home/data`

里面放了conda安装包，安装过程见[conda安装](###安装conda)

## 版本信息

`Ubuntu20.04`+`Driver 520.61.05`+`cuda 11.8`+`cudnn8+`

驱动问题：

```shell
#查看是否安装好驱动和可以适配的cuda版本号
nvidia-smi
#查看NAVIDIA型号
lspci | grep -i nvidia
#查看NVIDIA驱动版本
sudo dpkg --list | grep nvidia-*
#查看适合系统的NAVIDIA版本
nvidia-detector	
```



## IP地址

```shell
	LINGSHUI:
	A534:192.168.50.152
```

| wifi账号 | ip地址         |
| -------- | -------------- |
| A534     | 192.168.50.152 |
| LINGSHUI | 10.5.218.231   |



## 全局安装配置

### 前置组件

```shell
sudo apt update
sudo apt upgrade
sudo apt install gcc
sudo apt install make
```



### 安装驱动

| 驱动安装 | www.nvidia.cn/geforce/drivers/ |
| -------- | ------------------------------ |

```shell
#安装命令
bash NVIDIA-*****.run
```



```shell
#查看当前安装驱动命令
dpkg -l | grep NVIDIA
```



### 安装cuda

| cuda | https://developer.nvidia.com/cuda-downloads |
| ---- | ------------------------------------------- |

```shell
#安装工具包
sudo apt install nvidia-cuda-toolkits
#版本信息打印
nvcc -V
```



安装信息：

```shell
Installed in /usr/local/cuda-11.8	
```



### 安装cudnn

| cudn n | https://developer.nvidia.com/rdp/cudnn-download |
| ------ | ----------------------------------------------- |

```shell
#下载压缩包并解压
tar -xzvf cudnn-10.1-linux-x64-v7.6.5.32.tgz
打开终端，解压文件:

#将解压后的文件拷贝至指定目录并赋予权限:
#sudo cp cudnn/include/cudnn.h /usr/local/cuda/include
sudo cp cudnn/include/* /usr/local/cuda/include


sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*

```



### 配置远程登录

```shell
#查看后台是否有ssh服务
ps -e | grep ssh
#安装openssh-server
sudo apt install openssh-server
#查看ssh服务状态
service sshd status
#否则，键入下述命令启动服务，
sudo service sshd start
#获取服务器IP,获取当前局域网下的ip地址：
ifconfig

```

#### 远程可能出现的问题

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20221124182701463.png" alt="image-20221124182701463" style="zoom:50%;" />

原因：

> 此报错是由于远程的主机的公钥发生了变化导致的。
> ssh服务是通过公钥和私钥来进行连接的，它会把每个曾经访问过计算机或服务器的公钥（public key），记录在~/.ssh/known_hosts 中，当下次访问曾经访问过的计算机或服务器时，ssh就会核对公钥，如果和上次记录的不同，OpenSSH会发出警告。

解决方法

> 删除ip载known_hosts相关信息

## 用户添加

```shell
sudo useradd username -m
sudo passwd username
sudo usermod -s /bin/bash username
sudo chown username:username -R /home/user_dir
```



## 个人用户配置

### 安装conda

#镜像地址添加教程：
https://blog.csdn.net/feifei3211/article/details/80361227



安装时可以用下面三步，最source是为了更新下环境变量配置。

```shell
#下载命令
wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh
#断点重下命令
wget -cs https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh
#进行安装
bash Anaconda3-2022.10-Linux-x86_64.sh（下载的文件的文件名）
#安装完成，更新环境变量
source ~/.bashrc
```

conda配置环境【待勘误】

```shell
#配置配置文件
vim ~/.bashrc
#填入/bin地址
<!--  -->
#配置文件生效
source ~/.bashrc
```



安装虚拟环境

```shell
#创建虚拟环境
conda create -n #虚拟环境名称 Python=3.8（python版本号）
conda env list  #--查看有哪些虚拟环境
conda activate  #虚拟环境名称   -- 激活对应环境
python --vision#查看python版本


#安装pytorch
nvcc –V  	#查看cuda版本
conda install pytorch==1.7.1 torchvision==0.8.2 torchaudio==0.7.2 cudatoolkit=10.1
#关闭账户，重新登陆
#进入虚拟环境，进入python环境，import torch—查看是否安装成功
```

注：如果`source ~/.bashrc`后不生效，可能是因为环境变量没有写入

打开配置文件`vim ~/.bashrc`在最后输入

```shell
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/zmz/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/zmz/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/home/zmz/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/zmz/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

生效`source ~/.bashrc`

### Oh-my-zsh安装配置

命令行查看`shell`:

```shell
cat /etc/shells
```

查看当前`shell`

```shell
echo $SHELL
```

安装`zsh`

```shell
sudo apt-get install zsh
```

默认`shell`切换为`zsh`

```shell
chsh -s /bin/zsh
```

安装Oh-myzsh

> 手动安装
>
> ```
> git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
> ```
>
> 自动安装
>
> ```
> sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
> ```
>
> 修改oh-my-zsh配置文件
>
> ```shell
> vim ~/.zshrc
> ```
>
> 修改主题为ys
>
> ```shell
> ZSH_THEME=“ys”
> ```
>
> 添加插件【有的教程建议顺序不要搞反】
>
> ```shell
> plugins=(
>   git zsh-autosuggestions extract web-search zsh-syntax-highlighting 
> )
> ```
>
> 更新配置文件
>
> ```shell
> source ~/.zshrc   
> ```

上面配置的两个插件下载

> 语法高亮`zsh-syntax-highlighting`安装
>
> ```shell
> git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
> ```
>
> 插件自动补全`zsh-autosuggestions`
>
> ```shell
> git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
> ```



# Linux使用

## 下载命令

wget命令

```shell
#用来下载文件，下载到当前目录(默认)或者指定目录
wget https://xxxx.com/xxx
```

apt命令

```shell
#直接安装软件/工具包
apt install xxx
```

运行.run文件

```shell
#直接运行命令，如NVIDIA驱动的安装
bash xxxx.run
```

## 权限

### chmod

除了`u`、`g`、`o`，还有一个`a=ugo`代表所有

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20221124223916214.png" alt="image-20221124223916214" style="zoom:50%;" />

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20221124223856593.png" alt="image-20221124223856593" style="zoom:50%;" />

示例

```shell
chmod 777 file
chmod a=rwx file
```

## u盘挂载

```shell
#查看u盘所在分区
sudo fdisk -lu
#一般u盘分区在/dev/sdb1，使用命令创建挂载目录[这里命名为filename]
mkdir /mnt/filename
#挂载u盘到`/mnt/filename`
sudo mount /dev/sdb1 /mnt/filename
#卸载命令
sudo umount /mnt/filename
```



## 教程地址

h从零开始配一个深度学习服务器：固态+机械双硬盘ubuntu系统的安装、分区、配置超详细教程：https://blog.csdn.net/weixin_44120025/article/details/121714984

Ubuntu挂载硬盘方法：

https://blog.csdn.net/wshixinshouaaa/article/details/81275608?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2~default~BlogCommendFromBaidu~Rate-1-81275608-blog-118679843.pc_relevant_vip_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2~default~BlogCommendFromBaidu~Rate-1-81275608-blog-118679843.pc_relevant_vip_default&utm_relevant_index=1

# 软件配置

## Pycharm使用远程连接服务器

### pycharm中的配置

- windows中找到`settings`
- mac中找到`perferences`

在`build,Excution,Deployment`找到`Deployment`

**添加`SFTP`**

`Connection`配置

> `SSH configuration`：中添加服务器，如果之前没有配置过服务器则需要新建
>
> - `Host`中填写服务器 IP地址，在`username`处填写用户名，`password`中填写密码
> - `Test Connection`按钮测试连接
>
> `Root path`：设置服务器跟路径

`Mappings`配置

> `Local path` ：填写本地代码路径，
>
> `Deployment path`：填写对应服务器的路径【该路径是之前设置的服务器跟路径开始写的相对路径】

至此配置结束



### 上传

从本地写的文件就会被同步上传到服务器上。

如果没有及时上传，可以通过右键上传的文件，通过`Deployment`手动选择`upload`

### 运行

本地运行

> 此时设置了代码的同步上传，并没有设置运行
>
> 因此此时运行代码仍旧是在本地运行。

远程运行

> 如果让代码从服务器上运行，而非本地，需要设置`python`的解释器
>
> 1. 同设置`python`解释器一样，找到添加`python interpreter`，并选择`ssh interpreter`
>
> 2. 新添加或者选择一个已有的服务器端的`python`解释器，此时就可以完成点击运行。在服务器上运行代码
>
> ⚠️注意：要正确的设置服务器端的`python`解释器路径地址，以及映射地址【也就是本地地址和服务器上的地址之间的对应关系】

## ubuntu服务器上设置内网穿透

### 花生壳配置信息

```shell
SN:oraya3b64382067
Default passwd:admin
```

启动服务

```shell
#启动phddns服务
phddns start 
#重启phddns服务
phddns restart 
#重置phddns服务
phddns reset 
#查看phddns状态
phddns status 
#关闭phddns服务
phddns stop
```

### 安装花生壳

下载花生壳`deb`文件

```shell
#下载
wget https://dl-cdn.oray.com/hsk/linux/phddns-5.0.0- amd64.deb
#安装
dpkg -i phddns-5.0.0-amd64.deb
```



记住`SN`和`password`

```json
SN:oraya3b64382067
Default passwd:admin
```

启动服务

```shell
#启动phddns服务
phddns start 
#重启phddns服务
phddns restart 
#重置phddns服务
phddns reset 
#查看phddns状态
phddns status 
#关闭phddns服务
phddns stop
```

### 使用

浏览器访问[http://b.oray.com](http://b.oray.com/) ，输入花生壳Linux 5.0在安装时产生**SN码与默认登录密码admin**登录。

要在账号下添加设备的SN码进行登陆（设备的登陆）
