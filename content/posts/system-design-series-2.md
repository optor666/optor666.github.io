+++
title = '设计短链服务'
date = 2024-04-10T21:49:38+08:00
+++
互联网时代，网民经常需要在一些限制字符的平台上分享超链，这时候就要用到短链服务了。
<!--more-->
# 短链服务的实际用途
1. 缩短实际超链的长度，可以节省存储空间；
2. 美化超链，使链接更加简洁、清晰，更容易被人们接受和点击；
3. 易于复制、粘贴和分享，避免超链断裂；
4. 跟踪分析：短链服务可以追踪链接的点击次数、地理位置、来源等信息，有助于了解用户的行为和兴趣，从而进行更精准的营销和内容策略；
# 确认需求
1. 系统平均每秒需要产生多少个短链？
2. 短链可以使用哪些字符？
3. 短链的长度？
4. 短链是否可以被删除或被更新？
# 评估系统容量
1. 写操作 QPS？
2. 读操作 QPS？
3. 每个长链的平均长度？
4. 短链服务的数据需要保持多久？
# API 设计
## 创建短链
```
POST /api/v1/data/shorten
Content-Type: application/json

{
    "longUrl": "longURLString"
}


HTTP/1.1 200 Ok
Content-Type: application/json

{
    "shortUrl": "shortURLString"
}
```
## 查询短链
```
GET /api/v1/${shortUrl}


HTTP/1.1 200 Ok
Content-Type: application/json

{
    "longUrl": "longURLString"
}
```
# 客户端 & 服务器端通信流程
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240410221647.png" title="客户端 & 服务器端通信流程" >}}
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240411111157.png" title="客户端 & 服务器端通信流程" >}}

# 短链生成算法
## 哈希函数
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240411100329.png" title="哈希函数" >}}
### 常见哈希函数
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240411100649.png" title="常见哈希函数" >}}

## 62 进制表示
1. 先生成一个自增 ID
2. 将这个自增 ID 转换为 62 进制数字
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240411105304.png" title="62 进制表示" >}}

## 哈希函数 VS 62 进制表示
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/20240411102735.png" title="哈希函数 VS 62 进制表示" >}}
