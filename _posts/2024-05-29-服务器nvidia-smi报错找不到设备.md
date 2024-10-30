---
layout:     post   				    # 使用的布局（不需要改）
title:      服务器nvidia-smi报错找不到设备 # 标题 
subtitle:   Ubuntu #副标题
date:       2024-05-29				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	Ubuntu							#标签
---

[TOC]

# 一、问题

早上查看服务器nvidia-smi发现报错

~~~bash
Unable to determine the device handle for GPU0000:C2:00.0: Unknown Error
~~~

大概意思就是找不到这张卡

# 二、问题分析与解决方法

## 2.1.检查挂载的GPU

先查看系统是否可以读取当前挂载的设备

~~~bash
lspci| grep -i nvidia
~~~

输出如下

~~~bash
01:00.0 VGA compatible controller: NVIDIA Corporation Device 2684 (rev a1)
01:00.1 Audio device: NVIDIA Corporation Device 22ba (rev a1)
81:00.0 VGA compatible controller: NVIDIA Corporation Device 2684 (rev a1)
81:00.1 Audio device: NVIDIA Corporation Device 22ba (rev a1)
c1:00.0 VGA compatible controller: NVIDIA Corporation Device 2684 (rev a1)
c1:00.1 Audio device: NVIDIA Corporation Device 22ba (rev a1)
c2:00.0 VGA compatible controller: NVIDIA Corporation Device 2684 (rev ff)
c2:00.1 Audio device: NVIDIA Corporation Device 22ba (rev ff)
~~~

可以看到这张0000:C2:00.0没有挂载上，掉线了

## 2.2.生成log文件

我们再生成一下nvidia-bug-report的log文件

~~~bash
sudo nvidia-bug-report.sh
~~~

会在当前目录生成nvidia-bug-report.log.gz，解压后使用如下命令查看：

~~~bash
grep "fallen off" nvidia-bug-report.log
~~~

输出如下：

~~~bash
6月 28 02:52:53 jjf-Super-Server kernel: NVRM: Xid (PCI:0000:c2:00): 79, pid='<unknown>', name=<unknown>, GPU has fallen off the bus.
6月 28 02:52:53 jjf-Super-Server kernel: NVRM: GPU 0000:c2:00.0: GPU has fallen off the bus.
[1590188.789226] NVRM: Xid (PCI:0000:c2:00): 79, pid='<unknown>', name=<unknown>, GPU has fallen off the bus.
[1590188.789232] NVRM: GPU 0000:c2:00.0: GPU has fallen off the bus.
~~~

可以看到报错代号为79，查阅资料有人说是因为电力不足或者温度过高，因为还有一张卡在跑，所以想查看一下温度，但nvidia-smi又不能使用

大佬指出的原因如下：

~~~bash
One of the gpus is shutting down. Since it’s not always the same one, I guess they’re not damaged but either overheating or lack of power occurs. Please monitor temperatures, check PSU.
~~~

## 2.3.禁用报错的卡查看温度

所以先禁用这张报错的卡

~~~bash
sudo nvidia-smi drain -p 0000:C2:00.0 -m 1
~~~

这行命令解释如下：

* drain用于将指定的 GPU 标记为排空状态，防止新作业分配到该 GPU 上
* -p 0000:C2:00.0 指定要排空的 GPU 的 PCI 地址。在你的系统中，这个地址标识特定的 GPU
* `-m 1`: 设置 GPU 的排空模式。`-m` 选项有几个可能的值：
  - `0`: 关闭排空模式（即恢复 GPU 正常操作）
  - `1`: 开启排空模式，停止新作业分配到该 GPU，但允许当前运行的作业完成。
  - `2`: 立即停止所有当前运行的作业并关闭排空模式。

禁用成功

~~~bash
Successfully set GPU 00000000:C2:00.0 drain state to: draining.
~~~

然后再查看nvidia-smi，可以看到只有三张卡了（原本有四张）

~~~bash
Fri Jun 28 10:05:54 2024       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.90.07              Driver Version: 550.90.07      CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 4090        Off |   00000000:01:00.0 Off |                  Off |
|  0%   37C    P8             13W /  450W |      12MiB /  24564MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
|   1  NVIDIA GeForce RTX 4090        Off |   00000000:81:00.0 Off |                  Off |
|  0%   40C    P8             17W /  450W |     105MiB /  24564MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
|   2  NVIDIA GeForce RTX 4090        Off |   00000000:C1:00.0 Off |                  Off |
| 30%   64C    P2            386W /  450W |   22990MiB /  24564MiB |     98%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A      3267      G   /usr/lib/xorg/Xorg                              4MiB |
|    1   N/A  N/A      3267      G   /usr/lib/xorg/Xorg                             81MiB |
|    1   N/A  N/A      3787      G   /usr/bin/gnome-shell                           12MiB |
|    2   N/A  N/A      3267      G   /usr/lib/xorg/Xorg                              4MiB |
|    2   N/A  N/A    569947      C   python                                      22972MiB |
+-----------------------------------------------------------------------------------------+
~~~

可以看到温度很正常，所以我猜测是电源的原因导致这个报错

## 2.4.解决方法

执行如下命令调整显卡的时钟速度（实际就是锁住其最大功率）

~~~bash
sudo nvidia-smi -lgc 300,1500
~~~

PS:但我没有使用这条命令来解决，因为我觉得这是硬件的问题，所以只是重新来解决它。

# 参考资料

[服务器GPU温度过高挂掉排查记录Unable to determine the device handle for GPU 0000:01:00.0: Unknown Error](https://blog.csdn.net/qq_44850917/article/details/135431204?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ECtr-3-135431204-blog-103884592.235%5Ev43%5Epc_blog_bottom_relevance_base5&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ECtr-3-135431204-blog-103884592.235%5Ev43%5Epc_blog_bottom_relevance_base5&utm_relevant_index=4)

[解决[Unable to determine the device handle for GPU...: Unknown Error]问题](https://gitcode.csdn.net/65e93c981a836825ed78e7d2.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6NDMyNjYzLCJleHAiOjE3MTk3MTI0ODcsImlhdCI6MTcxOTEwNzY4NywidXNlcm5hbWUiOiJTTDEwMjlfIn0.KxSJNdD5XplbDhT2SjDYC6ZDrqJPm-22aSYlLbopMAY)

[Unable to determine the device handle for GPU 0000:02:00.0: Unknown Error](https://forums.developer.nvidia.com/t/unable-to-determine-the-device-handle-for-gpu-000000-0-unknown-error/197974?login=from_csdn)

