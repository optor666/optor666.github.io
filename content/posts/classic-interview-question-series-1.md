+++
title = 'LRU & LFU'
date = 2024-04-12T20:20:16+08:00
+++
一道好的面试题，即能彰显面试官的专业，亦能检验应聘者的专业能力。
<!--more-->
# LRU
相比于 LFU，LRU 更简单一些，直接使用经典实现方案：HashMap + Doubly Linked List 即可！

# LFU
首先，直接使用 LRU 的方案：HashMap + Doubly Linked List 也可以实现 LFU，主要改动点就是在链表结点中增加频率属性。

但是，这种方案在频率相同的节点数较多时效率较差，更优一点的方案是：再增加一个 HashMap，key 是频率，value 是该频率的双向链表。

# 自我校验
如何校验自己是否能徒手写出 Bug Free 的实现代码呢？直接上 LeetCode 即可！
1. [LeetCode 146.LRU Cache](https://leetcode.com/problems/lru-cache/)
2. [LeetCode 460.LFU Cache](https://leetcode.com/problems/lfu-cache/description/)

在 LeetCode 上同时可以检验不同方案的耗时，LeetCode 万岁！
