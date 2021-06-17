---
title: Windows下gcc/g++的安装与配置
date: 2021-06-17 14:40:08
tags:
  - C++
  - gcc
categories:
  - 工具

---
Windows下gcc/g++的安装与配置

# 1.下载mingw
打开链接： https://osdn.net/projects/mingw/downloads/68260/mingw-get-setup.exe/

点击mingw-get-setup.ext即开始下载

# 2.安装mingw
下载的安装程序是一个包管理器，只有几十kb， 包管理器安装后才能继续安装编译器等组件。
打开程序，按照提示，以默认选项安装即可。

在安装完成后的弹出页面中找到 ```mingw32-gcc-g++``` (注意class属性要为bin), 右键点击 ``` Mark for Installation ```
然后点击左上角的 Installation 菜单中的Apply changes 选项， 然后管理器将开始在线安装或更新被选中的组件。

如果由于某种原因未能成功， 退出程序前会给予提示，选择review changes 重新安装即可。

# 3.配置环境变量
打开控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 环境变量。

在系统环境变量列表中找到 PATH 选项，打开

点击新建， 找到MinGW安装路径下的bin， 添加项：
```
D:\program\MinGW\bin
```
点击确定即可

# 4.检验是否安装成功
打开cmd，输入 
```gcc -v ```测试gcc版本
```g++ -v ```测试g++版本