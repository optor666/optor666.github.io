+++
title = 'Tomcat 源码分析'
date = 2024-02-27T20:35:41+08:00
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

从以上分析可以看出，Acceptor 组件以单线程的方式接受客户端连接，然后交给 Poller 组件处理。Acceptor 仅接受客户端连接，不处理客户端连接。

在 Acceptor 组件中，一个 JDK 级别的客户端连接 SocketChannel 先被包装为 NioChannel 的形式，然后又被包装成了 SocketWrapper（= NioChannel + SocketBufferHandler）的形式，以 SocketWrapper 传递给 Poller 组件。

## Poller 组件
Poller 组件只有一个 Poller 类，且定义在 AbstractEdnpoint 类子类内部，它主要是为 Endpoint 组件服务，每个 Endpoint 组件实例都会有一个相应的 Poller 组件实例。

Poller 组件内部包含两个子组件：
- 存储 PollerEvent 事件的队列 events
- 处理客户端连接读写事件的 Selector

Poller 组件作为一个线程运行，在无限循环中执行如下逻辑：
1. 处理事件队列 events 中的 PollerEvent 事件，将 PollerEvent 事件中包含的客户端连接注册到 Selector 上
2. 基于 Selector，处理客户端连接读写事件
3. 调用 endpoint.processSocket() 方法处理可读、可写状态下的客户端连接，processSocket 方法主要处理逻辑如下：
    - 将 SocketWrapper 和表示读写事件的 SocketEvent 包装成 SocketProcessor 形式
    - 将 SocketProcessor 交给 Endpoint 的线程池 executor 运行，SocketProcessor 的运行逻辑是调用 Endpoint 组件的 Handler 组件处理 SocketWrapper 和 SocketEvent，而 Handler 组件的实现是在 Coyote 容器中的，Handler 组件是可以说是 Tomcat 网络层到 Coyote 容器层的一个接口。

从以上分析可以看出，Poller 组件以单线程的方式检测所有客户端连接上的读写事件，将有读写事件的客户端连接交给线程池处理。Poller 组件仅检测客户端连接读写事件，不处理客户端连接。

# Coyote 容器
## ConnectionHandler 组件
ConnectionHandler 组件定义在 AbstractProtocol 类中，它实现了接口 AbstractEndpoint.Handler，其 process 方法处理网络层传下来的 SocketWrapper 和 SocketEvent。

在 process 方法中，SocketWrapper 和 SocketEvent 又被交给 Processor 组件的 process 方法处理。

## Processor 组件
Processor 组件的 process 方法中，会将 SocketWrapper 交由 service 方法处理。

在 service 方法中，SocketWrapper 会被绑定到 Processor 组件上，所以一个 Processor 组件某一时刻只能处理一个客户端连接。

Processor 组件两个股肱之臣是 Http11InputBuffer 和 Http11OutputBuffer。

Http11InputBuffer 中，重点是 Coyote Request 属性，该属性保存了 HTTP 请求报文。

service 方法主要逻辑如下：
1. 调用 inputBuffer.parseRequestLine 方法，解析 HTTP 请求行
2. 调用 isConnectionToken(request.getMimeHeaders(), "upgrade") 方法，检查是否是升级请求
3. 解析 HTTP 请求头？哪里解析的？
4. prepareRequest() 方法中处理 HTTP 功能性请求头：Connection、transfer-encoding、Host
5. 调用 Coyote Adapter 组件的 service 方法处理 Coyote Request 和 Coyote Response。在这里，客户端连接 SocketWrapper 被转换为 Coyote Request 和 Coyote Response。Coyote Adapter 组件由 Catalina 容器的 CoyoteAdapter 实现，从此处理流程就离开了 Coyote 容器，进入了 Cataline 容器。在 Cataline 容器中，只能看到 Coyote Request 和 Coyote Response，而看不到表示客户端连接的 SocketWrapper 组件了。

# Catalina 容器
## CoyoteAdapter 组件
CoyoteAdapter 组件实现了 Coyote 容器中的 Adapter 方法，是 Coyote 容器和 Catalina 容器的衔接层。

客户端连接 SocketWrapper 在 Coyote 容器中被转换为 Coyote Request 和 Coyote Response，然后交给 CoyoteAdapter 组件的 service 方法处理。

CoyoteAdapter 组件的 service 方法逻辑如下：
1. 调用 Mapper 组件，根据请求头中的 Host 为请求关联相应的 Host 容器
2. 调用 Mapper 组件，根据请求 URI 中的第一段为请求关联相应的 Context 容器
3. 调用 Mapper 组件，根据请求 URI 中除了第一段之后的内容为请求关联相应的 Wrapper 容器
4. 执行 Engine 容器的 Pipeline 上的 Valve 组件 StandardEngineValve

## Valve 组件
### StandardEngineValve 组件
StandardEngineValve 组件逻辑比较简单：从 Catalina Request 上取得关联的 Host 容器，执行 Host 容器的 Pipeline 上的 Valve 组件。

### StandardHostValve 组件
StandardHostValve 组件逻辑比较简单：从 Catalina Request 上取得关联的 Context 容器，执行 Context 容器的 Pipeline 上的 Valve 组件。

### StandardContextValve 组件
StandardContextValve 组件逻辑比较简单：从 Catalina Request 上取得关联的 Wrapper 容器，执行 Wrapper 容器的 Pipeline 上的 Valve 组件。

### StandardWrapperValve 组件
StandardWrapperValve 组件逻辑如下：
1. 从自身 Valve 上取得关联的 Wrapper 容器
2. 分配一个 Servlet 实例，此 Servlet 实例通常便是用户继承 HttpServlet 实现业务逻辑的 Servlet
3. 创建 FilterChain，执行 FilterChain 的 doFilter 方法，此 FilterChain 中包含了 Tomcat 内置的 Filter 以及用户实现业务逻辑的 Filter。而且，在执行 doFilter 方法时，Catalina Request 和 Catalina Response 被转换为了 ServletRequest 和 ServletResponse

## FilterChain 组件
请求通过 FilterChain 所有 Filter 之后，FilterChain 会执行业务逻辑的 Servlet 的 service 方法。此时，就开始真正执行用户业务逻辑代码了
