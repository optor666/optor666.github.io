+++
title = 'URI, URL, URN 有何区别？'
date = 2023-12-25T20:45:50+08:00
+++
Web 开发中，经常会遇到 URI 和 URL，但却不清楚它们到底有啥区别？
<!--more-->
# URI
URI 的全称是 Uniform Resource Identifier，即统一资源标识符。要理解这个概念，需得先理解这个概念的三个组成成分：
1. 统一：类似于"书同文、车同轨"的效果，人们使用相同的名词指代一件事物，在沟通交流的过程中避免混淆。
2. 资源：不仅可以是 Web 开发中的 HTML 文档、图片、API 等资源，也可以是人类、公司和书籍。
3. 标识符：用于唯一标识某个资源的符号。

由以上概念可知，URI 并非只是 Web 开发领域的概念，它也可以用于现实世界任何行业。

## 示例
- ftp://ftp.is.co.za/rfc/rfc1808.txt
- http://www.ietf.org/rfc/rfc2396.txt
- ldap://[2001:db8::7]/c=GB?objectClass?one
- mailto:John.Doe@example.com
- news:comp.infosystems.www.servers.unix
- tel:+1-816-555-1212
- telnet://192.0.2.16:80/
- urn:oasis:names:specification:docbook:dtd:xml:4.1.2

# URL
URL 全称是 Uniform Resource Locator，即统一资源定位符。

URL 是 URI 的一个特定类型，可以说是 URI 的一个子集。

URL 不仅标识一个资源，而且还描述了资源的位置信息，便于定位和检索这个资源。

URL 包含协议（例如：HTTP、FTP 等）、主机名、端口号、路径等信息，即描述了资源，也描述了位置。

URL 常用于 Web 开发中，用于描述 HTML 文档、图片、API 等资源。

# URN
URN 的全称是 Uniform Resource Name，即统一资源名称。

URN 是 URI 的一个特定类型，也可以说是 URI 的另一个子集。

从全称中可以看出它和 URL 的明显区别，它仅描述资源的名称，而没有资源的位置信息。URN 主要关注于提供一个永久的、全球唯一的标识符，使得资源能够被永久地识别，而不受其位置的影响。

在 URN 中，标识符的格式类似于 "urn:namespace:identifier"，其中 "namespace" 表示命名空间，而 "identifier" 则是在该命名空间下唯一标识资源的字符串。

## 示例
- urn:isbn:0451450523，其中 ISBN 的全称是 "International Standard Book Number"，即国际标准书号。

# 参考
1. [RFC 3986](https://www.rfcreader.com/#rfc3986)
