---
title: win10和ubuntu双系统k380的配置
date: 2019-07-10 12:41:11
tags:
---

### 操作步骤

1.首先在 Windows 10 系统中连上蓝牙鼠标，目的是留下连接记录，方便之后来修改连接值。

2.在 Ubuntu 18.04 系统中连上蓝牙键盘。 

3.获取 Ubuntu 18.04 系统中的蓝牙配对 linkkey 值。

首先切换到 root 账户：su

然后执行：cd /var/lib/bluetooth/

执行（两个小写的L），获得电脑的蓝牙地址：ll

cd 这个地址，再次执行 ll，获得鼠标的蓝牙地址：ll

cd 鼠标的蓝牙地址，并执行：cat info

找到 [LinkKey]，记下这个值：

Key=966B5BDD8EAECD793FC26700B8A6B337。

![è§£å³Ubuntu 18.04ä¸Windows 10åç³»ç»èçé¼ æ è¿æ¥çé®é¢](https://ywnz.com/uploads/allimg/18/1-1PS1113152M4.JPG)

4.再回到 Windows 10 系统，此时蓝牙鼠标已经自动连接上了（之前有连接记录），但是不能操控。这个时候先到微软官网下载 [PSTools](https://docs.microsoft.com/zh-cn/sysinternals/downloads/psexec) 工具

下载 PSTools 完成后解压到文件夹即可，在文件夹内以管理员身份运行 cmd，执行：PsExec.exe -s -i regedit

找到 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\BTHPORT\Parameters\Keys\ 下的文件夹，正常情况下是以电脑 mac 地址命名的，找到文件夹内的以蓝牙鼠标 mac 地址命名的文件，修改它的值为之前第三步获取的 key 的值即可。

 

5.重新启动电脑，这时随便你进入 Ubuntu 18.04，或是进入 Windows 10 系统都能正常使用了。