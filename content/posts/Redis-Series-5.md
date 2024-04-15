+++
title = '2024 阅读 Redis 3.0 源代码：处理客户端请求'
date = 2024-04-03T19:45:58+08:00
+++
2024 年，Redis 已经到了 7.x 的版本了，但是我参考了书籍《Redis 设计与实现》，所以决定使用 3.0 版本的源代码。
<!--more-->
Redis 内部使用单线程+事件循环模型处理客户端请求。

在前面的文章中，我们已经说了：Client Socket 注册的回调函数是 networking.c/readQueryFromClient。（详情见：2024 阅读 Redis 3.0 源代码：服务器端启动）

所以，处理客户端请求的入口即是 readQueryFromClient。

# 读取客户端请求
在 C 语言 Socket 编程中，Client Socket 是一个文件描述符。

首先，Redis Server 需要使用 read 函数从 Client Socket 中读取请求数据：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408162924.png" title="Client Socket Read" >}}

# 解析客户端请求命令
在上一步中，请求数据被保存到了 `c->querybuf`，但此时这些数据还只是字节数组，在执行请求命令之前，还需要先将这些字节数组按照 Redis 客户端/服务器通信协议进行解析，这一步是在 networkings.c/processInputBuffer 中进行的：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408164719.png" title="processInputBuffer" >}}
这一步执行完成后，请求命令中的参数个数会被保存在 `c->argc`，具体的参数会被转换为 `REDIS_STRING Object`。接下来就是执行命令了：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408165444.png" title="processCommand" >}}

# 执行客户端请求命令
Redis Server 在函数 networking.c/processCommand 执行客户端请求命令。

首先，需要先根据请求命令中的第一个参数查找相应的处理函数：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408165941.png" title="lookupCommand" >}}
然后，就是调用 call 函数执行处理函数：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408170637.png" title="call" >}}
进入 call 函数之后，终于到了真正执行处理函数的入口位置：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408171206.png" title="c->cmd->proc(c);" >}}
这里，我执行的是 type 命令，它的处理函数非常简单（从 db 中查找到对应的 key，返回相应的 key 的类型）：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408171449.png" title="typeCommand" >}}
可以看到，返回命令响应的逻辑也在这个函数中，即 `addReplyStatus`。

# 向客户端发送响应
Redis Server 向客户端发送响应，同样是在事件循环中处理的。

首先，把写操作函数 sendReplyToClient 注册到事件循环中：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408173218.png" title="sendReplyToClient" >}}
然后，把响应数据拷贝到 `c->buf` 中：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408173548.png" title="c->buf" >}}
最后，就是在 sendReplyToClient 中真正执行 write 操作了：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240408180137.png" title="write" >}}
