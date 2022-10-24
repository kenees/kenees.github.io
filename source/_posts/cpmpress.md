---
title: linux中的常用压缩命令
date: 2022-10-24 14:40:08

categories:
  - 工具
---
## 1. tar格式压缩和解压

### 1.1 基本语法

    # 默认不做压缩， 只是打包归档
    tar [选项] xxx.tar.gz 将要打包进去的内容 (打包目录， 压缩后的文件格式.tar.gz)

### 1.2 选项说明

    -c: 产生.tar打包文件
    -v: 显示详细信息
    -f: 指定压缩后的文件名
    -z: 打包同时压缩
    -x: 解包.tar 文件
    -C: 解压到指定目录
    --remove-files 打包后移除源文件

### 1.3 常用命令搭配

    tar -zcvf xxx.tar.gz ./build --remove-files
    tar -zxvf xxx.tar.gz  -C /tmp 

## 2. gzip/gunzip 压缩

### 2.1 基本语法

    gzip <文件>
    gunzip <*.gz>

### 2.2 注意事项

    1. 只能压缩文件不能压缩目录
    2. 不保留远来的文件
    3. 同时多个文件会产生多个压缩包

### 2.3 常用命令搭配

    gzip count.txt
    gunzip count.txt.gz

## 3. zip/unzip 压缩

### 3.1 基本语法

    zip [选项] xxx.zip 将要压缩内容 压缩文件和目录
    unzip [选项] xxx.zip 解压缩文件

### 3.2 选项说明

    zip: -r 压缩目录
    unzip: -d 指定解压后文件的存放目录

### 3.3 经验技巧

    zip压缩命令在windows/Linux都通用， 可以压缩目录且保留源文件

### 3.4 常用命令搭配

    zip -r ./build/* build.zip
    unzip -d /data build.zip
