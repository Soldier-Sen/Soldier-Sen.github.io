---
title: vim插件之tags
date: 2019-06-09 14:37:25
tags: [vim]
categories: vim插件
---

## ctags找不到pthreadx相关函数
在生成ctags时，要使用命令：
>sudo ctags  -I __THROW  -I __THROWNL -I __attribute_pure__ -I __nonnull -I __attribute__ -R --c-kinds=+p --fields=+iaS --extra=+q --language-force=C  /usr/include/

否则ctags会不认识这些宏从而识别不了。