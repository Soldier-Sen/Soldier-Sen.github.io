---
title: 初尝hexo搭建个人博客
date: 2019-05-03 15:40:36
tags:
---

一直有写个人博客的想法，拖拖拉拉终于到今天开始实施，hexo + git 搭建了个人博客，欢迎来访：[微醺的个人博客](https://soldier-sen.github.io/archives/)

本人没有购买域名和服务器，仅使用github page平台托管自己的博客，这里可以安心写东西，不容费时间经理维护一个网站，hexo作为一个快速简洁的博客框架，用它来搭建博客还是非常容易的。

# 原由
    市面上各种博客平台很成熟，但是容易受各种限制以及诸多广告引起不适，觉得自己搭建个人博客。
    第一次使用hexo + git搭建个人博客，做些笔记，以备年久忘记！

# Hexo简介
Hexo 是一款基于Node.js的静态博客框架，依赖少、易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。大家可以进入[hexo官网](https://hexo.io/zh-cn/)进行详细查看，因为Hexo的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。

本教程分三部分：
>一、hexo搭建还有部署到github上。
>二、hexo的基本配置、更换主题、实现多终端工作，以及在coding page部署实现国内外分流。
>三、hexo添加各种功能，包括搜索的SEO、阅读量统计、访问量统计和评论系统。
-----------------------------------------------------------------------------------

# 一、hexo搭建和部署
hexo搭建还有部署到github上， 我使用的是ubunut系统，安装以ubuntu为讲。
## 搭建步骤
    1.安装git
    2.安装node.js
    3.安装hexo
    4.github创建个人仓库
    5.生成ssh密钥并添加到github
    6.将hexo部署到github
    7.发布文章
### 1.安装git
    sudo apt-get install git
    git --version查看版本号，确认是否安装好。
### 2.安装node.js
    Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。
    sudo apt-get install nodejs
    sudo apt-get install npm
    安装完成分别使用 node -v 和 npm -v检查是否安装成功。
### 3.安装hexo
    mkdir blog                  创建一个blog文件夹
    npm install -g hexo-cli     安装hexo, 如果报权限问题，请加sudo。
    hexo -v                     查看版本号。
到这里工具已经安装完毕。 初始化hexo:
>hexo init blog
>cd blog ; npm install

安装完成blog目录下有：
>    ├── _config.yml    博客的配置文件
    ├── db.json         不知
    ├── node_modules    依赖包
    ├── package.json    不知
    ├── public          存放生成的页面
    ├── scaffolds       生成文章的一些模板
    ├── source          用来存放自己的文章
    └── themes          主题

>   hexo g
>   hexo s
在浏览器输入localhost:4000就可以看到自己生成的博客了。

### 4.github创建个人仓库
注册github账号并登录， Repositories -> new 创建一个个人仓库。仓库名：账户名.github.io，只有这样后面部署到github page的时候，才会被识别。也就是账户名.github.io。点击create repository。

### 5.生成ssh密钥并添加到github
>   git config --global user.name "yourname"
    git config --global user.email "youremail"

这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户, 然后创建SSH,一路回车。
>   ssh-keygen -t rsa -C "youremail"
这个时候它会告诉你已经生成了.ssh的文件夹。把.ssh中id_rsa.pub内容拷贝出来， 而后在GitHub的setting中，找到SSH keys的设置选项，点击New SSH key把你的id_rsa.pub里面的信息复制进去。
>   ssh -T git@github.com       查看是否成功。
### 6. 将hexo部署到GitHub
    deploy:
    type: git
    repo: git@github.com:xxxxx/xxxxx.github.io.git
    branch: master
xxxxx是你的github账户名。这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。
`    npm install hexo-deployer-git --save`
然后
`   hexo clean;`
    `hexo generate;`
    `hexo deploy`
其中 hexo clean 清除你之前生成的东西,也可以不加。 hexo generate生成静态文章，也可以用hexo g缩写， hexo deploy 部署文章，hexo d为缩写。接下来在浏览器输入： `http://xxxxx.github.io` 就可以查看自己的博客了。

### 7.发布文章
`   hexo new “新的博客文章名”`
source/_post中打开markdown文件，就可以开始编辑了。当你写完的时候，再：
`    hexo g`
`    hexo d`
就可以看到更新了。

# 二、hexo配置
hexo的基本配置，更换主题，实现多终端工作，以及在coding page部署实现国内外分流。
## hexo基本配置
在根目录下的_config.yml，就是整个hexo框架的配置文件。可以在里面修改大部分配置。详情可参考[官方配置](https://hexo.io/zh-cn/docs/configuration)描述。

#### 网站
| 参数          |  描述       |
|:------------:|:-------------:|
| titie         | 网站标题 |           
| subtitle      | 网站副标题 |        
| description   | 网站描述  |
| author        | 作者      |
| language      | 语言      |
| timezone      | 网站时区，默认使用电脑的时区|
#### 网址
| 参数          |  描述       |
|:------------:|:-------------:|
| url         | 网址 |   
| root         | 网址跟目录     |
| permalink     | 文章的永久链接 |
| permalink_defaults | 永久链接中各部分的默认值 |

在这路，需要把url改为你的网站域名（如果购买了域名的）。permalink, 也就是你生成某个文章时的那个链接格式。
比如我新建一个文章叫tmp.md, 那么这个时候它自动生成的地址就是： http://localhost:4000/2019/05/03/tmp.md

>theme: next 选择的主题是next， 对应theme目录下的next文件夹。
### Front-matter
Front-matter 是文件最上方以 --------- 分隔的区域， 用于制定个别文件的变量，如：
>title: Hello World
>date: 2019-05-03 00:15:05

| 参数          |  描述       |
|:------------:|:-------------:|
| layout         | 局部      |
| title          | 标题       |
| date           | 建立日期    |
| updated        | 更新日期    |
| commets        | 开启文章的评论功能 |
| tags           | 标签\(不适用于分页\) |
| categories     | 分类\(不适用于分页\) |
| permalink      | 覆盖文章网址        |


# 删除文章
1.先删除本地文件
2.通过生成和部署命令进而将远程仓库中的文件也一起删除。
>以最开始的helloworld.md这篇文章为例：  进入到source/_post文件夹中，找到helloworld.md文件，在本地执行删除。然后一次执行hexo g 、hexo d，此时再去博客主页查看已经没有了。

# 第三部分
hexo 添加各种功能，包括搜索的SEO、阅读量统计、访问量统计和评论系统。本文参考了[zjufangzh](https://blog.csdn.net/sinat_37781304/article/details/82729029)以及[visugar.com](http://visugar.com/2017/08/01/20170801HexoPlugins/)的博客。前人种树，后人乘凉。
## 1. SEO优化
推广是很麻烦的事情，怎么样别人才能知道我们呢，首先需要让搜索引擎收录你的这个网站，别人才能搜索的到。那么这就需要SEO优化了。
>SEO是由英文Search Engine Optimization缩写而来， 中文意译为“搜索引擎优化”。SEO是指通过站内优化比如网站结构调整、网站内容建设、网站代码优化等以及站外优化。

# 总结 （[参考](https://www.cnblogs.com/mrwuzs/p/7942689.html)）
1.node_mudules文件夹中的的内容是整个*hexo*的依赖包，这个一般情况下不需要去做任何增删改。
2.public文件夹是当我们自行hexo generate 命令之后有hexo 根据我们的主题以及文章内容生成的一个文件夹。打开之后可以看到对应时间我们写的博客内容，例如我今天的博客就是public/2019/05/03/初尝hexo搭建个人博客/index.html
3.scaffolds其实就是手脚架的意思，有些人称之为模板，其实就是我们新生成一篇文章时从那个模板来生成。这里默认只有三个模板draft、page、post.
4.source和themes子文件夹，
5._config.yml文件，是主目录的配置，
6.db.json，没有仔细研究，后面再更新这部分内容。
7.package.json，没有研究。