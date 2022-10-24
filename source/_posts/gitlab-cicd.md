---
title: GitLab CI/CD工作流程
date: 2022-10-19 15:17:19
tags:
  - GitLab
  - Web
  - CI/CD
  - 自动部署
categories:
  - 工具
---
日常工作中总想着偷懒， 能自动化的东西绝不手动操作。所以研究了下GitLab上的CI/CD工作流来解放双手。

# 1. 工作流简介
![](/assets/cicd/1.png)
大致思路就是先在gitlab中创建工作流（具体操作后面讲）, 而工作流会因为一些操作而触发， 比如push, merge；触发工作流后具体的操作类似打包，拷贝， 删除， 发布就需要有一个地方来执行(runner)， 这样就实现自动化操作了。
# 2. 实现流程
## 2.1 gitlab中创建工作流
在项目根目录下创建.gitlab-ci.yml文件，gitlab会自动检测项目中是否有这个文件, 检测到后就可以在CI/CD中出现流水线列表.
    .gitlab-ci.yml格式如下

    ```
    stages:
        - install
        - build
        - deploy

    cache:
        key: modules-cache
        paths:
            - node_modules
            - build
        
    job_install:
        stage: install
        tags:
            - test
        only:
            - develop
        script:
            - yarn install

    job_build:
        stage: build
        tags:
            - test
        only:
            - develop
        script:
            - yarn build

    job_deploy:
        stage: deploy
        image: node
        tags:
            - test
        only:
            - develop
        script:
            - ls
    ```
Pipeline是Gitlab根据项目的.gitlab-ci.yml文件执行的流程，它由许多个任务节点组成, 而这些Pipeline上的每一个任务节点，都是一个独立的Job。每个Job都会配置一个stage属性，来表示这个Job所处的阶段。一个Pipleline有若干个stage,每个stage上有至少一个Job，如下图所示：

![](/assets/cicd/2.webp)

## 2.2 创建Runner
Runner可以理解为：在特定机器上根据项目的.gitlab-ci.yml文件，对项目执行pipeline的程序。Runner可以分为两种： Specific Runner 和 Shared Runner

- Shared Runner是Gitlab平台提供的免费使用的runner程序，它由Google云平台提供支持，每个开发团队有十几个。对于公共开源项目是免费使用的，如果是私人项目则有每月2000分钟的CI时间上限。
- Specific Runner是我们自定义的，在自己选择的机器上运行的runner程序，gitlab给我们提供了一个叫gitlab-runner的命令行软件，只要在对应机器上下载安装这个软件，并且运行gitlab-runner register命令，然后输入从gitlab-ci交互界面获取的token进行注册, 就可以在自己的机器上远程运行pipeline程序了。

Shared Runner 和 Specific Runner的区别

- Shared Runner是所有项目都可以使用的，而Specific Runner只能针对特定项目运行
- Shared Runner默认基于docker运行，没有提前装配的执行pipeline的环境，例如node等。而Specific Runner你可以自由选择平台，可以是各种类型的机器，如Linux/Windows等，并在上面装配必需的运行环境，当然也可以选择Docker/K8s等
- 私人项目使用Shared Runner受运行时间的限制，而Specific Runner的使用则是完全自由的。

### 2.2.1 使用docker在服务器上安装runner
   1. 先安装镜像： docker run -d --name gitlab-runner --restart always -v /src/gitlab-runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner:latest
   2. 再进入镜像文件中将需要监听的流程绑定上： docker exec -it gitlab-runner gitlab-runner register
   3. 此时会要求输入一些参数，按照步骤如下
       3.1 代码库所在域名 https://gitlab.com
       3.2 项目token， gitlab的在设置-CI/CD-一般流水线-Runner令牌
       3.3 runner描述， 不重要可自行编写
       3.4 为当前runner添加标签， 该标签会在gitlab-ci.yml中使用， 需要统一可以随便填写
       3.5 runner运行的环境， 这边填写docker
       3.6 指定运行基础环境， 因为是前端部署， 这边就填写node:16
    4. 注册成功后可在配置中查看， 绑定成功即可看到， 否则未绑定成功
    ![](/assets/cicd/4.png)


## 2.3 gitlab-ci.yml配置关键字
gitlab提供了很多配置关键字，其中最基础和常用的有这么几个
- stages
- stage
- script
- tags

#### stages & stage
stages定义在YML文件的最外层，它的值是一个数组，用于定义一个pipeline不同的流程节点
stage是一个字符串， 表示的是当前的pipeline节点。

#### script
它是当前pipeline节点运行的shell脚本（以项目根目录为上下文执行）。

#### tags 
tags是当前Job的标记，这个tags关键字是很重要，因为gitlab的runner会通过tags去判断能否执行当前这个Job
