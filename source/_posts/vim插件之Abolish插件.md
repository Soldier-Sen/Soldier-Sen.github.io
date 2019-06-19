---
title: vim插件之Abolish
date: 2019-06-08 17:58:55
tags: [vim]
categories: vim插件
toc: true
---

# 1.abolish.vim插件
abolish.vim插件可以快速替换字符串。
### 安装方法1
1) vim官网下载[abolish.vim插件](https://www.vim.org/scripts/script.php?script_id=1545)
2) 解压到.vim目录下即可。
3) 或是解压abolish.zip后将doc下的abolish.txt拷贝到.vim/doc下,abolish.vim拷贝到.vim/plugin下。

### 安装方法2
通过vim的vundle插件管理来安装的,编辑~/.vimrc中的配置
Bundle 'tpope/vim-abolish'
然后保存.vimrc, 在vim中:BundleInstall,即可安装完成。
<!-- more -->
#### 使用例子1
```
The dog bit the man.
执行， :%S/{dog,man}/{man,dog}/g 即可替换成如下：
The man bit the dog.
```
#### 使用例子2
将如下文本大小写都替换成building
facility
Facility
Facilities
执行, `:%S/facilit{y,ies}/building{,s}/g`
building
Building
Buildings