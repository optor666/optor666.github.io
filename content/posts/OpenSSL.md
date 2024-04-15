+++
title = 'OpenSSL 使用指南'
date = 2023-03-01T23:50:00+08:00
+++
OpenSSL 是一个强大而灵活的加密工具库，为构建安全、保密的网络通信提供了重要支持。
<!--more-->
# RSA 算法使用举例
RSA 是一种非对称加密算法，主要用于数据加密和数字签名。
## 创建 RSA 密钥
``` Shell
openssl genpkey -algorithm RSA -out private_key.pem
# 使用 aes-256-cbc 算法加密私钥文件
openssl genpkey -algorithm RSA -out private_key.pem -aes-256-cbc 
# 从私钥文件中导出公钥文件
openssl rsa -pubout -in private_key.pem -out public_key.pem
```
## 查询 RSA 密钥
``` Shell
openssl rsa -in private_key.pem -text -noout
openssl rsa -in public_key.pem -text -noout -pubin
```
## 公钥加密
``` Shell
# 这里 rsautl 命令在 3.0 版本被弃用了，可以用 pkeyutl 替代，下面解密同理。
echo 'Hello, RSA!' | openssl rsautl -encrypt -pubin -inkey public_key.pem -out encrypted.txt
openssl rsautl -encrypt -pubin -inkey public_key.pem -in plaintext.txt -out encrypted.txt
```
## 私钥解密
``` Shell
openssl rsautl -decrypt -inkey private_key.pem -in encrypted.txt
openssl rsautl -decrypt -inkey private_key.pem -in encrypted.txt -out decrypted.txt
```
## 私钥签名
``` Shell
echo 'Data to sign' | openssl dgst -sha256 -sign private_key.pem -out signature.bin
openssl dgst -sha256 -sign private_key.pem -out signature.bin data.txt
```
## 公钥验证签名
``` Shell
echo 'Data to sign' | openssl dgst -sha256 -verify public_key.pem -signature signature.bin
openssl dgst -sha256 -verify public_key.pem -signature signature.bin data.txt
```

# 创建自签名 Server 证书
## 创建 RSA 密钥
``` Shell
# 命令 genrsa 用于创建 RSA 密钥，和命令 genpkey -algorithm RSA 相比用法更简单一点
openssl genrsa -out server.key 2048
```
## 创建自签名证书
``` Shell
openssl req -new -x509 -key server.key -out server.crt -days 365
```
创建自签名证书，也可以分为两步：
``` Shell 
# 生成证书签名请求
openssl req -new -key server.key -out server.csr
# 使用自己的 key 对证书进行签名
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
## 查看证书
``` Shell
openssl x509 -in server.crt -text -noout
```
## 使用自签名 CA 证书对自签名 Server 证书进行签名
```
openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt
```

# 创建自签名 CA 证书
创建自签名 CA 证书，和创建自签名 Server 证书类似。
## 创建 RSA 密钥
``` Shell
# 命令 genrsa 用于创建 RSA 密钥，和命令 genpkey -algorithm RSA 相比用法更简单一点
openssl genrsa -out ca.key 2048
```
## 创建自签名证书
``` Shell
openssl req -new -x509 -key ca.key -out ca.crt -days 3650
```
创建自签名证书，也可以分为两步：
``` Shell 
# 生成证书签名请求
openssl req -new -key ca.key -out root_ca.csr
# 使用自己的 key 对证书进行签名
openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt
```
## 查看证书
``` Shell
openssl x509 -in ca.crt -text -noout
```

# 数字证书标准
数字证书标准指的是定义数字证书结构、内容、字段和验证规则的规范或标准。这些标准定义了数字证书的基本要素，包括证书的格式、证书办法和验证流程等。

例如，X.509 就是一种公认的数字证书标准，它规定了数字证书的结构、字段和验证规则，是公钥基础设施（PKI）中最常用的证书标准之一。
## 数字证书标准例子
- X.509
- SPKI (Simple Public Key Infrastructure)
- PGP (Pretty Good Privacy)

# 数字证书格式
数字证书格式指的是数字证书在计算机系统中的存储格式。

例如，PEM 就是一种常用的基于 ASCII 的存储证书的格式（它也可以用于存储密钥）。

## 数字证书格式例子
- PEM
- DER 
- PKCS#8
- PKCS#12


# 参考
- [OpenSSL 中文手册](https://www.openssl.net.cn/)
