+++
title = 'Jerry'
date = 2024-04-12T20:18:42+08:00
draft = true
+++
外企远程开发岗，Jerry！
<!--more-->
公司主页：https://getjerry.com/

# 2023
2023 年第一道笔试题 Range List 都未通过。。。

# 2024
## Coding test
线下笔试题，两天时间内提交，题目 Intensity Segment：[https://github.com/optor666/intensity-segments-management](https://github.com/optor666/intensity-segments-management)

收获四条求职建议：
1. [https://theundercoverrecruiter.com/4-vital-resume-tips-career-pros/](https://theundercoverrecruiter.com/4-vital-resume-tips-career-pros/)
2. [https://www.forbes.com/sites/forbescoachescouncil/2020/10/22/14-practical-steps-to-establish-a-standout-personal-brand-online/](https://www.forbes.com/sites/forbescoachescouncil/2020/10/22/14-practical-steps-to-establish-a-standout-personal-brand-online/)
3. [https://www.glassdoor.com/blog/job-hunting-techniques/](https://www.glassdoor.com/blog/job-hunting-techniques/)
4. [https://www.linkedin.com/pulse/20131217153926-658789-how-to-find-a-company-you-ll-love-working-for/](https://www.linkedin.com/pulse/20131217153926-658789-how-to-find-a-company-you-ll-love-working-for/)

## HR Screen
与 HR 沟通三观，一小时左右

## Tech1 - Coding Skill
45 分钟左右，两道算法题。

1. getNumberFromTheArray: [https://gist.github.com/optor666/720d7b6b35fbf42d52f46bd263689560](https://gist.github.com/optor666/720d7b6b35fbf42d52f46bd263689560)
2. [https://leetcode.com/problems/find-champion-i/](https://leetcode.com/problems/find-champion-i/)

## Tech2 - System Design
设计一个 Restful 风格的查询保险方案的 API，该 API 内部实现是调用两百多家保险公司的 API 获取数据，聚合这些数据，然后返回。

难点是：这两百多家保险公司的 API 响应时间从 1s - 2min 不等，但我们的 API 实现不能阻塞。

重点关注：使用场景、API 实现、存储、伸缩性。

我的方案是：收到请求时，生成一个请求 ID，多线程调用两百多家保险公司的 API，然后把响应数据写入 Redis 中，key 为请求 ID，value 是 List 类型保存多个响应数据；同时，使用另外一个 key 记录下来当前返回给客户端的数据在 List 中的索引位置。

白板绘图：
{{< optorImg src="https://blog-1259456425.cos.ap-beijing.myqcloud.com/draw.png" title="System Design" >}}

## Tech3 - General Experience
60 分钟左右，尚未参加。

## Non Tech - Cultural Fit
60 分钟左右，尚未参加。

## 收获一些常见系统设计问题
1. [https://www.educative.io/blog/top-10-system-design-interview-questions](https://www.educative.io/blog/top-10-system-design-interview-questions)
