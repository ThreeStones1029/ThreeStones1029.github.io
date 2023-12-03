---
layout:     post   				    # 使用的布局（不需要改）
title:      Ubuntu修复				# 标题 
subtitle:    解决libxkbcommon库编译完图形界面不能使用键盘  #副标题
date:       2023-12-03				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags: Ubuntu							#标签
---

[TOC]

# 一、前言

上个礼拜在qt界面不能输入中文，所以按照一些博客编译libfcitxplatforminputcontextplugin.so库，编译完后发现我的qt直接不能使用键盘了，使用键盘就会直接闪退然后，我尝试去重启后我发现我的图形界面直接崩溃，图形界面打不开了，只有左上角一个光标在闪。

# 二、（临时解决方案）更换图形界面

在不能打开桌面后，我尝试了很多办法，在stackoverflow，github、ubuntu官网都没有找到解决方案。不得已我当务之急是想恢复我的桌面，所以我安装了unity桌面，同时更换了lightdm图形管理器，才恢复我的图形界面。

## 2.1.安装lightdm图形管理器

~~~bash
sudo apt-get install lightdm
~~~

## 2.2.切换图形管理器

~~~bash
sudo dpkg-reconfigure lightdm
~~~

运行完后会需要选择图形管理器，选择lightdm，选择完后可以使用

~~~bash
cat /etc/x11/default-display-manager
~~~

查看当前图形管理器，是否切换成功

## 2.3.安装unity桌面

~~~bash
sudo apt-get install unity
~~~

然后重启就会能进图形管理器了。

## 2.4.图形界面美化

如果觉得图形界面不好看，可以安装unity-tweak-tools

~~~bash
sudo apt-get install unity-tweak-tools
~~~

打开命令

~~~bash
unity-tweak-tools
~~~

如果报错schema com.canonical.Unity.ApplicationsLens not installed

需要安装unity-lens-applications、unity-lens-files

~~~bash
sudo apt-get install unity-lens-applications
sudo apt-get install unity-lens-files
~~~

安装完可以选用主题等

# 三、问题依旧存在

虽然有了图形界面，但我依旧发现qt不能使用键盘，同时发现我的cmake、slicer都不能使用键盘了，只要键盘有输入就会闪退。

问题似乎更严重了，我重新回忆了当时的安装过程。由于已经过去了一周，所以可能有遗漏，当时我的编译安装过程如下：

## 3.1.下载fcitx-qt5

然后解压后在根目录新建build

~~~bash
cd /build
cmake ..
make
make install
~~~

## 3.2.安装extra-cmake-modules

~~~bash
CMake Error at CMakeLists.txt:49 (find_package):
  By not providing "FindFcitx5Utils.cmake" in CMAKE_MODULE_PATH this project
  has asked CMake to find a package configuration file provided by
  "Fcitx5Utils", but CMake did not find one.

  Could not find a package configuration file provided by "Fcitx5Utils"
  (requested version 5.0.16) with any of the following names:

    Fcitx5UtilsConfig.cmake
    fcitx5utils-config.cmake

  Add the installation prefix of "Fcitx5Utils" to CMAKE_PREFIX_PATH or set
  "Fcitx5Utils_DIR" to a directory containing one of the above files.  If
  "Fcitx5Utils" provides a separate development package or SDK, be sure it
  has been installed.


-- Configuring incomplete, errors occurred!
~~~

查找后发现需要安装extra-cmake-modules

所以下载extra-cmake-modules_1.4.0.orig.tar.xz，和上面一样安装

~~~bash
cmake .
make
make install
~~~

发现报错

~~~bash
[ 50%] sphinx-build html: see /home/jjf/Downloads/extra-cmake-modules_1.4.0.orig/extra-cmake-modules-1.4.0/docs/build-html.log
Traceback (most recent call last):
  File "/home/jjf/anaconda3/bin/sphinx-build", line 7, in <module>
    from sphinx.cmd.build import main
ModuleNotFoundError: No module named 'sphinx'
make[2]: *** [docs/CMakeFiles/documentation.dir/build.make:62: docs/doc_format_html] Error 1
make[1]: *** [CMakeFiles/Makefile2:175: docs/CMakeFiles/documentation.dir/all] Error 2
make: *** [Makefile:163: all] Error 2

Extension error:
Could not import extension ecm (exception: cannot import name 'htmlescape' from 'sphinx.util.pycompat' (/home/jjf/anaconda3/lib/python3.7/site-packages/sphinx/util/pycompat.py))
make[2]: *** [docs/CMakeFiles/documentation.dir/build.make:62: docs/doc_format_html] Error 2
make[1]: *** [CMakeFiles/Makefile2:175: docs/CMakeFiles/documentation.dir/all] Error 2
make: *** [Makefile:163: all] Error 2
~~~

