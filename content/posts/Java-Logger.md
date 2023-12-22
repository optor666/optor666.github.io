+++
title = 'Java 日志江湖'
date = 2023-12-22T17:37:43+08:00
+++
在应用程序开发中，日志记录是一个至关重要的组成部分。它不仅可以帮助开发人员在调试和排查问题时更轻松地追踪代码的执行流程，还能记录系统运行时的关键信息。
<!--more-->
但 Java 相关的日志框架却有多套，而且它们之间也有一些恩怨纠缠，像是一个江湖😂
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20231222174353.png" alt="Java 日志江湖" title="Java 日志江湖" >}}

# SLF4J
SLF4J 的全称是 Simple Logging Facade for Java，它是一个为各种 Java 日志框架提供统一抽象层的简单门面，旨在让开发者能够无缝切换不同的日志实现而不需要修改应用代码。

# Logback
Logback 是由 SLF4J 的创始人 Ceki Gülcü 开发的一个 SLF4J 实现。Logback 提供了强大而灵活的特性，使得开发者能够细致地控制日志的行为。

从[Logback项目源代码结构](https://github.com/qos-ch/logback/tree/master)可以看出，它分为多个模块：logback-core、logback-classic 和 logback-acess。其中，从名字也可以看出 logback-core 是核心功能模块，是 logback 项目的基础。它定义了日志时间和日志上下文等基本概念，并提供了一些基础设施，比如 Layout 接口（用于格式化日志消息）、Appender 接口（用于将日志消息输出到目的地）等。

logback-classic 模块在 logback-core 的基础上实现了 SLF4J 定义的门面接口。Maven 项目如果要使用 SLF4J + Logback 这套搭配记录日志的话，只需要引入特定版本的 logback-classic 模块依赖即可，logback-classic 本身已经引入了相关版本的 slf4j-api 依赖和 logback-core 依赖。

logback-access 为 Java Web 应用提供 HTTP 访问日志的支持，允许记录与 HTTP 请求和响应相关的信息，例如：客户端的 IP 地址、请求方法、URL 等信息。

# tinylog
tinylog 是一个轻量级的日志框架，它的设计目标是简单易用，同时保持足够的灵活性和性能。

# 参考
1. [http://www.slf4j.org/](http://www.slf4j.org/)
2. [https://github.com/qos-ch/slf4j](https://github.com/qos-ch/slf4j)
3. [https://logback.qos.ch/](https://logback.qos.ch/)
4. [https://github.com/qos-ch/logback](https://github.com/qos-ch/logback)
5. [https://tinylog.org/v2/](https://tinylog.org/v2/)
