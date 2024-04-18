+++
title = '2024 阅读 Redis 3.0 源代码：对象'
date = 2024-04-03T19:45:58+08:00
+++
2024 年，Redis 已经到了 7.x 的版本了，但是我参考了书籍《Redis 设计与实现》，所以决定使用 3.0 版本的源代码。
<!--more-->
Redis 内部对象系统在内部使用八种编码，对外部提供五种数据类型。

Redis 内部对象系统使用了 C 语言的结构体：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240409151838.png" title="redisObject" >}}

# 八种编码
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240409151436.png" title="八种编码" >}}
查询某个 key 的编码使用如下命令：
``` 
object encoding keyname
```

# 五种数据类型
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240409151720.png" title="五种数据类型" >}}

查询某个 key 的类型使用如下命令：
``` 
type keyname
```
每种数据类型底层可以使用哪些编码？
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240409152048.png" title="五种数据类型底层编码实现" >}}
`REDIS_ENCODING_EMBSTR` 和 `REDIS_ENCODING_RAW` 相比有啥优势？
1. 减少申请、释放内存的次数；
2. 分配连续内存，更利于 CPU 缓存；

这些优势，只有在字符串较短时才能体现出来。
