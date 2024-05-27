+++
title = '2024 阅读 MySQL 8.3 源代码：终端中下载、编译、运行、调试'
date = 2024-05-26T10:37:05+08:00
draft = true
+++
MySQL 八股文看不下去，不如结合源码和优质的书来学习，这里我参考了《MySQL 技术内幕：InnoDB 存储引擎》（第2版）。
<!--more-->
[MySQL Server](https://github.com/mysql/mysql-server) 百分之八十多的代码是 C++ 写得，百分之十的代码是 C 写得，构建工具使用的是 CMake + Make。编译、运行、调试，对于 C++ 程序员来说可能不算什么，但是对于一个 Java 程序员来说确是有点陌生的。
# 环境信息
``` Shell
MacBook Pro
Apple M2
macOS 14.4.1
```
# 下载源代码
本来我打算直接使用 Github 上最新的代码，但是编译的过程中出现了一些错误，没能理解和解决。

最终，我选择了参考 [Homebrew](https://github.com/Homebrew/homebrew-core/blob/master/Formula/m/mysql.rb) 的方式，来处理下载、编译、库依赖等问题。
``` Shell
wget https://cdn.mysql.com/Downloads/MySQL-8.3/mysql-boost-8.3.0.tar.gz
tar -xvf mysql-boost-8.3.0.tar.gz
```
# 编译
``` Shell
cd mysql-8.3.0 
mkdir build && cd build
```
