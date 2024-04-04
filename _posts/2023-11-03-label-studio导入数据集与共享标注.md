---
layout:     post   				    # 使用的布局（不需要改）
title:      label-studio导入数据集同时共享给他人标注 				# 标题 
subtitle:    label-studio使用
date:       2023-11-03 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	工具							#标签
---

[toc] 

# 一、前言

上次写博客还是三个月前，希望接下来的一段时间可以把前面几个月学到的一些内容沉淀下来。本文将介绍一个标注软件label studio如何导入数据集，以及如何分享链接给他人标注。

PS:本文默认已经安装好label studio，安装过程可以看[官方文档](https://labelstud.io/guide)，本文使用的是pip安装在虚拟环境。
系统：ubuntu20.04

# 二、导入数据集
## 2.1.设置环境变量
在启动虚拟环境之前需要先设置环境变量允许访问本地文档
~~~bash
export LABEL_STUDIO_LOCAL_FILES_SERVING_ENABLED=true
~~~
设置数据集路径,设置为数据集的上一个文件夹即可，例如：如果数据集在/home/user/desktop/A/dataset，则设置为/home/user/desktop/A
~~~bash
export LABEL_STUDIO_LOCAL_FILES_DOCUMENT_ROOT=/home/user...
~~~
## 2.2.启动label studio
### 2.3.1激活label studio安装的环境
例如：
~~~bash
conda activate label-studio
~~~
### 2.3.2.启动label studio
~~~bash
label-studio start 
~~~
也可以指定端口用于分享给他人,指定的端口号不要被占用即可
~~~bash
label-studio start --port 5000
~~~
## 2.3.设置clould storage
### 2.3.1.新建项目
需要先新建一个项目，来导入数据集标注
### 2.3.2.开始设置clould storage
依次点击项目对应的**settings**,然后点击**clould storage**，先点击**add source storage**设置输入数据集路径。
**Storage Type:**打开后选择**local files**
**Storage Title**设置显示的**数据集名字**
**Absolute local path**:数据集的路径，一般是设置的环境环境变量下的某个文件夹，例如这里:**/home/user/desktop/A/dataset**
**File Filter Regex**:文件过滤器，例如如果你只需要显示数据集里面的的png,则可以使用.*png过滤。
Treat every bucket object as a source file：打开
可以先点击check connection 如果设置没有错误，会显示**成功连接**
点击保存，再点击sync storage既可以看到数据集被导入了。

# 三、分享给他人标注
## 3.1.开放端口
这里的端口也即label-studio所运行的端口，运行(port_number为端口号)
~~~bash
sudo ufw allow port_number
sudo ufw enable
~~~
查看端口是否开放
~~~bash
sudo ufw status
~~~
若没有ufw需要先安装，安装命令
~~~bash
sudo apt-get update
sudp apt-get install ufw
~~~
## 3.2.查看ip
运行,得到ip地址
~~~bash
ifconfig
~~~
## 3.3.生成链接
输入这个即可访问
~~~bash
http://ip:port
~~~
值得注意的是，需要在同一个局域网才能访问