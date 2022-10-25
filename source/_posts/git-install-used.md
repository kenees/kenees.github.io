---
title: Git安装及使用
date: 2021-06-03 15:40:08
tags:
  - Git
categories:
  - 工具

---
Git安装教程及日常开发常用命令

## 1. 安装
### 1.1 Windows
打开链接https://git-scm.com/download/win 可自动下载，直接下一步到完成。

打开cmd，  输入git --version可查看版本号，安装成功如下

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAN8AAAAxCAYAAABat3zeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAS6SURBVHhe7ZxBbh03EETnALqKIB0isQ8RJzlEFo7hxMnSXmeTvQRI0Fl0Jwc00B+lQneTw09+jpVaPJBd1d3sGYvQl2x4e3p6+np1dSWEuDC6fEIsQpdPiEXo8gmxiO3L5y+uIYSYy/b4+OgaQoi5bG9//dM1hBBz2d7+8odrCCHmsr35+aNrCCHmsr1598E1hBBz2X5897trCCHmsv3w03vXEELMZfv78z+uIYSYy3Z3d+caQoi5bPf3964hhJjL9vDw4BpCiLlsf3365BqtPD8/u/rRONqcZR5vJtMR9DB3L+fWf88c8dm329vbU2B/2AYmZni53IfjFaw8G8E5eKbajOc8w1GefwVHfPbT5dv7RYBEuef0nMHq8wveDKi1zDjjOY7wbv5vfLt8tS8I1FpzPT3KuxSrzy/U3l/ZI5iHZF4Po/uJOuHl88i+IDydNS9GIi/S2Occ9sxnzfSoztMxxtXLYc32nsa+l294uYinYz57Uc4o+AyOWzX2OQc9T0M8H2P2RrLd3NwMae71YA3jXs9i1kzP4lYN4xaPV95jzHqkGa1etEd6z2FKbg9Wy728fRSzZnoWt2oYlz3Hth/Fdn19PaSx14M1L47qGPYxRrx8xPP25GNse16RohlZDmtG5LGOcWsNknmjsbP4zBIz7GOMePmI52X9M28U22//Pg1rXBs4Oqc1z6j5hSxnz3lZru15ZR/3nMNa1iPTTI+8Qq83GjuLz6zN0DJjlOPp2fmZN4pvHzvLpuWwomVD1Hpg3Ot5caT35mHcmtual+174pqOZDkt9SPxzmOtFkd6S29Pw7il57mcLl+hHGBgEvueV/A8q2EPdfbY9zT2PB89xPOyupqHK3sMe5jPHvtRvpHVMehzDnsziM7x5kCNPc/PvMyPNI5H8eLyjWD0gLP4XuZshZ/ntT3fa2T45RPrKBfO8HxxLF788zIhxOXQ5RNiEbsu32v4OHPJZ+j9GFir29vP6K3LOKdn9oyXZNUMunyT4HNaz83qyt7AnBq9dS2c23PGTK3MfC8tDPvYufIlHhF+H63vp6Wu91331s3kCDOtmmHYbzuP+Ad7JEZemJG9VnOEmVbNEP4lOw7kaZ4X5TCcy3Grxj7noOdpiOdjzN4eeuqy8y4xB56PK+8tNrwc9FH3fE/HGFcvh/UW9uaP4nT5cIBomGzIcx84O9+LWTM9i1s1jMueY9u30lNjeLW9/fbWWT6vSKShHu17PF5578U19uaP4vQzX/YwNb3Q8wBWw7UlZtjHGPHyEc/L+mdeC3vzGa++t2fv7LwiLRrG53q8GiVmrZXeunN58QuX2gP0ehFWw7W1Xi1nRTmenp2feTX25Bot5/X0Leyts3xekRYN43M9XplIz+ipGYH7sTMiy+l9AK+OtVoc6S29PQ3jlp4etZ5Rn6yupkU9jZ4686KcqGcU93oYt+bZnn2m5s/C/c7Hw6Du+ZzDXkaU7/VDjT3Pz7zMjzSOMzCXazhmvBrW2ecYwXzO45gxj3OsDmHdi7PaKB89XFFHPA819hAvbxbuz3xeLIQYS9N3PiHEeF5cPiHE5dDlE2IRunxCLEKXT4hF6PIJsQhdPiEWof9ASYglXH39D9DLnpck0mLpAAAAAElFTkSuQmCC)

### 1.2 Linux
  在Linux上是用yum安装Git，非常简单，只需要一行命令。
``` bash
$ yum -y install git
```

