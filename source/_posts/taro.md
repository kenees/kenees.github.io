---
title: Taro多版本管理
date: 2021-06-11 16:39:08
tags:
  - Taro
  - 小程序
  - 跨端
categories:
  - 前端

---
Taro多版本管理
## 1. 环境安装
Taro多版本管理利用的是Python的一个包(nodeenv)来实现的，但它实现的是创建node的虚拟环境，与python的虚拟环境类似。
首先需要保证本地环境安装了python，mac及linux系统自带2.x版本的python，windows环境需要自己安装。
### 1.1 安装nodeenv
```bash
$ sudo pip install nodeenv
```
### 1.2 在当前路径下创建虚拟node环境
```bash
$ nodeenv --node=system taro1.3.05
```
--node=system表示利用当前系统中的node创建虚拟环境，即不独立安装node，且在虚拟环境下能访问到系统node安装的全局工具包

taro1.3.05是要创建的虚拟环境的文件夹名称

## 2. 使用
### 2.1 启动虚拟环境
```bash
$ . taro1.3.05/bin/activate
```
启动后提示符
```
(taro1.3.05) $
```

### 2.2 在虚拟环境下安装指定版本的taro
```
(taro1.3.05)  $ npm install -g @tarojs/cli@1.3.05
```
### 2.3 安装完成后查看版本
```
(taro1.3.05) $ taro -V
👽 Taro v1.3.05
```
### 2.4 查看taro安装位置
```
(taro1.3.05) $ which taro
/Users/yogo/taro1.3.25/bin/taro
```
### 2.5 正常启动
```
(taro1.3.05) $ yarn dev:weapp
```
### 2.6 退出虚拟环境
```
(taro1.3.25) $ deactivate_node
$ 
```