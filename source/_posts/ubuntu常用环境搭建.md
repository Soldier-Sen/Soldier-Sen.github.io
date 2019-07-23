---
title: ubuntu 常用环境搭建
date: 2019-07-13 14:10:24
tags: [ubuntu]
categories: ubuntu环境
---


# 1 Ubuntu 安装远程登录 ssh 服务端
Secure shell（ssh）是一种加密网络协议，用于在不安全的网络上安装运行网络服务，利用ssh可以实现加密并且安全的登录计算机中。
ubuntu系统默认只安装了ssh客户端，只能在本地登录到远端的目标主机中，想要实现其他主机远程登录自己，需要安装部署ssh服务端。
<!-- more -->
## 1.1 安装步骤
1）进行安装：
sudo apt install openssh-server
2）装好之后检查ssh是否已经启动：
打开终端，输入: ps -ef|grep ssh， 如果有sshd说明已经启动成功，如果没有启动，输入sudo service ssh start 即可。
`终端打开方式：按快捷键“Ctrl+Alt+T”或直接点击名为“Terminal”的图标`
3）修改配置文件"/etc/ssh/sshd_config":
sudo vim /etc/ssh/sshd_config, 把配置文件中的"PermitRootLogin without-password"前加一个”#”号,把它注释掉， 再增加一句”PermitRootLogin yes“–>保存，修改成功。
## 1.2 使用
ifconfig 查看系统的ip地址，然后打开远程工具（xshell,Secure CRT等）进行连接登录即可。

注意: 确保进行登录的主机和ubuntu系统可以ping通，否则也无法登录。

## 1.3 常用功能
启动/重启/关闭/查看状态： 
`sudo service ssh <action>`
其中，<action>可以是start/stop/reload/force-reload/restart/try-restart/status任一种；常用的有start（启动）、stop（停止）、restart（重新启动），status（查看状态）等。

## 1.4 ssh服务详细配置
`man sshd_config`
可以查看详细的配置说明。



# 2 Ubuntu NFS服务
在嵌入式Linux开发中，利用NFS服务从开发板访问Linux主机是个高效&方便的调试方法，在程序调试过程中可以避免多次下载程序到开发板。但这需要在Linux主机上首先开通NFS服务。
## 2.1 安装NFS服务
1)安装
sudo apt-get install nfs-kernel-server
2）创建共享目录， mkdir /mnt/nfs-share
3）修改配置
```
编辑NFS配置文件： sudo vim /etc/exports
增加一行描述供NFS访问的的目录, /mnt/nfs-share *(rw,sync,no_root_squash)
/mnt/nfs-share为刚创建的共享目录，
其中"*"表示所有客户机都可以访问（只要能通过网络访问到你）
rw当然表示有读写权限，
no_root_squash表示客户机对此目录有root操作权限
```
配置完毕，可以重启nfs服务
```
sudo /etc/init.d/portmap restart
sudo /etc/init.d/nfs-kernel-server restart
```
## 2.2 测试NFS服务是否开启成功
```
在本机localhost（127.0.0.1）上挂载nfs目录到/opt，（挂载未在/etc/exports里面添加的目录是无效的）
sudo mount -t nfs localhost:/mnt/nfs-share /mnt
```
`ls /opt`可以看到/mnt/nfs-share目录下的内容，卸载可以使用`umount /mnt`。

另，查看NFS目录可以使用  ”showmount -e“ 命令

## 2.3 嵌入式设备中进行挂载
首先，确保嵌入式linux设备中有并且开启了NFS客户端，否则无法挂载；如果未开启，可到内核的menuconfig中修改打开，重新编译内核后更新设备的内核。
```
mount -t nfs -o nolock 192.168.0.216:/mnt/nfs-share /mnt
```
/mnt/nfs-share为刚才配置开启的nfs目录，最后的/mnt是嵌入式设备上的/mnt目录，即挂载点。
-o nolock是去除文件锁，否则会报错。

挂载完成后，这样在设备的/mnt目录直接访问主机的/mnt/nfs-share目录了，把要在设备上运行的程序拷贝到/mnt/nfs-share，然后设备到/mnt拷贝或是直接执行即可。



# 3 Ubuntu 安装Samba服务器及配置
局域网下使用samba服务在Linux系统与Windows系统直接共享文件是一项很方便的操作。
## 3.1 安装samba服务
sudo apt-get install samba

## 3.2 配置
1）首先备份配置文件，cp /etc/samba/smb.conf  /etc/samba/smb.conf-bak
2）打开并且编辑配置文件 vi /etc/samba/smb.conf
在其最后添加： 
```
[hhs]
    comment = Home Directories
    path = /home/hhs
    valid users = hhs
    available = yes  
    browseable = yes  
    writable = yes  
    public = no 
```
3）创建Samba用户
```
sudo useradd hhs
sudo smbpasswd -a hhs #根据提示输入用户密码(登陆Samba共享目录的时候需要)。
```
4）重启Samba， sudo service smbd restart
5）测试
在Ubuntu的Files底部的Network中选择Connect to Server, 然后在弹出来的对话框中深入smb://192.168.0.216/hhs(192.168.0.216为我的电脑ip地址)， 然后点击右下角的Connect按钮. 此时会提示输入密码(在第3步中创建)，输入密码后即可进入共享目录。
## 3.3 使用
在Windows电脑上，输入“\\192.168.0.216\hhs”， 然后输入用户名（hhs）和密码后，既可以访问了。

注，部分同学可能会遇到防火墙的问题，sudo iptables -F命关闭防火墙。
