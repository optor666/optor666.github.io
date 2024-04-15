+++
title = '设计一个聊天系统'
date = 2024-04-09T22:21:29+08:00
+++
网络高度发达的今天，聊天系统非常常见，聊天系统看似简单，但从头做一个聊天系统并不简单！
<!--more-->
# 明确需求
- 是否支持单聊？
- 是否支持群聊？群聊人数限制是？
- 消息类型是否包含富文本（音频、视频、图片）？
- 文本消息长度限制是？
- 是否需要端到端加密？
- 客户端是否包含移动设备 APP？
- 客户端是否包含Web APP？
- 是否支持同时登录多设备？
- 是否支持显示在线状态？
- 是否支持离线 Push 消息通知？
- DAU（daily active users）多少？

# 消息发送者、消息接收者和消息服务之间的关系
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226103627.png" alt="消息发送者、消息接收者和消息服务之间的关系" title="消息发送者、消息接收者和消息服务之间的关系" >}}

# 客户端与服务器之间通信机制
## HTTP Polling
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226104053.png" alt="Polling" title="Polling" >}}

## HTTP Long polling
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226104212.png" alt="Long polling" title="Long polling" >}}

## WebSocket
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226104519.png" alt="WebSocket" title="WebSocket" >}}
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226104617.png" alt="WebSocket" title="WebSocket" >}}

# 服务模块设计
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226105327.png" alt="服务模块设计" title="服务模块设计" >}}

# 弹性服务设计
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226105851.png" alt="弹性服务设计" title="弹性服务设计" >}}

# 数据模型
## 单聊消息
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226110124.png" alt="单聊消息" title="单聊消息" >}}

## 群聊消息
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226110223.png" alt="群聊消息" title="群聊消息" >}}

# 设计细节
## 服务发现
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226110641.png" alt="服务发现" title="服务发现" >}}

## 发送消息流程
### 单聊消息
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226110911.png" alt="单聊消息" title="单聊消息" >}}

### 多设备消息同步
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226111200.png" alt="多设备消息同步" title="多设备消息同步" >}}

### 群聊消息
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226113758.png" alt="群聊消息" title="群聊消息" >}}
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226113903.png" alt="群聊消息" title="群聊消息" >}}

## 在线状态
### 用户登录
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226114121.png" alt="用户登录" title="用户登录" >}}

### 用户退出
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226114237.png" alt="用户退出" title="用户退出" >}}

### 心跳检测
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226114555.png" alt="心跳检测" title="心跳检测" >}}

### 好友视角
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240226114719.png" alt="好友视角" title="好友视角" >}}

# 参考
- 《System Design Interview - An Insiders Guide》by Alex Xu, Chapter 12 - Design A Chat System
- [从无到有：微信后台系统的演进之路](https://www.infoq.cn/article/the-road-of-the-growth-weixin-background)
- [5 亿用户如何高效沟通？钉钉首次对外揭秘即时消息服务 DTIM](https://www.infoq.cn/article/NfyEB9CF6qL3MQO8CfUL)
