+++
title = '哈希算法'
date = 2015-03-06T19:25:27+08:00
+++
在计算机科学中，哈希算法是一种常用的密码学技术，用于将任意大小的数据映射到固定大小的数据上。
<!--more-->
# MD5 
MD5, Message Digest Algorithm 5，是一种广泛使用的哈希算法，用于生成 128 位（16 字节）的哈希值，通常以 32 位十六进制数表示。

OpenSSL 使用示例：
``` Shell
openssl md5 plaintext.txt
```

# SHA
SHA，Secure Hash Algorithm，安全哈希算法。SHA 包含多个版本：SHA-1、SHA-256、SHA-384 等，每个版本都具有不同的输出长度和安全性。

OpenSSL 使用示例：
``` Shell
openssl sha256 plaintext.txt
```
