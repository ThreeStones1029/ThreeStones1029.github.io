---
layout:     post   				    # 使用的布局（不需要改）
title:      服务器安装qbittorrent-nox下载数据集 				# 标题 
subtitle:   数据集下载 #副标题
date:       2024-04-29 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	Ubuntu							#标签
---



[TOC]

# 一.前言

最近需要下载一个数据集,但是本地没有那么大的空间,也想直接下载到服务器上,这样就不需要再传一遍,以下为安装过程.

# 二.下载与安装

qbittorrent-nox是一个无界面的qbittorrent,适合于服务器上安装.

## 2.1.安装qb

~~~bash
apt install qbittorrent-nox
~~~

## 2.2.创建qBittorrent开机自启

~~~bash
nano /etc/systemd/system/qbittorrent.service
~~~

加入以下内容并保存,ctrl+x退出.

~~~bash
[Unit]
Description=qBittorrent Daemon Service
After=network.target
[Service]
User=root
ExecStart=/usr/bin/qbittorrent-nox
ExecStop=/usr/bin/killall -w qbittorrent-nox
[Install]
WantedBy=multi-user.target
~~~

## 2.3.修改qBittorrent.conf文件

qBittorrent.conf一般位于.config/qBittorrent下, vim 打开qBittorrent.conf文件,加入以下内容

~~~bash
[Preferences] 
WebUI\HostHeaderValidation=false
WebUI\Password_PBKDF2="@ByteArray(ARQ77eY1NUZaQsuDHbIMCA==:0WMRkYTUWVT9wVvdDtHAjU9b3b7uB8NR1Gur2hmQCvCDpm39Q+PsJRJPaCU51dEiz+dTzh8qbPsL8WkFljQYFQ==)",
~~~

第一条来避免未授权错误

第二条保证默认帐号与密码有效

## 2.4.qb常用命令

~~~bash
# 启动qb
service qbittorrent start

# 关闭qb
service qbittorrent stop

# 查看qb状态
service qbittorrent status

# 开机自启
systemctl enable qbittorrent

# 关闭开机自启
systemctl disable qbitorrent
~~~

## 2.5.默认帐号与密码

~~~bash
# 帐号admin
# 密码:adminadmin
~~~

## 2.6.登录成功后界面

![](https://cdn.jsdelivr.net/gh/ThreeStones1029/blogimages/img/202404291519846.png)