输入 git --version 查看Git是否安装完成及查看其版本号。

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAaIAAAAkCAYAAADLlT4eAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAASRSURBVHhe7ZlRbuNADENz3d6q371kFvkQwHApaTyOx0lMAw+wSGrkzRYWmt5+f3/vPz8/99vtZowxxqyHF9Hf398TT+FFZHP52VSu8laQzc/0I1gxYxb8HM56zm72Wc/1Lpz5f2MuSraInkILyeYrfUt2JWc+14oZr0A9J2rKfxXV2UfO/RT8GZjleBG9njOfa8WMV6CeEzXlr+CsucZcmtFFhHrccw71rb7yHrAfdaaN6MgeHz0k81FXmdCQUV95D7C/gvOqPzRE5ZXPsI89DPZVcI/q54zSEcwcSTYP9bhHulz4rCufM5XHGayVb0zJlt+I+Acsu+f8iF9prMc9all2VOt8ZsYLPbzsnvMjfqWNwH1dXWVCYx9RHmrKr+B81T/rHUk2N3Tls8a57J7zqGX6iNbVxqTMLCJVs7fVrzTW4x61LDuqKZ+JDFLlOh1r9pSmMsxIRsF96hylIexXeeWhpvwKzlf9s15H9DIjPt4jmMvg7GidoXIjWlcbk7JnEXUeap1faazHPWpZdlRTPsJ+lc881rFmr9NYZ195FdyXnRN65bGuUFnUlF/B+ap/1juSbG7omY9syVQ55Y9oXW1MyqcvIs6hv0VTPsJ+lc881rFmL9M6r+qp4L7uHOV3PQhno1ZgXwZnq95Z72h4NtbsKUYyQZVV3ojW1cakvPMiyu47VLbTtvpcI5nHOtbsbfWVpvwMznb1aCZDZVFTfgXnq/5Rr8odRczk2VwrTWUqL8uPZlnramNSskXEP0SZjnCGc52vcpmufJWpvD0+1lkGvUpTGfZGfJVTfgb38Rnod17oTJVDTfkd0YN0GfY5o/yjyWbjc3Em0xHOqFzls5f5WW1MCi8iY74BvwSN+SC8iMw3wIvHi8iYD8KLyHwLsXy8hIz5MLyIjDHGnIoXkTHGmFPxIjLGGHMqM4uo+w7+07+fP/tvDLPzow9RuYy9/cGeXmPMBXn1IvqGl9CZ/4aYvXW+6tlyzt7+IHq29hljLsyrv5rzS2gefIlv/QxVz8w5yNb+yG/tM8ZcHFxE/BJRLxTOKB3BTEWWRz3ukS4XPuvK50zlcQZr5W9hplf1zJyDzPbvnWuMuRixiPjlUb1MZr2KrC905bPGueye86hl+ojW1aPs7UNUrmJv/4M9vcaYC/Jpi0jB2dE6Q+VGtK4eZaZP9cycg8z2751rjLkYn/wbEbMlU+WUP6J19Sgzfapn5hxktn/vXGPMxXiXRfSAe7FmTzGSCaqs8ka0rh5lpk/1zJyDzPbvnWuMuRhHLqIqlxE93Mu10lSm8rL8aJa1rh6l61P+qJbpo1qlB51vjDFPxCJ6FPECQbih8zmj/I6sF8/lTKYjnFG5ymcv87O6I/JMlst0hDOYy3SEM5jLdIZzxhjzBC4ixC8RY4wxS5j5as4YY4x5GdVXc/+FjTHGmFeTfTVnjDHGLMGLyBhjzKl4ERljjDkVXkTv8HeibC4/m8pV3gqy+Zl+BCtmzIKfwzs/pzFmIdkiegotJJuv9C3ZlZz5XCtmvAL1nKgp3xjzpdx9+fLly5ev0677/R872hUaXgDExwAAAABJRU5ErkJggg==)

## 2. 常用命令

### 
``` bash
$ git init                                         // 初始化本地仓库
```
``` bash
$ git remote add origin "remote repository"        // 链接远程仓库
```
``` bash
$ git add (.)                                      // 将内容从工作目录添加到暂存区("."可以添加所有更改)
```
``` bash
$ git commit -am "commit message"                  // 将暂存区里的改动提交到本地仓库
```
``` bash
$ git push -u origin master                        // 提交本地仓库代码到远程仓库(-u 初次提交添加)
```
``` bash
$ git push origin mastr --force                    // 覆盖提交到远程master分支
```
``` bash
$ git checkout (-f/-b) feature                     // 切到feature分支 (-f 强制切换,-b从当前分支新建并切到feature)
```
``` bash
$ git checkout -b feature origin feature           // 切换到远程feature分支
```
``` bash
$ git branch -a                                    // 查看分支(-a 查看远程分支)
```
``` bash
$ git remote update origin --prune                 // 更新git branch -a 查看的远程分支列表
```
``` bash
$ git branch -d branchname                         // 删除本地分支
```
``` bash
$ git push origin --delete branchname              // 删除远程分支
```
``` bash
$ git tag tagName                                  // 新增tag
```
``` bash
$ git tag -a tagName commitID -m "commit message"  // 绑定commit到tag
```
``` bash
$ git tag                                          // 查看本地tag
```
``` bash
$ git push origin tagname                          // 同步本地tag到远程
```
``` bash
$ git push origin --tags                           // 推送所有tag到远程
```
``` bash
$ git checkout tagname                             // 切换tag
```
``` bash
$ git tag -d tagname                               // 删除tag
```
``` bash
$ git push origin :refs/tags/<tagname>             // 删除远程tag
```
``` bash
$ git stash save "save massage"                    // 执行存储,添加备注,只有git stash也可以
$ git stash list                                   // 查看stash了哪些存储
$ git stash show                                   // 显示做了哪些改动， 默认显示第一个, 如果需要显示其他存储，后面加stash@{$num}, egg: git stash show stash@{1}
$ git stash show -p                                // 查看存储改动内容， 默认显示第一个，如果需要显示其他存储，中间加stash@{$num}, egg: git stash show stash@{1} -p
$ git stash apply                                  // 应用某个存储， 但是不会从列表中删除，默认第一个， 如果需要显示其他存储，后面加stash@{$num}, egg: git stash apply stash@{1}
$ git stash pop                                    // 回复支持缓存的工资目录， 并删除， 将修改应用到当前目录下，默认第一个，其他同上
$ git stash drop                                   // 丢弃存储， 默认第一个，其他同上
$ git stash clear                                  // 删除所有缓存的stash

* 注意：新增的文件无法stash，需要git add添加进版本库后才能保存到stash
```