---
title: Linux小知识
date: 2017-10-24 17:32:18
categories: [linux]
tags: [linux,小知识]
description: Linux 相关知识点
copyright: true
comments: true
---
<!-- more -->
## linux 执行jar

* nohup java -jar dlz-0.0.1-SNAPSHOT.jar --server.port=8899  >> background.log 2>&1  &

## linux命令

* 查看是否存在相关进程(Back)： ps ax | grep Back	

## 添加用户
```
首先用adduser命令添加一个普通用户，命令如下：
#adduser tommy  //添加一个名为tommy的用户
#passwd tommy   //修改密码
Changing password for user tommy.
New UNIX password:     //在这里输入新密码
Retype new UNIX password:  //再次输入新密码
passwd: all authentication tokens updated successfully.

```

## 赋予root权限

* 方法一
```
修改 /etc/sudoers 文件，找到下面一行，把前面的注释（#）去掉
## Allows people in group wheel to run all commands
%wheel    ALL=(ALL)    ALL
然后修改用户，使其属于root组（wheel），命令如下：
#usermod -g root tommy
修改完毕，现在可以用tommy帐号登录，然后用命令 su - ，即可获得root权限进行操作。

```

* 方法二

```
修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
tommy   ALL=(ALL)     ALL
修改完毕，现在可以用tommy帐号登录，然后用命令 su - ，即可获得root权限进行操作。

```

## 通常查找出错误日志 cat error.log | grep '错误error' , 这时候我们还有个需求就是输出当前这个日志的前后几行：
```
cat error.log | grep -C 5 '错误error' 显示error.log文件里匹配"错误error"字符串那行以及上下5行
cat error.log | grep -B 5 '错误error' 显示"错误error"及前5行
cat error.log | grep -A 5 '错误error' 显示"错误error"及后5行
```

## 批量删除某字符串开头的文件
```
删除“tomcat-开头的文件”
find . -name "tomcat-*" | xargs rm -rf
```

## 查看文件最后N行的命令
```
tail -n 20 filename
说明：显示filename最后20行
```