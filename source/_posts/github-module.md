---
title: GitHub上传私有npm包及使用
date: 2022-11-22 15:02:08

tags:
  - yarn
  - npm
  - node_modules
  - github
  - package
categories:
  - 工具
---
## 1. 介绍

GitHub Packages是GitHub提供的类似npm包管理工具或平台，通过GitHub Packages，我们可以直接使用npm相关的命令、功能，并且不用将自己的npm包发布到npm仓库。也就是说，我们的npm包将托管在github上，并且可以和我们相应的仓库保持同步。

GitHub Packages看起来是这个样子

![''](/assets/githubs/20221122150848.png)

![''](/assets/githubs/20221122150904.png)

### 1.1 创建私有仓库

    私有仓库如下, 目前没有东西， 后面可以将包的源码上传

![''](/assets/githubs/20221122151831.png)

### 1.2 克隆私有仓库到本地

    # git clone git@github.com:kenevy/cowa-npm-repo.git

### 1.3 创建Personal access tokens

    1. 进入github设置页面, 点击最后一个developer settings
    2. 点击Personal access tokens
    3. 点击Generate new token -> Generate new token(classic)
    4. 填写note, explration
    5. 勾选权限如下(最低限制)
    ![''](/assets/githubs/20221122153153.png)
    6. 点击Generat token
    7. 注意保存token， 只显示一次
    ![''](/assets/githubs/20221122153525.png)

### 1.4 创建npm包

    1. 进入刚刚clone的项目目录下
    2. 进行初始化, 命令npm init --scope=kenevy中的kenevy必须是你的github用户名， 其他随便填写一些description和author，然后一路回车就可以。
    ![''](/assets/githubs/20221122153855.png)

### 1.5 配置

    有两种配置方式， 第一种是在package.json所在目录创建一个.npmrc文件， 另一种是直接修改package.json文件并增加publishConfig项

#### 1.5.1 创建.npmrc(推荐)

    在package.json所在目录创建一个.npmrc文件， 然后添加如下内容, 注意更改用户名
    ```
        registry=https://npm.pkg.github.com/kenevy
    ```

#### 1.5.2 package.json中添加publishConfig

    ```javascript
        "publishConfig": {
            "registry":"https://npm.pkg.github.com/"
        },
    ```
    添加后package.json看起来如下

    ```javascript
        {
            "name": "@kenevy/cowa-npm-repo-tests",
            "version": "1.0.0",
            "description": "test",
            "main": "index.js",
            "scripts": {
                "test": "echo \"Error: no test specified\" && exit 1"
            },
            "repository": {
                "type": "git",
                "url": "git+https://github.com/kenevy/cowa-npm-repo.git"
            },
            "publishConfig": {
                "registry":"https://npm.pkg.github.com/"
            },
            "author": "wangcheng",
            "license": "ISC",
            "bugs": {
                "url": "https://github.com/kenevy/cowa-npm-repo/issues"
            },
            "homepage": "https://github.com/kenevy/cowa-npm-repo#readme"
            }
    ```

### 1.6 登陆

#### 1.6.1 方式1（推荐）

刚才已经创建了.npmrc文件, 现在为了授权，我们可以继续添加以下内容

    ```javascript
        //npm.pkg.github.com/:_authToken=ghp_T738OJCL2cBei0B2kGANumi94EBXHMDuNvJv
    ```
最终.npmrc内容应该包含两行， 如下

    ```javascript
        registry=<https://npm.pkg.github.com/kenevy>
        //npm.pkg.github.com/:_authToken=ghp_T738OJCL2cBei0B2kGANumi94EBXHMDuNvJv

    ```

#### 1.6.2 方式2

    ```shell
        $ npm login --registry=https://npm.pkg.github.com
        > Username: USERNAME
        > Password: TOKEN
        > Email: PUBLIC-EMAIL-ADDRESS
    ```
USERNAME是你的github用户名，Password就是刚才的token，邮箱虽然可以随便填写，为了规范，最好填写你的github邮箱。

登录成功类似以下输出

    ➜ cowa-npm-repo git:(master) ✗ npm login --registry=https://npm.pkg.github.com
    Username: kenevy
    Password:
    Email: (this IS public) cc9165@outlook.com
    Logged in as kenevy on https://npm.pkg.github.com/.

### 1.7 发布

   执行`npm publish`命令发布

   ```shell
    $ npm publish
    npm notice 
    npm notice 📦  @kenevy/cowa-npm-repo-tests@1.0.1
    npm notice === Tarball Contents ===
    npm notice 6B   README.md
    npm notice 27B  index.js
    npm notice 480B package.json
    npm notice 57B  src/index.js
    npm notice === Tarball Details ===
    npm notice name:          @kenevy/cowa-npm-repo-tests
    npm notice version:       1.0.1
    npm notice filename:      @kenevy/cowa-npm-repo-tests-1.0.1.tgz   
    npm notice package size:  451 B
    npm notice unpacked size: 570 B
    npm notice shasum:        d98fa2e7166a5841ca7bc23a69cd265c2a55cc5a
    npm notice integrity:     sha512-RX8lNfplRrRju[...]bwaIfY6WeR3AA==
    npm notice total files:   4
    npm notice
    npm notice Publishing to https://npm.pkg.github.com/kenevy
    + @kenevy/cowa-npm-repo-tests@1.0.1
   ```

 提示：当使用`npm publish`发布结束后，一般也会将代码修改同步提供到 `github` 仓库，这样的话，`npm`包和`github`仓库的内容就一直会保持一致。当然，如果你说我只需要`npm`包，又不需要管`github`仓库是什么，那也可以，但是，如果你不提交到`github`仓库，使其和`npm`包保持一致的话，那么当你换一台电脑要修改并且发布这个npm包时，就有可能出现版本和内容在两台电脑上都不一样的情况。比如，你在当前电脑发布了一个`v1.1.0`版本的npm包，在此之前，版本号是`v1.0.3`，发布`v1.1.0`之后，你没有提交到github远程仓库，当你在另一台电脑上把仓库克隆下来，`package.json`版本依然是`v1.0.3`（因为你的`v1.1.0`并没有提交），你修改了点内容，然后`npm version patch`将版本号修改为`v1.0.4`，然后`npm publish`发布后，你远程的`npm`包将以最新的一次发布即`v1.0.4`的内容为准，那么之前修改的`v1.1.0`的内容不起任何作用，这样的话，我们就难以保证版本号和内容的统一。因此，强烈建议发布`npm`包后，立即将改动的代码提交到远程的`github`仓库。

### 1.8 使用`npm`包

创建`.npmrc`
这里在项目里面创建`.npmrc`和上面`package`里面的`.npmrc`完全一样, 你也可以直接复制到项目中。

登陆(同创建npm包一样， 有2种方式)

### 1.9 安装`npm`包

执行
```
npm install @kenevy/cowa-npm-repo-tests@1.0.1
```

### 1.10 发布多个`npm`包


可以将多个npm包发到同一个私有仓库中, 很方便便于管理。目录结构如下

![](/assets/githubs/20221122162818.png)

本质上是将包的源码放到同一个项目中， 发布的时候需要进入每个包的目录下，即包含package.json的目录下， 然后执行npm publish.