+++
title = '2024 阅读 Redis 3.0 源代码：服务器端启动'
date = 2024-03-15T19:45:58+08:00
+++
2024 年，Redis 已经到了 7.x 的版本了，但是我参考了书籍《Redis 设计与实现》，所以决定使用 3.0 版本的源代码。
<!--more-->
服务器端 Socket 编程逻辑一般是固定的，经典三板斧：bind、listen、accept。

# bind & listen
Redis 源代码中把 bind 和 listen 这两步放到了一个函数中：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240315163616.png" alt="bind & listen" title="bind & listen" >}}

bind 和 listen 这两步执行完后，会给上层返回一个整数类型的文件描述符，代表 Server Socket。

这里需要注意的是，由于服务器可能有多网卡且同时支持 IPv4 和 IPv6，Server Socket 可能有多个。

# accept
bind 和 listen 只需要在启动时执行一次，但是 accept 确是要执行多次的，毕竟 Redis 要处理多个客户端的连接请求的。

并且，accept 是个异步行为，什么时候能收到客户端连接是不确定地，所以 accept 这一步逻辑是在事件处理循环中的。

在启动过程 redis.c/initServer 中，会将所有的 Server Socket 注册到事件循环中，回调函数指定了函数 acceptTcpHandler：
``` C
/* Create an event handler for accepting new connections in TCP and Unix
 * domain sockets. */
for (j = 0; j < server.ipfd_count; j++) {
    if (aeCreateFileEvent(server.el, server.ipfd[j], AE_READABLE,
        acceptTcpHandler,NULL) == AE_ERR)
        {
            redisPanic(
                "Unrecoverable error creating server.ipfd file event.");
        }
}
```

所以，真正的 accept 操作是在 acceptTcpHandler 函数中调用的：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240315182425.png" title="accept" >}}

accept 这步执行完后，也会给上层返回一个整数类型的文件描述符，代表服务器与客户端通信的 Socket，我们暂且称之为 Client Socket。

接下来就是在 redis.c/acceptCommonHandler > networking.c/createClient 中将 Client Socket 包装为一个 redisClient 结构。同时，也会将 Client Socket 注册到事件循环中，回调函数指定了函数 readQueryFromClient：

{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240318153535.png" title="readQueryFromClient" >}}

至此，服务器端启动流程就完了，接下来就是在事件循环中处理客户端的请求了。
