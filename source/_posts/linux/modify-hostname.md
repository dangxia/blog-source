title: modify hostname
date: 2015-10-26 12:16:19
tags: linux
categories: linux
---
### modify hostname
+ uname -a 查看hostname
+ hostname xxxxx 修改下，让hostname立刻生效。
+ vi /etc/hosts 修改原hostname为 xxxxx
+ vi  /etc/sysconfig/network 修改原hostname为 xxxxx,这样可以让reboot重启后也生效
+ reboot重启，uname -a 重新检查下。收工

