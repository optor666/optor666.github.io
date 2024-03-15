+++
title = '2024 阅读 Redis 3.0 源代码：服务器端启动'
date = 2024-03-15T16:45:58+08:00
+++
2024 年，Redis 已经到了 7.x 的版本了，但是我参考了书籍《Redis 设计与实现》，所以决定使用 3.0 版本的源代码。
<!--more-->
服务器端 Socket 编程逻辑一般是固定的，经典三板斧：bind、listen、accept。

# bind & listen
Redis 源代码中把 bind 和 listen 这两步放到了一个函数中：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240315163616.png" alt="bind & listen" title="bind & listen" >}}

bind 和 listen 这两步执行完后，会给上层返回一个整数类型的文件描述符，代表 Server Socket。

# accept
bind 和 listen 只需要在启动时执行一次，但是 accept 确是要执行多次的，毕竟 Redis 要处理多个客户端的连接请求的。

并且，accept 是个异步行为，什么时候能收到客户端连接是不确定地，所以 accept 这一步逻辑是在事件处理循环中的：

{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240315182425.png" title="accept" >}}

至此，服务器端启动流程就完了，接下来就是事件处理循环了。
