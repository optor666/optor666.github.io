+++
title = 'Tomcat 源码分析'
date = 2024-02-27T10:35:41+08:00
+++
Apache Tomcat（简称 Tomcat）是一个开源的、轻量级的 Web 应用服务器，属于 Java Servlet 和 JavaServer Pages（JSP）规范的实现之一。
<!--more-->
# IO 组件
## SocketBufferHandler 组件
SocketBufferHandler 组件较为简单，主要是封装了内部两个 ByteBuffer 字段（readBuffer、writeBuffer）相关读写操作。

每个客户端 Socket 连接都会有一个对应的 SocketBufferHandler 组件实例。

## SocketWrapper 组件
SocketWrapper 组件主要用于封装一个 NioChannel 组件（一个 NioChannel 组件封装了一个客户端 Socket 连接），每个客户端 Socket 连接都会有一个对应的 SocketWrapper 组件。

SocketWrapper 组件除了封装一个客户端 Socket 连接之外，同时也包含 Endpoint 组件引用，方便后续处理客户端 Socket 连接读写逻辑中使用。

## NioChannel 组件
NioChannel 组件主要是用于封装客户端 Socket 连接。其聚合了三个子组件：
1. 客户端 Socket 连接 SocketChannel 组件 sc，朴实无华的客户端 Socket 连接；
2. SocketBufferHandler 组件 bufHandler，主要用于处理客户端 Socket 连接读写操作；
3. NioSocketWrapper 组件 socketWrapper，SocketWrapper 组件和 NioChannel 组件是循环引用；

每个客户端 Socket 连接都会有一个对应的 NioChannel 组件实例。

# 连接器
## Endpoint 组件
Endpoint 组件类图如下：

### Endpoint 组件的生命周期
Endpoint 组件虽然没有像 Catalina 容器组件一样实现生命周期接口 Lifecycle，但在其顶层抽象类 AbstractEndpoint 中也包含了 start、stop 等生命周期相关方法。
#### start
1. 调用 bind 方法，创建 ServerSocket 并绑定到服务端口；


## Acceptor 组件
Acceptor 组件比较简单，只有一个 Acceptor 类，它主要是为 Endpoint 组件服务，每个 Endpoint 组件实例都会有一个相应的 Acceptor 组件实例。

Acceptor 组件作为一个线程运行，在无限循环中执行如下逻辑：
1. 调用 endpoint.serverSocketAccept() 方法接受客户端连接；
2. 调用 endpoint.setSocketOptions() 方法处理客户端连接，setSocketOptions 方法中主要处理逻辑如下：
    - 将客户端 socket 连接封装为 SocketWrapper；
    - 将 SocketWrapper 封装为 PollerEvent 实例添加到 Poller 组件的事件队列 events 中；

从以上分析可以看出，Acceptor 组件仅仅是接受客户端连接，然后交给 Poller 组件处理，Acceptor 本身并不会对客户端连接进行处理。

## Poller 组件
Poller 组件只有一个 Poller 类，且定义在 AbstractEdnpoint 类子类内部，它主要是为 Endpoint 组件服务，每个 Endpoint 组件实例都会有一个相应的 Poller 组件实例。

Poller 组件内部包含两个子组件：
- 存储 PollerEvent 事件的队列 events
- 处理客户端连接读写事件的 Selector
