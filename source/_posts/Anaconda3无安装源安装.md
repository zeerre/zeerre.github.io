---
title: Anaconda3无安装源安装
date: 2021-12-06 21:00:23
tags:
    - Anaconda
categories:
    - 技术文档
---



### Windows 版本

#### 清华镜像站下载 Anaconda3

[下载链接](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)

#### 上海交通大学镜像站下载 Anaconda3

[下载链接](https://s3.jcloud.sjtu.edu.cn/899a892efef34b1b944a19981040f55b-oss01/anaconda/miniconda/mirror_clone_list.html)

#### 北京大学开源镜像站下载 Anaconda3

[下载链接](https://mirrors.pku.edu.cn/anaconda/miniconda/)

#### 安装

双击下载的 ***.x86_64.exe** 文件（最好是选择最新版本，若需要特殊版本定位支持的 Python 版本即可），一路回车，默认安装，不提使用细节。

<!-- more -->

### Linux 版本

在所有的 Linux 发行版都可以利用 curl 直接从官网 (https://repo.anaconda.com) 下载，然后利用 **bash** 安装。

#### 下载

    curl https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh --output anaconda3-python-linux.sh

这里可以访问[官网](https://repo.anaconda.com/archive)查看最新版本，然后在利用 **curl** 下载。

#### bash 安装

    bash anaconda3-python-linux.sh

根据提示完成目录选择，生成环境变量等选择，完成安装。

#### 环境变量设定

    source ~/.bashrc

#### conda 命令操作

    conda update --all -y
    conda install conda="最新版本号" #更新 conda 。
    conda create -n '新创建环境名' python='所需 python 最新版本号' '其他需要的模块,多个利用空格隔开'
    conda env list
    conda info
    conda info list
    ...

### 问题及解决

但使用过程中是需要及时更新 Anaconda3 的，若不小心弄丢 `conda`，
那就需要注意一些操作细节：

若要使用 Anaconda3 的软件包，需要切换进 Anaconda3 ，然后利用 `conda` 命令进行进行操作，不详说。
更新时弄丢 `conda` 多半是没有切换进 Anaconda3 导致，原因很简单，Linux 发行版自带 Python，将
Anaconda3 写入环境变量那就会引起冲突，导致 `conda` “无地姿容”。因此，但凡进行更新或安装等操作时，
需要切换进 Anaconda3 然后再进行：

```
conda activate ENV_name

```
若真的弄丢了 `conda` 也不要紧，重新安装一下就 OK 了,只是需要加上 **-u** 参数。

```
bash anaconda3-python-linux.sh -u
```

好了，使用中总结经验，记录下来方便后来人和自己。不当之处，敬请指正。