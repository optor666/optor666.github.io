+++
title = 'Java 日志江湖'
date = 2017-12-22T20:37:43+08:00
+++
在应用程序开发中，日志记录是一个至关重要的组成部分。它不仅可以帮助开发人员在调试和排查问题时更轻松地追踪代码的执行流程，还能记录系统运行时的关键信息。
<!--more-->
但 Java 相关的日志框架却有多套，而且它们之间也有一些恩怨纠缠，像是一个江湖😂
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20231222174353.png" alt="Java 日志江湖" title="Java 日志江湖" >}}

# SLF4J
SLF4J 的全称是 Simple Logging Facade for Java，它是一个为各种 Java 日志框架提供统一抽象层的简单门面，主要是定义日志框架核心概念的接口，旨在让开发者能够无缝切换不同的日志实现而无需修改应用代码。

{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20231225144432.png" alt="SLF4J 源代码" title="SLF4J 源代码" >}}

SLF4J 核心模块是 slf4j-api，从上面源代码截图中，可以看出：SLF4J 的核心模块代码量非常少，主要就是定义了一个日志框架的几个核心概念接口：
1. ILoggerFactory 接口，日志记录器工厂，用于获取日志记录器 Logger 实例，该接口在 Logback 中的实现为 LoggerContext。
2. Logger 接口，日志记录器，用于在应用代码中记录日志，该接口在 Logback 中的实现类名也是 Logger。
3. LoggingEvent 接口，日志消息，表示应用中记录的一条日志，该接口在 Logback 中没有使用，Logback 内部使用接口 ILoggingEvent 进行了重新定义。
4. LoggerFactory 类并非核心概念定义，该类是日志框架向应用层提供的一个工具类，允许应用层通过该类获取日志记录器 Logger，并且，应用层通过该类第一次获取日志记录器时，该类内部会进行初始化并绑定日志框架的实现。

# Logback
Logback 项目是由 SLF4J 的创始人 Ceki Gülcü 开发的一个 SLF4J 实现。Logback 提供了强大而灵活的特性，使得开发者能够细致地控制日志的行为。

{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20231225152152.png" alt="Logback 源代码" title="Logback 源代码" >}}

从 Logback 项目源代码结构可以看出，它主要分为三个模块：logback-core、logback-classic 和 logback-acess。

## logback-core
从模块名字可以看出 logback-core 是核心功能模块，是 logback 项目的基础。

它定义了一个日志框架实现的几个核心概念接口：
1. Layout 接口，用于格式化日志消息。（现在建议使用 Encoder。）
2. Appender 接口，用于将日志消息输出到目的地。

## logback-classic
logback-classic 模块实现了 slf4j-api、logback-core 两个模块定义的核心接口，这三个模块结合在一起就是一个完整的日志框架库。

值得注意的是，logback-classic 模块自身的 Maven 依赖中已经引入了 slf4j-api 和 logback-core 这两个模块，所以 Maven 项目使用 SLF4J + Logback 这套日志框架的话，只需要引入 logback-classic 模块依赖即可。

## logback-access
logback-access 模块为 Java Web 应用提供 HTTP 访问日志的支持，允许记录与 HTTP 请求和响应相关的信息，例如：客户端的 IP 地址、请求方法、URL 等。

# tinylog
tinylog 是一个轻量级的日志框架，它的设计目标是简单易用，同时保持足够的灵活性和性能。

# 参考
1. [http://www.slf4j.org/](http://www.slf4j.org/)
2. [https://github.com/qos-ch/slf4j](https://github.com/qos-ch/slf4j)
3. [https://logback.qos.ch/](https://logback.qos.ch/)
4. [https://github.com/qos-ch/logback](https://github.com/qos-ch/logback)
5. [https://tinylog.org/v2/](https://tinylog.org/v2/)
