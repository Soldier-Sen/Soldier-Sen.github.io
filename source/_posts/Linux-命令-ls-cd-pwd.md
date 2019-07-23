---
title: Linux 命令 -- ls cd pwd
date: 2019-07-18 01:08:51
tags:
---

# 1 Linux pwd —— 打印当前工作目录名
pwd - print name of current/working directory
Linux pwd命令用于显示工作目录。
执行pwd指令可立刻得知目前所在的工作目录的绝对路径名称。
<!-- more -->
## 语法
```
pwd: pwd [-LP]
    打印当前工作目录的名字。
    
    选项：
      -L	打印 $PWD 变量的值，如果它包含了当前的工作目录
      -P	打印当前的物理路径，不带有任何的符号链接
    
    默认情况下，`pwd' 的行为和带 `-L' 选项一致
    
    退出状态：
    除非使用了无效选项或者当前目录不可读，否则返回状态为0。
```
## 实例
```
# pwd
/home/hhs/opt/Soldier-Sen.github.io
```


# 2 Linux cd —— 更改目录
Linux cd命令用于切换当前工作目录至 dirName(目录参数)。
其中 dirName 表示法可为绝对路径或相对路径。若目录名称省略，则变换至使用者的 home 目录 (也就是刚 login 时所在的目录)。
另外，"~" 也表示为 home 目录 的意思，"." 则是表示目前所在的目录，".." 则表示目前目录位置的上一层目录。

绝对路径:是以根目录(” / “)为起点的完整路径,以你所要到的目录为终点.
相对路径:是你当前的目录(” . “)为起点的路径,以你所要到的目录为终点. 
## 语法
```
cd [dirName]
```
dirName：要切换的目标目录
## 实例
跳到/usr/bin目录: `cd /usr/bin`
跳到自己的home目录: `cd ～`
跳到当前目录的上一层: `cd ..`
跳到当前目录的上上一层: `cd ../..`
跳回到之前的目录: `cd -`
# 3 Linux ls —— 列出目录内容
Linux ls命令用于显示指定工作目录下之内容（列出目前工作目录所含之文件及子目录)。

## 语法
    ls [-alrtAFR] [name...]
## 参数
-a 显示所有文件及目录 (ls内定将文件名或目录名称开头为"."的视为隐藏档，不会列出)
-b 把文件名中不可输出的字符用反斜杠加字符编号(就象在C语言里一样)的形式列出。
-c 输出文件的 i 节点的修改时间，并以此排序。
-d 将目录象文件一样显示，而不是显示其下的文件。
-f -U 对输出的文件不排序。
-i 输出文件的 i 节点的索引信息。
-k 以 k 字节的形式表示文件的大小。
-l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
-m 横向输出文件名，并以“，”作分格符。
-n 用数字的 UID,GID 代替名称。
-u 以文件上次被访问的时间排序。
-x 按列输出，横向排序。
-C 按列输出，纵向排序。
-G 输出文件的组的信息。
-L 列出链接文件名而不是链接到的文件。
-o 显示文件的除组信息外的详细信息。
-F 在每个文件名后附上一个字符以说明该文件的类型，“*”表示可执行的普通文件；“/”表示目录；“@”示符号链接；“|”表示FIFOs；“=”表示套接字(sockets)。
-r 将文件以相反次序显示(原定依英文字母次序)
-t 将文件依建立时间之先后次序列出
-A 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
-N 不限制文件长度。
-Q 把输出的文件名用双引号括起来。
-S 以文件大小排序。
-R 若目录下有文件，则以下之文件亦皆依序列出
-X 以文件的扩展名(最后一个 . 后的字符)排序。

## 实例
![图片ls -l](https://github.com/Soldier-Sen/Soldier-Sen.github.io/raw/hexo/picture/command/ls_l.png)
ls -m
```
_config.yml, db.json, debug.log, node_modules, package.json, picture, public，scaffolds, source,themes
```
ls -F
```
_config.yml  debug.log      package.json  public/     source/
db.json      node_modules/  picture/      scaffolds/  themes/
```
ls -i 
```
2103593 _config.yml  2103582 debug.log     2103588 package.json  3804283 public     3148925 source
2097664 db.json      3802635 node_modules  3803114 picture       3148921 scaffolds  3148929 themes
```
ls -lS ， 列出文件详细信息，并按文件大小进行排序
```
总用量 960
-rw-r--r--   1 hhs hhs 501668 5月   3 23:25 debug.log
-rw-r--r--   1 hhs hhs 430262 7月  18 01:47 db.json
drwxr-xr-x 298 hhs hhs  12288 5月   3 15:18 node_modules
drwxr-xr-x   2 hhs hhs   4096 7月  18 01:33 picture
drwxr-xr-x  14 hhs hhs   4096 7月  18  2019 public
drwxr-xr-x   2 hhs hhs   4096 5月   3 15:18 scaffolds
drwxr-xr-x   8 hhs hhs   4096 5月   3 23:41 source
drwxr-xr-x   5 hhs hhs   4096 5月   3 22:51 themes
-rw-r--r--   1 hhs hhs   2161 5月   4 13:00 _config.yml
-rw-r--r--   1 hhs hhs    483 5月   3 15:18 package.json
```
ls -tlr ， 列出文件详细信息，并按文件建立时间次序排序。
```
总用量 960
-rw-r--r--   1 hhs hhs    483 5月   3 15:18 package.json
drwxr-xr-x 298 hhs hhs  12288 5月   3 15:18 node_modules
drwxr-xr-x   2 hhs hhs   4096 5月   3 15:18 scaffolds
drwxr-xr-x   5 hhs hhs   4096 5月   3 22:51 themes
-rw-r--r--   1 hhs hhs 501668 5月   3 23:25 debug.log
drwxr-xr-x   8 hhs hhs   4096 5月   3 23:41 source
-rw-r--r--   1 hhs hhs   2161 5月   4 13:00 _config.yml
drwxr-xr-x   2 hhs hhs   4096 7月  18 01:33 picture
-rw-r--r--   1 hhs hhs 430262 7月  18 01:47 db.json
drwxr-xr-x  14 hhs hhs   4096 7月  18  2019 public
```
