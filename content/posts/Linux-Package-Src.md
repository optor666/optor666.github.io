+++
title = '如何查看 Linux 发行版软件包的源代码'
date = 2024-01-18T16:59:11+08:00
+++
为了彻底搞明白 Linux 系统远程登录的技术细节，我想找到 telnet 客户端、服务器端相关源代码来阅读。搜索一番之后，虽然能找到 Go 或 Java 版本的相关代码，但我更想找到 Linux 发行版中 telnet 软件包的 C 源代码来阅读。
<!--more-->
# CentOS
首先，Linux 发行版我一般是使用 CentOS 系统的，但搜索一番之后没找到 CentOS 系统上下载软件包源代码的方法。

# Ubuntu
CentOS 系统上没能搞定，只能来试试 Ubuntu 系统了。没想到，Ubuntu 系统是可以的。
## 安装 apt-src
``` Shell
sudo apt-get install apt-src
```
## 配置 apt-src 仓库地址
我使用的是腾讯云服务器，只需把第四行取消注释即可：
```
ubuntu@VM-1-1-ubuntu:~$ cat -n /etc/apt/sources.list
     1	# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
     2	# newer versions of the distribution.
     3	deb http://mirrors.tencentyun.com/ubuntu jammy main restricted
     4	deb-src http://mirrors.tencentyun.com/ubuntu jammy main restricted
     5
     6	## Major bug fix updates produced after the final release of the
     7	## distribution.
     8	deb http://mirrors.tencentyun.com/ubuntu jammy-updates main restricted
     9	# deb-src http://mirrors.tencentyun.com/ubuntu jammy-updates main restricted
    10
```
## 下载 telnet 客户端/服务器端源代码
```
sudo apt-src install telnetd
```
接下来，就是愉快地阅读 telnet 相关源代码了！
