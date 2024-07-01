---
layout:     post   				    # 使用的布局
title:      torch不能顺利导入torchvision				# 标题 
subtitle:   Could not find module 'C:\Users\user_name\anaconda3\envs\HRNet\Lib\site-packages\torchvision\image.pyd  #副标题
date:       2023-07-21 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	环境搭建							#标签
---

[TOC]

# 一、前言

最近再跑关键点预测网络，又到了**环境报错**的阶段，在预测时报错如下：

UserWarning: Failed to load image Python extension: Could not find module 'C:\Users\user_name\anaconda3\envs\HRNet\Lib\site-packages\torchvision\image.pyd' (or one of its dependencies). Try using the full path with constructor syntax.

warn(f"Failed to load image Python extension: {e}")

# 二、解决方法

由于之前是在官网的命令安装的，理论上不应该存在版本的问题，==猜测还是由于自己多次重复卸载安装torch和torchvision导致的版本混乱。==

找到了当时的安装命令：

```bash
pip install torch==1.10.1+cu102 torchvision==0.11.2+cu102 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu102/torch_stable.html
```

可以看到安装的是 torchvision==0.11.2

**PS:如果不记得安装的命令可以pip list查看安装的哪个版本**

具体解决方法：

  （1）==我们先卸载torchvision==

  ```bash
  pip uninstall torchvision
  ```

  （2）==再重新安装torchvision==

  在安装前可以先看一下是否卸载干净了，输入：

  ```bash
  pip list 
  ```

  ==确认卸载干净后再次重新安装==

  ```bash
  pip install torchvision==0.11.2
  ```


# 参考博客

[1.这篇博客评论大家有提到解决方法（文章博主的方法我有试过但是失败了）](https://blog.csdn.net/weixin_45640009/article/details/121000529)

[2.官方安装torch版本网址](https://pytorch.org/get-started/previous-versions/)



