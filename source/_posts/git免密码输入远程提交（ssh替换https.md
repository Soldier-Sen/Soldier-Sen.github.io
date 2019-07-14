---
title: git免密码输入远程提交（ssh替换https)
date: 2019-06-19 23:35:52
tags: [git]
categories: git使用
---

# 一、生成并设置ssh公钥
linux 中执行命令 ssh-keygen， 在～/.ssh目录下生成id_rsa.pub文件，将文件的内容复制拷贝到git上。
![设置ssh图片](https://github.com/Soldier-Sen/img/raw/master/git%E8%AE%BE%E5%A4%87ssh%E5%85%AC%E9%92%A5.png)

<!-- more -->

# 二、首先查看是否本项目初始化了git
>git remote -v 

不出意外你将会看到类似如下
>origin https://github.com/xxx/xxx.git (fetch)
>origin https://github.com/xxx/xxx.git (push)

# 三、ssh替换https
移除原有的依赖
>git remote rm origin

新增现在的ssh依赖
>git remote add origin git@github.com:xxx/xxx.git

提交
>git push origin master
