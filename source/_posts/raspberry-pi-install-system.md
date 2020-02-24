---
title: raspberry pi install system
date: 2020-02-23 21:44:54
tags: [raspberry-pi]
---

1.安装系统 

可在http://www.raspberrypi.org/downloads下载raspbian系统

使用win32diskimager烧写系统到sd卡 http://www.waveshare.net/w/upload/7/76/Win32DiskImager.zip

2.安装软件

​	2.1安装配置SSH服务: sudo apt-get install openssh-server

​	2.2检查树莓派SSH服务是否已经开启ps -e | grep ssh

​		ssh-agent为客户端，sshd为服务器端服务，只有ssh-agent没有sshd表明SSH服务还没有开启。

​	2.3开启SSH服务

​		openssh-server配置文件为“/etc/ssh/sshd_config”，可以配置SSH服务的各项参数，如端口配置，默认端口为22，可以配置为其他端口，配置后重启生效。

​	2.4树莓派SSH服务开机自启动：打开/etc/rc.local文件，在语句exit 0之前加入：/etc/init.d/ssh start

3,安装其它库

  3.1wiringPi、bcm2835、python库 ：http://www.waveshare.net/study/portal.php?mod=view&aid=742

