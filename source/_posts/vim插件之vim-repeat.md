---
title: vim插件之vim-repeat
date: 2019-06-09 15:41:11
tags: [vim]
categories: vim插件
---

# vim-repeat
### 作用
重复一个插件的操作，支持surround.vim，通过surround操作之后的行为.号重复。

### 安装
通过Bundle安装，在.vimrc中添加：
>Bundle 'tpope/vim-repeat'
然后BundleInstall安装即可。
<!-- more -->

示例，与surround结合使用。
1)将如下"双引号都替换成'单引号
```
"hello"
"hello"
"hello"
"hello"
"hello"
```
2)执行： `cs"'` 
```
'hello'
"hello"
"hello"
"hello"
"hello"
```
3)j.
```
'hello'
'hello'
"hello"
"hello"
"hello"
```
4)重复3)
```
'hello'
'hello'
'hello'
'hello'
'hello'
```