---
layout:     post   				    # 使用的布局（不需要改）
title:      vscode 				# 标题 
subtitle:   写cpp时编译器报错 #副标题
date:       2024-01-22 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	工具							#标签
---

[TOC]

# 一、问题描述与分析

在使用vscode和cmake写cpp代码时，虽然可以编译通过，但是有红色下划线提示。主要原因在于vscode定位不到头文件

# 二、解决方法

直接按crtl + shift + P，选择C/C++:Edit Configurations(JSON)，之后会在项目所在的根目录下生成.vscode文件夹以及c_cpp_properties.json文件。

然后打开终端输入

~~~bash
gcc -v -E -x c++ -
~~~

得到类似结果如下

~~~bash
gcc version 11.4.0 (Ubuntu 11.4.0-1ubuntu1~22.04) 
COLLECT_GCC_OPTIONS='-v' '-E' '-mtune=generic' '-march=x86-64'
 /usr/lib/gcc/x86_64-linux-gnu/11/cc1plus -E -quiet -v -imultiarch x86_64-linux-gnu -D_GNU_SOURCE - -mtune=generic -march=x86-64 -fasynchronous-unwind-tables -fstack-protector-strong -Wformat -Wformat-security -fstack-clash-protection -fcf-protection -dumpbase -
ignoring duplicate directory "/usr/include/x86_64-linux-gnu/c++/11"
ignoring nonexistent directory "/usr/local/include/x86_64-linux-gnu"
ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/11/include-fixed"
ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/include"
#include "..." search starts here:
#include <...> search starts here:
 /usr/include/c++/11
 /usr/include/x86_64-linux-gnu/c++/11
 /usr/include/c++/11/backward
 /usr/lib/gcc/x86_64-linux-gnu/11/include
 /usr/local/include
 /usr/include/x86_64-linux-gnu
 /usr/include
End of search list.
~~~

把#include <...> search starts here:对应的头文件路径部分加入到c_cpp_properties.json文件中的“includePath”部分变成这样

~~~bash
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/include/c++/11/**",
                "/usr/include/x86_64-linux-gnu/c++/11/**",
                "/usr/include/c++/11/backward/**",
                "/usr/lib/gcc/x86_64-linux-gnu/11/include/**",
                "/usr/local/include/**",
                "/usr/include/x86_64-linux-gnu/**",
                "/usr/include/**"
            ],
            "defines": [],
            "cStandard": "c17",
            "cppStandard": "gnu++17"
        }
    ],
    "version": 4
}
~~~

问题解决！

