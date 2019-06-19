---
title: ubuntu提示没有libmpfr.so.4
date: 2019-05-09 21:54:02
tags: [ubuntu]
categorise: ubuntu 
---

今天在ubuntu18上安mstart平台uclibc的交叉编译器进行交叉编译时，
提示：error while loading shared libraries:libmpfr.so.4: cannot open shared object file: No such file or directory.

解决：
```
sudo ln -s /usr/lib/x86_64-linux-gnu/libmpfr.so.6 /usr/lib/x86_64-linux-gnu/libmpfr.so.4
```