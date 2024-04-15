+++
title = '对称加密算法'
date = 2024-03-06T19:59:50+08:00
+++
对称加密算法以其简单、高效的特点，在信息安全领域中发挥着重要的作用。
<!--more-->
# 基本概念
## 密钥长度
密钥长度是指用于加密和解密数据的密钥的比特数。在对称加密算法中，密钥用于加密和解密数据，因此密钥的长度直接影响加密算法的安全性和强度。较短的密钥长度通常会导致更脆弱的加密，因为短密钥长度意味着攻击者需要尝试更少的可能性。

## 加密模式
- ECB，Electronic Codebook，电子密码本模式：将明文分成块并使用相同的密钥对每个块进行加密。这种模式在同一块出现相同的明文时会产生相同的密文，因此不适合对包含大量重复内容的数据进行加密。
- CBC，Cipher Block Chaining，密码块链接模式：每个明文块在加密之前与前一个密文块进行异或运算，增加了对数据的混淆度。需要一个初始化向量（IV）来保证每次加密的唯一性。
- CTR，Counter，计数器模式：使用一个计数器和一个随机数（Nonce）来产生密钥流，然后将密钥流与明文进行异或运算以产生密文。CTR 模式可以进行并行加密，因此在性能要求高的情况下很有用。
- CFB，Cipher Feedback，密文反馈模式：类似于 CTR 模式，但使用前一个密文块作为输入来产生密钥流。
- OFB，Output Feedback，输出反馈模式：类似于 CFB 模式，但使用加密算法的输出作为密钥流，而不是使用前一个密文块。

## 填充方式
填充方式是在加密过程中用于处理不完整数据块的一种技术。

块加密算法要求待加密的数据的长度必须是加密算法数据块长度的整数倍，如果原始数据长度不满足这个条件，就需要采用填充方式将其填充到正确的长度。

说到常见的填充方式，先要了解下 PKCS。PKCS，Public Key Cryptography Standards，密码学标准，是一系列定义了密码学算法和协议的标准集合。PKCS 包含了各种密码学相关的标准，包括数字证书、数字签名、加密算法、密钥管理、填充方式等。PKCS 标准包括许多不同的规范，例如：PKCS#1、PKCS#5、PKCS#7、PKCS#8、PKCS#12，每个规范都定义了不同的密码学技术或协议。

### PKCS5Padding
PKCS5Padding 最初定义在 PKCS#5 标准中，只支持块大小为 8 个字节的加密算法。PKCS5Padding 将需要填充的字节全部填充为待填充字节长度的值。

### PKCS7Padding
PKCS7Padding 最初定义在 PKCS#7 标准中，扩展了 PKCS5Padding，适用于块大小为 1 到 255 字节的加密算法。PKCS7Padding 将需要填充的字节全部填充为待填充字节长度的值。

# 加密算法分类
## 按数据处理方式分类
- Block Cipher，块加密算法：块加密算法将数据分成固定大小的数据块进行加密，每个数据块作为一个整体进行处理。常见的块加密算法包括：AES、DES、Triple-DES 等。
- Stream Cipher，流加密算法：流加密算法将数据按照流的方式逐字节或逐位进行加密。常见的流加密算法包括：RC4、ChaCha20 等。

# 块加密算法
## DES
DES，Data Encryption Standard，使用 56 位密钥对数据进行加密，以 64 位的数据块为单位进行加密操作。

## Triple DES
Triple DES 使用了两次或三次 DES 加密操作来提高加密强度。

## AES
AES，Advanced Encryption Standard，是 DES 和 Triple DES 的现代化替代方案，它使用更长的密钥（128 位、192 位或 256 位），并提供更高的安全性和更好地性能。

OpenSSL 使用示例：
``` Shell
# 加密
openssl enc -aes-128-cbc -in plaintext.txt -out encrypted.txt -iter 1000
# 解密
openssl enc -d -aes-128-cbc -in encrypted.txt -iter 1000
```

# 流加密算法
## RC4
RC4，Rivest Cipher 4，优点是简单且速度快，但存在一些安全性问题，目前已不被推荐使用。

## ChaCha20
ChaCha20, 安全可靠、性能优异。

OpenSSL 使用示例：
``` Shell
openssl enc -chacha20 -in plaintext.txt -out encrypted.txt -iter 1000
openssl enc -chacha20 -d -in encrypted.txt -iter 1000
```
