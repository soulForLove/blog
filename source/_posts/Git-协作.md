---
title: Git 协作
date: 2017-08-17 16:22:02
categories: [Git]
tags: [Git]
description: GitHub团队项目流程操作
copyright: true
comments: true
---
<!-- more -->
### Fork项目到个人的仓库
### Clone项目到本地
#### SSH （命令行操作）
* 复制ssh url在git命令行执行
* git clone git@github.com:XXXXXX/你要fork的项目名
```
    git branch -a   ------查看所有分支
    git checkout -b dev-local origin/dev    ----------创建一个dev-local分支（-b），并把远程dev分支（origin/dev）的内容放在该分支内。接着切换到该分支（checkout）
```
#### HTTPS（idea等工具操作）
* https: //github.com/XXXXXX/你要fork的项目名.git
	VCS->checkout from Version controller->git->clone
* 后期直接idea操作， 不需要进行以下操作

### push修改到自己的项目上
* git add --all
* git fetch
* git commit -m '提交的注释'
* git push 

### 请求合并到团队项目上
* 点击Pull request
* 添加标题跟描述信息，确认提交内容
* 点击Create pull request进行发送合并请求