---
title: 交叉编译zlib与openssl
date: 2019-05-06 17:57:25
tags: [rtmp,zlib,openssl]
categories: 第三方库,交叉编译
toc: true
---

本文主要记录在编译rtmp时，依赖的两个库：zlib、openssl。本次记录在x64和两个arm平台编译验证通过。

# 一、交叉编译zlib

<!-- more -->
开发环境：ubuntu 18.04

交叉编译工具：arm-xxx (多个平台验证通过)

zlib源码下载：http://www.zlib.net/


交叉编译zlib-1.2.11.tar.gz:
```
tar xf zlib-1.2.11.tar.gz
cd zlib-1.2.11
CC=arm-gk720x-linux-gcc ./configure --prefix=install --static
make && make install
```
CC指定交叉编译工具，此工具需先配置PATH路径，若未配置则需加上绝对路径；
--prefix  指定编译后安装路径
--static  指定仅编译出静态库


# 二、交叉编译openssl

开发环境：ubuntu 18.04

交叉编译工具：arm-xxx (多个平台验证通过)

openssl源码下载：http://www.linuxfromscratch.org/blfs/view/8.0/postlfs/openssl.html
```
tar xf openssl-1.0.2k.tar.gz
cd openssl--1.0.2k
CC=arm-xxxx-cc ./config --prefix=install no-asm
vim Makefile；搜索-m64并删除, 否则编译不过
make -j16 && make install
```
--prefix  指定安装路径为install
no-asm    需要加上这个参数，否则编译不过。
make -j16 指定多核编译，加快编译速度，可不加直接make即可。


# 三、librtmp编译
```
export C_INCLUDE_PATH=install/include
将# CRYPTO=OPENSSL改为CRYPTO=install/bin (此为openssl编译后安装的bin路径)
编译rtmp不通过，提示 对‘dlopen’未定义的引用时，加上-ldl
```
rtmp推流延时问题
    播放器端不要缓存即可非常实时，测试延时1-2秒。
>ffplay -fflags nobuffer rtmp://192.168.1.140:1935/live/test1