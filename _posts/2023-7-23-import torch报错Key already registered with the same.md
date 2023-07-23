---
layout:     post   				    # 使用的布局
title:      import torch报错		# 标题 
subtitle:   Key already registered with the same priority: GroupSpatialSoftmax  #副标题
date:       2023-07-21 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	环境搭建							#标签
---

[TOC]

# 一、前言

最近在跑关键点预测的网络，不出意外环境都要经过报错，这次直接import torch都报错了

具体报错信息如下：

```bash
Key already registered with the same priority: GroupSpatialSoftmax 
```

# 二、解决方法

问题原因：博主提到是==由于重复卸载安装导致的==，确实我个人确实是有重复安装卸载这个操作的。

解决方法很简单：==输入两次pip uninstall torch,再重新安装即可==。

我使用的版本安装命令：

```bash
pip install torch==1.10.1+cu102 torchvision==0.11.2+cu102 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu102/torch_stable.html
```

但报了其他的错误，具体有两个

（1）第一个报错

```bash
C:\Users\user_name\anaconda3\envs\HRNet\lib\site-packages\torchvision\io\image.py:11: UserWarning: Failed to load image Python extension: Could not find module 'C:\Users\user_name\anaconda3\envs\HRNet\Lib\site-packages\torchvision\image.pyd' (or one of its dependencies). Try using the full path with constructor syntax.
  warn(f"Failed to load image Python extension: {e}")
```

这个报错解决方法可以看我[这篇博客](https://blog.csdn.net/SL1029_/article/details/131861187?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22131861187%22%2C%22source%22%3A%22SL1029_%22%7D)

（2）第二个报错

```bash
OMP: Error #15: Initializing libiomp5md.dll, but found libiomp5md.dll already initialized.
OMP: Hint This means that multiple copies of the OpenMP runtime have been linked into the program. That is dangerous, since it can degrade performance or cause incorrect results. The best thing to do is to ensure that only a single OpenMP runtime is linked into the process, e.g. by avoiding static linking of the OpenMP runtime in any library. As an unsafe, unsupported, undocumented workaround you can set the environment variable KMP_DUPLICATE_LIB_OK=TRUE to allow the program to continue to execute, but that may cause crashes or silently produce incorrect results. For more information, please see http://www.intel.com/software/products/support/.
```

这个报错很熟悉了，可以看我[这篇博客](https://blog.csdn.net/SL1029_/article/details/128840668)

# 参考博客

[1.(完全解决）Key already registered with the same priority: GroupSpatialSoftmax](https://blog.csdn.net/qq_43391414/article/details/120096029)

[2.Error #15: Initializing libiomp5md.dll, but found libiomp5md.dll already initialized.](https://blog.csdn.net/SL1029_/article/details/128840668)

[3.torch 不能顺利导入torchvision](https://blog.csdn.net/SL1029_/article/details/131861187?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22131861187%22%2C%22source%22%3A%22SL1029_%22%7D)