然后按照这个链接解决完这个报错

https://github.com/KDE/extra-cmake-modules/commit/001f901ee297bb5346729a02e8920b7528e20717

## 3.3.安装libxkbcommon

继续重新编译fcitx-qt5发现报错

~~~bash
 “Could NOT find XKBCommon_XKBCommon (missing: XKBCommon_XKBCommon_LIBRARY XKBCommon_XKBCommon_INCLUDE_DIR) (found version "")” 
~~~

这时需要安装libxkbcommon库,我安装的是libxkbcommon0.5.0

~~~bash
./configure --disable-x11
make
sudo make install
~~~

重点注意：在我安装完libxkbcommon0.5.0，但这是我**图形界面崩溃埋的伏笔**

重新编译发现，我的fcitx-qt5编译成功

~~~bash
-- The following OPTIONAL packages have been found:

 * PkgConfig

-- The following REQUIRED packages have been found:

 * ECM (required version >= 1.4.0)
 * XKBCommon (required version >= 0.5.0), Keyboard handling library using XKB data, <http://xkbcommon.org>
 * Qt5DBus
 * Qt5Widgets
 * Qt5Concurrent
 * Qt5 (required version >= 5.1.0)
 * Qt5Gui (required version >= 5.1.0)
 * Qt5Core

-- Configuring done
-- Generating done
-- Build files have been written to: /home/jjf/Downloads/fcitx-qt5/build
(base) jjf@jjf-Precision-Tower-7810:~/Downloads/fcitx-qt5/build$ make
Scanning dependencies of target fcitxplatforminputcontextplugin_autogen
[  4%] Automatic MOC for target fcitxplatforminputcontextplugin
[  4%] Built target fcitxplatforminputcontextplugin_autogen
[  9%] Generating inputmethod1proxy.cpp, inputmethod1proxy.h
[ 14%] Generating inputcontextproxy.cpp, inputcontextproxy.h
[ 19%] Generating inputcontextproxy.moc
[ 23%] Generating inputcontext1proxy.cpp, inputcontext1proxy.h
[ 28%] Generating inputcontext1proxy.moc
[ 33%] Generating inputmethodproxy.cpp, inputmethodproxy.h
[ 38%] Generating inputmethodproxy.moc
[ 42%] Generating inputmethod1proxy.moc
Scanning dependencies of target fcitxplatforminputcontextplugin
[ 47%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/fcitxplatforminputcontextplugin_autogen/mocs_compilation.cpp.o
[ 52%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/fcitxinputcontextproxy.cpp.o
[ 57%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/fcitxqtdbustypes.cpp.o
[ 61%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/fcitxwatcher.cpp.o
[ 66%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/qfcitxplatforminputcontext.cpp.o
[ 71%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/main.cpp.o
[ 76%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/qtkey.cpp.o
[ 80%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/inputcontextproxy.cpp.o
[ 85%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/inputcontext1proxy.cpp.o
[ 90%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/inputmethodproxy.cpp.o
[ 95%] Building CXX object qt5/platforminputcontext/CMakeFiles/fcitxplatforminputcontextplugin.dir/inputmethod1proxy.cpp.o
[100%] Linking CXX shared module libfcitxplatforminputcontextplugin.so
[100%] Built target fcitxplatforminputcontextplugin
(base) jjf@jjf-Precision-Tower-7810:~/Downloads/fcitx-qt5/build$ sudo make install
[sudo] password for jjf: 
[  4%] Automatic MOC for target fcitxplatforminputcontextplugin
[  4%] Built target fcitxplatforminputcontextplugin_autogen
[100%] Built target fcitxplatforminputcontextplugin
Install the project...
-- Install configuration: ""
-- Installing: /opt/Qt5.14.2/5.14.2/gcc_64/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so
-- Set runtime path of "/opt/Qt5.14.2/5.14.2/gcc_64/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so" to ""
 
~~~

然而我加入了对应的libfcitxplatforminputcontextplugin.so库后，发现我的qt直接闪退。然后我去掉libfcitxplatforminputcontextplugin.so库，也没有效果。然后我重启了！！！，就是因为这一重启，我图形界面直接进不去了，图形界面直接崩溃。

我查看了系统日志

~~~bash
tail -f /var/log/syslog
~~~

发现了这一条：

~~~bash
segfault at 4 ip 00007fe31c7c5fe8 sp 00007ffdd5ab6480 error 4 in libxkbcommon.so.0.0.0[7fe31c7ad000+1b000]
~~~

这期间让我注意到libxkbcommon.so.0.0.0正是我前面编译过的库，所以我确定就是因为我编译了libxkbcommon导致图形界面崩溃。

## 3.4.总结

以上大体就是我的libfcitxplatforminputcontextplugin.so库编译安装过程，可能由于时间原因，我很难复现，但大体是这样的。期间第一个错误就是编译了libxkbcommon库，其次就是重启了。

# 四、最终解决方法

前面提到我临时安装了unity的桌面以及ibus的输入法（因为在拯救过程中卸载了fcitx）暂时解决桌面图形界面的问题。但还是qt、cmake、slicer都不能使用键盘。qt、cmake重新安装都没有解决闪退问题。

我昨天上午尝试回忆了当天libfcitxplatforminputcontextplugin.so库的编译过程，思来想起觉得最大的错误是编译的libxkbcommon库版本不对，导致与我的系统冲突（ubuntu20.04）。

所以我重新下载安装了libxkbcommon0.8.4，编译完成libxkbcommon发现我的qt以及cmake，slicer都可以使用键盘了。我意识到问题可能解决了，然后我尝试把桌面换回来。

果不其然，桌面可以使用默认桌面了，gdm3图形管理器也可以使用了。

我有在libxkbcommon的官方github上提过这个问题，但是似乎没有人遇到过。

https://github.com/xkbcommon/libxkbcommon/issues/412

# 五、待解决的问题

## 5.1.新出现的问题

但是，一波三折，虽然默认桌面可以使用，qt、cmake等软件可以使用键盘了。但我很快发现了我的键盘的super键一直处于按下的状态，不能正常的输入。数字键直接变成快捷键了，我很快意识到仿佛输入法按键被交换了，super键需要一直按着才能正常输入。

我在stackoverflow，github，ubuntu官网都没有找到解决方法，期间看到类似的问题，ubuntu官网上有人说可以在gnome-tweaks的dash to panel里面修改，安装这个工具后但是我发现我的设置是正常的，没有设置错误热键。[[gnome - Number keys switching applications (Ubuntu 21.10) - Ask Ubuntu](https://link.zhihu.com/?target=https%3A//askubuntu.com/questions/1372500/number-keys-switching-applications-ubuntu-21-10)]，这种方法对我无效。

## 5.2.我的解决方案

我尝试将默认输入法替换为了ibus输入法（因为默认桌面似乎默认使用fcitx输入法），同时替换了默认桌面为unity的桌面。然后惊奇的发现可以使用键盘了，super键不再被默认按着。

但是在默认桌面下不管怎么样，我的键盘super仿佛被一支无形的手按着。

感觉还是因为输入法的原因导致键盘热键冲突，但目前使用unity桌面+ibus输入法能够满足我的需求，所以我没有深究下去，如果后续有了更好的解决方案，我会及时更新的。

# 六、参考资料

有些资料和博客因为没有及时记录下来，以下是我目前记得的一些

[1.解决Ubuntu下安装Qt5.8无法输入中文的问题](https://blog.csdn.net/Wangguang_/article/details/90695530)

[2.ModuleNotFoundError: No module named 'sphinx'](https://github.com/KDE/extra-cmake-modules/commit/001f901ee297bb5346729a02e8920b7528e20717)

[3.extra-cmake-modules_1.4.0.orig.tar.xz](https://launchpad.net/ubuntu/+archive/primary/+files/extra-cmake-modules_1.4.0.orig.tar.xz)

[4.xkbcommon官网](https://xkbcommon.org/)

[5.我在xkbcommon的github提的issue](https://github.com/xkbcommon/libxkbcommon/issues/412)

[6.ubuntu官网提到的解决super按键方法](https://askubuntu.com/questions/1372500/number-keys-switching-applications-ubuntu-21-10)

[7.fcitx-qt5官网](https://github.com/fcitx/fcitx-qt5)

[8.ubuntu 20.04安装 unity-tweak-tools 启动时遇到错误](https://blog.csdn.net/xiaokan_001/article/details/107089499)

[9.查看与切换Ubuntu显示管理器](https://blog.csdn.net/weixin_43600140/article/details/118056384)

[10.Ubuntu进不去图形化界面的解决方案](https://blog.csdn.net/baidu_39594043/article/details/128364306?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-4-128364306-blog-115374530.235^v39^pc_relevant_anti_t3&spm=1001.2101.3001.4242.3&utm_relevant_index=7)

[11.Ubuntu开机没有图形界面 进入tty的拯救方法](https://blog.csdn.net/meng_152634/article/details/128050873?spm=1001.2101.3001.6650.17&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17-128050873-blog-120971356.235%5Ev39%5Epc_relevant_anti_t3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17-128050873-blog-120971356.235%5Ev39%5Epc_relevant_anti_t3&utm_relevant_index=20)